---
title: Azure App Service on Azure Stack の課金の概要と FAQ | Microsoft Docs
description: Azure App Service on Azure Stack の測定方法と課金方法について説明します。
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 06/10/2019
ms.openlocfilehash: 8b17020419183067793309055a6487f31da51246
ms.sourcegitcommit: d67ae2877e3b049bf70b2567b917e55394fb8984
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2019
ms.locfileid: "67712676"
---
# <a name="azure-app-service-on-azure-stack-billing-overview-and-faq"></a>Azure App Service on Azure Stack の課金の概要と FAQ

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

この記事では、Azure App Service on Azure Stack を提供するクラウド オペレーターに対する課金方法、およびテナントによるサービスの使用に対してクラウド オペレーターが課金できる方法を示します。

## <a name="billing-overview"></a>課金の概要

Azure Stack クラウド オペレーターは、Azure App Service on Azure Stack を Azure Stack スタンプにデプロイして、Azure App Service と Azure Functions のテナント機能を顧客に提供します。 Azure App Service リソース プロバイダーは、インフラストラクチャ層と worker 層の間で分割できる複数種類のロールで構成されています。

インフラストラクチャ ロールは、サービスの中核的な運用に必要なので課金されません。 クラウド オペレーターのテナントの要望に対応するために、必要に応じてインフラストラクチャ ロールをスケールすることができます。 インフラストラクチャ ロールは以下のとおりです。

- コントローラー
- 管理ロール
- 発行元
- フロント エンド

worker 層は、共有と専用という 2 つの主要な種類で構成されます。 worker の使用量には、次の基準に従ってクラウド オペレーターに課金されます。

## <a name="shared-workers"></a>共有 worker

マルチテナントの共有 worker では、多数のテナントに対して無料で共有の App Service プランと従量課金ベースの Azure 関数がホストされます。 Azure App Service リソース プロバイダーで準備完了とマークされている場合、共有 worker から使用量の測定が発行されます。

## <a name="dedicated-workers"></a>専用 worker

専用 worker は、テナントが作成した App Service プランに関連付けられています。 たとえば、S1 SKU では、テナントは既定で 10 インスタンスにスケーリングできます。 テナントが S1 App Service プランを作成すると、Azure App Service によって、Small worker 層スケール セット内のインスタンスの 1 つがそのテナントの App Service プランに割り当てられます。 割り当てられた worker は、他のテナントに割り当てることができなくなります。 テナントが App Service プランを 10 インスタンスにスケーリングすることを選択した場合、使用できるプールからさらに 9 個の worker が削除され、テナントの App Service プランに割り当てられます。

次の場合には、専用 worker に対して測定が発行されます。

- Azure App Service リソース プロバイダーで準備完了とマークされます。
- App Service プランに割り当てられます。

この課金モデルを使用すると、クラウド オペレーターは、テナントの App Service プランによって実質的に予約されるまで worker 料金を支払うことなく顧客が使用できる状態の専用 worker のプールをプロビジョニングすることができます。 

たとえば、Small worker 層に 20 個の worker があるとします。 その場合、5 件の顧客にそれぞれ 2 つの S1 App Service プランを作成し、それぞれが 2 つのインスタンスに App Service プランをスケールする場合、使用できる worker はなくなります。 その結果、顧客や新規の顧客がスケールアウトしたり、新しい App Service プランを作成したりする容量もなくなります。 

クラウド オペレーターは、Azure Stack 管理の Azure App Service 構成内の worker 層を調べることで、worker 層あたりの現在使用できる worker 数を確認できます。

![App Service - worker 層の画面][1]

## <a name="see-customer-usage-by-using-the-azure-stack-usage-service"></a>Azure Stack の使用状況サービスを使用して顧客の使用量を確認する

クラウド オペレーターは [Azure Stack テナント リソース使用量 API](azure-stack-tenant-resource-usage-api.md) に対してクエリを実行し、顧客の使用量情報を取得できます。 テナントの使用量を説明するために App Service によって発行される個々の測定についてはすべて、[使用量についての FAQ](azure-stack-usage-related-faq.md) に関する記事をご覧ください。 これらの測定を使用して顧客のサブスクリプションごとの使用量を計算し、料金を計算できます。

## <a name="frequently-asked-questions"></a>よく寄せられる質問

### <a name="how-do-i-license-the-sql-server-and-file-server-infrastructure-required-in-the-prerequisites"></a>前提条件に必要な SQL Server とファイル サーバー インフラストラクチャのライセンスを取得するにはどうすればよいですか?

Azure App Service リソース プロバイダーに必要な SQL Server およびファイル サーバー インフラストラクチャのライセンスについては、Azure App Service on Azure Stack の[使用開始前に必要なこと](azure-stack-app-service-before-you-get-started.md#licensing-concerns-for-required-file-server-and-sql-server)に関する記事をご覧ください。

### <a name="the-usage-faq-lists-the-tenant-meters-but-not-the-prices-for-those-meters-where-can-i-find-them"></a>「使用量に関する FAQ」には、テナントの測定が記載され、測定の価格は記載されていません。 どこで確認できますか?

クラウド オペレーターは、顧客に独自の価格モデルを自由に適用できます。 使用量サービスから使用量の測定が提供されます。 その測定量を使用し、独自に決定した価格モデルに基づいて、顧客に課金できます。 価格を設定できるので、オペレーターは他の Azure Stack オペレーターと差別化することができます。

### <a name="as-a-csp-how-can-i-offer-free-and-shared-skus-for-customers-to-try-out-the-service"></a>CSP は顧客がサービスを試すための無料 SKU と共有 SKU をどのような方法で提供できますか?

無料 SKU と共有 SKU は共有 worker でホストされるため、クラウド オペレーターには提供するコストが発生します。 そのコストを最小限に抑えるには、共有 worker のサービス レベルを最小限までスケールダウンできます。 

たとえば、無料と共有の App Service プラン SKU を提供し、使用量ベースの機能を提供するには、少なくとも 1 つの A1 インスタンスを使用できるようにする必要があります。 共有 worker はマルチテナントなので、複数の顧客アプリをホストし、App Service サンドボックスでそれぞれを個別に分離して保護することができます。 この方法で共有 worker サービス レベルをスケーリングすることによって、出費を 1 か月あたり 1 vCPU のコストに制限できます。

プランで使用するためのクォータを作成できます。それでは、無料 SKU と共有 SKU のみを提供し、顧客が作成できる無料および共有の App Service プランの数を制限します。

## <a name="sample-scripts-to-assist-with-billing"></a>請求を支援するためのサンプル スクリプト

Azure App Service チームは、Azure Stack 使用量サービスの照会を支援するため、PowerShell スクリプトのサンプルを作成しました。 クラウド オペレーターは、これらのサンプル スクリプトを使って、テナントに対する独自の課金を準備できます。 サンプル スクリプトは、GitHub の [Azure Stack Tools リポジトリ](https://github.com/Azure/AzureStack-tools)にあります。 App Service のスクリプトは、[Usage の下の AppService フォルダー](https://github.com/Azure/AzureStack-Tools/tree/master/Usage/AppService)にあります。

次のサンプル スクリプトを使用できます。

- [Get-AppServiceBillingRecords](https://github.com/Azure/AzureStack-Tools/blob/master/Usage/AppService/Get-AppServiceBillingRecords.ps1): このサンプルでは、Azure Stack Usage API から Azure App Service on Azure Stack の請求レコードがフェッチされます。
- [Get-AppServiceSubscriptionUsage](https://github.com/Azure/AzureStack-Tools/blob/master/Usage/AppService/Get-AppServiceSubscriptionUsage.ps1): このサンプルでは、サブスクリプションごとに Azure App Service on Azure Stack の使用料が計算されます。 このスクリプトでは、Usage API からのデータと、クラウド オペレーターによる測定ごとに提供される価格からのデータに基づいて、使用料が計算されます。
- [Suspend-UserSubscriptions](https://github.com/Azure/AzureStack-Tools/blob/master/Usage/AppService/Suspend-UserSubscriptions.ps1): このサンプルでは、クラウド オペレーターによって指定された使用量の制限に基づいて、サブスクリプションが中断または有効化されます。

## <a name="next-steps"></a>次の手順

- [Azure Stack テナント リソース使用量 API](azure-stack-tenant-resource-usage-api.md)

<!--Image references-->
[1]: ./media/app-service-billing-faq/app-service-worker-tiers.png
