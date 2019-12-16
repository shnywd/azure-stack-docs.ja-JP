---
title: SQL データベースを使用する
titleSuffix: Azure Stack
description: SQL Server リソース プロバイダーを使用して、SQL データベースを Azure Stack 上のサービスとして提供する方法を説明します。
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
ms.openlocfilehash: 438cf2d8a34046f29d156aadc1cc82571e4b8a12
ms.sourcegitcommit: d619612f54eeba3231ed73ed149ff894f9bf838a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2019
ms.locfileid: "74993893"
---
# <a name="use-sql-databases-on-azure-stack"></a>Azure Stack 上で SQL データベースを使用する

SQL リソース プロバイダーを使用して、SQL データベースを [Azure Stack](azure-stack-overview.md) のサービスとして提供します。 リソースプロバイダーをインストールして 1 つまたは複数の SQL Server インスタンスに接続すると、御社および御社のユーザーは次のものを作成できます。

- クラウドネイティブ アプリ向けデータベース。
- SQL を使用する Websites。
- SQL を使用するワークロード。

SQL リソース プロバイダーをインストールする前に、考慮すべき制限事項がいくつかあります。

- ユーザーは、個々のデータベースの作成と管理のみを行うことができます。 エンド ユーザーはデータベース サーバー インスタンスにアクセスできません。 これにより、マスター、Temp DB にアクセスしたり、データベースを動的に管理したりすることが必要なオンプレミス データベース アプリケーションとの互換性が制限される可能性があります。
- Azure Stack オペレーターは、SQL データベース サーバーおよびホストのデプロイ、更新、セキュリティ保護、構成、および保守を担当します。 RP サービスでは、ホストおよびデータベース サーバー インスタンスの管理機能は提供されません。 
- 異なるサブスクリプションの異なるユーザーのデータベースは、同じデータベース サーバー インスタンスに配置できます。 RP は、データベースを別のホストまたはデータベース サーバー インスタンスで分離するためのメカニズムを備えていません。
- RP では、データベースのテナント使用に関するレポートは提供されません。

## <a name="sql-resource-provider-adapter-architecture"></a>SQL リソースプロバイダー アダプターのアーキテクチャ

リソース プロバイダーは、次のコンポーネントで構成されています。

- **SQL リソース プロバイダーの仮想マシン (VM)** 。プロバイダー サービスを実行する Windows Server VM です。
- **リソース プロバイダー**。要求を処理し、データベース リソースにアクセスします。
- **SQL Server をホストするサーバー**。これはデータベースに容量を提供し、ホスティング サーバーと呼ばれます。

少なくとも 1 つの SQL Server インスタンスを作成するか、外部 SQL Server インスタンスへのアクセスを提供する必要があります。

> [!NOTE]
> Azure Stack 統合システムにインストールされたホスティング サーバーは、テナント サブスクリプションから作成する必要があります。 既定のプロバイダー サブスクリプションからは作成できません。 作成するには、適切なサインインで、テナント ポータルまたは PowerShell を使用する必要があります。 すべてのホスティング サーバーは課金対象の VM であり、ライセンスが必要です。 サービス管理者は、テナント サブスクリプションの所有者になることができます。

## <a name="next-steps"></a>次の手順

[SQL Server リソース プロバイダーのデプロイ](azure-stack-sql-resource-provider-deploy.md)
