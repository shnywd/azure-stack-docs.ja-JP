---
title: ASDK リリース ノート
description: Azure Stack Development Kit (ASDK) の機能強化、修正、既知の問題。
author: sethmanheim
ms.topic: article
ms.date: 03/20/2020
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 03/18/2020
ms.openlocfilehash: 32b4bfb500a9717f99085fe0759297f999244bbb
ms.sourcegitcommit: 961e3b1fae32d7f9567359fa3f7cb13cdc37e28e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/24/2020
ms.locfileid: "80152226"
---
# <a name="asdk-release-notes"></a>ASDK リリース ノート

この記事では、Azure Stack Development Kit (ASDK) の変更、修正、および既知の問題に関する情報を提供します。 実行しているバージョンが不明な場合は、[ポータルを使用して確認](../operator/azure-stack-updates.md)します。

[![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS フィード](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)をサブスクライブして、ASDK の新着情報を常に把握するようにしてください。

::: moniker range="azs-2002"
## <a name="build-12002035"></a>ビルド 1.2002.0.35

### <a name="new-features"></a>新機能

- このリリースでの修正された問題、変更点、および新機能の一覧については、[Azure Stack リリースノート](../operator/release-notes.md)の関連セクションを参照してください。

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

- 解読証明書のパスワードは、バックアップ データの解読に必要な秘密キーが含まれている自己署名証明書 (.pfx) のパスワードを指定するための新しいオプションです。 このパスワードは、バックアップが証明書を使用して暗号化されている場合にのみ必要です。
- このリリースでの Azure Stack の既知の問題の一覧については、[既知の問題](../operator/known-issues.md)に関する記事を参照してください。
- 使用可能な Azure Stack 修正プログラムが ASDK には適用できないことに注意してください。
::: moniker-end

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
