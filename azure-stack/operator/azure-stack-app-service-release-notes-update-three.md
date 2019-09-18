---
title: App Service on Azure Stack Update 3 のリリース ノート | Microsoft Docs
description: App Service on Azure Stack Update 3 の機能強化、修正、既知の問題の詳細を説明します。
services: azure-stack
documentationcenter: ''
author: bryanla
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 08/20/2018
ms.openlocfilehash: 88255a15a204f5dcb18dd83f9763e8297a1af51c
ms.sourcegitcommit: 7d7a4c8c46613b6104caf23763bfd2275f6a826b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2019
ms.locfileid: "70808244"
---
# <a name="app-service-on-azure-stack-update-3-release-notes"></a>App Service on Azure Stack Update 3 のリリース ノート

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

これらのリリース ノートでは、Azure App Service on Azure Stack Update 3 における機能強化、修正点、および既知の問題について説明します。 既知の問題は、デプロイに直接関係する問題、更新プロセスの問題、ビルド (インストール後) の問題の 3 つのセクションに分けられています。

> [!IMPORTANT]
> Azure App Service 1.3 をデプロイする前に、Azure Stack 統合システムに 1807 更新プログラムを適用するか、最新の Azure Stack Development Kit (ASDK) をデプロイします。

## <a name="build-reference"></a>ビルドのリファレンス

App Service on Azure Stack Update 3 のビルド番号は **74.0.13698.31** です。

### <a name="prerequisites"></a>前提条件

デプロイを開始する前に、[App Service on Azure Stack の前提条件](azure-stack-app-service-before-you-get-started.md)に関するページを参照してください。

Azure App Service on Azure Stack を 1.3 にアップグレードする前に、Azure Stack 管理ポータルの Azure App Service 管理画面ですべてのロールの準備ができていることを確認してください。

![App Service のロールの状態](media/azure-stack-app-service-release-notes-update-three/image01.png)

### <a name="new-features-and-fixes"></a>新機能と修正

Azure App Service on Azure Stack Update 3 には、次の機能強化と修正が含まれています。

- Azure App Service リソース プロバイダー データベースで SQL Server Always On の使用がサポートされます。

- Create-AADIdentityApp ヘルパー スクリプトで、さまざまな AAD リージョンを対象にできるよう新しい環境パラメーターを追加しました。

- **App Service のテナント ポータル、管理ポータル、Functions ポータル、Kudu ツール**の更新。 Azure Stack Portal SDK バージョンと一貫性があります。

- **Azure Functions ランタイム**が **v1.0.11820** に更新されました。

- 信頼性を高めるためのコア サービスと、一般的な問題を簡単に診断できるようにするエラー メッセージの更新。

- **次のアプリケーション フレームワークとツールの更新**:
  - ASP.NET Core 2.1.2 を追加
  - NodeJS 10.0.0 を追加
  - Zulu OpenJDK 8.30.0.1 を追加
  - Tomcat 8.5.31 と 9.0.8 を追加
  - 次の PHP バージョンを追加
    - 5.6.36
    - 7.0.30
    - 7.1.17
    - 7.2.5
  - Wincache 2.0.0.8 を追加
  - Git for Windows を v 2.17.1.2 に更新
  - Kudu を 74.10611.3437 に更新
  
- **すべてのロールの基になっているオペレーティング システムの更新プログラム**:
  - [x64 ベース システム用 Windows Server 2016 に対するサービス スタック更新プログラム (KB4132216)](https://support.microsoft.com/help/4132216/servicing-stack-update-for-windows-10-1607-may-17-2018)
  - [x64 ベース システム用 Windows Server 2016 に対する 2018-07 累積的更新プログラム (KB4338822)](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822)

### <a name="post-update-steps-optional"></a>更新後の手順 (省略可)

既存の Azure App Service on Azure Stack デプロイに関して包含データベースへの移行を検討している場合は、Azure App Service on Azure Stack 1.3 への更新が完了した後で、以下の手順を実行してください。

> [!IMPORTANT]
> この手順の所要時間は 5 分から 10 分程度です。 この手順には、既存のデータベース ログイン セッションを中止する作業が伴います。 Azure App Service on Azure Stack の移行とその後の検証に伴うダウンタイムを考慮してください。
>
>

1. [AppService データベース (appservice_hosting and appservice_metering) を可用性グループ](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database)に追加します。

1. 包含データベースを有効にします。
    ```sql

        sp_configure 'contained database authentication', 1;
        GO
        RECONFIGURE;
            GO
    ```

1. データベースを部分的包含に変換します。 この手順では、アクティブなセッションをすべて中止する必要があるため、ダウンタイムが伴います。

    ```sql
        /******** [appservice_metering] Migration Start********/
            USE [master];

            -- kill all active sessions
            DECLARE @kill varchar(8000) = '';  
            SELECT @kill = @kill + 'kill ' + CONVERT(varchar(5), session_id) + ';'  
            FROM sys.dm_exec_sessions
            WHERE database_id  = db_id('appservice_metering')

            EXEC(@kill);

            USE [master]  
            GO  
            ALTER DATABASE [appservice_metering] SET CONTAINMENT = PARTIAL  
            GO  

        /********[appservice_metering] Migration End********/

        /********[appservice_hosting] Migration Start********/

            -- kill all active sessions
            USE [master];

            DECLARE @kill varchar(8000) = '';  
            SELECT @kill = @kill + 'kill ' + CONVERT(varchar(5), session_id) + ';'  
            FROM sys.dm_exec_sessions
            WHERE database_id  = db_id('appservice_hosting')

            EXEC(@kill);

            -- Convert database to contained
            USE [master]  
            GO  
            ALTER DATABASE [appservice_hosting] SET CONTAINMENT = PARTIAL  
            GO  

            /********[appservice_hosting] Migration End********/
    '''

1. Migrate logins to contained database users.

    ```sql
        IF EXISTS(SELECT * FROM sys.databases WHERE Name=DB_NAME() AND containment = 1)
        BEGIN
        DECLARE @username sysname ;  
        DECLARE user_cursor CURSOR  
        FOR
            SELECT dp.name
            FROM sys.database_principals AS dp  
            JOIN sys.server_principals AS sp
                ON dp.sid = sp.sid  
                WHERE dp.authentication_type = 1 AND dp.name NOT IN ('dbo','sys','guest','INFORMATION_SCHEMA');
            OPEN user_cursor  
            FETCH NEXT FROM user_cursor INTO @username  
                WHILE @@FETCH_STATUS = 0  
                BEGIN  
                    EXECUTE sp_migrate_user_to_contained
                    @username = @username,  
                    @rename = N'copy_login_name',  
                    @disablelogin = N'do_not_disable_login';  
                FETCH NEXT FROM user_cursor INTO @username  
            END  
            CLOSE user_cursor ;  
            DEALLOCATE user_cursor ;
            END
        GO
    ```

**検証**

1. SQL Server で包含が有効になっているかどうかを確認します。

    ```sql
        sp_configure  @configname='contained database authentication'
    ```

1. 既存の包含の動作を確認します。
    ```sql
        SELECT containment FROM sys.databases WHERE NAME LIKE (SELECT DB_NAME())
    ```

### <a name="known-issues-post-installation"></a>既知の問題 (インストール後)

- App Service が既存の仮想ネットワークにデプロイされ、ファイル サーバーがプライベート ネットワークでしか使用できない場合、worker はファイル サーバーに到達することができません。 この問題は Azure Stack 上の Azure App Service のデプロイに関するドキュメントで言及されています。

ファイル サーバーに接続するために既存の仮想ネットワークと内部 IP アドレスへデプロイする場合は、worker サブネットとファイル サーバー間の SMB トラフィックを有効にする送信セキュリティ規則を追加する必要があります。 管理者ポータルで WorkersNsg に移動し、次のプロパティを持つ送信セキュリティ規則を追加します。

 * ソース:Any
 * 送信元ポート範囲: *
 * 変換先:IP アドレス
 * 宛先 IP アドレス範囲:ファイル サーバーの IP の範囲
 * 送信先ポート範囲:445
 * プロトコル:TCP
 * アクション:Allow
 * 優先順位:700
 * 名前:Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Azure App Service on Azure Stack を運用するクラウド管理者に対する既知の問題

Azure Stack 1807 リリース ノート内のドキュメントを参照してください。

## <a name="next-steps"></a>次の手順

- Azure App Service の概要については、「[Azure Stack 上の App Service の概要](azure-stack-app-service-overview.md)」をご覧ください。
- App Service on Azure Stack のデプロイを準備する方法の詳細については、「[App Service on Azure Stack のデプロイの前提条件](azure-stack-app-service-before-you-get-started.md)」をご覧ください。
