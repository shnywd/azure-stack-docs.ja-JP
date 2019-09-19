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
ms.date: 08/30/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 08/30/2019
ms.openlocfilehash: 98e43cfe0226e06ca936484a78da5a61915f5797
ms.sourcegitcommit: c46d913ebfa4cb6c775c5117ac5c9e87d032a271
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2019
ms.locfileid: "71101027"
---
# <a name="asdk-release-notes"></a>ASDK リリース ノート

この記事では、Azure Stack Development Kit (ASDK) の変更、修正、および既知の問題に関する情報を提供します。 実行しているバージョンが不明な場合は、[ポータルを使用して確認](../operator/azure-stack-updates.md)します。

[![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS フィード](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)をサブスクライブして、ASDK の新着情報を常に把握するようにしてください。

::: moniker range="azs-1908"
## <a name="build-11908020"></a>ビルド 1.1908.0.20

### <a name="new-features"></a>新機能

- このリリースでの新機能の一覧については、Azure Stack リリース ノートの[このセクション](/azure-stack/operator/release-notes?view=azs-1908#whats-new-1908)を参照してください。

<!-- ### Changes -->

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

<!-- - For a list of Azure Stack issues fixed in this release, see [this section](/azure-stack/operator/release-notes?view=azs-1908#fixes-1908) of the Azure Stack release notes. -->
- 既知の問題の一覧については、[この記事](/azure-stack/operator/known-issues?view=azs-1908)を参照してください。
- 使用可能な Azure Stack 修正プログラムが ASDK には適用できないことに注意してください。
::: moniker-end

::: moniker range="azs-1907"
## <a name="build-11907020"></a>ビルド 1.1907.0.20

### <a name="new-features"></a>新機能

- このリリースでの新機能の一覧については、Azure Stack リリース ノートの[このセクション](/azure-stack/operator/release-notes?view=azs-1907#whats-in-this-update-1907)を参照してください。

<!-- ### Changes -->

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

- 一部の Marketplace イメージを使用して VM のリソースを作成すると、デプロイを完了できない場合があります。 回避策として、 **[概要]** ページの **[テンプレートとパラメーターのダウンロード]** リンクをクリックして、 **[テンプレート]** ブレードで **[デプロイ]** ボタンをクリックします。
- このリリースで修正された Azure Stack の問題の一覧については、Azure Stack リリース ノートの[このセクション](/azure-stack/operator/release-notes?view=azs-1907#fixes-1907)を参照してください。
- 既知の問題の一覧については、[この記事](/azure-stack/operator/known-issues?view=azs-1907)を参照してください。
- [使用可能な Azure Stack 修正プログラム](/azure-stack/operator/release-notes?view=azs-1907#hotfixes-1907)が Azure Stack ASDK には適用できないことに注意してください。
::: moniker-end

::: moniker range="azs-1906"
## <a name="build-11906030"></a>ビルド 1.1906.0.30

### <a name="new-features"></a>新機能

- このリリースでの新機能の一覧については、Azure Stack リリース ノートの[このセクション](/azure-stack/operator/release-notes?view=azs-1906#whats-in-this-update-1906)を参照してください。

### <a name="changes"></a>変更点

- Azure Stack のログ コレクション サービスをホストする **AzS-SRNG01** サポート リング VM を追加しました。 詳しくは、[仮想マシン ロール](asdk-architecture.md)に関するページをご覧ください。

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

- 一部の Marketplace イメージを使用して VM のリソースを作成すると、デプロイを完了できない場合があります。 回避策として、 **[概要]** ページの **[テンプレートとパラメーターのダウンロード]** リンクをクリックして、 **[テンプレート]** ブレードで **[デプロイ]** ボタンをクリックします。
- このリリースで修正された Azure Stack の問題の一覧については、Azure Stack リリース ノートの[このセクション](/azure-stack/operator/release-notes?view=azs-1906#fixes-1906)を参照してください。
- 既知の問題の一覧については、[この記事](/azure-stack/operator/known-issues?view=azs-1906)を参照してください。
- [使用可能な Azure Stack 修正プログラム](/azure-stack/operator/release-notes?view=azs-1906#hotfixes-1906)が Azure Stack ASDK には適用できないことに注意してください。
::: moniker-end

::: moniker range="azs-1905"
## <a name="build-11905040"></a>ビルド 1.1905.0.40

<!-- ### Changes -->

### <a name="new-features"></a>新機能

- このリリースでの新機能の一覧については、Azure Stack リリース ノートの[このセクション](/azure-stack/operator/release-notes?view=azs-1905#whats-in-this-update-1905)を参照してください。

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

- [ASDK を正常に登録する](asdk-register.md)には、**RegisterWithAzure.psm1** PowerShell スクリプトを編集しなければならない問題を修正しました。
- このリリースで修正されたその他の Azure Stack の問題の一覧については、Azure Stack リリース ノートの[このセクション](/azure-stack/operator/release-notes?view=azs-1905#fixes-1905)を参照してください。
- 既知の問題の一覧については、[この記事](/azure-stack/operator/known-issues?view=azs-1905)を参照してください。
- [使用可能な Azure Stack 修正プログラム](/azure-stack/operator/release-notes?view=azs-1905#hotfixes-1905)が Azure Stack ASDK には適用できないことに注意してください。
::: moniker-end
