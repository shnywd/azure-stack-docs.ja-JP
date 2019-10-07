---
title: Azure Stack 統合システムの Azure から切断されたデプロイの決定 | Microsoft Docs
description: Azure Stack 統合システムの Azure から切断されたデプロイと考慮すべき計画の決定について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2019
ms.author: mabrigg
ms.reviewer: wfayed
ms.lastreviewed: 12/11/2018
ms.openlocfilehash: f4b8f73987df3067c8d69504934884ec4329cacf
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829151"
---
# <a name="azure-disconnected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Azure Stack 統合システムの Azure から切断されたデプロイ計画の決定
[Azure Stack をハイブリッド クラウド環境に統合する方法](azure-stack-connection-models.md)を決定したら、Azure Stack デプロイの決定を完了することができます。

インターネットに接続していなくても、Azure Stack をデプロイして使用することができます。 ただし、切断されたデプロイでは、Active Directory フェデレーション サービス (AD FS) ID ストアおよび容量ベースの課金モデルに制限されます。 マルチテナントには Azure Active Directory (Azure AD) の使用が必要であるため、切断されたデプロイではマルチテナントはサポートされません。

このオプションは、次の場合に選択します。
- インターネットに接続されていない環境で Azure Stack をデプロイすることが必要なセキュリティまたはその他の制限がある。
- データ (使用状況データを含む) を Azure に送信されないようにブロックしたい。
- Azure Stack を純粋に、社内イントラネットにデプロイされるプライベート クラウド ソリューションとして使用したい。ハイブリッド シナリオには関心がない。

> [!TIP]
> この種類の環境は "*サブマリン シナリオ*" と呼ばれる場合もあります。

切断されたデプロイは、後でハイブリッド テナント VM シナリオのために Azure Stack インスタンスを Azure に接続することを制限するものではありません。 これは、デプロイ中に Azure への接続がないか、または ID ストアとして Azure AD を使用したくないことを示しています。

## <a name="features-that-are-impaired-or-unavailable-in-disconnected-deployments"></a>切断されたデプロイで損なわれるか、または使用できない機能 
Azure Stack は Azure に接続されているときに最適に機能するように設計されているため、切断モードでは損なわれるか、または完全に使用できない機能がいくつか存在することに注意することが重要です。

|機能|切断モードでの影響|
|-----|-----|
|VM のデプロイ後を構成するための DSC 拡張機能を備えた VM デプロイ|損なわれる - DSC 拡張機能は、最新の WMF がないかどうかインターネットを参照します。|
|Docker コマンドを実行するための Docker 拡張機能を備えた VM デプロイ|損なわれる - Docker は最新バージョンがないかどうかインターネットをチェックするため、このチェックは失敗します。|
|Azure Stack ポータルでのドキュメント リンク|使用できない - インターネット URL を使用するリンク ([フィードバックの送信]、[ヘルプ]、[クイック スタート] など) は機能しません。|
|オンライン修復ガイドを参照する、アラートによる修復/軽減|使用できない - インターネット URL を使用するアラート修復リンクはすべて機能しません。|
|Marketplace - Azure Marketplace から直接ギャラリー パッケージを選択して追加する機能|損なわれる - Azure Stack を非接続モードでデプロイする場合、Azure Stack ポータルを使用して Marketplace 項目をダウンロードすることはできません。 ただし、[マーケットプレース シンジケーション ツール](azure-stack-download-azure-marketplace-item.md)を使用して、インターネットに接続されたマシンに Marketplace アイテムをダウンロードしてから、Azure Stack 環境に転送することができます。|
|Azure Stack デプロイを管理するための Azure AD フェデレーション アカウントの使用|使用できない - この機能には Azure への接続が必要です。 代わりに、ローカルの Active Directory インスタンスによる AD FS を使用する必要があります。|
|アプリケーション サービス|損なわれる - WebApps では、コンテンツの更新のためにインターネットへのアクセスが必要な場合があります。|
|コマンド ライン インターフェイス (CLI)|損なわれる - CLI では、サービス プリンシパルの認証およびプロビジョニングの機能が削減されます。|
|Visual Studio - クラウド検出|損なわれる - Cloud Discovery は別のクラウドを検出するか、まったく機能しないかのどちらかです。|
|Visual Studio - AD FS|損なわれる - AD FS 認証をサポートするのは、Visual Studio Enterprise と Visual Studio Code のみです。
テレメトリ|使用できない - Azure Stack のテレメトリ データや、テレメトリ データに依存するすべてのサード パーティ製ギャラリー パッケージ。|
|証明書|使用できない - HTTPS のコンテキストでの証明書失効リスト (CRL) およびオンライン証明書ステータス プロトコル (OSCP) サービスにはインターネット接続が必要です。|
|Key Vault|損なわれる - Key Vault の一般的なユースケースでは、実行時にアプリにシークレットを読み取らせます。 このユース ケースの場合、アプリはそのディレクトリ内にサービス プリンシパルを必要とします。 Azure AD では、通常のユーザー (非管理者) は、既定ではサービス プリンシパルの追加を許可されます。 Azure AD (AD FS を使用) では、そうではありません。 アプリの追加には必ずディレクトリ管理者を経由する必要があるため、この欠損により、エンド ツー エンドのエクスペリエンスに問題が発生します。

## <a name="learn-more"></a>詳細情報
- ユース ケース、購入、パートナー、OEM ハードウェア ベンダーの詳細については、[Azure Stack](https://azure.microsoft.com/overview/azure-stack/) の製品ページを参照してください。
- Azure Stack 統合システムのロードマップと地理的な可用性については、ホワイト ペーパー「[Azure Stack: An extension of Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)」 (Azure Stack: Azure の拡張機能) を参照してください。 
- Microsoft Azure Stack のパッケージと価格の詳細については、[.pdf をダウンロードしてください](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf)。 

## <a name="next-steps"></a>次の手順
[データセンターのネットワーク統合](azure-stack-network.md)
