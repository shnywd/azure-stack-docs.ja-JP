---
title: App Service on Azure Stack Update 7 のリリース ノート | Microsoft Docs
description: App Service on Azure Stack Update 7 の内容、既知の問題、更新プログラムをダウンロードする場所について説明します。
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
ms.date: 08/29/2019
ms.author: anwestg
ms.reviewer: ''
ms.openlocfilehash: ec07277043068835d1d1d5a41285ee5df5ee7691
ms.sourcegitcommit: 701685f0b59e5a3d1a8d39fe477b8df701a51cd2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2019
ms.locfileid: "70165360"
---
# <a name="app-service-on-azure-stack-update-7-release-notes"></a>App Service on Azure Stack update 7 のリリース ノート

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

これらのリリース ノートでは、Azure App Service on Azure Stack Update 7 における機能強化と修正点、および既知の問題について説明します。 既知の問題は、デプロイおよび更新プロセスに直接関係する問題と、ビルド (インストール後) に関する問題に分けられています。

> [!IMPORTANT]
> Azure App Service 1.7 をデプロイする前に、Azure Stack 統合システムに 1907 更新プログラムを適用するか、最新の Azure Stack Development Kit をデプロイします。


## <a name="build-reference"></a>ビルドのリファレンス

App Service on Azure Stack Update 7 のビルド番号は **84.0.2.10** です

### <a name="prerequisites"></a>前提条件

デプロイを開始する前に、[前提条件に関するドキュメント](azure-stack-app-service-before-you-get-started.md)をご覧ください。

Azure App Service on Azure Stack の 1.7 へのアップグレードを開始する前に:

- Azure Stack 管理ポータルにおいて Azure App Service の [管理] ですべてのロールが [準備完了] になっていることを確認します。

- App Service とマスター データベースをバックアップします。
  - AppService_Hosting
  - AppService_Metering
  - Master

- テナント アプリのコンテンツ ファイル共有をバックアップします。

- Marketplace から**カスタム スクリプト拡張機能**バージョン **1.9.3** を配信します

### <a name="new-features-and-fixes"></a>新機能と修正

Azure App Service on Azure Stack Update 7 には、次の機能強化と修正が含まれています。

- **App Service のテナント ポータル、管理ポータル、Functions ポータル、Kudu ツール**の更新。 Azure Stack Portal SDK バージョンと一致しています。

- **Azure Functions ランタイム**が **v1.0.12582** に更新されました。

- 信頼性を高めるためのコア サービスと、一般的な問題を簡単に診断できるようにするエラー メッセージの更新。

- **次のアプリケーション フレームワークとツールの更新**:
  - ASP.NET Core 2.2.46
  - Zul OpenJDK 8.38.0.13
  - Tomcat 7.0.94
  - Tomcat 8.5.42
  - Tomcat 9.0.21
  - PHP 5.6.40
  - PHP 7.3.6
  - Kudu が 82.10503.3890 に更新されました

- **すべてのロールの基になっているオペレーティング システムの更新プログラム**:
  - [x64 ベース システム用 Windows Server 2016 に対する 2019-08 累積的な更新プログラム (KB4512495)](https://support.microsoft.com/help/4512495)

- **ユーザー ポータルでアクセス制限が有効になりました**:
  - このリリースの時点では、ユーザーは、公開済みのドキュメント (「[Azure App Service のアクセス制限](https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions)」) に従って、Web/API/Functions アプリケーションのアクセス制限を構成できます。**注**:Azure Stack 上の Azure App Service ではサービス エンドポイントはサポートされていません。

- **デプロイ オプション (クラシック) 機能が復元されました**:
  - ユーザーはもう一度デプロイ オプション (クラシック) を使用して、GitHub、Bitbucket、Dropbox、OneDrive、ローカルおよび外部リポジトリからアプリのデプロイを構成し、アプリケーションのデプロイ資格情報を設定することができます。

- **Azure Function の監視**が正しく構成されました。

- **Windows Update の動作**:お客様からのフィードバックに基づいて、Update 7 の App Service ロールで Windows Update を構成する方法が変更されました。
  - 3 つのモード:
    - **無効** - Windows Update サービスは無効になっていて、Windows は Azure Stack リリースの Azure App Service に付属している KB で更新されます。
    - **自動** - Windows Update サービスは有効になっていて、Windows Update によって更新の方法とタイミングが決まります。
    - **マネージド** - Windows Update サービスは無効になっていて、Azure App Service は、個々のロールの OnStart 中に Windows Update サイクルを実行します。

  **新規**のデプロイ - Windows Update サービスは既定では無効になっています。

  **既存**のデプロイ - コントローラーの設定を変更した場合、値は **False** から **[無効]** に変更され、**true** の前の値は **[自動]** になります

### <a name="post-deployment-steps"></a>デプロイ後の手順

> [!IMPORTANT]
> SQL Always On インスタンスを使用して App Service リソース プロバイダーを提供している場合は、データベースのフェールオーバーが発生したときにサービスが失われないように、[appservice_hosting と appservice_metering データベースを可用性グループに追加](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database)し、それらのデータベースを同期する必要があります。

### <a name="known-issues-post-installation"></a>既知の問題 (インストール後)

- App Service が既存の仮想ネットワークにデプロイされ、ファイル サーバーがプライベート ネットワークでしか使用できない場合、worker はファイル サーバーに到達することができません。Azure App Service on Azure Stack のデプロイ ドキュメントで説明されているとおりです。

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

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Azure App Service on Azure Stack を運用するクラウド管理者に関する既知の問題

[Azure Stack 1907 リリース ノート](azure-stack-release-notes-1907.md)内のドキュメントを参照してください

## <a name="next-steps"></a>次の手順

- Azure App Service の概要については、「[Azure Stack 上の App Service の概要](azure-stack-app-service-overview.md)」をご覧ください。
- App Service on Azure Stack のデプロイの準備をする方法の詳細については、「[App Service on Azure Stack を開始する前に](azure-stack-app-service-before-you-get-started.md)」をご覧ください。
