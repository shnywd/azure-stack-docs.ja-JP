---
title: Azure Stack でコマンド ラインを使ってテンプレートをデプロイする | Microsoft Docs
description: Azure クロスプラットフォームのコマンド ライン インターフェイス (CLI) を使用して、Azure Stack にテンプレートをデプロイする方法を説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: article
ms.date: 10/07/2019
ms.author: mabrigg
ms.reviewer: unknown
ms.lastreviewed: 05/09/2019
ms.openlocfilehash: 7b3daaefd8fa7e7bce9c6d5708e664911fc906fe
ms.sourcegitcommit: 7226979ece29d9619c959b11352be601562b41d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2019
ms.locfileid: "72304093"
---
# <a name="deploy-a-template-with-the-command-line-in-azure-stack"></a>Azure Stack でコマンド ラインを使ってテンプレートをデプロイする

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure コマンド ライン インターフェイス (CLI) を使用して、Azure Resource Manager テンプレートを Azure Stack にデプロイできます。 Azure Resource Manager テンプレートでは、単一の調整されたアクションで、アプリのためのリソースがデプロイされて設定されます。

## <a name="deploy-template"></a>テンプレートのデプロイ

1. [AzureStack-QuickStart-Templates リポジトリ](https://aka.ms/AzureStackGitHub)を参照して、**101-create-storage-account** テンプレートを見つけます。 テンプレート (`azuredeploy.json`) とパラメーター ファイル (`(azuredeploy.parameters.json`) を、ローカル ドライブ上の場所に保存します (`C:\templates\` など)
2. ファイルをダウンロードしたフォルダーに移動します。 
3. Azure CLI で Azure Stack を[インストールして接続](azure-stack-version-profiles-azurecli2.md)します。
4. 次のコマンドで、リージョンと場所を更新します。 ASDK を使用している場合は、場所パラメーターに `local` を使用します。 テンプレートをデプロイするには、次の手順に従います。
    ```azurecli
    az group create --name testDeploy --location local
    az group deployment create --resource-group testDeploy --template-file ./azuredeploy.json --parameters ./azuredeploy.parameters.json
    ```

このコマンドでは、Azure Stack インスタンス内のリソース グループ **testDeploy** にテンプレートがデプロイされます。

## <a name="validate-template-deployment"></a>テンプレートのデプロイを検証する

リソース グループとストレージ アカウントを確認するには、次の CLI コマンドを実行します。

```azurecli
az group list
az storage account list
```

## <a name="next-steps"></a>次の手順

[PowerShell を使用したテンプレートのデプロイ](azure-stack-deploy-template-powershell.md)方法を確認します。
