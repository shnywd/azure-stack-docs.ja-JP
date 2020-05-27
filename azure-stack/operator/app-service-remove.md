---
title: Azure Stack Hub からの Azure App Service を削除する
description: Azure Stack Hub から Azure App Service を削除する方法について説明します
author: apwestgarth
ms.topic: article
ms.date: 04/17/2020
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 04/17/20207
ms.openlocfilehash: b371b319ae87bb112633b64d77d66f8916a13124
ms.sourcegitcommit: 355e21dd9b8c3f44e14abaae0b4f176443cf7495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2020
ms.locfileid: "81636492"
---
# <a name="remove-azure-app-service-from-azure-stack-hub"></a>Azure Stack Hub からの Azure App Service を削除する

この記事では、Azure App Service リソース プロバイダーと関連コンポーネントを Azure Stack Hub から削除する方法について説明します。

## <a name="remove-resource-provider"></a>リソース プロバイダーを削除する

> [!Important]
> この操作を実行すると、すべてのテナント リソースが削除され、サービスと割り当てがすべてのプランから削除され、Azure App Service リソース プロバイダー全体が削除されます。  App Service の高可用性ファイル サーバーと SQL Server クイックスタート テンプレートをデプロイした場合、これらのリソースは Azure App Service on Azure Stack Hub と同じリソース グループにデプロイされるため、これらのリソースも削除されます。

Azure App Service を Azure Stack Hub から削除するには、次の 1 つの手順を実行します。

1. Azure App Service on Azure Stack Hub リソースを保持するリソース グループ (AppService.local など) を削除します。

## <a name="remove-databases-and-file-share-content"></a>データベースとファイル共有のコンテンツを削除する

SQL Server、ファイル サーバー、またはその両方がオフスタンプ、つまり別のリソース グループにデプロイされている場合にのみ、このセクションに従う必要があります。それ以外の場合は、次のセクションに進みます。

### <a name="remove-databases-and-logins"></a>データベースとログインを削除する

1. **SQL Server Always On** を使用している場合は、可用性グループから **AppService_Hosting** および **AppService_Metering** データベースを削除します。

1. 次の SQL スクリプトを実行してデータベースとログインを削除します。

   ```sql
   --******************************************************************
   /*
   Script to clean up App Service objects (databases and logins).
   */
   USE [master]
   GO

   DROP DATABASE [appservice_hosting]
   GO

   DROP DATABASE [appservice_metering]
   GO

   DECLARE @sql NVARCHAR(MAX) = N'';    
 
   SELECT @sql += '
   DROP LOGIN [' + name + '];' 
   from master.sys.sql_logins
   WHERE name LIKE  '%_hosting_%' OR 
   name LIKE  '%_metering_%' OR
   name LIKE  '%WebWorker_%';

   PRINT @sql;
   EXEC sp_executesql @sql;
   PRINT 'Completed';

   --******************************************************************
   ```

### <a name="remove-the-application-file-content-from-the-file-server"></a>ファイル サーバーからアプリケーション ファイルのコンテンツを削除する

1. ファイル サーバーからコンテンツ ファイル共有を削除します。

## <a name="next-steps"></a>次のステップ

再インストールするには、「[App Service on Azure Stack Hub のデプロイの前提条件](azure-stack-app-service-before-you-get-started.md)」の記事に戻ります。
