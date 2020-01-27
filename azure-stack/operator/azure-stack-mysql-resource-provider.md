---
title: Azure Stack Hub で MySQL データベースを PaaS として使用する | Microsoft Docs
description: Azure Stack Hub で、MySQL リソース プロバイダーをデプロイし、MySQL データベースをサービスとして提供する方法を説明します。
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
ms.date: 1/22/2020
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: 9351ce5b2dde1fc7e2dfeddc2d3520a4f5d1fe2b
ms.sourcegitcommit: a1abc27a31f04b703666de02ab39ffdc79a632f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2020
ms.locfileid: "76534619"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack-hub"></a>Microsoft Azure Stack Hub で MySQL データベースを使用する

MySQL リソース プロバイダーを使用して、[Azure Stack Hub](azure-stack-overview.md) で MySQL データベース サービスを提供します。 リソース プロバイダーをデプロイし、1 つ以上の MySQL サーバー インスタンスに接続すると、次のことができます。

* Azure Resource Manager デプロイ テンプレートを使用して、MySQL データベースを作成します。
* MySQL データベースをサービスとして提供します。  

SQL リソース プロバイダーをインストールする前に、考慮すべき制限事項がいくつかあります。

- ユーザーは、個々のデータベースの作成と管理のみを行うことができます。 エンド ユーザーはデータベース サーバー インスタンスにアクセスできません。 これにより、マスター、Temp DB にアクセスしたり、データベースを動的に管理したりすることが必要なオンプレミス データベース アプリケーションとの互換性が制限される可能性があります。
- Azure Stack Hub オペレーターは、MySQL データベース サーバーおよびホストのデプロイ、更新、セキュリティ保護、構成、および保守を担当します。 RP サービスでは、ホストおよびデータベース サーバー インスタンスの管理機能は提供されません。 
- 異なるサブスクリプションの異なるユーザーのデータベースは、同じデータベース サーバー インスタンスに配置できます。 RP は、データベースを別のホストまたはデータベース サーバー インスタンスで分離するためのメカニズムを備えていません。
- RP では、データベースのテナント使用に関するレポートは提供されません。

## <a name="mysql-resource-provider-adapter-architecture"></a>MySQL リソースプロバイダー アダプターのアーキテクチャ

リソース プロバイダーには次のコンポーネントがあります。

* **MySQL リソース プロバイダー アダプター仮想マシン (VM)** 。これはプロバイダー サービスを実行する Windows Server VM です。
* **リソース プロバイダー**。要求を処理し、データベース リソースにアクセスします。
* **MySQL サーバーをホストするサーバー**。これはデータベースに容量を提供し、ホスティング サーバーと呼ばれます。 MySQL インスタンスを自分で作成することも、外部の MySQL インスタンスへのアクセスを提供することもできます。 [Azure Stack Hub クイックスタート ギャラリー](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows)のテンプレートの例を使用して、以下を行うことができます。

  * 自力で MySQL サーバーを作成する。
  * Azure Marketplace から MySQL Server をダウンロードしてデプロイする。

> [!NOTE]
> Azure Stack Hub 統合システムにインストールされたホスティング サーバーは、テナント サブスクリプションから作成する必要があります。 既定のプロバイダー サブスクリプションからは作成できません。 ユーザー ポータル、または適切にサインインした PowerShell セッションから作成する必要があります。 すべてのホスティング サーバーは課金対象の VM であり、ライセンスが必要です。 サービス管理者は、テナント サブスクリプションの所有者になることができます。

### <a name="required-privileges"></a>必要な特権

システム アカウントには、次の特権が必要です。

* **データベース:** 作成、ドロップ
* **ログイン:** 作成、設定、ドロップ、許可、取り消し  

## <a name="next-steps"></a>次のステップ

[MySQL リソースプロバイダーをデプロイする](azure-stack-mysql-resource-provider-deploy.md)
