---
title: Microsoft Azure Stack のトラブルシューティング | Microsoft Docs
description: Azure Stack のトラブルシューティング。
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
ms.date: 09/30/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 09/30/2019
ms.openlocfilehash: 0fb46cd1b92c1b811ba1c72a91188201a7d2af96
ms.sourcegitcommit: 79ead51be63c372b23b7fca6ffeaf95fd44de786
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2019
ms.locfileid: "71687966"
---
# <a name="microsoft-azure-stack-troubleshooting"></a>Microsoft Azure Stack のトラブルシューティング

このドキュメントでは、Azure Stack 統合環境のトラブルシューティング情報を提供します。 Azure Stack Development Kit のヘルプについては、[ASDK のトラブルシューティング](../asdk/asdk-troubleshooting.md)に関する記事を参照するか、[Azure Stack MSDN フォーラム](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)で専門家にお問い合わせください。 

## <a name="frequently-asked-questions"></a>よく寄せられる質問

以下のセクションには、Microsoft カスタマー サポート サービス (CSS) に送られてくる一般的な質問を取り上げたドキュメントのリンクが含まれています。

### <a name="purchase-considerations"></a>購入に関する考慮事項

* [購入方法](https://azure.microsoft.com/overview/azure-stack/how-to-buy/)
* [Azure Stack の概要](azure-stack-overview.md)

### <a name="updates-and-diagnostics"></a>更新と診断

* [Azure Stack で診断ツールを使用する方法](azure-stack-diagnostics.md)
* [Azure Stack システム状態を確認する方法](azure-stack-diagnostic-test.md)
* [更新プログラム パッケージのリリース周期](azure-stack-servicing-policy.md#update-package-release-cadence)

### <a name="supported-operating-systems-and-sizes-for-guest-vms"></a>ゲスト VM でサポートされているオペレーティング システムとサイズ

* [Azure Stack でサポートされているゲスト オペレーティング システム](azure-stack-supported-os.md)
* [Azure Stack でサポートされている VM サイズ](../user/azure-stack-vm-sizes.md)

### <a name="azure-marketplace"></a>Azure Marketplace

* [Azure Stack 用の Azure Marketplace 項目](azure-stack-marketplace-azure-items.md)

### <a name="manage-capacity"></a>キャパシティの管理

#### <a name="memory"></a>メモリ

Azure Stack に関して使用可能な総メモリ容量を増やすために、新たにメモリを増設することができます。 Azure Stack では、物理サーバーは スケール ユニット ノードと呼ばれることもあります。 単一のスケール ユニットに属しているすべてのスケール ユニット ノードには、[同量のメモリ](azure-stack-manage-storage-physical-memory-capacity.md)が確保されている必要があります。

#### <a name="retention-period"></a>保持期間

保有期間の設定では、クラウド オペレーターは削除されたアカウントが回復できる可能性がある期間を日数で指定できます (0 から 9999 日の間)。 既定の保有期間は **0** 日に設定されています。 値を **0** に設定すると、削除されたすべてのアカウントがすぐに保有期間外になり、定期的なガベージ コレクションの対象としてマークされます。

* [保有期間の設定](azure-stack-manage-storage-accounts.md#set-the-retention-period)

### <a name="security-compliance-and-identity"></a>セキュリティ、コンプライアンス、ID  

#### <a name="manage-rbac"></a>RBAC の管理

Azure Stack のユーザーは、サブスクリプション、リソース グループ、またはサービスの各インスタンスの閲覧者、所有者、または共同作業者になることができます。

* [Azure Stack での RBAC の管理](azure-stack-manage-permissions.md)

Azure リソースの組み込みロールが組織の特定のニーズを満たさない場合は、独自のカスタム ロールを作成することができます。 このチュートリアルでは、Azure PowerShell を使用して、Reader Support Tickets というカスタム ロールを作成します。

* [チュートリアル:Azure PowerShell を使用して Azure リソースのカスタム ロールを作成する](https://docs.microsoft.com/azure/role-based-access-control/tutorial-custom-role-powershell)

### <a name="manage-usage-and-billing-as-a-csp"></a>使用量と課金を CSP として管理する

* [使用量と課金を CSP として管理する](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription)
* [CSP または APSS サブスクリプションを作成する](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription)

Azure Stack で使用する共有サービス アカウントの種類を選択します。 マルチテナント Azure Stack の登録に使用できるサブスクリプションの種類は次のとおりです。

* Cloud Service Provider
* Partner Shared Services サブスクリプション


## <a name="troubleshoot-deployment"></a>デプロイのトラブルシューティング 
### <a name="general-deployment-failure"></a>一般的なデプロイの失敗
インストール時に障害が発生した場合、デプロイ スクリプトの -rerun オプションを使用して、失敗した手順からデプロイを再開できます。  

### <a name="template-validation-error-parameter-osprofile-is-not-allowed"></a>パラメーター osProfile が許可されないというテンプレート検証エラー

テンプレートの検証時に、パラメーター "osProfile" が許可されないというエラー メッセージが表示される場合は、以下のコンポーネントの正しいバージョンが使用されていることを確認してください。

- [Compute](https://docs.microsoft.com/azure-stack/user/azure-stack-profiles-azure-resource-manager-versions#microsoftcompute)
- [ネットワーク](https://docs.microsoft.com/azure-stack/user/azure-stack-profiles-azure-resource-manager-versions#microsoftnetwork)

Azure から Azure Stack に VHD をコピーするには、[AzCopy 7.3.0](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-transfer#download-and-install-azcopy)を使用します。 イメージ自体の問題については、ベンダーと協力して解決してください。 Azure Stack の WALinuxAgent の要件の詳細については、「[Azure LinuX エージェント](azure-stack-linux.md#azure-linux-agent)」を参照してください。

### <a name="deployment-fails-due-to-lack-of-external-access"></a>外部アクセスがないことが原因でデプロイが失敗する
外部アクセスが必要な段階でデプロイが失敗する場合は、次の例のような例外が返されます。

```
An error occurred while trying to test identity provider endpoints: System.Net.WebException: The operation has timed out.
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.GetResponse(WebRequest request)
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.ProcessRecord()at, <No file>: line 48 - 8/12/2018 2:40:08 AM
```
このエラーが発生する場合は、[デプロイ ネットワークのトラフィックに関するドキュメント](deployment-networking.md)を見直して、最小限のネットワーク要件がすべて満たされていることを確認してください。 パートナー ツールキットの一部として、ネットワーク チェッカー ツールも提供されています。

その他のデプロイ エラーは、一般に、インターネット上でのリソースへの接続の問題が原因です。

インターネット上でのリソースへの接続を確認するには、次の手順を実行します。

1. PowerShell を開きます。
2. WAS01 またはいずれかの ERCS VM に対して Enter-PSSession を実行します。
3. 次のコマンドレットを実行します。 
   ```powershell
   Test-NetConnection login.windows.net -port 443
   ```

このコマンドが失敗する場合は、TOR スイッチおよびその他のネットワーク デバイスがすべて、[ネットワーク トラフィックを許可する](azure-stack-network.md)ように構成されていることを確認します。

## <a name="troubleshoot-virtual-machines"></a>仮想マシンのトラブルシューティング
### <a name="default-image-and-gallery-item"></a>既定のイメージとギャラリー アイテム
Azure Stack に VM をデプロイする前に、まず Windows Server イメージとギャラリー アイテムを追加する必要があります。


### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk"></a>一部の仮想マシンの削除後に、まだディスクに VHD ファイルが表示される
この動作は仕様によるものです。

* VM を削除しても VHD は削除されません。 ディスクはリソース グループ内の個別のリソースです。
* ストレージ アカウントが削除されると、削除は Azure Resource Manager にただちに表示されますが、それを格納している可能性のあるディスクは、ガベージ コレクションが実行されるまで、記憶域に維持されます。

"孤立" VHD が表示された場合、それが削除されたストレージ アカウントのフォルダーの一部であるかどうかを知ることが重要です。 ストレージ アカウントが削除されていない場合は、まだ存在していても正常です。

リテンション期間しきい値と、オンデマンドの再利用の設定について詳しくは、[ストレージ アカウントの管理](azure-stack-manage-storage-accounts.md)に関するページを参照してください。

## <a name="troubleshoot-storage"></a>ストレージのトラブルシューティング
### <a name="storage-reclamation"></a>記憶域の再利用
ポータルに再利用された容量が表示されるまで、最大で 14 時間かかる場合があります。 領域の再利用は、ブロック BLOB ストア内の内部コンテナー ファイルの使用率をなど、さまざまな要因に依存します。 そのため、削除されるデータの量によって、ガベージ コレクターの実行時に再利用可能になる領域の量に対する保証はありません。

## <a name="troubleshooting-app-service"></a>App Service のトラブルシューティング
### <a name="create-aadidentityappps1-script-fails"></a>Create-AADIdentityApp.ps1 スクリプトが失敗する

App Service に必要な Create-AADIdentityApp.ps1 スクリプトが失敗する場合は、スクリプトの実行時に必須の -AzureStackAdminCredential パラメーターが指定されていることを確認してください。 詳細については、「[App Service on Azure Stack のデプロイの前提条件](azure-stack-app-service-before-you-get-started.md#create-an-azure-active-directory-app)」を参照してください。

