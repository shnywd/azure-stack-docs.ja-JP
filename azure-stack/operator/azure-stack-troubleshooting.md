---
title: Azure Stack Hub のトラブルシューティングを行う
titleSuffix: Azure Stack
description: VM、ストレージ、App Service に関する問題を含む、Azure Stack Hub のトラブルシューティング方法について学習します。
author: justinha
ms.topic: article
ms.date: 11/05/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: f5c223e08207518bde315725fd69ddb3fb97a578
ms.sourcegitcommit: 74ce7c12a93d47315d70427b02bcacbd3b44f854
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2020
ms.locfileid: "77037269"
---
# <a name="troubleshoot-issues-in-azure-stack-hub"></a>Azure Stack Hub の問題のトラブルシューティングを行う

このドキュメントでは、Azure Stack Hub 統合環境のトラブルシューティング情報を提供します。 Azure Stack Development Kit のヘルプについては、[ASDK のトラブルシューティング](../asdk/asdk-troubleshooting.md)に関するページを参照するか、[Azure Stack Hub MSDN フォーラム](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)で専門家にお問い合わせください。

## <a name="frequently-asked-questions"></a>よく寄せられる質問

以下のセクションには、Microsoft カスタマー サポート サービス (CSS) に送られてくる一般的な質問を取り上げたドキュメントのリンクが含まれています。

### <a name="purchase-considerations"></a>購入に関する考慮事項

* [購入方法](https://azure.microsoft.com/overview/azure-stack/how-to-buy/)
* [Azure Stack Hub の概要](azure-stack-overview.md)

### <a name="updates-and-diagnostics"></a>更新と診断

* [Azure Stack Hub で診断ツールを使用する方法](azure-stack-diagnostics.md)
* [Azure Stack Hub システムの状態を検証する方法](azure-stack-diagnostic-test.md)
* [更新プログラム パッケージのリリース周期](azure-stack-servicing-policy.md#update-package-release-cadence)

### <a name="supported-operating-systems-and-sizes-for-guest-vms"></a>ゲスト VM でサポートされているオペレーティング システムとサイズ

* [Azure Stack Hub でサポートされているゲスト オペレーティング システム](azure-stack-supported-os.md)
* [Azure Stack Hub でサポートされている VM サイズ](../user/azure-stack-vm-sizes.md)

### <a name="azure-marketplace"></a>Azure Marketplace

* [Azure Stack Hub で使用できる Azure Marketplace 項目](azure-stack-marketplace-azure-items.md)

### <a name="manage-capacity"></a>キャパシティの管理

#### <a name="memory"></a>メモリ

Azure Stack Hub で使用可能な総メモリ容量を増やすために、メモリをさらに追加することができます。 Azure Stack Hub では、物理サーバーはスケール ユニット ノードと呼ばれることもあります。 単一のスケール ユニットに属しているすべてのスケール ユニット ノードには、[同量のメモリ](azure-stack-manage-storage-physical-memory-capacity.md)が確保されている必要があります。

#### <a name="retention-period"></a>保持期間

保有期間の設定では、クラウド オペレーターは削除されたアカウントを回復できる可能性がある期間を日数 (0 から 9999 日の間) で指定できます。 既定の保有期間は **0** 日に設定されています。 値を **0** に設定すると、削除されたすべてのアカウントがすぐに保有期間外になり、定期的なガベージ コレクションの対象としてマークされます。

* [保有期間の設定](azure-stack-manage-storage-accounts.md#set-the-retention-period)

### <a name="security-compliance-and-identity"></a>セキュリティ、コンプライアンス、ID  

#### <a name="manage-rbac"></a>RBAC の管理

Azure Stack Hub のユーザーは、サブスクリプション、リソース グループ、またはサービスの各インスタンスの閲覧者、所有者、または共同作成者になることができます。

* [Azure Stack Hub での RBAC の管理](azure-stack-manage-permissions.md)

Azure リソースの組み込みロールが組織の特定のニーズを満たさない場合は、独自のカスタム ロールを作成することができます。 このチュートリアルでは、Azure PowerShell を使用して、Reader Support Tickets というカスタム ロールを作成します。

* [チュートリアル:Azure PowerShell を使用して Azure リソースのカスタム ロールを作成する](https://docs.microsoft.com/azure/role-based-access-control/tutorial-custom-role-powershell)

### <a name="manage-usage-and-billing-as-a-csp"></a>使用量と課金を CSP として管理する

* [使用量と課金を CSP として管理する](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription)
* [CSP または APSS サブスクリプションを作成する](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription)

Azure Stack Hub で使用する共有サービス アカウントの種類を選択します。 マルチテナント Azure Stack Hub の登録に使用できるサブスクリプションの種類は、次のとおりです。

* クラウド ソリューション プロバイダー
* Partner Shared Services サブスクリプション

### <a name="get-scale-unit-metrics"></a>スケール ユニットのメトリックを取得する

PowerShell を使用して、CSS を利用せずに、スタンプ使用状況の情報を取得できます。 スタンプの使用状況を取得するには:

1. PEP セッションを作成します。
2. `test-azurestack` を実行します。
3. PEP セッションを終了します。
4. invoke-command 呼び出しを使用して、`get-azurestacklog -filterbyrole seedring` を実行します。
5. seedring .zip を抽出します。 `test-azurestack` を実行した ERCS フォルダーから、検証レポートを取得することができます。

詳細については、[Azure Stack Hub の診断](azure-stack-configure-on-demand-diagnostic-log-collection.md#use-the-privileged-endpoint-pep-to-collect-diagnostic-logs)に関する記述を参照してください。

## <a name="troubleshoot-virtual-machines-vms"></a>仮想マシン (VM) のトラブルシューティングを行う

### <a name="default-image-and-gallery-item"></a>既定のイメージとギャラリー アイテム

Azure Stack Hub に VM をデプロイする前に、Windows Server イメージとギャラリー アイテムを追加する必要があります。

### <a name="ive-deleted-some-vms-but-still-see-the-vhd-files-on-disk"></a>一部の VM の削除後に、まだディスクに VHD ファイルが表示される

この動作は仕様によるものです。

* VM を削除しても VHD は削除されません。 ディスクはリソース グループ内の個別のリソースです。
* ストレージ アカウントが削除されると、削除は Azure Resource Manager にすぐに表示されます。 しかし、含まれる可能性のあるディスクは、ガベージ コレクションが実行されるまで引き続きストレージに保持されます。

"孤立" VHD が表示された場合、それが削除されたストレージ アカウントのフォルダーの一部であるかどうかを知ることが重要です。 ストレージ アカウントが削除されていない場合は、まだそこにあっても正常です。

リテンション期間しきい値と、オンデマンドの再利用の設定について詳しくは、[ストレージ アカウントの管理](azure-stack-manage-storage-accounts.md)に関するページを参照してください。

## <a name="troubleshoot-storage"></a>ストレージのトラブルシューティング

### <a name="storage-reclamation"></a>記憶域の再利用

ポータルに再利用された容量が表示されるまで、最大で 14 時間かかる場合があります。 領域の再利用は、ブロック BLOB ストア内の内部コンテナー ファイルの使用率など、さまざまな要因に依存します。 そのため、削除されるデータの量によって、ガベージ コレクターの実行時に再利用可能になる領域の量に対する保証はありません。

### <a name="azure-storage-explorer-not-working-with-azure-stack-hub"></a>Azure Stack Hub で Azure Storage Explorer が動作しない

切断されたシナリオで統合システムを使用する場合は、エンタープライズ証明機関 (CA) を利用することをお勧めします。 ルート証明書を Base-64 形式でエクスポートしてから、Azure Storage Explorer にインポートします。 必ず、Resource Manager エンドポイントから末尾のスラッシュ (`/`) を削除してください。 詳細については、「[Azure Stack Hub への接続を準備する](/azure-stack/user/azure-stack-storage-connect-se)」を参照してください。

## <a name="troubleshooting-app-service"></a>App Service のトラブルシューティング

### <a name="create-aadidentityappps1-script-fails"></a>Create-AADIdentityApp.ps1 スクリプトが失敗する

App Service に必要な Create-AADIdentityApp.ps1 スクリプトが失敗する場合は、スクリプトの実行時に必須の `-AzureStackAdminCredential` パラメーターを必ず含めるようにしてください。 詳細については、「[App Service on Azure Stack Hub のデプロイの前提条件](azure-stack-app-service-before-you-get-started.md#create-an-azure-active-directory-app)」を参照してください。
