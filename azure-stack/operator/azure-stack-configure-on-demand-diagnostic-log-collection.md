---
title: 今すぐ Azure Stack 診断ログを収集する |Microsoft Docs
description: Azure Stack ヘルプとサポートでオンデマンドの診断ログ収集を構成する方法。
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 07/31/2019
ms.openlocfilehash: 988c24f004222d80b866024c2c392473cdf73bab
ms.sourcegitcommit: ddb625bb01de11bfb75d9f7a1cc61d5814b3bc31
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/01/2019
ms.locfileid: "68712954"
---
# <a name="collect-azure-stack-diagnostic-logs-now-on-demand"></a>今すぐ Azure Stack 診断ログを収集する (オンデマンド)

*適用対象:Azure Stack 統合システム*

トラブルシューティングの一環として、Microsoft カスタマー サポート サービス (CSS) が診断ログを分析する必要がある場合があります。 1907 リリース以降、Azure Stack オペレーターは、**ヘルプとサポート**を使用して、オンデマンドの診断ログを Azure の BLOB コンテナーにアップロードできます。 ポータルが使用できない場合の別の方法として、オペレーターは特権エンドポイント (PEP) 経由で Get-AzureStackLog を使用してログを収集できます。 このトピックでは、オンデマンドで診断ログを収集するための両方の方法について説明します。

## <a name="using-help-and-support"></a>ヘルプとサポートの使用

問題のトラブルシューティングを行うために、CSS は、Azure Stack オペレーターに、前週の特定の時間枠に対して、オンデマンドで診断ログを収集するように要求する場合があります。 その場合、CSS はオペレーターにコレクションをアップロードするための SAS URL を提供します。 CSS からの SAS URL を使用して、オンデマンドのログ収集を構成するには、次の手順に従います。

1. **[Help and Support Overview]\(ヘルプとサポートの概要\)** を開き、 **[Collect logs now]\(今すぐログを収集する\)** をクリックします。 
1. 過去 7 日間の 1 時間から 4 時間のスライディング ウィンドウを選択します。 
1. ローカル タイム ゾーンを選択します。
1. CSS によって提供された SAS URL を入力します。

   ![オンデマンドのログ収集のスクリーンショット](media/azure-stack-automatic-log-collection/collect-logs-now.png)

>[!NOTE]
>自動診断ログ収集が有効になっている場合、 **[ヘルプとサポート]** に、ログ収集が進行中であるタイミングが表示されます。 自動ログ収集の進行中に、 **[Collect logs now]\(今すぐログを収集する\)** をクリックして特定の時間からログを収集した場合、自動ログ収集の完了後にオンデマンドの収集が開始されます。 

## <a name="using-pep"></a>PEP の使用

<!--how do you look up the PEP IP address. You look up the azurestackstampinfo.json--->

Azure Stack の診断ツールにより、ログ収集を簡単かつ効率的にすることができます。 次の図は、診断ツールの動作を示しています。

![Azure Stack の診断ツール](media/azure-stack-diagnostics/get-azslogs.png)

### <a name="trace-collector"></a>トレース コレクター

トレース コレクターは既定で有効になり、Azure Stack コンポーネント サービスからすべての Windows イベント トレーシング (ETW) ログを収集するためにバック グラウンドで継続的に実行されます。 ETW ログは、一般的なローカル共有に 5 日間を上限として格納されます。 この制限に達した場合、新しいファイルが作成されると最も古いファイルは削除されます。 各ファイルで許容される既定の最大サイズは 200 MB です。 サイズ チェックは 2 分間隔で行われ、現在のファイルが 200 MB 以上であれば保存され、新しいファイルが生成されます。 また、イベント セッションごとに生成されるファイルの合計サイズには、8 GB の制限が設定されています。

### <a name="get-azurestacklog"></a>Get-AzureStackLog

PowerShell コマンドレット Get-AzureStackLog を使用して、Azure Stack 環境のすべてのコンポーネントからログを収集できます。 これらは、ユーザーの定義した場所に zip ファイルで保存されます。 Azure Stack テクニカル サポート チームが問題のトラブルシューティングを行うためにログを必要とする場合、Get-AzureStackLog を実行するようにお願いすることがあります。

> [!CAUTION]
> これらのログ ファイルには個人を特定できる情報 (PII) が含まれている可能性があります。 ログ ファイルを公開する前に、この点を考慮してください。

以下に、収集されるログの種類をいくつか例示します。

* **Azure Stack デプロイ ログ**
* **Windows イベント ログ**
* **Panther ログ**
* **クラスター ログ**
* **Storage 診断ログ**
* **ETW ログ**

これらのファイルは、トレース コレクターによって収集され、共有に保存されます。 必要に応じて、Get-AzureStackLog を使用して、それらを収集できます。

#### <a name="to-run-get-azurestacklog-on-azure-stack-integrated-systems"></a>Azure Stack 統合システムで Get-AzureStackLog を実行するには

統合システムで Get-AzureStackLog を実行するには、特権エンド ポイント (PEP) へのアクセス権が必要です。 PEP を使用して統合システムでログを収集するのに実行できるスクリプト例を次に示します。

```powershell
$ipAddress = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.

$password = ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object -TypeName System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $password)

$shareCred = Get-Credential

$session = New-PSSession -ComputerName $ipAddress -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2) # Provide the time that includes the period for your issue

Invoke-Command -Session $session { Get-AzureStackLog -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if ($session) {
    Remove-PSSession -Session $session
}
```

#### <a name="run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system"></a>Azure Stack Development Kit (ASDK) システムで Get-AzureStackLog を実行する

ASDK ホスト コンピューター上で `Get-AzureStackLog` を実行する際に使用する手順は次のとおりです。

1. ASDK ホスト コンピューター上で **AzureStack\CloudAdmin** としてサインインします。
2. 管理者として、新しい PowerShell ウィンドウを開きます。
3. **Get-AzureStackLog** PowerShell コマンドレットを実行します。

#### <a name="examples"></a>例

* すべてのロールのすべてのログを収集します。

  ```powershell
  Get-AzureStackLog -OutputSharePath "<path>" -OutputShareCredential $cred
  ```

* VirtualMachines ロールと BareMetal ロールからログを収集します。

  ```powershell
  Get-AzureStackLog -OutputSharePath "<path>" -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal
  ```

* ログ ファイルに対する過去 8 時間以内の日付範囲のフィルター処理で、VirtualMachines ロールと BareMetal ロールからログを収集します。

  ```powershell
  Get-AzureStackLog -OutputSharePath "<path>" -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)
  ```

* ログ ファイルに対する過去 8 時間から 2 時間以内の日付範囲のフィルター処理で、VirtualMachines ロールと BareMetal ロールからログを収集します。

  ```powershell
  Get-AzureStackLog -OutputSharePath "<path>" -OutputShareCredential $cred -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)
  ```

* ログを収集して、指定された Azure Storage BLOB コンテナーに保存します。 この操作の一般的な構文は次のとおりです。

  ```powershell
  Get-AzureStackLog -OutputSasUri "<Blob service SAS Uri>"
  ```

  例:

  ```powershell
  Get-AzureStackLog -OutputSasUri "https://<storageAccountName>.blob.core.windows.net/<ContainerName><SAS token>"
  ```

  > [!NOTE]
  > この手順は、ログをアップロードする場合に役立ちます。 アクセス可能な SMB 共有やインターネット アクセスがない場合でも、Azure Stack で BLOB ストレージ アカウントを作成してログを転送し、クライアントを使用してそれらのログを取得することができます。  

  ストレージ アカウントの SAS トークンを生成するには、次のアクセス許可が必要です。

  * Blob Storage サービスへのアクセス
  * コンテナー リソースの種類へのアクセス

  `-OutputSasUri` パラメーターに使用する SAS URI の値を生成するには、次の手順を実行します。

  1. [この記事](/azure/storage/common/storage-quickstart-create-account)の手順に従って、ストレージ アカウントを作成します。
  2. Azure Storage Explorer のインスタンスを開きます。
  3. 手順 1 で作成したストレージ アカウントに接続します。
  4. **[Storage Service]\(Storage サービス\)** の **[BLOB コンテナー]** に移動します。
  5. **[Create a new container]\(新しいコンテナーの作成\)** を選択します。
  6. 新しいコンテナーを右クリックして、 **[Get Shared Access Signature]\(Shared Access Signature の取得\)** を選択します。
  7. お客様の要件に合わせて、有効な **[開始時間]** と **[終了時間]** を選択します。
  8. 必要なアクセス許可については、 **[読み取り]** 、 **[書き込み]** 、 **[一覧]** を選択します。
  9. **作成** を選択します。
  10. Shared Access Signature が表示されます。 URL の部分をコピーし、`-OutputSasUri` パラメーターに入力します。

### <a name="parameter-considerations-for-both-asdk-and-integrated-systems"></a>ASDK および統合システムの両方に関するパラメーターの考慮事項

* **OutputSharePath** パラメーターと **OutputShareCredential** パラメーターは、ユーザー指定の場所にログを格納するために使用されます。

* **FromDate** パラメーターと **ToDate** パラメーターを使用して、特定の期間のログを収集できます。 これらのパラメーターが指定されない場合、既定では過去 4 時間のログが収集されます。

* コンピューター名でログをフィルター処理するには、**FilterByNode** パラメーターを使用します。 例:

    ```powershell
    Get-AzureStackLog -OutputSharePath "<path>" -OutputShareCredential $cred -FilterByNode azs-xrp01
    ```

* 種類でログをフィルター処理するには、**FilterByLogType** パラメーターを使用します。 File、Share、または WindowsEvent を選択してフィルター処理できます。 例:

    ```powershell
    Get-AzureStackLog -OutputSharePath "<path>" -OutputShareCredential $cred -FilterByLogType File
    ```

* **TimeOutInMinutes** パラメーターを使用して、ログ収集のタイムアウトを設定できます。 既定では 150 (2.5 時間) に設定されています。
* ダンプ ファイルのログ収集は既定で無効になっています。 これを有効にするには、**IncludeDumpFile** スイッチ パラメーターを使用します。
* 現時点では **FilterByRole** パラメーターを使用して、次のロールごとにログ収集をフィルター処理できます。

  |   |   |   |    |     |
  | - | - | - | -  |  -  |
  |ACS                   |CA                             |HRP                            |OboService                |VirtualMachines|
  |ACSBlob               |CacheService                   |IBC                            |OEM                       |WAS            |
  |ACSDownloadService    |Compute                        |InfraServiceController         |OnboardRP                 |WASPUBLIC|
  |ACSFabric             |CPI                            |KeyVaultAdminResourceProvider  |PXE                       |         |
  |ACSFabric           |CRP                            |KeyVaultControlPlane           |QueryServiceCoordinator   |         | 
  |ACSMetrics            |DeploymentMachine              |KeyVaultDataPlane              |QueryServiceWorker        |         |
  |ACSMigrationService   |DiskRP                         |KeyVaultInternalControlPlane   |SeedRing                  |         |
  |ACSMonitoringService  |Domain                         |KeyVaultInternalDataPlane      |SeedRingServices          |         |
  |ACSSettingsService    |ECE                            |KeyVaultNamingService          |SLB                       |         |
  |ACSTableMaster        |EventAdminRP                   |MDM                            |SQL                       |         |
  |ACSFabric        |EventRP                        |MetricsAdminRP                 |SRP                       |         |
  |ACSWac                |ExternalDNS                    |MetricsRP                      |Storage                   |         |
  |ADFS                  |FabricRing                     |MetricsServer                  |StorageController         |         |
  |ApplicationController |FabricRingServices             |MetricsStoreService            |URP                       |         |
  |ASAppGateway          |FirstTierAggregationService    |MonAdminRP                     |SupportBridgeController   |         |
  |AzureBridge           |FRP                            |MonRP                          |SupportRing               |         |
  |AzureMonitor          |Gateway                        |NC                             |SupportRingServices       |         |
  |BareMetal             |HealthMonitoring               |NonPrivilegedAppGateway        |SupportBridgeRP           |         |
  |BRP                   |HintingServiceV2               |NRP                            |UsageBridge               |         |
  |   |   |   |    |     | 

### <a name="additional-considerations"></a>追加の考慮事項

* このコマンドは、どのロールに基づいてログが収集されるかによって、実行にいくらかの時間がかかります。 また、関連する要素には、ログ収集に指定した期間と、Azure Stack 環境のノード数が含まれます。
* ログの収集と同時に、コマンドで指定した **OutputSharePath** パラメーターに作成された新しいフォルダーを確認します。
* ロールごとに、個別の zip ファイル内にログがあります。 収集されたログのサイズによっては、1 つのロールのログが複数の zip ファイルに分割されることがあります。 そのようなロールでは、すべてのログ ファイルを単一のフォルダーに解凍したい場合は、一括で解凍できるツールを使用します。 そのロールのすべての圧縮済みファイルを選択し、 **[ここに展開]** を選択します。 これで、そのロールのすべてのログ ファイルが 1 つのマージされたフォルダーに解凍されます。
* また、**Get-AzureStackLog_Output.log** と呼ばれるファイルが、圧縮済みログ ファイルを含むフォルダーに作成されます。 このファイルはコマンド出力のログで、ログ収集中の問題のトラブルシューティングに使用できます。 ログ収集の実行後に予期したログ ファイルがない場合を除き、無視してかまわない `PS>TerminatingError` エントリがログ ファイルに含まれることがあります。
* 特定のエラーを調べるには、複数のコンポーネントのログが必要な場合があります。

  * インフラストラクチャのすべての VM のシステム ログとイベント ログは、**VirtualMachines** ロールで収集されます。
  * すべてのホストのシステム ログとイベント ログは、**BareMetal** ロールで収集されます。
  * フェールオーバー クラスターおよび Hyper-V のイベント ログは、**Storage** ロールで収集されます。
  * ACS ログは、**Storage** ロールと **ACS** ロールで収集されます。

> [!NOTE]
> 記憶域スペースを効率的に使用し、ログで占領されないようにすることは極めて重要であるため、収集されるログにはサイズと有効期間の制限が強制されます。 ただし、問題を診断する場合に、こうした制限のためにもう存在していない可能性のあるログが必要となることがあります。 このため、外部の記憶域スペース (Azure のストレージ アカウント、オンプレミスの追加の記憶装置など) に 8 から 12 時間ごとにログをオフロードし、要件に応じて 1 から 3 か月間そこに保存しておくことを**強くお勧めします**。 また、この記憶域の場所が暗号化されていることを確認します。

### <a name="invoke-azurestackondemandlog"></a>Invoke-AzureStackOnDemandLog

**Invoke-AzureStackOnDemandLog** コマンドレットを使用して、特定のロールのオンデマンド ログを生成できます (このセクションの末尾にある一覧を参照)。 このコマンドレットで生成されるログは、既定では、**Get-AzureStackLog** コマンドレットの実行時に受け取るログ バンドルには表示されません。 また、これらのログは、Microsoft サポート チームから要求された場合にのみ収集することをお勧めします。

現時点では `-FilterByRole` パラメーターを使用して、次のロールごとにログ収集をフィルター処理できます。

* OEM
* NC
* SLB
* Gateway

#### <a name="example-of-collecting-on-demand-logs"></a>オンデマンド ログの収集例

```powershell
$ipAddress = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.

$password = ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object -TypeName System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $password)

$shareCred = Get-Credential

$session = New-PSSession -ComputerName $ipAddress -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2) # Provide the time that includes the period for your issue

Invoke-Command -Session $session {
   Invoke-AzureStackOnDemandLog -Generate -FilterByRole "<on-demand role name>" # Provide the supported on-demand role name e.g. OEM, NC, SLB, Gateway
   Get-AzureStackLog -OutputSharePath "<external share address>" -OutputShareCredential $using:shareCred -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate
}

if ($session) {
   Remove-PSSession -Session $session
}
```

