---
title: ASDK リリースノート | Microsoft Docs
description: Azure Stack Development Kit (ASDK) の機能強化、修正、既知の問題。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 07/25/2019
ms.openlocfilehash: 3f11a7b5066d0b50d85a40be1df47dfe1a5ade38
ms.sourcegitcommit: 7968f9f0946138867323793be9966ee2ef99dcf4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/26/2019
ms.locfileid: "70025844"
---
# <a name="asdk-release-notes"></a>ASDK リリース ノート

この記事では、Azure Stack Development Kit (ASDK) の変更、修正、および既知の問題に関する情報を提供します。 実行しているバージョンが不明な場合は、[ポータルを使用して確認](../operator/azure-stack-updates.md)します。

[![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS フィード](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)をサブスクライブして、ASDK の新着情報を常に把握するようにしてください。

## <a name="build-11907020"></a>ビルド 1.1907.0.20

### <a name="new-features"></a>新機能

- このリリースでの新機能の一覧については、Azure Stack リリース ノートの[このセクション](../operator/azure-stack-release-notes-1907.md#whats-in-this-update)を参照してください。

<!-- ### Changes -->

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

- 一部の Marketplace イメージを使用して VM のリソースを作成すると、デプロイを完了できない場合があります。 回避策として、 **[概要]** ページの **[テンプレートとパラメーターのダウンロード]** リンクをクリックして、 **[テンプレート]** ブレードで **[デプロイ]** ボタンをクリックします。
- このリリースで修正された Azure Stack の問題の一覧については、Azure Stack リリース ノートの[このセクション](../operator/azure-stack-release-notes-1907.md#fixes)を参照してください。
- 既知の問題の一覧については、[この記事](../operator/azure-stack-release-notes-known-issues-1907.md)を参照してください。
- [使用可能な Azure Stack 修正プログラム](../operator/azure-stack-release-notes-1907.md#hotfixes)が Azure Stack ASDK には適用できないことに注意してください。

## <a name="build-11906030"></a>ビルド 1.1906.0.30

### <a name="new-features"></a>新機能

- このリリースでの新機能の一覧については、Azure Stack リリース ノートの[このセクション](../operator/azure-stack-release-notes-1906.md#whats-in-this-update)を参照してください。

### <a name="changes"></a>変更点

- Azure Stack のログ コレクション サービスをホストする **AzS-SRNG01** サポート リング VM を追加しました。 詳しくは、[仮想マシン ロール](asdk-architecture.md)に関するページをご覧ください。

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

- 一部の Marketplace イメージを使用して VM のリソースを作成すると、デプロイを完了できない場合があります。 回避策として、 **[概要]** ページの **[テンプレートとパラメーターのダウンロード]** リンクをクリックして、 **[テンプレート]** ブレードで **[デプロイ]** ボタンをクリックします。
- このリリースで修正された Azure Stack の問題の一覧については、Azure Stack リリース ノートの[このセクション](../operator/azure-stack-release-notes-1906.md#fixes)を参照してください。
- 既知の問題の一覧については、[この記事](../operator/azure-stack-release-notes-known-issues-1906.md)を参照してください。
- [使用可能な Azure Stack 修正プログラム](../operator/azure-stack-release-notes-1906.md#hotfixes)が Azure Stack ASDK には適用できないことに注意してください。

## <a name="build-11905040"></a>ビルド 1.1905.0.40

<!-- ### Changes -->

### <a name="new-features"></a>新機能

- このリリースでの新機能の一覧については、Azure Stack リリース ノートの[このセクション](../operator/azure-stack-release-notes-1905.md#whats-in-this-update)を参照してください。

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

- [ASDK を正常に登録する](asdk-register.md)には、**RegisterWithAzure.psm1** PowerShell スクリプトを編集しなければならない問題を修正しました。
- このリリースで修正されたその他の Azure Stack の問題の一覧については、Azure Stack リリース ノートの[このセクション](../operator/azure-stack-release-notes-1905.md#fixes)を参照してください。
- 既知の問題の一覧については、[この記事](../operator/azure-stack-release-notes-known-issues-1905.md)を参照してください。
- [使用可能な Azure Stack 修正プログラム](../operator/azure-stack-release-notes-1905.md#hotfixes)が Azure Stack ASDK には適用できないことに注意してください。

## <a name="build-11904036"></a>ビルド 1.1904.0.36

<!-- ### Changes -->

### <a name="new-features"></a>新機能

- このリリースでの新機能の一覧については、Azure Stack リリース ノートの[このセクション](../operator/azure-stack-release-notes-1904.md#whats-in-this-update)を参照してください。

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

- 登録スクリプトの実行中にサービス プリンシパルのタイムアウトが起こるため、正常に [ASDK を登録](asdk-register.md)するには、**RegisterWithAzure.psm1** PowerShell スクリプトを編集する必要があります。 以下の手順を実行します。

  1. ASDK ホスト コンピューター上で、管理者特権のアクセス許可を使用してエディターでファイル **C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1** を開きます。
  2. 1249 行目の末尾に、`-TimeoutInSeconds 1800` パラメーターを追加します。 この追加が必要なのは、登録スクリプトの実行中にサービス プリンシパルのタイムアウトが起こるためです。 1249 行目は次のようになります。

     ```powershell
      $servicePrincipal = Invoke-Command -Session $PSSession -ScriptBlock { New-AzureBridgeServicePrincipal -RefreshToken $using:RefreshToken -AzureEnvironment $using:AzureEnvironmentName -TenantId $using:TenantId -TimeoutInSeconds 1800 }
      ```

- リリース 1902 で特定された VPN 接続の問題が修正されました。

- このリリースで修正されたその他の Azure Stack の問題の一覧については、Azure Stack リリース ノートの[このセクション](../operator/azure-stack-release-notes-1904.md#fixes)を参照してください。
- 既知の問題の一覧については、[この記事](../operator/azure-stack-release-notes-known-issues-1904.md)を参照してください。
- [使用可能な Azure Stack 修正プログラム](../operator/azure-stack-release-notes-1904.md#hotfixes)が Azure Stack ASDK には適用できないことに注意してください。

