---
title: Azure Stack Hub で Visual Studio を使用してテンプレートをデプロイする
description: Azure Stack Hub で Visual Studio を使用してテンプレートをデプロイする方法について学習します。
author: mattbriggs
ms.topic: article
ms.date: 5/27/2020
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 05/21/2019
ms.openlocfilehash: ea268d367ab529fabe8a8b363ed14ce17bc790aa
ms.sourcegitcommit: db3c9179916a36be78b43a8a47e1fd414aed3c2e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84146803"
---
# <a name="deploy-templates-in-azure-stack-hub-using-visual-studio"></a>Azure Stack Hub での Visual Studio を使用したテンプレートのデプロイ

Visual Studio を使用して、Azure Resource Manager テンプレートを Azure Stack Hub にデプロイできます。

## <a name="to-deploy-a-template"></a>テンプレートをデプロイするには

1. [インストールして Visual Studio で Azure Stack Hub に接続](azure-stack-install-visual-studio.md)します。
2. Visual Studio を開きます。
3. **[ファイル]** を選択してから、 **[新規]** を選択します。 **[新しいプロジェクト]** で、 **[Azure リソース グループ]** を選択します。
4. 新しいプロジェクトの**名前**を入力し、 **[OK]** を選択します。
5. **[Azure テンプレートの選択]** で、ドロップダウン リストから **[Azure Stack Hub Quickstart]\(Azure Stack Hub クイックスタート\)** を選択します。
6. **[101-create-storage-account]** 、 **[OK]** の順に選択します。
7. 新しいプロジェクトで、**ソリューション エクスプローラー**で **[テンプレート]** ノードを展開して、使用可能なテンプレートを表示します。
8. **ソリューション エクスプローラー**で、プロジェクトの名前を選択し、 **[デプロイ]** を選択します。 **[新しいデプロイ]** を選択します。
9. **[リソース グループに配置する]** で、 **[サブスクリプション]** ドロップダウン リストを使用して、ご利用の Microsoft Azure Stack Hub サブスクリプションを選択します。
10. **[リソース グループ]** ボックスの一覧から、既存のリソース グループを選択するか、新しいリソース グループを作成します。
11. **[リソース グループの場所]** の一覧から場所を選択して、 **[デプロイ]** を選択します。
12. **[パラメーターの編集]** で、パラメーターの値 (テンプレートごとに異なる) を入力し、 **[保存]** を選択します。

## <a name="next-steps"></a>次のステップ

* [コマンド ラインを使用したテンプレートのデプロイ](azure-stack-deploy-template-command-line.md)
* [Azure Stack Hub のテンプレートの開発](azure-stack-develop-templates.md)
