---
title: Azure Stack でコマンド ラインを使ってテンプレートをデプロイする | Microsoft Docs
description: クロスプラットフォームのコマンド ライン インターフェイス (CLI) を使用して、Azure Stack にテンプレートをデプロイする方法を説明します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: article
ms.date: 05/09/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 05/09/2019
ms.openlocfilehash: 92c9189f8144804f36e551ab89d8b4fc4c1f8598
ms.sourcegitcommit: 7f39bdc83717c27de54fe67eb23eb55dbab258a9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2019
ms.locfileid: "66691365"
---
# <a name="deploy-a-template-with-the-command-line-in-azure-stack"></a>Azure Stack でコマンド ラインを使ってテンプレートをデプロイする

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure コマンド ライン インターフェイス (CLI) を使用して、Azure Resource Manager テンプレートを Azure Stack にデプロイします。 Azure Resource Manager テンプレートは、お使いのアプリのリソースを、単一の連携した操作でデプロイしてプロビジョニングします。

## <a name="before-you-begin"></a>開始する前に

- Azure CLI で Azure Stack を[インストールして接続](azure-stack-version-profiles-azurecli2.md)します。
- [ストレージ アカウントの作成例のテンプレート](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account)からファイル *azuredeploy.json* と *azuredeploy.parameters.json* をダウンロードします。

## <a name="deploy-template"></a>テンプレートのデプロイ

これらのファイルをダウンロードしたフォルダーに移動し、次のコマンドを実行してテンプレートをデプロイします。

```azurecli
az group create "cliRG" "local" -f azuredeploy.json -d "testDeploy" -e azuredeploy.parameters.json
```

このコマンドで、テンプレートが Azure Stack POC の既定の場所のリソース グループ **cliRG** にデプロイされます。

## <a name="validate-template-deployment"></a>テンプレートのデプロイを検証する

このリソース グループとストレージ アカウントを表示するには、次の CLI コマンドを使用します。

```azurecli
az group list

az storage account list
```

## <a name="next-steps"></a>次の手順

PowerShell を使用したテンプレートのデプロイの詳細について、[こちら](azure-stack-deploy-template-powershell.md)を参照してください。
