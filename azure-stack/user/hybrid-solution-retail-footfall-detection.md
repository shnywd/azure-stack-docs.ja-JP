---
title: Azure および Azure Stack を使用して AI ベースの足取り検出パターンを実装するためのハイブリッド ソリューション
description: Azure および Azure Stack サービスを使用して AI ベースの足取り検出パターンを実装し、小売店内のトラフィックを分析する方法について説明します。
services: azure-stack
documentationcenter: ''
author: BryanLa
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 09/23/2019
ms.openlocfilehash: e87baeba1b961c134fc5754fda8c9f1688f3fe90
ms.sourcegitcommit: f38903cb925276e96dd62ebe8ffe92df8bf6e5c8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256554"
---
# <a name="footfall-detection-solution"></a>足取り検出ソリューション

この記事では、AI ベースの足取り検出パターンを実装するためのソリューションについて概説します。 このパターンは、小売店内での買い物客のトラフィックを分析する際に役立ちます。 このソリューションでは、Azure、Azure Stack、Custom Vision AI Dev Kit を使用して、実際の行動から分析情報を生成します。

## <a name="context-and-problem"></a>コンテキストと問題

Contoso Stores では、店舗のレイアウトに関連して、顧客が現在の製品をどのように受け取っているかについて分析情報を得たいと考えています。 すべてのセクションにスタッフを 1 人ずつ配置することはできません。また、カメラ撮影した店舗内の映像すべてをアナリスト チームに確認してもらうのも非効率です。 さらに、分析用の映像をすべてのカメラからクラウドにストリーミングできるだけの十分な帯域幅は、どの店舗にもありません。 

Contoso では、顧客の人口統計、ロイヤルティ、店舗のディスプレイと製品に対する反応を確認するための、邪魔にならずプライバシーに配慮した方法を見つけたいと考えています。

## <a name="solution"></a>解決策

この小売分析ソリューションでは、階層型アプローチを使用してエッジで推論を行います。 Custom Vision AI Dev Kit を使用することで、人の顔が含まれる画像のみが、分析のためにプライベート Azure Stack に送信されます。Azure Stack では、Azure Cognitive Services が実行されています。 集計された匿名データが Azure に送信され、すべての店舗にわたって集計されて Power BI で視覚化されます。 エッジとパブリック クラウドを組み合わせることで、Contoso では最新の AI テクノロジを活用できるようになります。 同時に、企業ポリシーへの準拠状態を維持し、顧客のプライバシーを尊重することができます。

[![足取り検出パターン](media/hybrid-pattern-ai-footfall-detection/solution-architecture.png)](media/hybrid-pattern-ai-footfall-detection/solution-architecture.png)

このソリューションのしくみの概要を以下に示します。 

1. Custom Vision AI Dev Kit は IoT Hub から構成を取得します。これにより IoT Edge ランタイムと ML モデルがインストールされます。
2. モデルが人を認識した場合、写真が撮影され、Azure Stack BLOB ストレージにアップロードされます。 
3. Blob service によって Azure Stack で Azure 関数がトリガーされます。 
4. Azure 関数は、Face API を含むコンテナーを呼び出し、画像から人口統計データと感情データを取得します。
5. このデータは匿名化され、Azure Event Hubs に送信されます。
6. Event Hubs により、データは Stream Analytics にプッシュされます。
7. Stream Analytics によってデータが集計され、Power BI にプッシュされます。

## <a name="components"></a>コンポーネント

このソリューションでは、次のコンポーネントを使用します。

| レイヤー | コンポーネント | 説明 |
|----------|-----------|-------------|
| 店舗内のハードウェア | [Custom Vision AI Dev Kit](https://azure.github.io/Vision-AI-DevKit-Pages/) | 分析用に人の画像のみをキャプチャするローカル ML モデルを使用して、ストア内でフィルター処理を行います。 IoT Hub によって安全にプロビジョニングと更新が行われます。<br><br>|
| Azure Stack | [App Service](../operator/azure-stack-app-service-overview.md) | App Service リソース プロバイダー (RP) は、エッジ コンポーネントの基盤となります。 Web アプリ/API と Functions のためのホスティング機能および管理機能が含まれます。 |
| | [Azure Kubernetes Service (AKS) エンジン](https://github.com/Azure/aks-engine) クラスター | AKS RP では AKS エンジンが Azure Stack にデプロイされているため、Face API コンテナーを実行するためのスケーラブルで回復力のあるエンジンが提供されます。 |
| | Azure Cognitive Services の [Face API コンテナー](/azure/cognitive-services/face/face-how-to-install-containers)| Face API コンテナーを備えた Azure Cognitive Services RP により、Contoso のプライベート ネットワーク上で人口統計、感情、および買い物客の一意検出が実現します。 |
| | Blob Storage | キャプチャされた画像は、AI Dev Kit から Azure Stack の BLOB ストレージにアップロードされます。 |
| | Azure Functions | Azure Stack で実行されている Azure Functions は、BLOB ストレージからの入力を受け取り、Face API とのやり取りを管理します。 次に、匿名データを、Azure にある Event Hubs に出力します。<br><br>|
| Azure |  |  |
|  | [Azure Event Hubs](/azure/event-hubs/) | Azure Event Hubs は、Azure Stream Analytics と密接に統合された、匿名データを取り込むためのスケーラブルなプラットフォームを提供します。 |
|  | [Azure Stream Analytics](/azure/stream-analytics/) | Azure Stream Analytics ジョブは、匿名データを集計し、これを視覚化できるように 15 秒間隔でグループ化します。 |
|  | [Microsoft Power BI](https://powerbi.microsoft.com/) | Power BI には、Azure Stream Analytics からの出力を表示するための、使いやすいダッシュボード インターフェイスが用意されています。 |

## <a name="issues-and-considerations"></a>問題と注意事項

このパターンの実装方法を決めるときには、以下の点に注意してください。

### <a name="scalability"></a>スケーラビリティ 

このソリューションを複数のカメラと場所にわたってスケーリングできるようにするには、増えた負荷をすべてのコンポーネントで処理できるようにする必要があります。 次のような措置を取る必要が生じる場合があります。

- Stream Analytics のストリーミング ユニット数を増やす
- Face API デプロイをスケールアウトする
- Event Hubs のスループットを増やす
- 極端なケースでは、Azure Functions から仮想マシンへの移行が必要になる場合があります。

### <a name="availability"></a>可用性

このソリューションは階層化されているため、ネットワークや電源の障害に対処する方法を検討しておく必要があります。 ビジネス ニーズに応じて、画像をローカルにキャッシュし、接続が回復したときに Azure Stack に転送するメカニズムを実装することが適切な場合があります。 場所に余裕がある場合は、Face API コンテナーを備えた Data Box Edge をその場所に配置する方が適切である可能性もあります。

### <a name="manageability"></a>管理容易性

このソリューションは多数のデバイスと場所にまたがることがあるため、扱いにくくなる可能性があります。 [Azure の IoT サービス](/azure/iot-fundamentals/)を使用することで、新しい場所とデバイスを自動的にオンラインにし、最新の状態に保つことができます。 

### <a name="security"></a>セキュリティ

このソリューションでは顧客の画像をキャプチャするため、セキュリティが最も重要な考慮事項となります。 すべてのストレージ アカウントが適切なアクセス ポリシーで保護されていること、またキーが定期的にローテーションされていることを確認します。 ストレージ アカウントと Event Hubs に、企業および政府のプライバシー規制に準拠したアイテム保持ポリシーが備わっていることを確認します。 また、システムにアクセスする必要があるすべてのユーザーが、そのロールに必要なデータにのみアクセスできるように、アクセス レベルを階層化します。

## <a name="next-steps"></a>次の手順

このソリューションで使用される関連パターンの詳細については、[階層化されたデータ パターン](azure-stack-solution-staged-data.md)に関するページをご覧ください。 

Custom Vision の使用方法の詳細については、[Custom Vision AI Dev Kit](https://azure.github.io/Vision-AI-DevKit-Pages/) に関するページをご覧ください。 