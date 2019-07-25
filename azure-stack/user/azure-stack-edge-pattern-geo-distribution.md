---
title: Azure Stack を使用したインテリジェント エッジの DevOps パターン | Microsoft Docs
description: Azure Stack を使用したインテリジェント エッジの DevOps パターンについて説明します。
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
ms.date: 07/11/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/11/2019
ms.openlocfilehash: 07e0c3f0ca52c7079b879050ab36bf6a5f166f92
ms.sourcegitcommit: 51ec68b5e6dbf437aaca19a9f35ba07d2c402892
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856297"
---
# <a name="geo-distributed-pattern"></a>地理的に分散されたパターン

複数のリージョンにわたってアプリのエンドポイントを提供します。 場所とコンプライアンスのニーズに基づいてユーザー トラフィックをルーティングします。

## <a name="context-and-problem"></a>コンテキストと問題

広い地理的範囲にまたがる組織は、必要なレベルのセキュリティ、コンプライアンス、およびパフォーマンスを、境界を越えてユーザー、場所、およびデバイスごとに確保しながら、データへのアクセスを安全かつ正確に分散させて実現するために努力します。

## <a name="solution"></a>解決策

Azure Stack の地理的トラフィック ルーティング パターン (Geo-Distributed Apps) では、さまざまなメトリックに基づいてトラフィックを特定のエンドポイントに振り向けることができます。 地理的ベースのルーティングとエンドポイント構成を使用して Traffic Manager を作成すると、リージョンの要件、企業および国際的な規制、およびデータ ニーズに基づいてトラフィックをエンドポイントにルーティングできます。

![地理的に分散されたパターン](media/azure-stack-edge-pattern-geo-distribution/geo-distribution.png)

**Traffic Manager**  
図では、これはパブリック クラウドの外側にありますが、ローカル データ センターとパブリック クラウドの両方でトラフィックを調整できる必要があります。 バランサーは地理的な場所にトラフィックをルーティングします。

**ドメイン ネーム システム (DNS)**  
ドメイン ネーム システム (DNS) は、Web サイトまたはサービスの名前をその IP アドレスに変換する (または解決する) 役割を担います。

### <a name="public-cloud"></a>パブリック クラウド

**クラウド エンドポイント**  
パブリック IP アドレスは、受信トラフィックをトラフィック マネージャー経由でパブリック クラウド アプリケーションのリソース エンドポイントにルーティングするために使用されます。  

### <a name="local-clouds"></a>ローカル クラウド

**ローカル エンドポイント**  
パブリック IP アドレスは、受信トラフィックをトラフィック マネージャー経由でパブリック クラウド アプリケーションのリソース エンドポイントにルーティングするために使用されます。

## <a name="issues-and-considerations"></a>問題と注意事項

このパターンの実装方法を決めるときには、以下の点に注意してください。

### <a name="scalability-considerations"></a>スケーラビリティに関する考慮事項

このパターンでは、トラフィックの増加に合わせてスケールするのではなく、地理的なトラフィック ルーティングを処理します。 ただし、このパターンを他の Azure およびオンプレミスのソリューションと組み合わせることができます。 たとえば、このパターンはクラウド間スケーリング パターンと一緒に使用できます。

### <a name="availability-considerations"></a>可用性に関する考慮事項

オンプレミス ハードウェア構成およびソフトウェア デプロイを通じて高可用性をもたらすように、ローカルでデプロイされたアプリが構成されていることを確認します。

### <a name="manageability-considerations"></a>管理容易性に関する考慮事項

パターンは、複数の環境にわたるシームレスな管理と使い慣れたインターフェイスを実現します。

## <a name="when-to-use-this-pattern"></a>このパターンを使用する状況

-   組織には、リージョンのセキュリティおよび分散に関するカスタム ポリシーが必要な支店が世界中にあります。

-   組織の支店それぞれは、従業員、ビジネス、および施設のデータをプルしますが、これには地域の規制やタイム ゾーンに従ったレポート アクティビティが必要になります。

-   高スケール要件を満たすには、極端に負荷の大きい要件に対応できるように、単一のリージョン内、または複数のリージョンにわたって、複数のアプリ デプロイメントを使用してアプリを水平方向に拡張する必要があります。

-   1 つのリージョンで障害が発生した場合でも、アプリケーションは高可用性とクライアント要求への応答性を備えている必要があります。

## <a name="example"></a>例

地理的分散アプリ パターンを使用して、さまざまなメトリックに基づき、特定のエンドポイントにトラフィックを送信する方法について説明します。 地理的ベースのルーティングとエンドポイント構成で Traffic Manager プロファイルを作成すると、リージョンの要件、企業および国際的な規制、およびデータ ニーズに基づいて、情報がエンドポイントにルーティングされます。

[Azure と Azure Stack を使用して地理的に分散されたアプリ ソリューションを作成する](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-geo-distributed)

## <a name="next-steps"></a>次の手順

[Azure Stack 向けのハイブリッド クラウド設計パターン](azure-stack-edge-pattern-overview.md)について学ぶ
