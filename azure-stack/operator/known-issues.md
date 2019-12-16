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
ms.date: 11/21/2019
ms.author: sethm
ms.reviewer: prchint
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: 684f6925c32eabca51fa89776ec95b57a852a987
ms.sourcegitcommit: d9430072dd96ae305101da6d8a47d6c23a0a64c2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2019
ms.locfileid: "74995588"
---
# <a name="azure-stack-known-issues"></a>Azure Stack の既知の問題

この記事では、Azure Stack のリリースにおける既知の問題を一覧表示します。 新しい問題が特定されると、この一覧は更新されます。

別のバージョンの既知の問題にアクセスするには、左側の目次の上部にあるバージョン セレクターのドロップダウンを使用します。

::: moniker range=">=azs-1906"
> [!IMPORTANT]  
> 更新プログラムを適用する前に、このセクションを確認してください。
::: moniker-end
::: moniker range="<azs-1906"
> [!IMPORTANT]  
> ご利用の Azure Stack インスタンスが 2 つ前の更新プログラムより遅れている場合、コンプライアンスに対応していないとみなされます。 [サポートを受けるためには、少なくともサポートされる最小バージョンまで更新する](azure-stack-servicing-policy.md#keep-your-system-under-support)必要があります。 
::: moniker-end

<!---------------------------------------------------------->
<!------------------- SUPPORTED VERSIONS ------------------->
<!---------------------------------------------------------->

::: moniker range="azs-1910"
## <a name="update"></a>更新

Azure Stack Hub の更新に関する既知の問題については、[Azure Stack Hub の更新プログラムに関するトラブルシューティング](azure-stack-updates-troubleshoot.md)に関する記事を参照してください。


## <a name="portal"></a>ポータル

### <a name="administrative-subscriptions"></a>管理サブスクリプション

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: バージョン 1804 で導入された 2 つの管理サブスクリプションは使用しないでください。 このサブスクリプションの種類は **Metering** サブスクリプションと **Consumption** サブスクリプションです。
- 修復: これら 2 つのサブスクリプション上でリソースが実行されている場合は、ユーザー サブスクリプションで再作成してください。
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

### <a name="upload-blob-with-oauth-error"></a>OAuth を使用した BLOB のアップロードのエラー

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで **[OAuth(preview)]\(OAuth (プレビュー)\)** オプションを使用して BLOB をアップロードしようとすると、タスクがエラー メッセージにより失敗します。
- 修復: SAS オプションを使用して BLOB をアップロードします。
- 発生頻度: 一般

### <a name="upload-blob-option-unsupported"></a>サポートされていない BLOB のアップロード オプション

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルのアップロード ブレードで BLOB をアップロードしようとすると、 **[AAD]** または **[Key Authentication]\(キー認証\)** を選択するオプションが表示されますが、Azure Stack では **[AAD]** はサポートされていません。
- 発生頻度: 一般

### <a name="load-balancer-backend-pool"></a>ロード バランサー バックエンド プール

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで**ロード バランサー** バックエンド プールを追加すると、結果として "**ロード バランサー バックエンド プールを保存できませんでした**" というエラー メッセージが表示されますが、実際には操作は成功しています。
- 発生頻度: 一般

### <a name="incorrect-tooltip-when-creating-vm"></a>VM 作成時のヒントが正しくない

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、Premium SSD のディスクの種類でマネージド ディスクを選択すると、ドロップダウン リストに **[OS ディスク]** と表示されます。 そのオプションの横には、"**Azure の無料アカウントでは、一定の OS ディスク サイズを無料で利用できます**" というヒントが表示されますが、Azure Stack はこの対象外です。 さらに、リストには **[Free account eligible]\(無料アカウント利用可能\)** が含まれていますが、これも Azure Stack は対象外です。
- 発生頻度: 一般

### <a name="vpn-troubleshoot-and-metrics"></a>VPN のトラブルシューティングとメトリック

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルでは、VPN ゲートウェイ リソースに **[VPN のトラブルシューティング]** 機能と **[メトリック]** が表示されますが、これは Azure Stack ではサポートされていません。
- 発生頻度: 一般

### <a name="adding-extension-to-vm-scale-set"></a>VM スケール セットへの拡張機能の追加

- 適用先:この問題は、1907 以降のリリースに適用されます。
- 原因: 仮想マシン スケール セットを作成しても、ユーザー ポータルの UI で拡張機能を追加できません。
- 発生頻度: 一般

### <a name="delete-a-storage-container"></a>ストレージ コンテナーを削除する

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルでストレージ コンテナーを削除しようとしたときに、 **[Azure Policy と RBAC 役割の設定を上書きする]** を切り替えないと、操作が失敗します。
- 修復: **[Azure Policy と RBAC 役割の設定を上書きする]** チェック ボックスがオンになっていることを確認します。
- 発生頻度: 一般

### <a name="refresh-button-on-virtual-machines-fails"></a>仮想マシンの [更新] ボタンが失敗する

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで **[Virtual Machines]** に移動し、上部のボタンを使用して更新しようとすると、状態が正確に更新されません。
- 修復: [更新] ボタンがクリックされたかどうかに関係なく、状態は 5 分ごとに自動的に更新されます。 5 分待ってから状態を確認してください。
- 発生頻度: 一般

### <a name="virtual-network-gateway"></a>Virtual Network ゲートウェイ

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルでルート テーブルを作成すると、次のホップの種類オプションの 1 つとして **[Virtual Network ゲートウェイ]** が表示されますが、これは Azure Stack ではサポートされていません。
- 発生頻度: 一般

### <a name="storage-account-options"></a>ストレージ アカウントのオプション

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルでは、ストレージ アカウントの名前が **[ストレージ アカウント - Blob、File、Table、Queue]** と表示されますが、**File** は Azure Stack ではサポートされていません。
- 発生頻度: 一般

### <a name="storage-account-configuration"></a>Storage account configuration

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルでストレージ アカウントを作成し、その **[構成]** を表示すると、AJAX エラーが発生するため、構成の変更を保存できません。
- 発生頻度: 一般

### <a name="capacity-monitoring-in-sql-resource-provider-keeps-loading"></a>SQL リソース プロバイダーの容量監視で読み込み処理が続く

- 適用先:この問題は、SQL リソース プロバイダー バージョン 1.1.33.0 以前がインストールされた、Azure Stack 1910 更新プログラム以降に適用されます。
- 原因: SQL リソース プロバイダーの現在のバージョンは、1910 更新プログラムに含まれる最新のポータル変更の一部と互換性がありません。
- 修復: Azure Stack が 1910 更新プログラム ([SQL RP バージョン 1.1.47.0](https://aka.ms/azurestacksqlrp11470)) にアップグレードされた後、リソース プロバイダーの更新プロセスに従って、SQL リソース プロバイダーの修正プログラム 1.1.47.0 を適用します。 MySQL リソース プロバイダーの場合、Azure Stack を 1910 更新プログラム ([MySQL RP バージョン 1.1.47.0](https://aka.ms/azurestackmysqlrp11470)) にアップグレードした後、MySQL リソース プロバイダーの修正プログラム 1.1.47.0 を適用することもお勧めします。
- 発生頻度: 一般

### <a name="access-control-iam"></a>アクセス制御 (IAM)

- 適用先:この問題は、1903 以降のリリースに適用されます
- 原因: IAM 拡張機能が古すぎます。 Ibiza に付属の Azure Stack に導入されている新しい動作は、ユーザーがグローバル サブスクリプション ピッカーで選択されていないサブスクリプションから RBAC 拡張機能を開いている場合に、RBAC 拡張機能が中断する原因となります。
- 修復: グローバル サブスクリプション ピッカー内でサブスクリプションがオンになっていることを確認します。 

## <a name="networking"></a>ネットワーク

### <a name="load-balancer"></a>Load Balancer

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 可用性セットの VM をロード バランサーのバックエンド プールに追加すると、"**ロード バランサー バックエンド プールを保存できませんでした**" というエラー メッセージがポータルに表示されます。 これはポータルの表面的な問題です。機能は有効であり、内部的には VM はバックエンド プールに正常に追加されます。
- 発生頻度: 一般

### <a name="network-security-groups"></a>ネットワーク セキュリティ グループ

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。 
- 原因: 明示的な **DenyAllOutbound** 規則は、VM のデプロイを完了するために必要なインフラストラクチャへの内部通信がすべて妨げられるため、NSG に作成することはできません。
- 発生頻度: 一般

### <a name="service-endpoints"></a>サービス エンドポイント

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[仮想ネットワーク]** ブレードに、**サービス エンドポイント**を使用するオプションが表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

### <a name="network-interface"></a>Linux

#### <a name="addingremoving-network-interface"></a>ネットワーク インターフェイスの追加/削除

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 新しいネットワーク インターフェイスを、**実行**状態にある VM に追加することはできません。
- 修復: 仮想マシンを停止してから、ネットワーク インターフェイスを追加または削除します。
- 発生頻度: 一般

#### <a name="primary-network-interface"></a>プライマリ ネットワーク インターフェイス

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: VM のプライマリ NIC を変更することはできません。 プライマリ NIC を削除またはデタッチすると、VM の起動時に問題が発生します。
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

## <a name="compute"></a>Compute

### <a name="vm-boot-diagnostics"></a>VM ブート診断

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 新しい Windows 仮想マシン (VM) を作成するときに、次のエラーが表示されることがあります。**仮想マシン 'vm-name' を起動できませんでした。エラー:Failed to update serial output settings for VM 'vm-name'** . (VM 'vm-name' のシリアル出力設定を更新できませんでした。) このエラーは、VM でブート診断を有効にしても、ブート診断ストレージ アカウントを削除した場合に発生します。
- 修復: 以前使用したものと同じ名前のストレージ アカウントを再作成します。
- 発生頻度: 一般

### <a name="consumed-compute-quota"></a>コンピューティング クォータの消費

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 新しい仮想マシンを作成すると、"**This subscription is at capacity for Total Regional vCPUs on this location.This subscription is using all 50 Total Regional vCPUs available.** " (このサブスクリプションはこの場所の [リージョンの vCPU の合計] の容量に達しています。このサブスクリプションは、[リージョンの vCPU の合計] の使用可能な 50 個をすべて使用しています。)" などのエラーを受け取ることがあります。 これは、使用可能な合計コア数のクォータに達したことを示します。
- 修復: クォータを追加したアドオン プランをオペレーターに依頼してください。 現在のプランのクォータの編集では解決せず、クォータの増加は反映されません。
- 発生頻度: 珍しい

### <a name="virtual-machine-scale-set"></a>仮想マシン スケール セット

#### <a name="create-failures-during-patch-and-update-on-4-node-azure-stack-environments"></a>4 ノードの Azure Stack 環境でのパッチと更新プログラムの適用中に作成が失敗します

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 4 ノードの Azure Stack 環境では、障害ドメインが 3 つの可用性セット内での VM の作成および仮想マシン スケール セット インスタンスの作成が更新プロセス中に **FabricVmPlacementErrorUnsupportedFaultDomainSize** エラーで失敗します。
- 修復: 障害ドメインが 2 つの可用性セット内には 1 つの VM を正常に作成できます。 ただし、4 ノードの Azure Stack のデプロイでは、依然として更新プロセス中にスケール セット インスタンスを作成することはできません。

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->
::: moniker-end

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

### <a name="load-balancer"></a>Load Balancer

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。 
- 原因: 可用性セットの VM をロード バランサーのバックエンド プールに追加すると、"**ロード バランサー バックエンド プールを保存できませんでした**" というエラー メッセージがポータルに表示されます。 これはポータルの表面的な問題です。機能は有効であり、内部的には VM はバックエンド プールに正常に追加されます。 
- 発生頻度: 一般

### <a name="network-security-groups"></a>ネットワーク セキュリティ グループ

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。 
- 原因: 明示的な **DenyAllOutbound** 規則は、VM のデプロイを完了するために必要なインフラストラクチャへの内部通信がすべて妨げられるため、NSG に作成することはできません。
- 発生頻度: 一般

### <a name="service-endpoints"></a>サービス エンドポイント

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[仮想ネットワーク]** ブレードに、**サービス エンドポイント**を使用するオプションが表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

### <a name="network-interface"></a>Linux

#### <a name="addingremoving-network-interface"></a>ネットワーク インターフェイスの追加/削除

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 新しいネットワーク インターフェイスを、**実行**状態にある VM に追加することはできません。
- 修復: 仮想マシンを停止してから、ネットワーク インターフェイスを追加/削除します。
- 発生頻度: 一般

#### <a name="primary-network-interface"></a>プライマリ ネットワーク インターフェイス

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

### <a name="load-balancer"></a>Load Balancer

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。 
- 原因: 可用性セットの VM をロード バランサーのバックエンド プールに追加すると、"**ロード バランサー バックエンド プールを保存できませんでした**" というエラー メッセージがポータルに表示されます。 これはポータルの表面的な問題です。機能は有効であり、内部的には VM はバックエンド プールに正常に追加されます。 
- 発生頻度: 一般

### <a name="network-security-groups"></a>ネットワーク セキュリティ グループ

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。 
- 原因: 明示的な **DenyAllOutbound** 規則は、VM のデプロイを完了するために必要なインフラストラクチャへの内部通信がすべて妨げられるため、NSG に作成することはできません。
- 発生頻度: 一般

### <a name="service-endpoints"></a>サービス エンドポイント

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[仮想ネットワーク]** ブレードに、**サービス エンドポイント**を使用するオプションが表示されます。 この機能は現在 Azure Stack でサポートされていません。
- 発生頻度: 一般

### <a name="network-interface"></a>Linux

#### <a name="addingremoving-network-interface"></a>ネットワーク インターフェイスの追加/削除

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 新しいネットワーク インターフェイスを、**実行**状態にある VM に追加することはできません。
- 修復: 仮想マシンを停止してから、ネットワーク インターフェイスを追加/削除します。
- 発生頻度: 一般

#### <a name="primary-network-interface"></a>プライマリ ネットワーク インターフェイス

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

### <a name="update"></a>更新

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

::: moniker range=">=azs-1906"
## <a name="archive"></a>アーカイブ

アーカイブされた、以前のバージョンの既知の問題にアクセスするには、左側の目次の上部にあるバージョン セレクターのドロップダウンを使用して、表示するバージョンを選択します。

## <a name="next-steps"></a>次の手順

- [更新アクティビティのチェック リストを確認する](release-notes-checklist.md)
- [セキュリティ更新プログラムの一覧を確認する](release-notes-security-updates.md)
::: moniker-end

<!------------------------------------------------------------>
<!------------------- UNSUPPORTED VERSIONS ------------------->
<!------------------------------------------------------------>
::: moniker range="azs-1905"
## <a name="1905-archived-known-issues"></a>1905 アーカイブされた既知の問題
::: moniker-end
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

::: moniker range="<azs-1906"
[以前のバージョンの Azure Stack の既知の問題は TechNet ギャラリー](https://aka.ms/azsarchivedrelnotes)でアクセスできます。 これらのアーカイブされたドキュメントは、参照のみを目的に提供されており、これらのバージョンのサポートを意味しているわけではありません。 Azure Stack のサポートについては、「[Azure Stack サービス ポリシー](azure-stack-servicing-policy.md)」をご覧ください。 さらにサポートが必要な場合は、Microsoft カスタマー サポート サービスにお問い合わせください。
::: moniker-end
