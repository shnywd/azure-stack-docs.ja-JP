---
title: Azure Stack の既知の問題
description: Azure Stack リリースの既知の問題について学習します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2019
ms.author: sethm
ms.reviewer: prchint
ms.lastreviewed: 09/13/2019
ms.openlocfilehash: ba5b8358dced378f499c30f9b8d409497319fc86
ms.sourcegitcommit: 2c37ac8e88de19430080128bac1b70e33557d354
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/20/2019
ms.locfileid: "71164248"
---
# <a name="azure-stack-known-issues"></a>Azure Stack の既知の問題

この記事では、Azure Stack のリリースにおける既知の問題を一覧表示します。 新しい問題が特定されると、この一覧は更新されます。

別のバージョンの既知の問題にアクセスするには、左側の目次の上部にあるバージョン セレクターのドロップダウンを使用します。

::: moniker range=">=azs-1905"
> [!IMPORTANT]  
> 更新プログラムを適用する前に、このセクションを確認してください。
::: moniker-end
::: moniker range="<azs-1905"
> [!IMPORTANT]  
> ご利用の Azure Stack インスタンスが 2 つ前の更新プログラムより遅れている場合、コンプライアンスに対応していないとみなされます。 [サポートを受けるためには、少なくともサポートされる最小バージョンまで更新する](azure-stack-servicing-policy.md#keep-your-system-under-support)必要があります。 
::: moniker-end

<!---------------------------------------------------------->
<!------------------- SUPPORTED VERSIONS ------------------->
<!---------------------------------------------------------->

::: moniker range="azs-1908"
## <a name="1908-update-process"></a>1908 の更新処理

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: Azure Stack の更新プログラムをインストールしようとしたときに、更新の状態が失敗して、状態が **PreparationFailed** に変更される場合があります。 これは、更新リソース プロバイダー (URP) が、処理のためにストレージ コンテナーから内部インフラストラクチャ共有にファイルを正しく転送できないことが原因です。
- 修復: バージョン 1901 (1.1901.0.95) 以降、この問題は、 **[今すぐ更新]** ( **[再開]** ではない) をもう一度クリックすることで回避できるようになりました。 それにより、URP は前回の試行のファイルをクリーンアップして、ダウンロードを再度開始します。 問題が解決しない場合は、[更新プログラムのインストールのセクション](azure-stack-apply-updates.md#install-updates-and-monitor-progress)に従って、更新プログラム パッケージを手動でアップロードすることをお勧めします。
- 発生頻度: 一般

## <a name="portal"></a>ポータル

### <a name="administrative-subscriptions"></a>管理サブスクリプション

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: バージョン 1804 で導入された 2 つの管理サブスクリプションは使用しないでください。 このサブスクリプションの種類は **Metering** サブスクリプションと **Consumption** サブスクリプションです。
- 修復: これら 2 つのサブスクリプション上でリソースが実行されている場合は、ユーザー サブスクリプションで再作成してください。
- 発生頻度: 一般

### <a name="subscriptions-properties-blade"></a>サブスクリプションの [プロパティ] ブレード

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 管理者ポータルで、サブスクリプションの **[プロパティ]** ブレードが正しく読み込まれません
- 修復: これらのサブスクリプションのプロパティは、 **[サブスクリプションの概要]** ブレードの **[要点]** ウィンドウで表示できます。
- 発生頻度: 一般

### <a name="subscriptions-lock-blade"></a>サブスクリプションの [ロック] ブレード

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 管理者ポータルのユーザー サブスクリプションの **[ロック]** ブレードには、**サブスクリプション**という 2 つのボタンがあります。
- 発生頻度: 一般

### <a name="subscription-permissions"></a>サブスクリプションのアクセス許可

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: Azure Stack ポータルを使用して、サブスクリプションへのアクセス許可を表示することはできません。
- 修復: [PowerShell を使用してアクセス許可を確認](/powershell/module/azurerm.resources/get-azurermroleassignment)します。
- 発生頻度: 一般

### <a name="storage-account-settings"></a>Storage アカウントの設定

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、ストレージ アカウントの **[構成]** ブレードに、**安全な転送の種類**を変更するオプションが表示されます。 機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

### <a name="upload-blob"></a>BLOB のアップロード

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで **[OAuth(preview)]\(OAuth (プレビュー)\)** オプションを使用して BLOB をアップロードしようとすると、タスクがエラー メッセージにより失敗します。
- 修復: SAS オプションを使用して BLOB をアップロードします。
- 発生頻度: 一般

## <a name="networking"></a>ネットワーク

### <a name="service-endpoints"></a>サービス エンドポイント

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[仮想ネットワーク]** ブレードに、**サービス エンドポイント**を使用するオプションが表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

### <a name="network-interface"></a>Linux

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 新しいネットワーク インターフェイスを、**実行**状態にある VM に追加することはできません。
- 修復: 仮想マシンを停止してから、ネットワーク インターフェイスを追加/削除します。
- 発生頻度: 一般

### <a name="virtual-network-gateway"></a>Virtual Network ゲートウェイ

#### <a name="local-network-gateway-deletion"></a>ローカル ネットワーク ゲートウェイの削除

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、**ローカル ネットワーク ゲートウェイ**を削除すると、次のエラー メッセージが表示されます。アクティブな接続がない場合でも、**アクティブな接続のあるローカル ネットワーク ゲートウェイを削除できません**。
- 軽減策: この問題の修正プログラムは 1907 でリリースされます。 この問題の回避策は、別の名前で、同じ IP アドレス、アドレス空間、および構成の詳細を持つ新しいローカル ネットワーク ゲートウェイを作成することです。 環境が 1907 に更新されたら、古い LNG を削除できます。
- 発生頻度: 一般

#### <a name="alerts"></a>アラート

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[仮想ネットワーク ゲートウェイ]** ブレードに、使用する**アラート** オプションが表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

#### <a name="active-active"></a>アクティブ/アクティブ

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、作成中、および**仮想ネットワーク ゲートウェイ**のリソース メニューで、**アクティブ/アクティブ**構成を有効にするオプションが表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

#### <a name="vpn-troubleshooter"></a>VPN トラブルシューティング ツール

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[接続]** ブレードに **VPN トラブルシューティング ツール**と呼ばれる機能が表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

#### <a name="documentation"></a>ドキュメント

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 仮想ネットワーク ゲートウェイの概要ページにあるドキュメントのリンクは、Azure Stack ではなく Azure 固有のドキュメントにリンクされています。 Azure Stack のドキュメントについては、次のリンクを使用してください。

  - [ゲートウェイ SKU](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [高可用性接続](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [Azure Stack での BGP の構成](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute 回線](azure-stack-connect-expressroute.md)
  - [カスタムの IPsec/IKE ポリシーの指定](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

## <a name="compute"></a>Compute

### <a name="vm-boot-diagnostics"></a>VM ブート診断

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 新しい Windows 仮想マシン (VM) を作成するときに、次のエラーが表示されることがあります。**仮想マシン 'vm-name' を起動できませんでした。エラー:Failed to update serial output settings for VM 'vm-name'** . (VM 'vm-name' のシリアル出力設定を更新できませんでした。) このエラーは、VM でブート診断を有効にしても、ブート診断ストレージ アカウントを削除した場合に発生します。
- 修復: 以前使用したものと同じ名前のストレージ アカウントを再作成します。
- 発生頻度: 一般

### <a name="virtual-machine-scale-set"></a>仮想マシン スケール セット

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>4 ノードの Azure Stack 環境でのパッチと更新プログラムの適用中に作成が失敗します

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 4 ノードの Azure Stack 環境では、障害ドメインが 3 つの可用性セット内での VM の作成および仮想マシン スケール セット インスタンスの作成が更新プロセス中に **FabricVmPlacementErrorUnsupportedFaultDomainSize** エラーで失敗します。
- 修復: 障害ドメインが 2 つの可用性セット内には 1 つの VM を正常に作成できます。 ただし、4 ノードの Azure Stack では、依然として更新プロセス中にスケール セット インスタンスを作成することはできません。

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH アクセス

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: SSH の承認を有効にして作成した Ubuntu 18.04 VM では、SSH キーを使用してサインインすることはできません。
- 修復: プロビジョニング後に Linux 拡張機能用の VM アクセスを使用して SSH キーを実装するか、パスワードベースの認証を使用します。
- 発生頻度: 一般

### <a name="virtual-machine-scale-set-reset-password-does-not-work"></a>仮想マシン スケール セットのパスワードのリセットが機能しない

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 新しいパスワードのリセット ブレードはスケール セット UI には表示されますが、Azure Stack ではスケール セットでのパスワードのリセットがまだサポートされていません。
- 修復: なし。
- 発生頻度: 一般

### <a name="rainy-cloud-on-scale-set-diagnostics"></a>スケール セット診断の雨雲

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 仮想マシン スケール セットの概要ページに、空のグラフが表示されます。 空のグラフをクリックすると、"雨雲" ブレードが開きます。 これは CPU 使用率などのスケール セット診断情報のグラフで、現在の Azure Stack ビルドでサポートされている機能ではありません。
- 修復: なし。
- 発生頻度: 一般

### <a name="virtual-machine-diagnostic-settings-blade"></a>仮想マシンの診断設定ブレード

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。    
- 原因: 仮想マシンの診断設定ブレードには、**Application Insight アカウント**を求める **[シンク]** タブがあります。 これは新しいブレードの結果で、Azure Stack ではまだサポートされていません。
- 修復: なし。
- 発生頻度: 一般

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range="azs-1907"
## <a name="1907-update-process"></a>1907 の更新処理

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 1907 Azure Stack の更新プログラムをインストールしようとしたときに、更新の状態が失敗して、状態が **PreparationFailed** に変更される場合があります。 これは、更新リソース プロバイダー (URP) が、処理のためにストレージ コンテナーから内部インフラストラクチャ共有にファイルを正しく転送できないことが原因です。
- 修復: バージョン 1901 (1.1901.0.95) 以降、この問題は、 **[今すぐ更新]** ( **[再開]** ではない) をもう一度クリックすることで回避できるようになりました。 それにより、URP は前回の試行のファイルをクリーンアップして、ダウンロードを再度開始します。 問題が解決しない場合は、[更新プログラムのインポートとインストールのセクション](azure-stack-apply-updates.md)に従って、更新プログラム パッケージを手動でアップロードすることをお勧めします。
- 発生頻度: 一般

## <a name="portal"></a>ポータル

### <a name="administrative-subscriptions"></a>管理サブスクリプション

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: バージョン 1804 で導入された 2 つの管理サブスクリプションは使用しないでください。 このサブスクリプションの種類は **Metering** サブスクリプションと **Consumption** サブスクリプションです。
- 修復: これら 2 つのサブスクリプション上でリソースが実行されている場合は、ユーザー サブスクリプションで再作成してください。
- 発生頻度: 一般

### <a name="subscriptions-properties-blade"></a>サブスクリプションの [プロパティ] ブレード

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 管理者ポータルで、サブスクリプションの **[プロパティ]** ブレードが正しく読み込まれません
- 修復: これらのサブスクリプションのプロパティは、 **[サブスクリプションの概要]** ブレードの **[要点]** ウィンドウで表示できます。
- 発生頻度: 一般

### <a name="subscription-permissions"></a>サブスクリプションのアクセス許可

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: Azure Stack ポータルを使用して、サブスクリプションへのアクセス許可を表示することはできません。
- 修復: [PowerShell を使用してアクセス許可を確認](/powershell/module/azurerm.resources/get-azurermroleassignment)します。
- 発生頻度: 一般

### <a name="storage-account-settings"></a>Storage アカウントの設定

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、ストレージ アカウントの **[構成]** ブレードに、**安全な転送の種類**を変更するオプションが表示されます。 機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

### <a name="upload-blob"></a>BLOB のアップロード

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで **[OAuth(preview)]\(OAuth (プレビュー)\)** オプションを使用して BLOB をアップロードしようとすると、タスクがエラー メッセージにより失敗します。
- 修復: SAS オプションを使用して BLOB をアップロードします。
- 発生頻度: 一般

## <a name="networking"></a>ネットワーク

### <a name="service-endpoints"></a>サービス エンドポイント

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[仮想ネットワーク]** ブレードに、**サービス エンドポイント**を使用するオプションが表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

### <a name="network-interface"></a>Linux

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 新しいネットワーク インターフェイスを、**実行**状態にある VM に追加することはできません。
- 修復: 仮想マシンを停止してから、ネットワーク インターフェイスを追加/削除します。
- 発生頻度: 一般

### <a name="virtual-network-gateway"></a>Virtual Network ゲートウェイ

#### <a name="alerts"></a>アラート

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[仮想ネットワーク ゲートウェイ]** ブレードに、使用する**アラート** オプションが表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

#### <a name="active-active"></a>アクティブ/アクティブ

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、作成中、および**仮想ネットワーク ゲートウェイ**のリソース メニューで、**アクティブ/アクティブ**構成を有効にするオプションが表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

#### <a name="vpn-troubleshooter"></a>VPN トラブルシューティング ツール

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[接続]** ブレードに **VPN トラブルシューティング ツール**と呼ばれる機能が表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

### <a name="network-connection-type"></a>ネットワーク接続の種類

- 適用先:この問題は、すべての 1906 または 1907 環境に適用されます。 
- 原因: ユーザーポータルの **[AddConnection]** ブレードに、**VNet 間**を使用するオプションが表示されます。 この機能は現在 Azure Stack でサポートされていません。 
- 発生頻度: 一般 

#### <a name="documentation"></a>ドキュメント

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 仮想ネットワーク ゲートウェイの概要ページにあるドキュメントのリンクは、Azure Stack ではなく Azure 固有のドキュメントにリンクされています。 Azure Stack のドキュメントについては、次のリンクを使用してください。

  - [ゲートウェイ SKU](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [高可用性接続](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [Azure Stack での BGP の構成](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute 回線](azure-stack-connect-expressroute.md)
  - [カスタムの IPsec/IKE ポリシーの指定](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

## <a name="compute"></a>Compute

### <a name="vm-boot-diagnostics"></a>VM ブート診断

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 新しい Windows 仮想マシン (VM) を作成するときに、次のエラーが表示されることがあります。**仮想マシン 'vm-name' を起動できませんでした。エラー:Failed to update serial output settings for VM 'vm-name'** . (VM 'vm-name' のシリアル出力設定を更新できませんでした。) このエラーは、VM でブート診断を有効にしても、ブート診断ストレージ アカウントを削除した場合に発生します。
- 修復: 以前使用したものと同じ名前のストレージ アカウントを再作成します。
- 発生頻度: 一般

### <a name="virtual-machine-scale-set"></a>仮想マシン スケール セット

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>4 ノードの Azure Stack 環境でのパッチと更新プログラムの適用中に作成が失敗します

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 4 ノードの Azure Stack 環境では、障害ドメインが 3 つの可用性セット内での VM の作成および仮想マシン スケール セット インスタンスの作成が更新プロセス中に **FabricVmPlacementErrorUnsupportedFaultDomainSize** エラーで失敗します。
- 修復: 障害ドメインが 2 つの可用性セット内には 1 つの VM を正常に作成できます。 ただし、4 ノードの Azure Stack では、依然として更新プロセス中にスケール セット インスタンスを作成することはできません。

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH アクセス

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: SSH の承認を有効にして作成した Ubuntu 18.04 VM では、SSH キーを使用してサインインすることはできません。
- 修復: プロビジョニング後に Linux 拡張機能用の VM アクセスを使用して SSH キーを実装するか、パスワードベースの認証を使用します。
- 発生頻度: 一般

### <a name="virtual-machine-scale-set-reset-password-does-not-work"></a>仮想マシン スケール セットのパスワードのリセットが機能しない

- 適用先:この問題は、1906 および 1907 リリースに適用されます。
- 原因: 新しいパスワードのリセット ブレードはスケール セット UI には表示されますが、Azure Stack ではスケール セットでのパスワードのリセットがまだサポートされていません。
- 修復: なし。
- 発生頻度: 一般

### <a name="rainy-cloud-on-scale-set-diagnostics"></a>スケール セット診断の雨雲

- 適用先:この問題は、1906 および 1907 リリースに適用されます。
- 原因: 仮想マシン スケール セットの概要ページに、空のグラフが表示されます。 空のグラフをクリックすると、"雨雲" ブレードが開きます。 これは CPU 使用率などのスケール セット診断情報のグラフで、現在の Azure Stack ビルドでサポートされている機能ではありません。
- 修復: なし。
- 発生頻度: 一般

### <a name="virtual-machine-diagnostic-settings-blade"></a>仮想マシンの診断設定ブレード

- 適用先:この問題は、1906 および 1907 リリースに適用されます。    
- 原因: 仮想マシンの診断設定ブレードには、**Application Insight アカウント**を求める **[シンク]** タブがあります。 これは新しいブレードの結果で、Azure Stack ではまだサポートされていません。
- 修復: なし。
- 発生頻度: 一般

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range="azs-1906"
## <a name="1906-update-process"></a>1906 の更新処理

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 1906 Azure Stack の更新プログラムをインストールしようとしたときに、更新の状況が失敗して、状態が **PreparationFailed** に変更される場合があります。 これは、更新リソース プロバイダー (URP) が、処理のためにストレージ コンテナーから内部インフラストラクチャ共有にファイルを正しく転送できないことが原因です。 
- 修復: バージョン 1901 (1.1901.0.95) 以降、この問題は、 **[今すぐ更新]** ( **[再開]** ではない) をもう一度クリックすることで回避できるようになりました。 それにより、URP は前回の試行のファイルをクリーンアップして、ダウンロードを再度開始します。 問題が解決しない場合は、[更新プログラムのインポートとインストールのセクション](azure-stack-apply-updates.md)に従って、更新プログラム パッケージを手動でアップロードすることをお勧めします。
- 発生頻度: 一般

## <a name="portal"></a>ポータル

### <a name="administrative-subscriptions"></a>管理サブスクリプション

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: バージョン 1804 で導入された 2 つの管理サブスクリプションは使用しないでください。 このサブスクリプションの種類は **Metering** サブスクリプションと **Consumption** サブスクリプションです。
- 修復: これら 2 つのサブスクリプション上でリソースが実行されている場合は、ユーザー サブスクリプションで再作成してください。
- 発生頻度: 一般

### <a name="subscription-resources"></a>サブスクリプション リソース

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー サブスクリプションを削除すると、リソースが孤立します。
- 修復: まず、ユーザー リソースまたはリソース グループ全体を削除してから、ユーザー サブスクリプションを削除します。
- 発生頻度: 一般

### <a name="subscription-permissions"></a>サブスクリプションのアクセス許可

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: Azure Stack ポータルを使用して、サブスクリプションへのアクセス許可を表示することはできません。
- 修復: [PowerShell を使用してアクセス許可を確認](/powershell/module/azurerm.resources/get-azurermroleassignment)します。
- 発生頻度: 一般

### <a name="subscriptions-properties-blade"></a>サブスクリプションの [プロパティ] ブレード
- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 管理者ポータルで、サブスクリプションの **[プロパティ]** ブレードが正しく読み込まれません
- 修復: これらのサブスクリプションのプロパティは、[サブスクリプションの概要] ブレードの [要点] ウィンドウで表示できます
- 発生頻度: 一般

### <a name="storage-account-settings"></a>Storage アカウントの設定

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、ストレージ アカウントの **[構成]** ブレードに、**安全な転送の種類**を変更するオプションが表示されます。 機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

### <a name="upload-blob"></a>BLOB のアップロード

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで **[OAuth(preview)]\(OAuth (プレビュー)\)** オプションを使用して BLOB をアップロードしようとすると、タスクがエラー メッセージにより失敗します。
- 修復: SAS オプションを使用して BLOB をアップロードします。
- 発生頻度: 一般

### <a name="update"></a>アップデート

- 適用先:この問題は、1906 リリースに適用されます。
- 原因: オペレーター ポータルで、修正プログラムの更新状態が、更新プログラムに対して正しくない状態を示しています。 初期状態は、まだ進行中であっても、更新プログラムのインストールに失敗したことを示します。
- 修復: ポータルを更新すると、状態が "進行中" に更新されます。
- 発生頻度: 間欠的

## <a name="networking"></a>ネットワーク

### <a name="service-endpoints"></a>サービス エンドポイント

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[仮想ネットワーク]** ブレードに、**サービス エンドポイント**を使用するオプションが表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

### <a name="network-interface"></a>Linux

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 新しいネットワーク インターフェイスを、**実行**状態にある VM に追加することはできません。
- 修復: 仮想マシンを停止してから、ネットワーク インターフェイスを追加/削除します。
- 発生頻度: 一般

### <a name="virtual-network-gateway"></a>Virtual Network ゲートウェイ

#### <a name="alerts"></a>アラート

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[仮想ネットワーク ゲートウェイ]** ブレードに、使用する**アラート** オプションが表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

#### <a name="active-active"></a>アクティブ/アクティブ

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、作成中、および**仮想ネットワーク ゲートウェイ**のリソース メニューで、**アクティブ/アクティブ**構成を有効にするオプションが表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

#### <a name="vpn-troubleshooter"></a>VPN トラブルシューティング ツール

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[接続]** ブレードに **VPN トラブルシューティング ツール**と呼ばれる機能が表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

#### <a name="documentation"></a>ドキュメント

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 仮想ネットワーク ゲートウェイの概要ページにあるドキュメントのリンクは、Azure Stack ではなく Azure 固有のドキュメントにリンクされています。 Azure Stack のドキュメントについては、次のリンクを使用してください。

  - [ゲートウェイ SKU](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-skus)
  - [高可用性接続](../user/azure-stack-vpn-gateway-about-vpn-gateways.md#gateway-availability)
  - [Azure Stack での BGP の構成](../user/azure-stack-vpn-gateway-settings.md#gateway-requirements)
  - [ExpressRoute 回線](azure-stack-connect-expressroute.md)
  - [カスタムの IPsec/IKE ポリシーの指定](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)

### <a name="load-balancer"></a>Load Balancer

#### <a name="add-backend-pool"></a>バックエンド プールを追加する

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで **[バックエンド プール]** を **[Load Balancer]** に追加しようとすると、"**failed to update Load Balancer...** " (Load Balancer の更新に失敗しました) というエラー メッセージで操作が失敗します。
- 修復: PowerShell、CLI、または Resource Manager テンプレートを使用して、バックエンド プールをロード バランサー リソースに関連付けます。
- 発生頻度: 一般

#### <a name="create-inbound-nat"></a>インバウンド NAT を作成する

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで **[Load Balancer]** に対して**インバウンド NAT 規則**を作成しようとすると、"**Failed to update Load Balancer**" (Load Balancer の更新に失敗しました) というエラー メッセージで操作が失敗します。
- 修復: PowerShell、CLI、または Resource Manager テンプレートを使用して、バックエンド プールをロード バランサー リソースに関連付けます。
- 発生頻度: 一般

## <a name="compute"></a>Compute

### <a name="vm-boot-diagnostics"></a>VM ブート診断

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 新しい Windows 仮想マシン (VM) を作成するときに、次のエラーが表示されることがあります。**仮想マシン 'vm-name' を起動できませんでした。エラー:Failed to update serial output settings for VM 'vm-name'** . (VM 'vm-name' のシリアル出力設定を更新できませんでした。) このエラーは、VM でブート診断を有効にしても、ブート診断ストレージ アカウントを削除した場合に発生します。
- 修復: 以前使用したものと同じ名前のストレージ アカウントを再作成します。
- 発生頻度: 一般

### <a name="virtual-machine-scale-set"></a>仮想マシン スケール セット


#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>4 ノードの Azure Stack 環境でのパッチと更新プログラムの適用中に作成が失敗します

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 4 ノードの Azure Stack 環境では、障害ドメインが 3 つの可用性セット内での VM の作成および仮想マシン スケール セット インスタンスの作成が更新プロセス中に **FabricVmPlacementErrorUnsupportedFaultDomainSize** エラーで失敗します。
- 修復: 障害ドメインが 2 つの可用性セット内には 1 つの VM を正常に作成できます。 ただし、4 ノードの Azure Stack では、依然として更新プロセス中にスケール セット インスタンスを作成することはできません。

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH アクセス

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: SSH の承認を有効にして作成した Ubuntu 18.04 VM では、SSH キーを使用してサインインすることはできません。
- 修復: プロビジョニング後に Linux 拡張機能用の VM アクセスを使用して SSH キーを実装するか、パスワードベースの認証を使用します。
- 発生頻度: 一般

### <a name="virtual-machine-scale-set-reset-password-does-not-work"></a>仮想マシン スケール セットのパスワードのリセットが機能しない

- 適用先:この問題は、1906 リリースに適用されます。
- 原因: 新しいパスワードのリセット ブレードはスケール セット UI には表示されますが、Azure Stack ではスケール セットでのパスワードのリセットがまだサポートされていません。
- 修復: なし。
- 発生頻度: 一般

### <a name="rainy-cloud-on-scale-set-diagnostics"></a>スケール セット診断の雨雲

- 適用先:この問題は、1906 リリースに適用されます。
- 原因: 仮想マシン スケール セットの概要ページに、空のグラフが表示されます。 空のグラフをクリックすると、"雨雲" ブレードが開きます。 これは CPU 使用率などのスケール セット診断情報のグラフで、現在の Azure Stack ビルドでサポートされている機能ではありません。
- 修復: なし。
- 発生頻度: 一般

### <a name="virtual-machine-diagnostic-settings-blade"></a>仮想マシンの診断設定ブレード

- 適用先:この問題は、1906 リリースに適用されます。
- 原因: 仮想マシンの診断設定ブレードには、**Application Insight アカウント**を求める **[シンク]** タブがあります。 これは新しいブレードの結果で、Azure Stack ではまだサポートされていません。
- 修復: なし。
- 発生頻度: 一般

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range="azs-1905"
## <a name="1905-update-process"></a>1905 の更新処理

### <a name="host-node-update-prerequisite-failure"></a>ホスト ノードの更新プログラムの前提条件エラー

- 適用先:この問題は、1905 の更新プログラムに適用されます。
- 原因: 1905 Azure Stack の更新プログラムをインストールしようとしたときに、**ホスト ノードの更新プログラムの前提条件**のために更新プログラムの状態が失敗する場合があります。 これは一般に、ホスト ノードの空きディスク領域が十分ではないことが原因です。
- 修復: Azure Stack のサポートに問い合わせて、ホスト ノードのディスク領域のクリアに関する支援を受けます。
- 発生頻度: まれ

### <a name="preparation-failed"></a>準備の失敗

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 1905 Azure Stack の更新プログラムをインストールしようとしたときに、更新の状況が失敗して、状態が **PreparationFailed** に変更される場合があります。 これは、更新リソース プロバイダー (URP) が、処理のためにストレージ コンテナーから内部インフラストラクチャ共有にファイルを正しく転送できないことが原因です。 1905 の更新プログラム パッケージは以前の更新プログラム パッケージより大きく、そのためにこの問題が発生する可能性が高くなります。
- 修復: バージョン 1901 (1.1901.0.95) 以降、この問題は、 **[今すぐ更新]** ( **[再開]** ではない) をもう一度クリックすることで回避できるようになりました。 それにより、URP は前回の試行のファイルをクリーンアップして、ダウンロードを再度開始します。 問題が解決しない場合は、[更新プログラムのインポートとインストールのセクション](azure-stack-apply-updates.md)に従って、更新プログラム パッケージを手動でアップロードすることをお勧めします。
- 発生頻度: 一般

## <a name="portal"></a>ポータル

### <a name="subscription-resources"></a>サブスクリプション リソース

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー サブスクリプションを削除すると、リソースが孤立します。
- 修復: まず、ユーザー リソースまたはリソース グループ全体を削除してから、ユーザー サブスクリプションを削除します。
- 発生頻度: 一般

### <a name="subscription-permissions"></a>サブスクリプションのアクセス許可

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: Azure Stack ポータルを使用して、サブスクリプションへのアクセス許可を表示することはできません。
- 修復: [PowerShell を使用してアクセス許可を確認](/powershell/module/azurerm.resources/get-azurermroleassignment)します。
- 発生頻度: 一般

### <a name="marketplace-management"></a>Marketplace Management

- 適用先:この問題は、1904 と 1905 で発生します
- 原因: 管理者ポータルにサインインしたときに、Marketplace Management 画面が表示されません。
- 修復: ブラウザーを最新の情報に更新するか、 **[設定]** に移動してオプション **[既定の設定にリセット]** を選択します。
- 発生頻度: 間欠的

### <a name="docker-extension"></a>Docker 拡張機能

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 管理者ポータルとユーザー ポータルの両方で、**Docker** を検索すると、返される項目が正しくありません。 これは Azure Stack でご利用になれません。 作成しようとすると、エラーが表示されます。
- 修復: 軽減策はありません。
- 発生頻度: 一般

### <a name="upload-blob"></a>BLOB のアップロード

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで **[OAuth(preview)]\(OAuth (プレビュー)\)** オプションを使用して BLOB をアップロードしようとすると、タスクがエラー メッセージにより失敗します。
- 修復: SAS オプションを使用して BLOB をアップロードします。
- 発生頻度: 一般

### <a name="template"></a>Template

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、テンプレート デプロイ UI によって "_" (アンダースコア文字) で始まるテンプレート名のパラメーターの設定が行われません。
- 修復: テンプレート名から "_" (アンダースコア文字) を削除します。
- 発生頻度: 一般

## <a name="networking"></a>ネットワーク

### <a name="load-balancer"></a>Load Balancer

#### <a name="add-backend-pool"></a>バックエンド プールを追加する

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで **[バックエンド プール]** を **[Load Balancer]** に追加しようとすると、"**failed to update Load Balancer...** " (Load Balancer の更新に失敗しました) というエラー メッセージで操作が失敗します。
- 修復: PowerShell、CLI、または Resource Manager テンプレートを使用して、バックエンド プールをロード バランサー リソースに関連付けます。
- 発生頻度: 一般

#### <a name="create-inbound-nat"></a>インバウンド NAT を作成する

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで **[Load Balancer]** に対して**インバウンド NAT 規則**を作成しようとすると、"**Failed to update Load Balancer**" (Load Balancer の更新に失敗しました) というエラー メッセージで操作が失敗します。
- 修復: PowerShell、CLI、または Resource Manager テンプレートを使用して、バックエンド プールをロード バランサー リソースに関連付けます。
- 発生頻度: 一般

#### <a name="create-load-balancer"></a>ロード バランサーの作成

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[ロード バランサーの作成]** ウィンドウに **Standard** Load Balancer SKU を作成するためのオプションが表示されます。 このオプションは Azure Stack ではサポートされていません。
- 修復: 代わりに **Basic** ロード バランサー オプションを使用します。
- 発生頻度: 一般

### <a name="public-ip-address"></a>パブリック IP アドレス

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[パブリック IP アドレスの作成]** ウィンドウに **Standard** SKU を作成するためのオプションが表示されます。 **Standard** SKU は Azure Stack ではサポートされていません。
- 修復: パブリック IP アドレスに **Basic** SKU を使用します。
- 発生頻度: 一般

## <a name="compute"></a>Compute

### <a name="vm-boot-diagnostics"></a>VM ブート診断

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 新しい Windows 仮想マシン (VM) を作成するときに、次のエラーが表示されることがあります。**仮想マシン 'vm-name' を起動できませんでした。エラー:Failed to update serial output settings for VM 'vm-name'** . (VM 'vm-name' のシリアル出力設定を更新できませんでした。)
このエラーは、VM でブート診断を有効にしても、ブート診断ストレージ アカウントを削除した場合に発生します。
- 修復: 以前使用したものと同じ名前のストレージ アカウントを再作成します。
- 発生頻度: 一般

### <a name="vm-resize"></a>VM のサイズ変更

- 適用先:この問題は、1905 リリースに適用されます。
- 原因: マネージド ディスク VM のサイズ変更ができません。 VM のサイズ変更を試行すると、"コード": "InternalOperationError"、"メッセージ": "操作中に内部エラーが発生しました" を伴うエラーが発生します。
- 修復: 次のリリースで修復できるよう取り組んでいます。 現時点では、新しい VM サイズで VM を再作成する必要があります。
- 発生頻度: 一般

### <a name="virtual-machine-scale-set"></a>仮想マシン スケール セット

#### <a name="centos"></a>CentOS

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 仮想マシン スケール セットの作成エクスペリエンスでは、デプロイのオプションとして CentOS-based 7.2 が提供されます。 CentOS 7.2 は Azure Stack Marketplace では利用できず、イメージが見つからないという理由でデプロイ エラーが発生します。
- 修復: デプロイ用に別のオペレーティング システムを選択するか、またはデプロイする前にオペレーターが Marketplace からダウンロードしておいた別の CentOS イメージを指定する Azure Resource Manager テンプレートを使用します。
- 発生頻度: 一般

#### <a name="remove-scale-set"></a>スケール セットを削除する

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: スケール セットを**仮想マシン スケール セット** ブレードから削除することはできません。
- 修復: 削除するスケール セットを選択し、 **[概要]** ウィンドウから **[削除]** ボタンをクリックします。
- 発生頻度: 一般

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>4 ノードの Azure Stack 環境でのパッチと更新プログラムの適用中に作成が失敗します

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 4 ノードの Azure Stack 環境では、障害ドメインが 3 つの可用性セット内での VM の作成および仮想マシン スケール セット インスタンスの作成が更新プロセス中に **FabricVmPlacementErrorUnsupportedFaultDomainSize** エラーで失敗します。
- 修復: 障害ドメインが 2 つの可用性セット内には 1 つの VM を正常に作成できます。 ただし、4 ノードの Azure Stack では、依然として更新プロセス中にスケール セット インスタンスを作成することはできません。

#### <a name="scale-set-instance-view-blade-doesnt-load"></a>スケール セット インスタンス ビュー ブレードが読み込まれない

- 適用先:この問題は、1904 と 1905 リリースで発生します。
- 原因: Azure Stack ポータル ([ダッシュボード] -> [仮想マシン スケール セット] -> [AnyScaleSet - インスタンス] -> [AnyScaleSetInstance]) にある仮想マシン スケール セットのインスタンス ビュー ブレードが、読み込みに失敗し、泣いているクラウドの画像を表示します。
- 修復: 現在、修復方法はなく、修正に取り組んでいます。 それまでは、スケール セットのインスタンス ビューの取得には、CLI コマンド `az vmss get-instance-view` を使用してください。

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH アクセス

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: SSH の承認を有効にして作成した Ubuntu 18.04 VM では、SSH キーを使用してサインインすることはできません。
- 修復: プロビジョニング後に Linux 拡張機能用の VM アクセスを使用して SSH キーを実装するか、パスワードベースの認証を使用します。
- 発生頻度: 一般

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

::: moniker range=">=azs-1905"
## <a name="archive"></a>アーカイブ

アーカイブされた、以前のバージョンの既知の問題にアクセスするには、左側の目次の上部にあるバージョン セレクターのドロップダウンを使用して、表示するバージョンを選択します。

## <a name="next-steps"></a>次の手順

- [更新アクティビティのチェック リストを確認する](release-notes-checklist.md)
- [セキュリティ更新プログラムの一覧を確認する](release-notes-security-updates.md)
::: moniker-end

<!------------------------------------------------------------>
<!------------------- UNSUPPORTED VERSIONS ------------------->
<!------------------------------------------------------------>
::: moniker range="azs-1904"
## <a name="1904-archived-known-issues"></a>1904 アーカイブされた既知の問題
::: moniker-end
::: moniker range="azs-1903"
## <a name="1903-archived-known-issues"></a>1903 アーカイブされた既知の問題
::: moniker-end
::: moniker range="azs-1902"
## <a name="1902-archived-known-issues"></a>1902 アーカイブされた既知の問題
::: moniker-end
::: moniker range="azs-1901"
## <a name="1901-archived-known-issues"></a>1901 アーカイブされた既知の問題
::: moniker-end
::: moniker range="azs-1811"
## <a name="1811-archived-known-issues"></a>1811 アーカイブされた既知の問題
::: moniker-end
::: moniker range="azs-1809"
## <a name="1809-archived-known-issues"></a>1809 アーカイブされた既知の問題
::: moniker-end
::: moniker range="azs-1808"
## <a name="1808-archived-known-issues"></a>1808 アーカイブされた既知の問題
::: moniker-end
::: moniker range="azs-1807"
## <a name="1807-archived-known-issues"></a>1807 アーカイブされた既知の問題
::: moniker-end
::: moniker range="azs-1805"
## <a name="1805-archived-known-issues"></a>1805 アーカイブされた既知の問題
::: moniker-end
::: moniker range="azs-1804"
## <a name="1804-archived-known-issues"></a>1804 アーカイブされた既知の問題
::: moniker-end
::: moniker range="azs-1803"
## <a name="1803-archived-known-issues"></a>1803 アーカイブされた既知の問題
::: moniker-end
::: moniker range="azs-1802"
## <a name="1802-archived-known-issues"></a>1802 アーカイブされた既知の問題
::: moniker-end

::: moniker range="<azs-1905"
[以前のバージョンの Azure Stack の既知の問題は TechNet ギャラリー](https://aka.ms/azsarchivedrelnotes)でアクセスできます。 これらのアーカイブされたドキュメントは、参照のみを目的に提供されており、これらのバージョンのサポートを意味しているわけではありません。 Azure Stack のサポートについては、「[Azure Stack サービス ポリシー](azure-stack-servicing-policy.md)」をご覧ください。 さらにサポートが必要な場合は、Microsoft カスタマー サポート サービスにお問い合わせください。
::: moniker-end