---
title: Azure と Azure Stack Hub を使用してハイブリッド リレー ソリューションを実装するためのパターン。
description: ファイアウォールで保護されているエッジ リソースまたはエッジ デバイスに接続するために、Azure および Azure Stack Hub サービスを使用する方法について説明します。
author: BryanLa
ms.topic: article
ms.date: 11/05/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/05/2019
ms.openlocfilehash: 7315013253c04ebbb231a0915dba610e459848e7
ms.sourcegitcommit: 959513ec9cbf9d41e757d6ab706939415bd10c38
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2020
ms.locfileid: "76890070"
---
# <a name="hybrid-relay-pattern"></a>ハイブリッド リレー パターン

Azure Service Bus Relay を使用して、ファイアウォールで保護されているエッジ リソースまたはエッジ デバイスに接続する方法について説明します。

## <a name="context-and-problem"></a>コンテキストと問題

多くの場合、エッジ デバイスは、企業のファイアウォールまたは NAT デバイスの背後にあります。 これらはセキュリティで保護されていますが、パブリック クラウド、または他の企業ネットワーク上のエッジ デバイスと通信できない可能性があります。 このため、パブリック クラウド内のユーザーに特定のポートおよび機能を安全な方法で公開することが必要になる場合があります。 

## <a name="solution"></a>解決策

ハイブリッド リレー パターンでは、Azure Service Bus Relay を使用して、直接通信できない 2 つのエンドポイント間に WebSocket トンネルを確立します。 オンプレミスではないデバイスをオンプレミスのエンドポイントに接続する必要がある場合、そのデバイスはパブリック クラウド内のエンドポイントに接続されます。 このエンドポイントは、セキュリティ保護されたチャネルを介して、事前定義されたルートでトラフィックをリダイレクトします。 オンプレミスの環境内にあるエンドポイントはトラフィックを受信し、それを正しい宛先にルーティングします。 

![ハイブリッド リレー ソリューションのアーキテクチャ](media/pattern-hybrid-relay/solution-architecture.png)

このソリューションのしくみを次に示します。 

1. デバイスが、事前定義されたポートで、Azure 内の仮想マシン (VM) に接続されます。
2. トラフィックが、Azure 内の Service Bus Relay に転送されます。
3. Azure Stack Hub 上の VM は、Service Bus Relay への長期間有効な接続を既に確立しており、トラフィックを受信して、それを宛先に転送します。
4. オンプレミスのサービスまたはエンドポイントがこの要求を処理します。 

## <a name="components"></a>Components

このソリューションでは、次のコンポーネントを使用します。

| レイヤー | コンポーネント | [説明] |
|----------|-----------|-------------|
| Azure | Azure VM | Azure VM は、オンプレミス リソースに対して、パブリックにアクセス可能なエンドポイントを提供します。 |
| | Azure Service Bus Relay | [Azure Service Bus Relay](/azure/service-bus-relay/) は、Azure VM と Azure Stack Hub VM 間のトンネルと接続を維持するためのインフラストラクチャを提供します。|
| Azure Stack Hub | Compute | Azure Stack Hub VM は、サーバー側のハイブリッド リレー トンネルを提供します。 |
| | ストレージ | Azure Stack Hub にデプロイされた AKS エンジン クラスターによって、Face API コンテナーを実行するためのスケーラブルで回復性があるエンジンが提供されます。|

## <a name="issues-and-considerations"></a>問題と注意事項

このソリューションの実装方法を決めるときには、以下の点を考慮してください。

### <a name="scalability"></a>スケーラビリティ 

このパターンでは、クライアントとサーバーで 1:1 のポート マッピングのみを使用できます。 たとえば、ポート 80 が Azure エンドポイント上のあるサービス用にトンネリングされている場合、このポートを別のサービスに使用することはできません。 ポート マッピングを適切に計画する必要があります。 また、トラフィックを処理できるように、Service Bus Relay と VM を適切にスケーリングする必要があります。

### <a name="availability"></a>可用性

これらのトンネルと接続は冗長ではありません。 高可用性を確保するために、エラー チェック コードを実装することをお勧めします。 もう 1 つの方法は、ロード バランサーの背後に、Service Bus Relay に接続された VM のプールを作成することです。

### <a name="manageability"></a>管理の容易性

このソリューションは多数のデバイスと場所にまたがることがあるため、扱いにくくなる可能性があります。 Azure の IoT サービスにより、新しい場所とデバイスを自動的にオンラインにし、最新の状態に保つことができます。

### <a name="security"></a>Security

図のように、このパターンでは、エッジから内部デバイス上のポートに自由にアクセスできます。 内部デバイス上のサービス、またはハイブリッド リレー エンドポイントの前面に、認証メカニズムを追加することを検討してください。 

## <a name="next-steps"></a>次のステップ

この記事で紹介したトピックの関連情報:
- このパターンでは、Azure Service Bus Relay を使用します。 詳細については、[Azure Service Bus Relay のドキュメント](/azure/service-bus-relay/)を参照してください。
- ベスト プラクティスの詳細とその他の疑問の回答を確認するには、「[ハイブリッド アプリの設計上の考慮事項](overview-app-design-considerations.md)」を参照してください。
- 製品とソリューションのポートフォリオ全体の詳細について、[Azure Stack ファミリの製品とソリューション](/azure-stack)を参照してください。

ソリューションの例をテストする準備ができたら、[ハイブリッド リレーのソリューション デプロイ ガイド](https://aka.ms/hybridrelaydeployment)に進んでください。 デプロイ ガイドでは、コンポーネントをデプロイしてテストするための詳細な手順について説明します。