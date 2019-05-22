---
title: Azure Stack でポータルを使用してテンプレートをデプロイする | Microsoft Docs
description: Azure Stack ポータルを使用してテンプレートをデプロイする方法について説明します。
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
ms.date: 05/07/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: 6e848af9049c7f105e6a31fdef8dc03308bd276a
ms.sourcegitcommit: a78c0d143eadcab65a601746b9ea24be28091ad2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2019
ms.locfileid: "65212481"
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Azure Stack ポータルを使用したテンプレートのデプロイ

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

ポータルを使用して、Azure Resource Manager テンプレートを Azure Stack にデプロイできます。

## <a name="to-deploy-a-template"></a>テンプレートをデプロイするには

1. ポータルにサインインし、**[+ リソースの作成]** を選択し、**[カスタム]** を選択します。

   ![Create](media/azure-stack-deploy-template-portal/template-deploy1.png)

1. **[テンプレートのデプロイ]** を選択します。

   ![テンプレートのデプロイ](media/azure-stack-deploy-template-portal/template-deploy2.png)

1. **[テンプレートの編集]** を選択してから、JSON テンプレート コードをコード ウィンドウに貼り付けます。 **[保存]** を選択します。

   ![テンプレートの編集](media/azure-stack-deploy-template-portal/template-deploy3.png)

1. **[パラメーターの編集]** を選択し、表示されているパラメーターの値を指定して、**[OK]** を選択します。

   ![パラメーターの編集](media/azure-stack-deploy-template-portal/template-deploy4.png)

1. **[サブスクリプション]** を選択します。 使用するサブスクリプションを選択してから、**[OK]** を選択します。

   ![サブスクリプション](media/azure-stack-deploy-template-portal/template-deploy5.png)

1. **[リソース グループ]** を選択します。 既存のリソース グループを選択するか、新しいリソース グループを作成して、**[OK]** を選択します。

   ![リソース グループ](media/azure-stack-deploy-template-portal/template-deploy6.png)

1. **作成** を選択します。 ダッシュボードの新しいタイルにより、テンプレート デプロイの進行状況を追跡します。

   ![テンプレートの作成](media/azure-stack-deploy-template-portal/template-deploy7.png)

## <a name="next-steps"></a>次の手順

テンプレートのデプロイの詳細については、次の記事を参照してください。

- [PowerShell を使用したテンプレートのデプロイ](azure-stack-deploy-template-powershell.md)
