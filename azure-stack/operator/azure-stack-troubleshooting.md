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
ms.date: 09/04/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 09/04/2019
ms.openlocfilehash: a9d62640b2baabfd3283099656719a880dd0a41b
ms.sourcegitcommit: a8379358f11db1e1097709817d21ded0231503eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/05/2019
ms.locfileid: "70377240"
---
# <a name="microsoft-azure-stack-troubleshooting"></a>Microsoft Azure Stack のトラブルシューティング

このドキュメントでは、Azure Stack のトラブルシューティング情報を提供します。 


## <a name="frequently-asked-questions"></a>よく寄せられる質問

以下のセクションには、Microsoft カスタマー サポート サービス (CSS) に送られてくる一般的な質問を取り上げたドキュメントのリンクが含まれています。

### <a name="purchase-considerations"></a>購入に関する考慮事項

* [購入方法](https://azure.microsoft.com/overview/azure-stack/how-to-buy/)
* [Azure Stack の概要](azure-stack-overview.md)

### <a name="azure-stack-development-kit-asdk"></a>Azure Stack Development Kit (ASDK)

[Azure Stack Development Kit](../asdk/asdk-what-is.md) のヘルプについては、[Azure Stack MSDN フォーラム](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)で専門家に問い合わせます。 ASDK は、CSS によるサポートがない評価環境として提供されています。 ASDK に対してオープンされたサポート ケースは MSDN フォーラムに回されます。

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

保有期間の設定では、クラウド オペレーターは削除されたアカウントが回復できる可能性がある期間を日数で指定できます (0 から 9999 日の間)。 既定の保有期間は 0 日に設定されています。 値を "0" に設定すると、削除されたすべてのアカウントがすぐに保有期間外になり、定期的なガベージ コレクションの対象としてマークされます。

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

### <a name="at-the-end-of-asdk-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>ASDK のデプロイの最後に、PowerShell セッションがまだ開いており、出力が表示されません。
この動作は、PowerShell コマンド ウィンドウが選択されている場合の既定の動作の結果に過ぎない可能性があります。 開発キットのデプロイは成功しましたが、ウィンドウを選択したときに、スクリプトが一時停止しました。 設定が完了したことを確認するには、コマンド ウィンドウのタイトル バーで、"select" という単語を探します。 ESC キーを押してその選択を解除すると、その後に完了メッセージが表示されるはずです。

### <a name="deployment-fails-due-to-lack-of-external-access"></a>外部アクセスがないことが原因でデプロイが失敗する
外部アクセスが必要な段階でデプロイが失敗する場合は、次の例のような例外が返されます。

```
An error occurred while trying to test identity provider endpoints: System.Net.WebException: The operation has timed out.
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.GetResponse(WebRequest request)
   at Microsoft.PowerShell.Commands.WebRequestPSCmdlet.ProcessRecord()at, <No file>: line 48 - 8/12/2018 2:40:08 AM
```
このエラーが発生する場合は、[デプロイ ネットワークのトラフィックに関するドキュメント](deployment-networking.md)を見直して、最小限のネットワーク要件がすべて満たされていることを確認してください。 パートナー ツールキットの一部として、ネットワーク チェッカー ツールも提供されています。

上記の例外が表示されるデプロイ エラーは、一般に、インターネットのリソースへの接続の問題が原因です

これが問題であることを確認するため、次の手順を実行できます。

1. PowerShell を開きます
2. WAS01 または ERC VM のいずれかに対する PSSession に入ります
3. 次のコマンドレットを実行します: Test-NetConnection login.windows.net -port 443

このコマンドが失敗する場合は、TOR スイッチおよびその他のネットワーク デバイスがすべて、[ネットワーク トラフィックを許可する](azure-stack-network.md)ように構成されていることを確認します。

## <a name="troubleshoot-virtual-machines"></a>仮想マシンのトラブルシューティング
### <a name="default-image-and-gallery-item"></a>既定のイメージとギャラリー アイテム
Azure Stack に VM をデプロイする前に、まず Windows Server イメージとギャラリー アイテムを追加する必要があります。

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>Azure Stack ホストの再起動後、一部の VM が自動的に起動しないことがあります。
ホストの再起動後、Azure Stack サービスがすぐに使用できないことに気付く場合があります。  これは、Azure Stack [インフラストラクチャ VM](../asdk/asdk-architecture.md#virtual-machine-roles ) およびリソース プロバイダーが一貫性をチェックするために少し時間がかかるためですが、最終的に自動的に起動します。

また、Azure Stack Development Kit ホストの再起動後に、そのテナント VM が自動的に起動しないことに気付く場合もあります。 これは既知の問題であり、それらをオンラインにするために、いくつかの手動の手順が必要です。

1.  Azure Stack Development Kit ホストで、[スタート] メニューから**フェールオーバー クラスター マネージャー**を起動します。
2.  クラスター **S-Cluster.azurestack.local** を選択します。
3.  **[役割]** を選びます。
4.  テナント VM が*保存済み*状態で表示されます。 すべてのインフラストラクチャ VM が実行されたら、テナント VM を右クリックし、 **[開始]** を選択して、それらを再開します。

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>一部の仮想マシンを削除しましたが、まだディスクに VHD ファイルが表示されます。 これは期待される動作ですか。
はい、これは正しい動作です。 これがそのように設計された理由は次のとおりです。

* VM を削除しても VHD は削除されません。 ディスクはリソース グループ内の個別のリソースです。
* ストレージ アカウントが削除されると、削除は Azure Resource Manager にただちに表示されますが、それを格納している可能性のあるディスクは、ガベージ コレクションが実行されるまで、記憶域に維持されます。

"孤立" VHD が表示された場合、それが削除されたストレージ アカウントのフォルダーの一部であるかどうかを知ることが重要です。 ストレージ アカウントが削除されていない場合は、まだ存在していても正常です。

リテンション期間しきい値と、オンデマンドの再利用の設定について詳しくは、[ストレージ アカウントの管理](azure-stack-manage-storage-accounts.md)に関するページを参照してください。

## <a name="troubleshoot-storage"></a>ストレージのトラブルシューティング
### <a name="storage-reclamation"></a>記憶域の再利用
ポータルに再利用された容量が表示されるまで、最大で 14 時間かかる場合があります。 領域の再利用は、ブロック BLOB ストア内の内部コンテナー ファイルの使用率をなど、さまざまな要因に依存します。 そのため、削除されるデータの量によって、ガベージ コレクターの実行時に再利用可能になる領域の量に対する保証はありません。

