---
title: Azure Stack Hub 用アプリの開発 | Microsoft Docs
description: Azure サービスを使用して Azure Stack Hub でアプリのプロトタイプを作成する際の開発の考慮事項。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: d3ebc6b1-0ffe-4d3e-ba4a-388239d6cdc3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 05/21/2019
ms.openlocfilehash: ba5aefa61db489f5f7063ebc4785785ba2f26f4c
ms.sourcegitcommit: d450dcf5ab9e2b22b8145319dca7098065af563b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75883730"
---
# <a name="develop-for-azure-stack-hub"></a>Azure Stack Hub 向けの開発

Azure Stack Hub 環境へのアクセス権がなくても、アプリの開発にすぐに取り掛かることができます。 Azure Stack Hub では、お使いのデータセンターで実行されている Microsoft Azure サービスが提供されます。つまり、Azure Stack Hub で同じ Azure ツールとプロセスを使用して、開発できるということです。

## <a name="development-considerations"></a>開発の考慮事項

いくつかの準備をして次のトピックのガイダンスに従えば、Azure を使用して Azure Stack Hub 環境をエミュレートできます。

* Azure で、Azure Stack Hub にデプロイできる Azure Resource Manager テンプレートを作成できます。 移植性が確保されたテンプレートの作成について詳しくは、[テンプレートに関する考慮事項](azure-stack-develop-templates.md)をご覧ください。
* Azure と Azure Stack Hub 間には、サービスの可用性とサービスのバージョン管理に違いがあります。 [Azure Stack Hub ポリシー モジュール](azure-stack-policy-module.md)を使用して、Azure サービスの可用性やリソースの種類を Azure Stack Hub で使用できるものに制限できます。 サービスを制限することで、Azure Stack Hub で使用できるサービスにアプリが依存することを確実にします。
* [Azure Stack Hub クイックスタート テンプレート](https://github.com/Azure/AzureStack-QuickStart-Templates)は、Azure と Azure Stack Hub の両方にデプロイできるテンプレートを開発する方法を示す一般的なシナリオ例です。

## <a name="next-steps"></a>次のステップ

Azure Stack 開発の詳細については、次の記事をご覧ください。

* [Azure Resource Manager テンプレートのベスト プラクティス](azure-stack-develop-templates.md)
* [GitHub 上の Azure Stack Hub クイックスタート テンプレート](https://github.com/Azure/AzureStack-QuickStart-Templates)
