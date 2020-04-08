---
title: Azure Stack Hub 上の Event Hubs の概要
description: Azure Stack Hub の Event Hubs リソースプロバイダーについて説明します。
author: BryanLa
ms.author: bryanla
ms.service: azure-stack
ms.topic: how-to
ms.date: 11/27/2019
ms.reviewer: jfggdl
ms.lastreviewed: 11/27/2019
ms.openlocfilehash: b0a4e18afdc8ce68ffa212bcc7bc78506420c235
ms.sourcegitcommit: dd53af1b0fc2390de162d41e3d59545d1baad1a7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/31/2020
ms.locfileid: "80424741"
---
# <a name="event-hubs-on-azure-stack-hub-overview"></a>Azure Stack Hub 上の Event Hubs の概要

[!INCLUDE [preview-banner](../includes/event-hubs-preview.md)]

Azure Stack Hub 上の Event Hubs を使用すると、ハイブリッド クラウド シナリオを実現できます。 オンプレミスと Azure クラウドの両方の処理で、ストリーミングやイベントベースのソリューションがサポートされます。 ハイブリッド (接続型) シナリオであれ非接続型のシナリオであれ、大規模なイベント処理またはストリーム処理に対応したソリューションを実現できます。 シナリオは Event Hubs クラスターによってのみ制限され、その点は、必要に応じてプロビジョニングすることができます。 

## <a name="features"></a>特徴 

(Azure Stack Hub 上および Azure 上の) Event Hubs エディションには、高度な機能パリティが実現されています。 ここでいうパリティとは、SDK、サンプル、PowerShell、CLI、ポータルからいずれも同様のエクスペリエンスが得られ、違いがほとんどないということです。 次の表は、各エディションで利用できる機能の相違を簡単にまとめたものです。  

| 機能 | Azure Stack Hub 上の Event Hubs | Azure Event Hubs |
|-|-|-|-|
| オペレーター管理者エクスペリエンス | ✔ | ✘ |
| Kafka サポート | ✔ | ✔ |
| 同じ SDK のセット | ✔ | ✔ |
| Azure Active Directory を使用した Event Hubs へのアクセス承認 | ✘ | ✔ |
| キャプチャ機能 | ✘ | ✔ |
| geo ディザスター リカバリー | ✘ | ✔ |
| Azure Monitor | ✔ | ✔ |
| 自動インフレ機能 | ✘ | ✔ |

Azure Resource Management の操作は、Azure Resource Manager テンプレート、[PowerShell](/powershell/module/azurerm.eventhub/)、[Azure CLI](/cli/azure/eventhubs/eventhub/) を使用して行うこともできます。 PowerShell および Azure CLI では現在、Operator Administration の操作はサポートされません。

## <a name="feature-documentation"></a>機能のドキュメント

[Azure Event Hubs のドキュメント](/azure/event-hubs/)は、Azure Stack Hub 上の Event Hubs と Azure Event Hubs の両方のエディションの Event Hubs を対象としています。 このドキュメントでは、Event Hubs の使用とアクティビティに関して、次のようなトピックが取り上げられています。

- [Event Hubs の概念](/azure/event-hubs/event-hubs-features)に関する詳細情報
- [Event Hubs クラスターと名前空間を作成](event-hubs-quickstart-cluster-portal.md)する方法
- [イベント ハブ](/azure/event-hubs/event-hubs-create#create-an-event-hub)の作成方法
- [Kafka プロトコルを使用](/azure/event-hubs/event-hubs-quickstart-kafka-enabled-event-hubs)してストリーム配信する方法

### <a name="operator-documentation"></a>オペレーター ドキュメント 
 
Azure Stack Hub 上の Event Hubs のオペレーター エクスペリエンスについて詳しくは、[Event Hubs のオペレーター ドキュメント](/azure-stack/operator/event-hubs-rp-overview)を参照してください。 このドキュメントでは、次のようなアクティビティについての情報を取り上げています。

- Event Hubs をインストールする。
- Event Hubs をユーザーが利用できるようにする。
- サービスの正常性に関する情報を取得する。
- ログを収集する。


## <a name="next-steps"></a>次のステップ

ご利用のサブスクリプションで Event Hubs を利用できない場合は、管理者と協力して [Azure Stack Hub リソースプロバイダーに Event Hubs をインストール](../operator/event-hubs-rp-overview.md)してください。

Event Hubs をインストール済みで、使用する準備が整っている場合は、[Event Hubs のドキュメント](/azure/event-hubs/event-hubs-about)を参照して、サービスの詳細を確認してください。
