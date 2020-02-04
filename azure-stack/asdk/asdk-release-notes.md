---
title: ASDK リリース ノート
description: Azure Stack Development Kit (ASDK) の機能強化、修正、既知の問題。
author: sethmanheim
ms.topic: article
ms.date: 11/21/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: 608374ce5e788a82378aaf9c62414b05d871c983
ms.sourcegitcommit: fd5d217d3a8adeec2f04b74d4728e709a4a95790
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76873483"
---
# <a name="asdk-release-notes"></a>ASDK リリース ノート

この記事では、Azure Stack Development Kit (ASDK) の変更、修正、および既知の問題に関する情報を提供します。 実行しているバージョンが不明な場合は、[ポータルを使用して確認](../operator/azure-stack-updates.md)します。

[![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS フィード](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)をサブスクライブして、ASDK の新着情報を常に把握するようにしてください。

::: moniker range="azs-1910"
## <a name="build-11910058"></a>ビルド 1.1910.0.58

### <a name="new-features"></a>新機能

- このリリースでの修正された問題、変更点、および新機能の一覧については、[Azure Stack リリースノート](../operator/release-notes.md)の関連セクションを参照してください。

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

- ログを収集して Azure Storage BLOB コンテナーに格納する際の問題が修正されました。 この操作の構文は次のとおりです。

  ```powershell
  Get-AzureStackLog -OutputSasUri "<Blob service SAS Uri>"
  ``` 

- スプーラ サービスの読み込みに時間がかかることで一部の Windows 機能の削除が妨げられ、再起動が必要になるというデプロイの問題が修正されました。
- このリリースでの Azure Stack の既知の問題の一覧については、[既知の問題](../operator/known-issues.md)に関する記事を参照してください。
- 使用可能な Azure Stack 修正プログラムが ASDK には適用できないことに注意してください。
::: moniker-end

::: moniker range="azs-1908"
  
## <a name="build-11908020"></a>ビルド 1.1908.0.20

### <a name="new-features"></a>新機能

- このリリースでの新機能の一覧については、Azure Stack リリース ノートの[このセクション](/azure-stack/operator/release-notes?view=azs-1908#whats-new-1)を参照してください。

<!-- ### Changes -->

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

<!-- - For a list of Azure Stack issues fixed in this release, see [this section](/azure-stack/operator/release-notes?view=azs-1908#fixes-1) of the Azure Stack release notes. -->
- 既知の問題の一覧については、[この記事](/azure-stack/operator/known-issues?view=azs-1908)を参照してください。
- 使用可能な Azure Stack 修正プログラムが ASDK には適用できないことに注意してください。
::: moniker-end

::: moniker range="azs-1907"
## <a name="build-11907020"></a>ビルド 1.1907.0.20

### <a name="new-features"></a>新機能

- このリリースでの新機能の一覧については、Azure Stack リリース ノートの[このセクション](/azure-stack/operator/release-notes?view=azs-1907#whats-in-this-update)を参照してください。

<!-- ### Changes -->

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

- 一部の Marketplace イメージを使用して VM のリソースを作成すると、デプロイを完了できない場合があります。 回避策として、 **[概要]** ページの **[テンプレートとパラメーターのダウンロード]** リンクをクリックして、 **[テンプレート]** ブレードで **[デプロイ]** ボタンをクリックします。
- このリリースで修正された Azure Stack の問題の一覧については、Azure Stack リリース ノートの[このセクション](/azure-stack/operator/release-notes?view=azs-1907#fixes-2)を参照してください。
- 既知の問題の一覧については、[この記事](/azure-stack/operator/known-issues?view=azs-1907)を参照してください。
- [使用可能な Azure Stack 修正プログラム](/azure-stack/operator/release-notes?view=azs-1907#hotfixes-2)が Azure Stack ASDK には適用できないことに注意してください。
::: moniker-end

::: moniker range="azs-1906"
## <a name="build-11906030"></a>ビルド 1.1906.0.30

### <a name="new-features"></a>新機能

- このリリースでの新機能の一覧については、Azure Stack リリース ノートの[このセクション](/azure-stack/operator/release-notes?view=azs-1906#whats-in-this-update-1)を参照してください。

### <a name="changes"></a>[変更点]

- Azure Stack のログ コレクション サービスをホストする **AzS-SRNG01** サポート リング VM を追加しました。 詳しくは、[仮想マシン ロール](asdk-architecture.md)に関するページをご覧ください。

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

- 一部の Marketplace イメージを使用して VM のリソースを作成すると、デプロイを完了できない場合があります。 回避策として、 **[概要]** ページの **[テンプレートとパラメーターのダウンロード]** リンクをクリックして、 **[テンプレート]** ブレードで **[デプロイ]** ボタンをクリックします。
- このリリースで修正された Azure Stack の問題の一覧については、Azure Stack リリース ノートの[このセクション](/azure-stack/operator/release-notes?view=azs-1906#fixes-3)を参照してください。
- 既知の問題の一覧については、[この記事](/azure-stack/operator/known-issues?view=azs-1906)を参照してください。
- [使用可能な Azure Stack 修正プログラム](/azure-stack/operator/release-notes?view=azs-1906#hotfixes-3)が Azure Stack ASDK には適用できないことに注意してください。
::: moniker-end
