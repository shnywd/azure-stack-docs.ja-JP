---
title: Azure Stack Hub でポータルを使用してテンプレートをデプロイする | Microsoft Docs
description: Azure Stack Hub ポータルを使用してテンプレートをデプロイする方法について学習します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/22/2020
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: 6feca120c01704aaf999899c660c64ecfecd3d97
ms.sourcegitcommit: a1abc27a31f04b703666de02ab39ffdc79a632f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2020
ms.locfileid: "76536387"
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

1. **作成** を選択します。 ダッシュボードの新しいタイルにより、テンプレート デプロイの進行状況を追跡します。

   ![Azure Stack Hub ポータルでテンプレートを作成する](media/azure-stack-deploy-template-portal/template-deploy7.png)

## <a name="next-steps"></a>次のステップ

テンプレートのデプロイの詳細については、次の記事を参照してください。

- [PowerShell を使用したテンプレートのデプロイ](azure-stack-deploy-template-powershell.md)
