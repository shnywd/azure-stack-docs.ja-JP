---
title: Azure Stack で MySQL データベースを PaaS として使用する | Microsoft Docs
description: Azure Stack で MySQL リソース プロバイダーをデプロイし、MySQL データベースをサービスとして提供する方法を説明します。
services: azure-stack
documentationCenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: 8f498896489a0c217b1f7c51ec4dda6b493ddda7
ms.sourcegitcommit: b5eb024d170f12e51cc852aa2c72eabf26792d8d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "72534019"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Microsoft Azure Stack で MySQL データベースを使用する

MySQL データベースは、Web サイトでよく使用されており、多くの Web サイト プラットフォームをサポートしています。 たとえば、App Services リソース プロバイダー (PaaS) アドオンを使用して WordPress Web サイトを作成できます。

リソース プロバイダーをデプロイした後は、次のことができます。

* Azure Resource Manager デプロイ テンプレートを使って、MySQL サーバーとデータベースを作成します。
* MySQL データベースをサービスとして提供します。  

## <a name="mysql-resource-provider-adapter-architecture"></a>MySQL リソースプロバイダー アダプターのアーキテクチャ

リソース プロバイダーには次のコンポーネントがあります。

* **MySQL リソース プロバイダー アダプター仮想マシン (VM)** 。これはプロバイダー サービスを実行する Windows Server VM です。
* **リソース プロバイダー**。要求を処理し、データベース リソースにアクセスします。
* **MySQL サーバーをホストするサーバー**。これはデータベースに容量を提供し、ホスティング サーバーと呼ばれます。 MySQL インスタンスを自分で作成することも、外部の MySQL インスタンスへのアクセスを提供することもできます。 [Azure Stack Quickstart Gallery](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) にあるテンプレートの例を使って、次のことができます。

  * 自力で MySQL サーバーを作成する。
  * Azure Marketplace から MySQL Server をダウンロードしてデプロイする。

> [!NOTE]
> Azure Stack 統合システムにインストールされたホスティング サーバーは、テナント サブスクリプションから作成する必要があります。 既定のプロバイダー サブスクリプションからは作成できません。 それらは、テナント ポータルから、または適切なサインインで PowerShell セッションから作成する必要があります。 すべてのホスティング サーバーは課金対象の VM であり、ライセンスが必要です。 サービス管理者は、テナント サブスクリプションの所有者になることができます。

### <a name="required-privileges"></a>必要な特権

システム アカウントには、次の特権が必要です。

* **データベース:** 作成、ドロップ
* **ログイン:** 作成、設定、ドロップ、許可、取り消し  

## <a name="next-steps"></a>次の手順

[MySQL リソースプロバイダーをデプロイする](azure-stack-mysql-resource-provider-deploy.md)
