---
title: Azure Stack Hub リソース プロバイダーに更新プログラムを適用する方法。
description: Azure Stack Hub のリソース プロバイダーにサービス更新プログラムを適用する方法について説明します。
author: BryanLa
ms.author: bryanla
ms.service: azure-stack
ms.topic: how-to
ms.date: 11/18/2019
ms.reviewer: jfggdl
ms.lastreviewed: 11/18/2019
ms.openlocfilehash: 13ea59064ff653ff24fac97a867725c5d9f036d0
ms.sourcegitcommit: a630894e5a38666c24e7be350f4691ffce81ab81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "80424771"
---
# <a name="how-to-update-an-azure-stack-hub-resource-provider"></a>Azure Stack Hub リソース プロバイダーを更新する方法

Marketplace からインストールされたリソース プロバイダーには、定期的な保守が必要です。 保守は、Microsoft から定期的に提供されるサービス更新プログラムを適用することによって行います。 更新プログラムには、新機能と修正プログラムの両方が含まれます。  

## <a name="check-for-updates"></a>更新プログラムをチェックする

リソース プロバイダーの更新には、Azure Stack Hub の更新プログラムの適用に使用されるのと同じ更新機能を使用します。

1. Azure Stack Hub 管理者ポータルにサインインします。
2. 左側にある **[すべてのサービス]** リンクを選択し、 **[管理]** セクションで **[更新プログラム]** を選択します。
   ![[すべてのサービス] ページ](media/resource-provider-apply-updates/1-all-services.png)

3. **[更新プログラム]** ページの **[リソース プロバイダー]** セクションに、リソース プロバイダーの更新プログラムが表示されます。

   [![使用可能な更新プログラム ページ](media/resource-provider-apply-updates/3-update-available.png)](media/resource-provider-apply-updates/3-update-available.png#lightbox)

## <a name="apply-an-update"></a>更新プログラムを適用する

特定のリソース プロバイダーで使用可能な更新プログラムがある場合:

1. 更新するリソース プロバイダーの行を選択します。 ページの上部にある **[ダウンロード]** リンクが有効になっていることを確認します。
   [![使用可能な更新プログラム ページ](media/resource-provider-apply-updates/4-download.png)](media/resource-provider-apply-updates/3-update-available.png#lightbox)

2. **[ダウンロード]** リンクをクリックして、リソース プロバイダーのインストール パッケージのダウンロードを開始します。 リソース プロバイダー行の **[状態]** 列が "使用可能" から "ダウンロード中" に変更されていることに注目してください。
3. **[状態]** が "インストールの準備完了" に変更されたら、ダウンロードが完了です。 ページの上部にある **[今すぐインストール]** リンクが有効になっていることも確認します。
4. **[今すぐインストール]** リンクを選択すると、リソース プロバイダーの **[インストール]** ページが開きます。 
5. **[インストール]** ボタンを選択して、インストールを開始します。
6. "インストール中" という通知が右上に表示され、 **[更新プログラム]** ページに戻ります。 リソース プロバイダー行の **[状態]** 列も "インストール中" に変更されます。
7. インストールが完了すると、別の通知によって成功または失敗が示されます。 インストールが成功すると、 **[Marketplace Management - リソース プロバイダー]** ページの **[バージョン]** も更新されます。

## <a name="next-steps"></a>次のステップ

[管理者ダッシュボードの更新機能](azure-stack-apply-updates.md)の詳細を確認します。
