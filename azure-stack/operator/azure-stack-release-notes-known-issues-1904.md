---
title: Azure Stack リリース ノート - 1904 の既知の問題 | Microsoft Docs
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
ms.date: 05/07/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: 782e0aaea0eadddae24795616aaba6053b728134
ms.sourcegitcommit: 39ba6d18781aed98b29ac5e08aac2d75c37bf18c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2019
ms.locfileid: "65387172"
---
# <a name="azure-stack-1904-known-issues"></a>Azure Stack 1904 の既知の問題

この記事では、Azure Stack 1904 リリースの既知の問題について説明します。 新しい問題が特定されると、この一覧は更新されます。

> [!IMPORTANT]  
> 更新プログラムを適用する前に、このセクションを確認してください。

## <a name="portal"></a>ポータル

### <a name="add-on-plans"></a>アドオン プラン

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: アドオン プランとしてユーザー サブスクリプションに追加されたプランは、ユーザー サブスクリプションからプランを削除しても削除できません。 アドオン プランを参照するサブスクリプションも削除されるまで、プランは残ります。
- 修復: 軽減策はありません。
- 発生頻度: 一般

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

- 適用先:この問題は、1904 に適用されます。
- 原因: 管理者ポータルにサインオンしているときに、マーケットプレース管理画面が表示されません。
- 修復: ブラウザーを更新します。
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
- 修復: PowerShell、CLI、または Resource Manager テンプレートを使用して、バックエンド プールをロード バランサー リソースに関連付けます。
- 発生頻度: 一般

#### <a name="create-inbound-nat"></a>インバウンド NAT を作成する

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで **[Load Balancer]** に対して**インバウンド NAT 規則**を作成しようとすると、"**Failed to update Load Balancer**" (Load Balancer の更新に失敗しました) というエラー メッセージで操作が失敗します。
- 修復: PowerShell、CLI、または Resource Manager テンプレートを使用して、バックエンド プールをロード バランサー リソースに関連付けます。
- 発生頻度: 一般

#### <a name="create-load-balancer"></a>Load Balancer の作成

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、**[ロード バランサーの作成]** ウィンドウに **Standard** Load Balancer SKU を作成するためのオプションが表示されます。 このオプションは Azure Stack ではサポートされていません。
- 修復: 代わりに Basic ロード バランサー オプションを使用します。
- 発生頻度: 一般

#### <a name="public-ip-address"></a>パブリック IP アドレス

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: ユーザー ポータルで、**[パブリック IP アドレスの作成]** ウィンドウに **Standard** SKU を作成するためのオプションが表示されます。 **Standard** SKU は Azure Stack ではサポートされていません。
- 修復: パブリック IP アドレスではなく Basic SKU を使用します。
- 発生頻度: 一般

## <a name="compute"></a>Compute

### <a name="vm-boot-diagnostics"></a>VM ブート診断

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: 新しい Windows 仮想マシン (VM) を作成するときに、次のエラーが表示されることがあります。**仮想マシン 'vm-name' を起動できませんでした。エラー:Failed to update serial output settings for VM 'vm-name'**. (VM 'vm-name' のシリアル出力設定を更新できませんでした。)
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
- 修復: 削除するスケール セットを選択し、**[概要]** ウィンドウから **[削除]** ボタンをクリックします。
- 発生頻度: 一般

### <a name="ubuntu-ssh-access"></a>Ubuntu SSH アクセス

- 適用先:この問題は、サポートされているすべてのリリースに適用されます。
- 原因: SSH の承認を有効にして作成した Ubuntu 18.04 VM では、SSH キーを使用してサインインすることはできません。
- 修復: プロビジョニング後に Linux 拡張機能用の VM アクセスを使用して SSH キーを実装するか、パスワードベースの認証を使用します。
- 発生頻度: 一般

## <a name="infrastructure-backup"></a>Infrastructure Backup

<!--Bug 3615401 - scheduler config lost; new issue in YYMM;  hectorl-->
自動バックアップを有効にした後、スケジューラ サービスが予期せず無効状態になります。 バックアップ コントローラー サービスによって、自動バックアップが無効であることが検出され、管理者ポータルに警告が生成されます。 この警告は、自動バックアップが無効になっていると発生する可能性があります。

- 適用先:これはリリース 1904 に関する新しい問題です。
- 原因: この問題の原因は、サービスのバグによってスケジューラの構成が失われることです。 このバグによって、保存場所、ユーザー名、パスワード、暗号化キーが変わることはありません。
- 修復: この問題を軽減するには、Infrastructure Backup リソース プロバイダーのバックアップ コントローラー設定ブレードを開き、**[自動バックアップを有効にする]** を選択します。 必要な頻度と保持期間を設定してください。
- 発生頻度: 低

<!-- ## Storage -->
<!-- ## SQL and MySQL-->
<!-- ## App Service -->
<!-- ## Usage -->
<!-- ### Identity -->
<!-- ### Marketplace -->

## <a name="next-steps"></a>次の手順

- [更新アクティビティのチェック リストを確認する](azure-stack-release-notes-checklist.md)
- [セキュリティ更新プログラムの一覧を確認する](azure-stack-release-notes-security-updates-1904.md)
