---
title: Azure Stack Hub の診断ログの収集の概要
description: ログのオンデマンドおよび自動収集など、Azure Stack Hub のヘルプとサポートの診断ログの収集について説明します。
author: justinha
ms.topic: article
ms.date: 11/07/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 11/07/2019
ms.openlocfilehash: 05d54d3db2429faa410cc67a46fba234d285a1af
ms.sourcegitcommit: 4ac711ec37c6653c71b126d09c1f93ec4215a489
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77700053"
---
# <a name="overview-of-azure-stack-hub-diagnostic-log-collection"></a>Azure Stack Hub の診断ログの収集の概要 

Azure Stack Hub は、相互に連携して作用する複数のコンポーネントの大規模なコレクションです。 これらすべてのコンポーネントでは、独自の一意のログが生成されます。 このため、特に相互に作用する複数の Azure Stack Hub コンポーネントからのエラーで、問題の診断が困難になることがあります。 この課題に対処するために、診断ログの収集エクスペリエンスを設計しました。 

1907 より前では、[Test-AzureStack](azure-stack-diagnostic-test.md) を使用して、システム正常性を検証し、[特権エンドポイント (PEP) ](azure-stack-configure-on-demand-diagnostic-log-collection.md#use-the-privileged-endpoint-pep-to-collect-diagnostic-logs) を使用して、トラブルシューティングのログを収集する診断エクスペリエンスが含まれていました。 

1907 リリース以降、 **[ヘルプとサポート]** ページに、**診断ログの収集**を使用した簡単なエクスペリエンスが追加されています。 
**診断ログの収集**は、Azure Stack Hub のオペレーターのトラブルシューティングの一連の作業のエクスペリエンスを向上するために継続して行う投資の一部です。 これらの向上により、オペレーターは診断ログを迅速に収集し、Microsoft カスタマーサポートサービス (CSS) と共有することができます。 ログは、Azure の BLOB コンテナーに格納でき、そこで必要に応じてアクセスをカスタマイズできます。    
   
**診断ログの収集**は、次の 2 つの方法で動作します。

- **自動収集**:有効にした (推奨) 場合、ログ収集は特定の正常性アラートによって自動的にトリガーされ、Azure ストレージ アカウントに格納されます
- **Collect logs now (今すぐログを収集する)** :これは、オンデマンドのオプションで、過去 7 日間の 1 時間から 4 時間のスライディング ウィンドウからログを収集することを選択できます

![診断ログ収集オプションのスクリーンショット](media/azure-stack-automatic-log-collection/azure-stack-log-collection-overview.png)

**診断ログの収集**には簡単なユーザー インターフェイスがあり、PowerShell は必要ありません。 インフラストラクチャ サービスが停止している場合でも、ログは確実に収集されます。
ポリシーで CSS との診断ログの共有が許されている場合、**診断ログの収集**は、1907 リリース以降の推奨される収集方法です。 ヘルプとサポートで**診断ログの収集**を使用できない場合にのみ、[PEP](azure-stack-configure-on-demand-diagnostic-log-collection.md#use-the-privileged-endpoint-pep-to-collect-diagnostic-logs) を使用してログを収集する必要があります。

## <a name="automatic-diagnostic-log-collection"></a>自動診断ログ収集 

[特定の正常性アラート](azure-stack-configure-automatic-diagnostic-log-collection.md#automatic-diagnostic-log-collection-alerts)が発生すると、診断ログの自動収集が開始され、Azure Stack Hub から Azure のストレージ BLOB に先を見越して診断ログがアップロードされます。これにより、診断ログを CSS と共有する時間が大幅に短縮されます。 診断ログは、アラートが発生した場合にのみ収集されます。  

ログの自動収集の詳細については、「[自動 Azure Stack Hub 診断ログ収集の構成](azure-stack-configure-automatic-diagnostic-log-collection.md)」を参照してください。

## <a name="on-demand-diagnostic-log-collection"></a>オンデマンド診断ログ収集

オンデマンドの収集では、Azure Stack Hub のオペレーターが収集を手動でトリガーしたときに、診断ログが Azure Stack Hub から Azure のストレージ BLOB にアップロードされます。
CSS は、CSS が所有しているストレージ BLOB への Shared Access Signature (SAS) URL を提供します。 Azure Stack Hub のオペレーターは **[Collect logs now]** \(今すぐログを収集する\) をクリックして SAS URL を入力できます。 これにより、診断ログは、中間共有を必要とせずに、CSS の BLOB に直接アップロードされます。 

オンデマンドでのログ収集の詳細については、「[オンデマンドで Azure Stack Hub 診断ログを収集する](azure-stack-configure-on-demand-diagnostic-log-collection.md)」を参照してください。

## <a name="bandwidth-considerations"></a>帯域幅に関する考慮事項

診断ログの収集の平均サイズは、オンデマンドで実行するか自動で実行するかによって異なります。 自動のログ収集の平均サイズは約 2 GB ですが、オンデマンドのログ収集のサイズは、収集される時間数によって異なります。 

次の表に、Azure への制限付きまたは従量制課金接続の環境の場合の考慮事項を示します。

| ネットワーク接続 | 影響 |
|--------------------|--------|
| 低帯域幅/待機時間が長い接続 | ログのアップロードは、完了までに長時間かかります | 
| 共有接続 | アップロードは、ネットワーク接続を共有する他のアプリケーションやユーザーに影響を与える場合があります |
| 従量制課金接続 | 追加のネットワーク使用に対して、ISP からの追加料金が発生する場合があります |

詳細については、「[自動 Azure Stack Hub ログ収集のベスト プラクティス](azure-stack-best-practices-automatic-diagnostic-log-collection.md)」を参照してください。

## <a name="see-also"></a>関連項目

[Azure Stack Hub でのログおよび顧客データの処理](https://docs.microsoft.com/azure-stack/operator/azure-stack-data-collection)

[Shared Access Signatures (SAS) の使用](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)

[自動 Azure Stack Hub ログ収集のベスト プラクティス](azure-stack-best-practices-automatic-diagnostic-log-collection.md)
