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
ms.openlocfilehash: d2ce6c0af2912a2658db80301c9a64c8e3d5c066
ms.sourcegitcommit: 62283e9826ea78b218f5d2c6c555cc44196b085d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74780730"
---
# <a name="use-sql-databases-on-azure-stack"></a>Azure Stack 上で SQL データベースを使用する

SQL Server リソース プロバイダーを使用して、SQL データベースを [Azure Stack](azure-stack-overview.md) のサービスとして提供します。 リソースプロバイダーをインストールして 1 つまたは複数の SQL Server インスタンスに接続すると、御社および御社のユーザーは次のものを作成できます。

- クラウドネイティブ アプリ向けデータベース。
- SQL を使用する Websites。
- SQL を使用するワークロード。

リソース プロバイダーでは、[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) のすべてのデータベース管理機能が提供されているわけではありません。 たとえば、リソースを自動的に割り当てるエラスティック プールはサポートされていません。 ただし、リソース プロバイダーでは、SQL Server データベースに対して同様の作成、読み取り、更新、および削除 (CRUD) 操作がサポートされています。

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
