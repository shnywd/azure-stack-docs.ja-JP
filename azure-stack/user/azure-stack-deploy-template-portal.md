---
title: Azure Stack Hub でポータルを使用してテンプレートをデプロイする
description: Azure Stack Hub ポータルを使用してテンプレートをデプロイする方法について学習します。
author: mattbriggs
ms.topic: article
ms.date: 1/22/2020
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: 8425b4f8492e57502ca93b0be802b63601f6ba34
ms.sourcegitcommit: 4ac711ec37c6653c71b126d09c1f93ec4215a489
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77704320"
---
# <a name="deploy-a-template-using-the-portal-in-azure-stack-hub"></a>Azure Stack Hub でポータルを使用してテンプレートをデプロイする

ポータルを使用して、Azure Resource Manager テンプレートを Azure Stack Hub にデプロイできます。

## <a name="to-deploy-a-template"></a>テンプレートをデプロイするには

1. ポータルにサインインし、 **[+ リソースの作成]** を選択し、 **[カスタム]** を選択します。

   ![Azure Stack Hub ポータルでリソースを作成する](media/azure-stack-deploy-template-portal/template-deploy1.png)

1. **[テンプレートのデプロイ]** を選択します。

   ![Azure Stack Hub ポータルでテンプレートをデプロイする](media/azure-stack-deploy-template-portal/template-deploy2.png)

1. **[テンプレートの編集]** を選択してから、JSON テンプレート コードをコード ウィンドウに貼り付けます。 **[保存]** を選択します。

   ![Azure Stack Hub ポータルでテンプレートを編集する](media/azure-stack-deploy-template-portal/template-deploy3.png)

1. **[パラメーターの編集]** を選択し、表示されているパラメーターの値を指定して、 **[OK]** を選択します。

   ![Azure Stack Hub ポータルでパラメーターを編集する](media/azure-stack-deploy-template-portal/template-deploy4.png)

1. **[サブスクリプション]** を選択します。 使用するサブスクリプションを選択してから、 **[OK]** を選択します。

   ![Azure Stack Hub ポータルでサブスクリプションを選択する](media/azure-stack-deploy-template-portal/template-deploy5.png)

1. **[リソース グループ]** を選択します。 既存のリソース グループを選択するか、新しいリソース グループを作成して、 **[OK]** を選択します。

   ![Azure Stack Hub ポータルでリソース グループを選択する](media/azure-stack-deploy-template-portal/template-deploy6.png)

1. **［作成］** を選択します ダッシュボードの新しいタイルにより、テンプレート デプロイの進行状況を追跡します。

   ![Azure Stack Hub ポータルでテンプレートを作成する](media/azure-stack-deploy-template-portal/template-deploy7.png)

## <a name="next-steps"></a>次のステップ

テンプレートのデプロイの詳細については、次の記事を参照してください。

- [PowerShell を使用したテンプレートのデプロイ](azure-stack-deploy-template-powershell.md)
