---
title: Azure Stack 1905 の既知の問題 | Microsoft Docs
description: Azure Stack 1905 の既知の問題について説明します。
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
ms.date: 06/05/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 06/05/2019
ms.openlocfilehash: 38c7ec337ba8cdb73925b1c07f77331c05b25d8a
ms.sourcegitcommit: e51cdc84a09250e8fa701bb2cb09de38d7de2c07
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "66836677"
---
# <a name="azure-stack-1905-known-issues"></a>Azure Stack 1905 の既知の問題

この記事では、Azure Stack 1905 リリースの既知の問題について説明します。 新しい問題が特定されると、この一覧は更新されます。

> [!IMPORTANT]  
> 更新プログラムを適用する前に、このセクションを確認してください。

## <a name="update-process"></a>更新処理

### <a name="host-node-update-prerequisite-failure"></a>ホスト ノードの更新プログラムの前提条件エラー

- 適用先:この問題は、1905 の更新プログラムに適用されます。
- 原因: 1905 Azure Stack の更新プログラムをインストールしようとしたときに、**ホスト ノードの更新プログラムの前提条件**のために更新プログラムの状態が失敗する場合があります。 これは一般に、ホスト ノードの空きディスク領域が十分ではないことが原因です。
- 修復: Azure Stack のサポートに問い合わせて、ホスト ノードのディスク領域のクリアに関する支援を受けます。
- 発生頻度: まれ

### <a name="preparation-failed"></a>準備の失敗

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 1905 Azure Stack の更新プログラムをインストールしようとしたときに、更新の状況が失敗して、状態が **PreparationFailed** に変更される場合があります。 これは、更新リソース プロバイダー (URP) が、処理のためにストレージ コンテナーから内部インフラストラクチャ共有にファイルを正しく転送できないことが原因です。 1905 の更新プログラム パッケージは以前の更新プログラム パッケージより大きく、そのためにこの問題が発生する可能性が高くなります。
- 修復: バージョン 1901 (1.1901.0.95) 以降、この問題は、 **[今すぐ更新]** ( **[再開]** ではない) をもう一度クリックすることで回避できるようになりました。 それにより、URP は前回の試行のファイルをクリーンアップして、ダウンロードを再度開始します。 問題が解決しない場合は、[更新プログラムのインポートとインストールのセクション](azure-stack-apply-updates.md#import-and-install-updates)に従って、更新プログラム パッケージを手動でアップロードすることをお勧めします。
- 発生頻度: 一般

## <a name="portal"></a>ポータル

### <a name="administrative-subscriptions"></a>管理サブスクリプション

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: バージョン 1804 で導入された 2 つの管理サブスクリプションは使用しないでください。 このサブスクリプションの種類は **Metering** サブスクリプションと **Consumption** サブスクリプションです。
- 修復: これらのサブスクリプションは 1906 から一時停止され、最終的には削除される予定です。 これら 2 つのサブスクリプション上でリソースが実行されている場合は、1906 より前のユーザー サブスクリプションで再作成してください。
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

#### <a name="vmss-instance-view-blade-doesnt-load"></a>VMSS インスタンス ビュー ブレードが読み込まれない
 
- 適用先:この問題は、1904 と 1905 リリースで発生します。
- 原因: Azure Stack ポータル ([ダッシュボード] -> [仮想マシン スケール セット] -> [AnyScaleSet - インスタンス] -> [AnyScaleSetInstance]) にあるスケール セットのインスタンス ビュー ブレードが、クラウドの問題で読み込みに失敗します。
- 修復: 現在、修復方法はなく、修正に取り組んでいます。 それまでは、CLI コマンド az vmss get-instance-view を使って VMSS のインスタンス ビューを取得してください。

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

## <a name="next-steps"></a>次の手順

- [更新アクティビティのチェック リストを確認する](azure-stack-release-notes-checklist.md)
- [セキュリティ更新プログラムの一覧を確認する](azure-stack-release-notes-security-updates-1905.md)
