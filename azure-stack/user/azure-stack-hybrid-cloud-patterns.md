---
title: 'ソリューションの構築: Azure Stack 向けのハイブリッド クラウド設計パターン | Microsoft Docs'
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
ms.date: 07/16/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/20/2019
ms.openlocfilehash: 538a626b2e89d15aa4b816674dbb8c374ec4a262
ms.sourcegitcommit: 2a4cb9a21a6e0583aa8ade330dd849304df6ccb5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286813"
---
#  <a name="build-solutions-hybrid-cloud-design-patterns-for-azure-stack"></a>ソリューションの構築: Azure Stack 向けのハイブリッド クラウド設計パターン

Microsoft Azure Stack は Azure の拡張機能です。 Azure Stack は、ハイブリッド クラウドの有効化により、オンプレミス環境とエッジにクラウド コンピューティングの機敏性を提供します。 Azure でハイブリッド クラウド アプリを構築し、接続されている/いないを問わず、あらゆる場所にあるデータセンターにデプロイすることができます。

Microsoft Azure は一貫性のあるハイブリッド クラウドです。 Azure では、Azure で開発されたコードを再利用したり、アプリを Azure のソブリン クラウドと Azure Stack にデプロイしたりできます。 クラウドをまたぐアプリケーションも、*ハイブリッド アプリケーション*と呼ばれます。

ハイブリッド シナリオは、開発で使用できるリソースによって大きく異なり、地理、セキュリティ、インターネット アクセスなど、考慮事項は多岐にわたります。 このようなシナリオは特定の要件に対処できない場合がありますが、ハイブリッド ソリューションの実装に関する主要なガイドラインと例は提供できます。

## <a name="hybrid-cloud-patterns"></a>ハイブリッド クラウド パターン

- [クラウド間スケーリング パターン](azure-stack-edge-pattern-cross-cloud-scaling.md)
- [地理的に分散されたパターン](azure-stack-edge-pattern-geo-distribution.md)
- [DevOps パターン](azure-stack-edge-pattern-hybrid-ci-cd.md)

## <a name="step-by-step-tutorials"></a>ステップバイステップのチュートリアル

- [Azure と Azure Stack の両方でアプリをデプロイする](azure-stack-solution-pipeline.md)
- [Azure Stack と Azure にアプリをデプロイする](azure-stack-solution-hybrid-identity.md)
- [Azure と Azure Stack のアプリでハイブリッド クラウド ID を構成する](azure-stack-solution-hybrid-connectivity.md)
- [Azure と Azure Stack でハイブリッド クラウド接続を構成する](azure-stack-solution-staged-data-analytics.md)
- [Azure と Azure Stack を使用してステージング データ分析ソリューションを作成する](azure-stack-solution-staged-data.md)
- [Azure でクラウド間スケーリング ソリューションを作成する](azure-stack-solution-cloud-burst.md)
- [Azure と Azure Stack を使用して地理的に分散されたアプリ ソリューションを作成する](azure-stack-solution-geo-distributed.md)
- [Azure と Azure Stack を使用してハイブリッド クラウド ソリューションをデプロイする](azure-stack-solution-hybrid-cloud.md)
- [Azure と Azure Stack で MongoDB をデプロイする](azure-stack-solution-mongodb-ha.md)
- [Azure と Azure Stack で SQL Server 2016 をデプロイする](azure-stack-solution-sql-ha.md)


## <a name="next-steps"></a>次の手順

- [ハイブリッド アプリケーションのための設計の考慮事項](azure-stack-edge-pattern-overview.md)に関する記事で、ハイブリッド アプリケーションを設計、デプロイ、および運用するためのソフトウェア品質の重要な要素を確認します。
- [Azure Stack で開発環境を設定](azure-stack-dev-start.md)し、Azure Stack で[最初のアプリをデプロイ](azure-stack-dev-start-deploy-app.md)します。
