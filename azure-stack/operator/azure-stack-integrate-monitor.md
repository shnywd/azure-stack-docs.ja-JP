---
title: Azure Stack と外部の監視ソリューションとの統合 | Microsoft Docs
description: Azure Stack とデータセンターの外部の監視ソリューションを統合する方法を説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 06/05/2019
ms.author: jeffgilb
ms.reviewer: thoroet
ms.lastreviewed: 06/05/2019
ms.openlocfilehash: 489859720df8a2d0c20bb476b285fe9cb65b797e
ms.sourcegitcommit: 3af71025e85fc53ce529de2f6a5c396b806121ed
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/20/2019
ms.locfileid: "71159628"
---
# <a name="integrate-external-monitoring-solution-with-azure-stack"></a>Azure Stack と外部の監視ソリューションとの統合

Azure Stack インフラストラクチャの外部の監視のためには、Azure Stack ソフトウェア、物理コンピューター、物理ネットワーク スイッチを監視する必要があります。 これらにはそれぞれ、正常性とアラートの情報を取得する方法があります。

- Azure Stack ソフトウェアでは、REST ベースの API を使用して正常性とアラートの情報を取得します。 記憶域スペース ダイレクトのようなソフトウェアによるテクノロジを使用すると、ストレージの正常性およびアラートはソフトウェアの監視に含まれます。
- 物理コンピューターでは、ベースボード管理コントローラー (BMC) を通して、正常性とアラートの情報を取得できます。
- 物理ネットワーク デバイスでは、SNMP プロトコルを通して、正常性とアラートの情報を取得できます。

各 Azure Stack ソリューションは、ハードウェア ライフサイクル ホストに含まれています。 このホストは、物理サーバーとネットワーク デバイス用に OEM (Original Equipment Manufacturer) ハードウェア ベンダーの監視ソフトウェアを実行します。 OEM プロバイダーに、そのプロバイダーの監視ソリューションをデータセンター内の既存の監視ソリューションと統合できるかどうかを確認してください。

> [!IMPORTANT]
> 使用する外部の監視ソリューションは、エージェントレスである必要があります。 Azure Stack コンポーネント内にサード パーティ製エージェントをインストールすることはできません。

次の図は、Azure Stack 統合システム、ハードウェア ライフサイクル ホスト、外部の監視ソリューション、外部のチケット発行/データ収集システムの間のトラフィック フローを示しています。

![Azure Stack 監視とチケット発行ソリューションとの間のトラフィックを示す図。](media/azure-stack-integrate-monitor/MonitoringIntegration.png)  

> [!NOTE]
> 物理サーバーとの直接の外部監視統合は許可されておらず、アクセス制御リスト (ACL) によって積極的にブロックされています。  物理ネットワーク デバイスとの直接の外部監視統合はサポートされているので、この機能を有効にする方法については OEM プロバイダーに確認してください。

この記事では、Azure Stack を System Center Operations Manager や Nagios などの外部の監視ソリューションと統合する方法を説明します。 PowerShell を使用して、または REST API 呼び出しを介して、アラートをプログラムで操作する方法も説明します。

## <a name="integrate-with-operations-manager"></a>Operations Manager との統合

Azure Stack の外部の監視に Operations Manager を使用できます。 Microsoft Azure Stack 用 System Center 管理パックを使用すれば、複数の Azure Stack デプロイを 1 つの Operations Manager インスタンスで監視できます。 この管理パックは、正常性リソースプロバイダーと更新リソースプロバイダーの REST API を使用して Azure Stack と通信します。 ハードウェア ライフサイクル ホストで実行されている OEM 監視ソフトウェアを使用しない場合は、ベンダー管理パックをインストールして物理サーバーを監視できます。 Operations Manager のネットワーク デバイス検出機能を使用して、ネットワーク スイッチを監視することもできます。

Azure Stack 用管理パックには、次の機能があります。

- 複数の Azure Stack デプロイを監視できます
- Azure Active Directory (Azure AD) と Active Directory フェデレーション サービス (AD FS) をサポートしています
- アラートを取得したり終了したりできます
- 正常性と容量についてのダッシュボードがあります
- パッチおよび更新プログラム (P&U) 進行時の自動メンテナンス モード検出機能を備えています
- デプロイ用とリージョン用の強制更新タスクが含まれます
- リージョンにカスタム情報を追加できます
- 通知とレポートがサポートされています

Microsoft Azure Stack 用 System Center 管理パックと、関連するユーザー ガイドを[こちら](https://www.microsoft.com/en-us/download/details.aspx?id=55184)から、または Operations Manager から直接ダウンロードできます。

チケット発行ソリューションについては、System Center Service Manager と Operations Manager を統合できます。 統合された製品コネクタによって双方向通信が可能になり、それによって、Service Manager のサービス要求を解決したあとに Azure Stack と Operations Manager のアラートを終了できます。

次の図は、既存の System Center デプロイと Azure Stack の統合を示しています。 System Center Orchestrator または Service Management Automation (SMA) を使用してさらに Service Manager を自動化し、Azure Stack 内で操作を実行できます。

![OM、Service Manager、SMA との統合を示す図。](media/azure-stack-integrate-monitor/SystemCenterIntegration.png)

## <a name="integrate-with-nagios"></a>Nagios との統合

Microsoft Azure Stack 用に Nagios プラグインを設定して構成することができます。

Nagios 監視プラグインは、制約のない無料ソフトウェア ライセンスである MIT (Massachusetts Institute of Technology) ライセンスの下で使用できるパートナー クラウドベース ソリューションと合わせて開発されました。

このプラグインは Python で書かれており、正常性リソースプロバイダーの REST API を使用します。 また、Azure Stack でアラートを取得したり終了したりする基本的な機能を提供します。 System Center 管理パックと同じように、このプラグインで複数の Azure Stack デプロイを追加したり、通知を送信したりすることが可能になります。

バージョン 1.2 の場合、Azure Stack - Nagios プラグインでは Microsoft ADAL ライブラリを活用し、シークレットまたは証明書を使用したサービス プリンシパルを利用する認証をサポートします。 また、構成は、新しいパラメーターを含む単一の構成ファイルを使用して簡略化されています。 現在、ID システムとして Azure AD と AD FS を使用した、Azure Stack のデプロイがサポートされています。

プラグインは Nagios 4x および XI で動作します。 [こちら](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)からダウンロードできます。 このダウンロード サイトでインストールと詳細な構成も行えます。

### <a name="requirements-for-nagios"></a>Nagios の要件

1.  Nagios の最小バージョンは 4.x です

2.  Microsoft Azure Active Directory Python ライブラリ。 これは、Python PIP を使用してインストールできます。

    ```bash  
    sudo pip install adal pyyaml six
    ```

### <a name="install-plugin"></a>プラグインをインストールする

このセクションでは、Nagios の既定のインストールと仮定し、Azure Stack プラグインをインストールする方法について説明します。

このプラグイン パッケージには、次のファイルが含まれています。

```
azurestack_plugin.py
azurestack_handler.sh
samples/etc/azurestack.cfg
samples/etc/azurestack_commands.cfg
samples/etc/azurestack_contacts.cfg
samples/etc/azurestack_hosts.cfg
samples/etc/azurestack_services.cfg
```

1.  プラグイン `azurestack_plugin.py` をディレクトリ `/usr/local/nagios/libexec` にコピーします。

2.  ハンドラー `azurestack_handler.sh` をディレクトリ `/usr/local/nagios/libexec/eventhandlers` にコピーします。

3.  プラグイン ファイルが実行可能ファイルとして設定されていることを確認します

    ```bash
    sudo cp azurestack_plugin.py <PLUGINS_DIR>
    sudo chmod +x <PLUGINS_DIR>/azurestack_plugin.py
    ```

### <a name="configure-plugin"></a>プラグインを構成する

次のパラメーターは azurestack.cfg ファイルで構成できます。 太字のパラメーターは、選択した認証モデルとは別に構成する必要があります。

SPN の作成方法に関する詳細情報は、[ここ](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals)に記載されています。

| パラメーター | 説明 | 認証 |
| --- | --- | --- |
| **External_domain_fqdn ** | 外部ドメイン FQDN |    |
| **region: ** | リージョン名 |    |
| **tenant_id: ** | テナント ID\* |    |
| client_id: | クライアント ID | シークレットを使用する SPN |
| client_secret: | クライアント パスワード | シークレットを使用する SPN |
| client_cert\*\*: | 証明書へのパス | 証明書を使用する SPN |
| client_cert_thumbprint\*\*: | 証明書のサムプリント | 証明書を使用する SPN |

\*テナント ID は、ADFS を使用する Azure Stack デプロイでは必要ありません。

\*\* クライアント シークレットとクライアント証明書は、相互に排他的です。

その他の構成ファイルには、Nagios でも構成できるため、オプションの構成設定が含まれます。

> [!Note]  
> azurestack_hosts.cfg と azurestack_services.cfg で目的の場所を確認します。

| 構成 | 説明 |
| --- | --- |
| azurestack_commands.cfg | ハンドラー構成を変更する必要はありません |
| azurestack_contacts.cfg | 通知設定 |
| azurestack_hosts.cfg | Azure Stack デプロイの名前付け |
| azurestack_services.cfg | サービスの構成 |

### <a name="setup-steps"></a>セットアップの手順

1.  構成ファイルを変更します

2.  フォルダー `/usr/local/nagios/etc/objects` に変更された構成ファイルをコピーします。

### <a name="update-nagios-configuration"></a>Nagios 構成を更新する

Azure Stack – Nagios プラグインが確実に読み込まれるように、Nagios 構成を更新する必要があります。

1.  次のファイルを開きます

```bash  
/usr/local/nagios/etc/nagios.cfg
```

2.  次のエントリを追加します

```bash  
# Load the Azure Stack Plugin Configuration
cfg_file=/usr/local/Nagios/etc/objects/azurestack_contacts.cfg
cfg_file=/usr/local/Nagios/etc/objects/azurestack_commands.cfg
cfg_file=/usr/local/Nagios/etc/objects/azurestack_hosts.cfg
cfg_file=/usr/local/Nagios/etc/objects/azurestack_services.cfg
```

3.  Nagios を再度読み込みます

```bash  
sudo service nagios reload
```

### <a name="manually-close-active-alerts"></a>手動でアクティブなアラートを閉じる

カスタム通知機能を使用して、Nagios 内でアクティブなアラートを閉じることができます。 以下のようなカスタム通知である必要があります。

```
/close-alert <ALERT_GUID>
```

ターミナルを使って、次のコマンドを使用してアラートを閉じることもできます。

```bash
/usr/local/nagios/libexec/azurestack_plugin.py --config-file /usr/local/nagios/etc/objects/azurestack.cfg --action Close --alert-id <ALERT_GUID>
```

### <a name="troubleshooting"></a>トラブルシューティング

プラグインのトラブルシューティングは、ターミナルでプラグインを手動で呼び出して行うことができます。 次のメソッドを使用します。

```bash
/usr/local/nagios/libexec/azurestack_plugin.py --config-file /usr/local/nagios/etc/objects/azurestack.cfg --action Monitor
```

## <a name="use-powershell-to-monitor-health-and-alerts"></a>PowerShell を使用した正常性とアラートの監視

Operations Manager、Nagios、または Nagios ベースのソリューションを使用していない場合は、PowerShell でさまざまな監視ソリューションを使用して Azure Stack と統合できます。

1. PowerShell を使用するには、Azure Stack オペレーター環境用に [PowerShell がインストールされ構成されている](azure-stack-powershell-install.md)必要があります。 Resource Manager (管理者) エンドポイント (https://adminmanagement.[region].[External_FQDN]) にアクセスできるローカル コンピューターに PowerShell をインストールします。

2. 次のコマンドを実行して、Azure Stack オペレーターとして Azure Stack 環境に接続します。

   ```powershell
   Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint https://adminmanagement.[Region].[External_FQDN] `
      -AzureKeyVaultDnsSuffix adminvault.[Region].[External_FQDN] `
      -AzureKeyVaultServiceEndpointResourceId https://adminvault.[Region].[External_FQDN]

   Connect-AzureRmAccount -EnvironmentName "AzureStackAdmin"
   ```

3. 次の例のようなコマンドを使用して、アラートを操作します。
   ```powershell
    # Retrieve all alerts
    $Alerts = Get-AzsAlert
    $Alerts

    # Filter for active alerts
    $Active = $Alerts | Where-Object { $_.State -eq "active" }
    $Active

    # Close alert
    Close-AzsAlert -AlertID "ID"

    #Retrieve resource provider health
    $RPHealth = Get-AzsRPHealth
    $RPHealth

    # Retrieve infrastructure role instance health
    $FRPID = $RPHealth | Where-Object { $_.DisplayName -eq "Capacity" }
    Get-AzsRegistrationHealth -ServiceRegistrationId $FRPID.RegistrationId
    ```

## <a name="learn-more"></a>詳細情報

組み込みの正常性監視の詳細については、「[Azure Stack での正常性およびアラートの監視](azure-stack-monitor-health.md)」をご覧ください。

## <a name="next-steps"></a>次の手順

[セキュリティの統合](azure-stack-integrate-security.md)
