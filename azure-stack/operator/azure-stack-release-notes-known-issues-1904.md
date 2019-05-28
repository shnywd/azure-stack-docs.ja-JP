---
title: Azure Stack 1904 の既知の問題 | Microsoft Docs
description: Azure Stack 1904 の既知の問題について説明します。
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
ms.date: 05/15/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 05/15/2019
ms.openlocfilehash: 52279a7498e253771e16e66e0c5025b9afd4494d
ms.sourcegitcommit: 442bd62d1dfbc1597592d7285aba1453298261ce
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969834"
---
# <a name="azure-stack-1904-known-issues"></a>Azure Stack 1904 の既知の問題

この記事では、Azure Stack 1904 リリースの既知の問題について説明します。 新しい問題が特定されると、この一覧は更新されます。

> [!IMPORTANT]  
> 更新プログラムを適用する前に、このセクションを確認してください。

## <a name="update-process"></a>更新処理

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: Azure Stack の更新プログラムをインストールしようとしたときに、更新の状況が失敗して、状態が **PreparationFailed** に変更される場合があります。 これは、更新リソース プロバイダー (URP) が、処理のためにストレージ コンテナーから内部インフラストラクチャ共有にファイルを正しく転送できないことが原因です。
- 修復: バージョン 1901 (1.1901.0.95) 以降、この問題は、 **[今すぐ更新]** ( **[再開]** ではない) をもう一度クリックすることで回避できるようになりました。 それにより、URP は前回の試行のファイルをクリーンアップして、もう一度ダウンロードを開始します。
- 発生頻度: 一般

## <a name="portal"></a>ポータル

### <a name="administrative-subscriptions"></a>管理サブスクリプション

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: バージョン 1804 で導入された 2 つの管理サブスクリプションは使用しないでください。 このサブスクリプションの種類は **Metering** サブスクリプションと **Consumption** サブスクリプションです。
- 修復: これらのサブスクリプションは 1905 から一時停止され、最終的には削除される予定です。 これら 2 つのサブスクリプション上でリソースが実行されている場合は、1905 より前のユーザー サブスクリプションで再作成してください。
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

- 適用先:これはリリース 1904 に関する新しい問題です。
- 原因: 管理者ポータルにサインインしたときに、Marketplace Management 画面が表示されません。
- 修復: ブラウザーを更新します。
- 発生頻度: 間欠的

### <a name="marketplace-management"></a>Marketplace Management

- 適用先:この問題は、1904 に適用されます。
- 原因: 管理者ポータル内の Marketplace management タブの **[Azure から追加]** ブレードで結果をフィルター処理したときに、間違ったフィルター処理結果が表示されることがあります。 
- 修復: [名前] 列で結果の並べ替えを行うと、結果が修正されます。 
- 発生頻度: 間欠的

### <a name="marketplace-management"></a>Marketplace Management

- 適用先:この問題は、1904 に適用されます。
- 原因: 管理者ポータルの Marketplace Management で結果をフィルター処理すると、パブリッシャー ドロップダウンに、重複するパブリッシャー名が表示されます。 
- 修復: そのパブリッシャーで利用可能なすべてのマーケットプレース製品の正しい一覧を得るには、すべての重複を選択します。 
- 発生頻度: 間欠的

### <a name="upload-blob"></a>BLOB のアップロード

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで [OAuth (プレビュー)] オプションを使用して BLOB をアップロードしようとすると、タスクがエラー メッセージで失敗します。
- 修復: SAS オプションを使用して BLOB をアップロードします。
- 発生頻度: 一般

## <a name="networking"></a>ネットワーク

### <a name="load-balancer"></a>Load Balancer

#### <a name="add-backend-pool"></a>バックエンド プールを追加する

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで **[バックエンド プール]** を **[Load Balancer]** に追加しようとすると、"**Failed to update Load Balancer**" (Load Balancer の更新に失敗しました) というエラー メッセージで操作が失敗します。
- 修復: PowerShell、CLI、または Azure Resource Manager テンプレートを使用して、バックエンド プールをロード バランサー リソースに関連付けます。
- 発生頻度: 一般

#### <a name="create-inbound-nat"></a>インバウンド NAT を作成する

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで **[Load Balancer]** に対して**インバウンド NAT 規則**を作成しようとすると、"**Failed to update Load Balancer**" (Load Balancer の更新に失敗しました) というエラー メッセージで操作が失敗します。
- 修復: PowerShell、CLI、または Azure Resource Manager テンプレートを使用して、バックエンド プールをロード バランサー リソースに関連付けます。
- 発生頻度: 一般

#### <a name="create-load-balancer"></a>ロード バランサーの作成

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[ロード バランサーの作成]** ウィンドウに **Standard** Load Balancer SKU を作成するためのオプションが表示されます。 このオプションは Azure Stack ではサポートされていません。
- 修復: 代わりに Basic ロード バランサー オプションを使用します。
- 発生頻度: 一般

#### <a name="public-ip-address"></a>パブリック IP アドレス

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、 **[パブリック IP アドレスの作成]** ウィンドウに **Standard** SKU を作成するためのオプションが表示されます。 **Standard** SKU は Azure Stack ではサポートされていません。
- 修復: パブリック IP アドレスではなく **Basic** SKU を使用します。
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
- 原因: Virtual Machine Scale Set (VMSS) の作成エクスペリエンスには、デプロイのオプションとして CentOS-based 7.2 が表示されます。 CentOS 7.2 は Azure Stack では利用できません。
- 修復: デプロイ用に別のオペレーティング システムを選択するか、またはデプロイする前にオペレーターが Marketplace からダウンロードしておいた別の CentOS イメージを指定する Azure Resource Manager テンプレートを使用します。
- 発生頻度: 一般

#### <a name="remove-scale-set"></a>スケール セットを削除する

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: スケール セットを **[Virtual Machine Scale Sets]** ブレードから削除することはできません。
- 修復: 削除するスケール セットを選択し、 **[概要]** ウィンドウから **[削除]** ボタンをクリックします。
- 発生頻度: 一般

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH アクセス

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: SSH の承認を有効にして作成した Ubuntu 18.04 VM では、SSH キーを使用してサインインすることはできません。
- 修復: プロビジョニング後に Linux 拡張機能用の VM アクセスを使用して SSH キーを実装するか、パスワードベースの認証を使用します。
- 発生頻度: 一般

### <a name="compute-host-agent-alert"></a>コンピューティング ホスト エージェントのアラート

- 適用先:これはリリース 1904 に関する新しい問題です。
- 原因: **コンピューティング ホスト エージェント**の警告は、スケール ユニット内のノードの再起動後に表示されます。 再起動により、コンピューティング ホスト エージェント サービスの既定のスタートアップ設定が変更されます。
- 修復: 
  - このアラートは無視してかまいません。 応答していないエージェントは、オペレーターとユーザーの操作やユーザーのアプリケーションに何の影響もありません。 アラートは、手動で閉じられた場合、24 時間後に再び表示されます。
  - Microsoft サポートは、サービスのスタートアップ設定を変更することにより問題を修復することができます。 このためには、サポート チケットを開く必要があります。 ノードがもう一度再起動されると、新しいアラートが表示されます。
- 発生頻度: 一般

## <a name="app-service"></a>App Service

- テナントは、サブスクリプションで最初の Azure 関数を作成する前に、ストレージ リソースプロバイダーを登録する必要があります。
- 1903 のポータル フレームワークと互換性がないため、テナント ポータルの一部のユーザー エクスペリエンス (主に、デプロイ スロット、運用環境でのテスト、およびサイト拡張機能のユーザー エクスペリエンス) は壊れています。 この問題を回避するには、[Azure App Service PowerShell モジュール](/azure/app-service/deploy-staging-slots#automate-with-powershell)または [Azure CLI](/cli/azure/webapp/deployment/slot?view=azure-cli-latest) を使用します。 ポータル エクスペリエンスは、Azure Stack 1.6 (更新 6).上の Azure App Service の今後のリリースで復元されます。

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->

## <a name="next-steps"></a>次の手順

- [更新アクティビティのチェック リストを確認する](azure-stack-release-notes-checklist.md)
- [セキュリティ更新プログラムの一覧を確認する](azure-stack-release-notes-security-updates-1904.md)
