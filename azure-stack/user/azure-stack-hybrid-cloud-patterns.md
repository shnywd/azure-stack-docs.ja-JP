---
title: Azure と Azure Stack を使用してハイブリッド クラウド アプリを作成する | Microsoft Docs
description: Azure と Azure Stack を使用してハイブリッド クラウド アプリを作成する方法について説明します
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/06/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 05/06/2019
ms.openlocfilehash: a1e13b471c9eaed9dcac79c4002ceca6b3b8e7d2
ms.sourcegitcommit: a78c0d143eadcab65a601746b9ea24be28091ad2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2019
ms.locfileid: "65212671"
---
# <a name="create-hybrid-cloud-apps-with-azure-and-azure-stack"></a>Azure と Azure Stack を使用してハイブリッド クラウド アプリを作成する

Microsoft Azure Stack は Azure の拡張機能です。 Azure Stack は、ハイブリッド クラウドの有効化により、オンプレミス環境とエッジにクラウド コンピューティングの機敏性を提供します。 Azure でハイブリッド クラウド アプリを構築し、接続されている/いないを問わず、あらゆる場所にあるデータセンターにデプロイすることができます。

Microsoft Azure は一貫性のあるハイブリッド クラウドです。 Azure では、Azure で開発されたコードを再利用したり、アプリを Azure のソブリン クラウドと Azure Stack にデプロイしたりできます。 クラウドをまたぐアプリケーションも、*ハイブリッド アプリケーション*と呼ばれます。

ハイブリッド シナリオは、開発で使用できるリソースによって大きく異なり、地理、セキュリティ、インターネット アクセスなど、考慮事項は多岐にわたります。 このようなシナリオは特定の要件に対処できない場合がありますが、ハイブリッド ソリューションの実装に関する主要なガイドラインと例は提供できます。

ハイブリッド クラウドは次を処理する場合に使用できます。
- サンプルの階層化データ。
- Azure および Azure Stack での SQL Server。
- Azure および Azure Stack での Mongo データベースのエンドツーエンド デプロイ。
- Microsoft Edge の人工知能による推論の無効検出。

## <a name="step-by-step-tutorials"></a>ステップバイステップのチュートリアル

- [Azure と Azure Stack の両方でアプリをデプロイする](azure-stack-solution-pipeline.md)
- [Azure Stack と Azure にアプリをデプロイする](azure-stack-solution-hybrid-identity.md)
- [Azure と Azure Stack のアプリでハイブリッド クラウド ID を構成する](azure-stack-solution-hybrid-connectivity.md)
- [Azure と Azure Stack でハイブリッド クラウド接続を構成する](azure-stack-solution-staged-data-analytics.md)
- [Azure と Azure Stack を使用してステージング データ分析ソリューションを作成する](azure-stack-solution-cloud-burst.md)
- [Azure でクラウド間スケーリング ソリューションを作成する](azure-stack-solution-cloud-burst.md)
- [Azure と Azure Stack を使用して地理的に分散されたアプリ ソリューションを作成する](azure-stack-solution-geo-distributed.md)
- [Azure と Azure Stack を使用してハイブリッド クラウド ソリューションをデプロイする](azure-stack-solution-hybrid-cloud.md)

## <a name="next-steps"></a>次の手順

- [ハイブリッド アプリケーションのための設計の考慮事項](https://aka.ms/hybrid-cloud-applications-pillars)に関するホワイト ペーパーで、ハイブリッド アプリケーションを設計、デプロイ、および運用するためのソフトウェア品質の重要な要素を確認します。
- [Azure Stack で開発環境を設定](azure-stack-dev-start.md)し、Azure Stack で[最初のアプリをデプロイ](azure-stack-dev-start-deploy-app.md)します。
