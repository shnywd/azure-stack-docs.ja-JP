---
title: App Service on Azure Stack Hub Update 6 のリリース ノート | Microsoft Docs
description: App Service on Azure Stack Hub Update 6 の内容、既知の問題、更新プログラムをダウンロードする場所について説明します。
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2019
ms.author: anwestg
ms.reviewer: ''
ms.openlocfilehash: 2c96d45aa433fdbae490ed5c1c304b1c000e8c02
ms.sourcegitcommit: d450dcf5ab9e2b22b8145319dca7098065af563b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75880109"
---
# <a name="app-service-on-azure-stack-hub-update-6-release-notes"></a>App Service on Azure Stack Hub update 6 のリリース ノート

これらのリリース ノートでは、Azure App Service on Azure Stack Hub Update 6 における機能強化と修正点、および既知の問題について説明します。 既知の問題は、デプロイおよび更新プロセスに直接関係する問題と、ビルド (インストール後) に関する問題に分けられています。

> [!IMPORTANT]
> Azure App Service 1.6 をデプロイする前に、Azure Stack Hub 統合システムに 1904 更新プログラムを適用するか、最新の Azure Stack Hub Development Kit をデプロイします。


## <a name="build-reference"></a>ビルドのリファレンス

App Service on Azure Stack Hub Update 6 のビルド番号は **82.0.1.50** です。

### <a name="prerequisites"></a>前提条件

デプロイを開始する前に、[前提条件に関するドキュメント](azure-stack-app-service-before-you-get-started.md)をご覧ください。

Azure App Service on Azure Stack Hub の 1.6 へのアップグレードを開始する前に:

- Azure Stack Hub 管理ポータルの Azure App Service の管理で確実にすべてのロールの準備ができているようにします。

- App Service とマスター データベースをバックアップします。
  - AppService_Hosting
  - AppService_Metering
  - Master

- テナント アプリのコンテンツ ファイル共有をバックアップします。

- Marketplace から**カスタム スクリプト拡張機能**バージョン **1.9.1** を配信します。

### <a name="new-features-and-fixes"></a>新機能と修正

Azure App Service on Azure Stack Hub Update 6 には、次の機能強化と修正が含まれています。

- **App Service のテナント ポータル、管理ポータル、Functions ポータル、Kudu ツール**の更新。 Azure Stack Hub Portal SDK バージョンと一貫性があります。

- **Azure Functions ランタイム**が **v1.0.12299** に更新されました。

- 信頼性を高めるためのコア サービスと、一般的な問題を簡単に診断できるようにするエラー メッセージの更新。

- **次のアプリケーション フレームワークとツールの更新**:
  - ASP.NET Core 2.2.4
  - NodeJS 10.15.2
  - Zulu OpenJDK 8.36.0.1
  - Tomcat 7.0.81
  - Tomcat 8.5.37
  - Tomcat 9.0.14
  - PHP 5.6.39
  - PHP 7.0.33
  - PHP 7.1.25
  - PHP 7.2.13
  - Kudu が 81.10329.3844 に更新されました

- **すべてのロールの基になっているオペレーティング システムの更新プログラム**:
  - [x64 ベース システム用 Windows Server 2016 に対する 2019-04 累積的な更新プログラム (KB4493473)](https://support.microsoft.com/help/4493473/windows-10-update-kb4493473)

### <a name="post-deployment-steps"></a>デプロイ後の手順

> [!IMPORTANT]
> SQL Always On インスタンスを使用して App Service リソース プロバイダーを提供している場合は、データベースのフェールオーバーが発生したときにサービスが失われないように、[appservice_hosting と appservice_metering データベースを可用性グループに追加](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database)し、それらのデータベースを同期する必要があります。

### <a name="known-issues-post-installation"></a>既知の問題 (インストール後)

- App Service が既存の仮想ネットワークにデプロイされ、ファイル サーバーがプライベート ネットワークでしか使用できない場合、worker はファイル サーバーに到達することができません。Azure App Service on Azure Stack Hub のデプロイ ドキュメントで説明されているとおりです。

ファイル サーバーに接続するために既存の仮想ネットワークと内部 IP アドレスへデプロイする場合は、送信セキュリティ規則を追加して、worker サブネットとファイル サーバー間の SMB トラフィックを有効にする必要があります。 管理者ポータルで WorkersNsg に移動し、次のプロパティを持つ送信セキュリティ規則を追加します。
 * ソース:Any
 * 送信元ポート範囲: *
 * 変換先:IP アドレス
 * 宛先 IP アドレス範囲:ファイル サーバーの IP の範囲
 * 送信先ポート範囲:445
 * プロトコル:TCP
 * アクション:Allow
 * 優先順位:700
 * 名前:Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack-hub"></a>Azure App Service on Azure Stack Hub を運用するクラウド管理者に対する既知の問題

[Azure Stack Hub 1908 リリース ノート](/azure-stack/operator/release-notes?view=azs-1908)内のドキュメントを参照してください。

### <a name="known-issues-for-tenants-deploying-applications-on-azure-app-service-on-azure-stack-hub"></a>Azure App Service on Azure Stack Hub にアプリケーションをデプロイするテナントに関する既知の問題

- デプロイ センターが淡色表示される

テナントでは、デプロイ センターをまだ利用できません。これは、2018 年後半にパブリック クラウドにリリースされた機能です。  テナントではまだ、portal、CLI、PowerShell による標準のデプロイ方法 (FTP、Web 配置、Git など) を使用できます。

- デプロイ オプション (クラシック) UX およびデプロイ資格情報ポータルのオプションを使用できない

Azure Stack Hub のデプロイでデプロイ オプションおよびデプロイ資格情報ユーザー エクスペリエンスを使用するには、テナントで https://portal.&lt ;*region*&gt;.&lt;*FQDN*&gt; /?websitesExtension_oldvsts=true という URL 形式を使ってポータルにアクセスし (ASDK の場合は [https://portal.local.azurestack.external/?websitesExtension_oldvsts=true](https://portal.local.azurestack.external/?websitesExtension_oldvsts=true))、その後で普通にアプリケーションに移動する必要があります。

- ポータルで Azure Function の監視が継続して "読み込み中" と表示される

個々の関数を監視しようとしても、ユーザー ポータルに呼び出しのログ、成功数、エラー数が表示されません。  この機能を再度有効にするには、 **[関数アプリ]** 、 **[プラットフォーム機能]** 、 **[アプリケーション設定]** の順に移動します。  **AzureWebJobsDashboard** という名前の新しいアプリの設定を追加し、AzureWebJobsStorage と同じ値を設定します。  それから、関数の監視ビューに移動すると、監視情報が表示されます。

## <a name="next-steps"></a>次のステップ

- Azure App Service の概要については、「[Azure Stack Hub 上の App Service の概要](azure-stack-app-service-overview.md)」を参照してください。
- App Service on Azure Stack Hub のデプロイの準備をする方法の詳細については、「[App Service on Azure Stack Hub を開始する前に](azure-stack-app-service-before-you-get-started.md)」を参照してください。
