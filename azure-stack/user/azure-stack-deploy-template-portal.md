---
title: Azure Stack でポータルを使用してテンプレートをデプロイする | Microsoft Docs
description: Azure Stack ポータルを使用してテンプレートをデプロイする方法について学習します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/13/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: e6a9c9569f7a2aeff044232d15817313b43ed69f
ms.sourcegitcommit: aefcf9c61bd8089a0aaa569af7643e5e15f4947c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2019
ms.locfileid: "68991858"
---
# <a name="deploy-a-template-using-the-portal-in-azure-stack"></a>Azure Stack でポータルを使用してテンプレートをデプロイする

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

ポータルを使用して、Azure Resource Manager テンプレートを Azure Stack にデプロイできます。

## <a name="to-deploy-a-template"></a>テンプレートをデプロイするには

1. ポータルにサインインし、 **[+ リソースの作成]** を選択し、 **[カスタム]** を選択します。

   ![Azure Stack ポータルでリソースを作成する](media/azure-stack-deploy-template-portal/template-deploy1.png)

1. **[テンプレートのデプロイ]** を選択します。

   ![Azure Stack ポータルでテンプレートをデプロイする](media/azure-stack-deploy-template-portal/template-deploy2.png)

1. **[テンプレートの編集]** を選択してから、JSON テンプレート コードをコード ウィンドウに貼り付けます。 **[保存]** を選択します。

   ![Azure Stack ポータルでテンプレートを編集する](media/azure-stack-deploy-template-portal/template-deploy3.png)

1. **[パラメーターの編集]** を選択し、表示されているパラメーターの値を指定して、 **[OK]** を選択します。

   ![Azure Stack ポータルでパラメーターを編集する](media/azure-stack-deploy-template-portal/template-deploy4.png)

1. **[サブスクリプション]** を選択します。 使用するサブスクリプションを選択してから、 **[OK]** を選択します。

   ![Azure Stack ポータルでサブスクリプションを選択する](media/azure-stack-deploy-template-portal/template-deploy5.png)

1. **[リソース グループ]** を選択します。 既存のリソース グループを選択するか、新しいリソース グループを作成して、 **[OK]** を選択します。

   ![Azure Stack ポータルでリソース グループを選択する](media/azure-stack-deploy-template-portal/template-deploy6.png)

1. **作成** を選択します。 ダッシュボードの新しいタイルにより、テンプレート デプロイの進行状況を追跡します。

   ![Azure Stack ポータルでテンプレートを作成する](media/azure-stack-deploy-template-portal/template-deploy7.png)

## <a name="next-steps"></a>次の手順

テンプレートのデプロイの詳細については、次の記事を参照してください。

- [PowerShell を使用したテンプレートのデプロイ](azure-stack-deploy-template-powershell.md)
