---
title: App Service on Azure Stack Hub update 8 のリリース ノート
description: Azure App Service on Azure Stack Hub Update 8 の内容、既知の問題、更新プログラムをダウンロードする場所について説明します。
author: apwestgarth
manager: stefsch
ms.topic: article
ms.date: 01/13/2020
ms.author: anwestg
ms.reviewer: ''
ms.openlocfilehash: 639c9267a9d42b20a15bc30ab6b72706816bf7ee
ms.sourcegitcommit: fd5d217d3a8adeec2f04b74d4728e709a4a95790
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76874486"
---
# <a name="app-service-on-azure-stack-hub-update-8-release-notes"></a>App Service on Azure Stack Hub update 8 のリリース ノート

これらのリリース ノートでは、Azure App Service on Azure Stack Hub Update 8 における機能強化と修正点、および既知の問題について説明します。 既知の問題は、デプロイおよび更新プロセスに直接関係する問題と、ビルド (インストール後) に関する問題に分けられています。

> [!IMPORTANT]
> Azure App Service 1.8 をデプロイする前に、Azure Stack Hub 統合システムに 1910 更新プログラムを適用するか、最新の Azure Stack Hub Development Kit をデプロイします。


## <a name="build-reference"></a>ビルドのリファレンス

Azure App Service on Azure Stack Hub Update 8 のビルド番号は **86.0.2.13** です。

### <a name="prerequisites"></a>前提条件

デプロイを開始する前に、[前提条件に関するドキュメント](azure-stack-app-service-before-you-get-started.md)をご覧ください。

Azure App Service on Azure Stack Hub の 1.8 へのアップグレードを開始する前に:

- Azure Stack 管理ポータルにおいて Azure App Service の [管理] ですべてのロールが [準備完了] になっていることを確認します。

- App Service とマスター データベースをバックアップします。
  - AppService_Hosting
  - AppService_Metering
  - Master

- テナント アプリのコンテンツ ファイル共有をバックアップします。

- Marketplace から**カスタム スクリプト拡張機能**バージョン **1.9.3** を配信します

### <a name="new-features-and-fixes"></a>新機能と修正

Azure App Service on Azure Stack Hub Update 8 には、次の機能強化と修正が含まれています。

- **App Service のテナント ポータル、管理ポータル、Functions ポータル、Kudu ツール**の更新。 Azure Stack Portal SDK バージョンと一致しています。

- **Azure Functions ランタイム**が **v1.0.12615** に更新されました。

- 信頼性を高めるためのコア サービスと、一般的な問題を簡単に診断できるようにするエラー メッセージの更新。

- **次のアプリケーション フレームワークとツールの更新**:
  - ASP.NET Core 3.1.0
  - ASP.NET Core 3.0.1
  - ASP.NET Core 2.2.8
  - ASP.NET Core Module v2 13.1.19331.0
  - Azul OpenJDK 8.38.0.13
  - Tomcat 7.0.94
  - Tomcat 8.5.42
  - Tomcat 9.0.21
  - PHP 7.1.32
  - PHP 7.2.22
  - PHP 7.3.9
  - Kudu を 85.11024.4154 に更新
  - MSDeploy 3.5.80916.15
  - NodeJS 10.16.3
  - NPM 6.9.0
  - Git for Windows 2.19.1.0

- **すべてのロールの基になっているオペレーティング システムの更新プログラム**:
  - [x64 ベース システム用 Windows Server 2016 に対する 2019-12 累積的な更新プログラム (KB4530689)](https://support.microsoft.com/help/4530689)

- **新しいデプロイのマネージド ディスクのサポート**

Azure App Service on Azure Stack Hub のすべての新しいデプロイでは、Virtual Machines および Virtual Machine Scale Sets に対してマネージド ディスクが使用されます。  既存のすべてのデプロイでは、アンマネージド ディスクが引き続き使用されます。

- **フロント エンド ロード バランサーによって適用される TLS 1.2**

この更新の際に、**TLS 1.2** がすべてのアプリケーションに適用されます。

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

## <a name="next-steps"></a>次のステップ

- Azure App Service の概要については、「[Azure Stack 上の App Service の概要](azure-stack-app-service-overview.md)」をご覧ください。
- App Service on Azure Stack のデプロイの準備をする方法の詳細については、「[App Service on Azure Stack を開始する前に](azure-stack-app-service-before-you-get-started.md)」をご覧ください。
