---
title: App Service on Azure Stack Update 6 のリリース ノート | Microsoft Docs
description: App Service on Azure Stack Update 6 の内容、既知の問題、更新プログラムをダウンロードする場所について説明します。
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
ms.date: 05/28/2019
ms.author: anwestg
ms.reviewer: ''
ms.openlocfilehash: d280ffacf9cb74c519857ebafc907debc915ec21
ms.sourcegitcommit: 85367001c332ed53fba0d2294eae3c06e8578070
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66307833"
---
# <a name="app-service-on-azure-stack-update-6-release-notes"></a>App Service on Azure Stack update 6 のリリース ノート

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

これらのリリース ノートでは、Azure App Service on Azure Stack Update 6 における機能強化と修正点、および既知の問題について説明します。 既知の問題は、デプロイおよび更新プロセスに直接関係する問題と、ビルド (インストール後) に関する問題に分けられています。

> [!IMPORTANT]
> Azure App Service 1.6 をデプロイする前に、Azure Stack 統合システムに 1904 更新プログラムを適用するか、最新の Azure Stack Development Kit をデプロイします。


## <a name="build-reference"></a>ビルドのリファレンス

App Service on Azure Stack Update 6 のビルド番号は **82.0.1.50** です

### <a name="prerequisites"></a>前提条件

デプロイを開始する前に、[前提条件に関するドキュメント](azure-stack-app-service-before-you-get-started.md)をご覧ください。

Azure App Service on Azure Stack の 1.6 へのアップグレードを開始する前に:

- Azure Stack 管理ポータルにおいて Azure App Service の [管理] ですべてのロールが [準備完了] になっていることを確認します。

- App Service とマスター データベースをバックアップします。
  - AppService_Hosting
  - AppService_Metering
  - Master

- テナント アプリのコンテンツ ファイル共有をバックアップします。

- Marketplace から**カスタム スクリプト拡張機能**バージョン **1.9.1** を配信します。

### <a name="new-features-and-fixes"></a>新機能と修正

Azure App Service on Azure Stack Update 6 には、次の機能強化と修正が含まれています。

- **App Service のテナント ポータル、管理ポータル、Functions ポータル、Kudu ツール**の更新。 Azure Stack Portal SDK バージョンと一致しています。

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

- App Service が既存の仮想ネットワークにデプロイされ、ファイル サーバーがプライベート ネットワークでしか使用できない場合、worker はファイル サーバーに到達することができません。Azure App Service on Azure Stack のデプロイ ドキュメントで説明されているとおりです。

ファイル サーバーに接続するために既存の仮想ネットワークと内部 IP アドレスへデプロイする場合は、送信セキュリティ規則を追加して、worker サブネットとファイル サーバー間の SMB トラフィックを有効にする必要があります。 管理者ポータルで WorkersNsg に移動し、次のプロパティを持つ送信セキュリティ規則を追加します。
 * ソース:任意
 * 送信元ポート範囲: *
 * 変換先:IP アドレス
 * 宛先 IP アドレス範囲:ファイル サーバーの IP の範囲
 * 送信先ポート範囲:445
 * プロトコル:TCP
 * アクション:ALLOW
 * 優先順位:700
 * 名前:Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Azure App Service on Azure Stack を運用するクラウド管理者に関する既知の問題

[Azure Stack 1904 リリース ノート](azure-stack-release-notes-1904.md)内のドキュメントを参照してください。

## <a name="next-steps"></a>次の手順

- Azure App Service の概要については、「[Azure Stack 上の App Service の概要](azure-stack-app-service-overview.md)」をご覧ください。
- App Service on Azure Stack のデプロイの準備をする方法の詳細については、「[App Service on Azure Stack を開始する前に](azure-stack-app-service-before-you-get-started.md)」をご覧ください。
