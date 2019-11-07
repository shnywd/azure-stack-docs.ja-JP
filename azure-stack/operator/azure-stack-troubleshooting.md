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
ms.date: 11/05/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: 4c04eafab93da233859b5b67571b70899b081b95
ms.sourcegitcommit: c583f19d15d81baa25dd49738d53d8fc01463bef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2019
ms.locfileid: "73659230"
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

* クラウド ソリューション プロバイダー
* Partner Shared Services サブスクリプション

### <a name="get-scale-unit-metrics"></a>スケール ユニットのメトリックを取得する

PowerShell を使用して、CSS を利用せずに、スタンプ使用状況の情報を取得できます。 スタンプの使用状況を取得するには: 

1. PEP セッションを作成します
2. test-azurestack を実行します
3. PEP セッションを終了します
4. invoke-command 呼び出しを使用して、get-azurestacklog -filterbyrole seedring を実行します。
5. Seedring の .zip を抽出し、test-azurestack を実行した ERCS フォルダーから検証レポートを取得できます。

詳細については、「[Azure Stack の診断](azure-stack-configure-on-demand-diagnostic-log-collection.md#to-run-get-azurestacklog-on-azure-stack-integrated-systems)」を参照してください。

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

### <a name="azure-storage-explorer-not-working-with-azure-stack"></a>Azure Stack で Azure Storage Explorer が動かない 
 
切り離されたシナリオで統合システムを使用している場合は、エンタープライズ証明機関 (CA) を使用することをお勧めします。 ルート証明書を Base-64 形式でエクスポートしてから、Azure Storage Explorer にインポートします。 ARM エンドポイントから末尾のスラッシュ ("/") を削除してください。 詳細については、「[Azure Stack への接続を準備する](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-connect-se#prepare-for-connecting-to-azure-stack)」を参照してください。
 

## <a name="troubleshooting-app-service"></a>App Service のトラブルシューティング
### <a name="create-aadidentityappps1-script-fails"></a>Create-AADIdentityApp.ps1 スクリプトが失敗する

App Service に必要な Create-AADIdentityApp.ps1 スクリプトが失敗する場合は、スクリプトの実行時に必須の -AzureStackAdminCredential パラメーターが指定されていることを確認してください。 詳細については、「[App Service on Azure Stack のデプロイの前提条件](azure-stack-app-service-before-you-get-started.md#create-an-azure-active-directory-app)」を参照してください。

