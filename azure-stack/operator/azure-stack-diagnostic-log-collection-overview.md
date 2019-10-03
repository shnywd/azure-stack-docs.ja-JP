---
title: Azure Stack 診断ログの収集の概要 |Microsoft Docs
description: オンデマンドおよび自動ログ収集など、Azure Stack のヘルプとサポートの診断ログ収集について説明します。
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 09/23/2019
ms.openlocfilehash: cd65f51867a4626e8f9b288c5113909bfa32b1c2
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829206"
---
# <a name="overview-of-azure-stack-diagnostic-log-collection"></a>Azure Stack 診断ログの収集の概要 

*適用対象:Azure Stack 統合システム*

Azure Stack は、相互に連携して作用する複数のコンポーネントによって構成された大規模なコレクションです。 これらすべてのコンポーネントでは、独自の一意のログが生成されます。 このため、問題を診断するのは困難な作業になります。相互に作用する複数の Azure Stack コンポーネントのエラーについては、特にそう言えます。 この課題に対処するために、診断ログの収集エクスペリエンスを設計しました。 

1907 より前では、[Test-AzureStack](azure-stack-diagnostic-test.md) を使用して、システム正常性を検証し、[特権エンドポイント (PEP) ](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep-to-collect-diagnostic-logs) を使用して、トラブルシューティングのログを収集する診断エクスペリエンスが含まれていました。 

1907 リリース以降、 **[ヘルプとサポート]** ページに、**診断ログの収集**を使用した簡単なエクスペリエンスが追加されています。 
**診断ログ**の収集は、トラブルシューティング プロセスで Azure Stack オペレーターのエクスペリエンスを向上させるための継続的な投資の一部です。 これらの向上により、オペレーターは診断ログを迅速に収集し、Microsoft カスタマーサポートサービス (CSS) と共有することができます。 ログは、Azure の BLOB コンテナーに格納でき、そこで必要に応じてアクセスをカスタマイズできます。    
   
**診断ログの収集**は、次の 2 つの方法で動作します。

- **自動収集**:有効にした (推奨) 場合、ログ収集は特定の正常性アラートによって自動的にトリガーされ、Azure ストレージ アカウントに格納されます
- **Collect logs now (今すぐログを収集する)** :これは、オンデマンドのオプションで、過去 7 日間の 1 時間から 4 時間のスライディング ウィンドウからログを収集することを選択できます

![診断ログ収集オプションのスクリーンショット](media/azure-stack-automatic-log-collection/azure-stack-log-collection-overview.png)

**診断ログの収集**には簡単なユーザー インターフェイスがあり、PowerShell は必要ありません。 インフラストラクチャ サービスが停止している場合でも、ログは確実に収集されます。
ポリシーで CSS との診断ログの共有が許されている場合、**診断ログの収集**は、1907 リリース以降の推奨される収集方法です。 ヘルプとサポートで**診断ログの収集**を使用できない場合にのみ、[PEP](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep-to-collect-diagnostic-logs) を使用してログを収集する必要があります。

## <a name="automatic-diagnostic-log-collection"></a>自動診断ログ収集 

[特定の正常性アラート](azure-stack-configure-automatic-diagnostic-log-collection.md#automatic-diagnostic-log-collection-alerts)が発生したときに、自動診断ログ収集が開始され、Azure Stack から Azure のストレージ BLOB に診断ログが事前にアップロードされるため、診断ログを CSS と共有するために必要な時間が大幅に短縮されます。 診断ログは、アラートが発生した場合にのみ収集されます。  

自動ログの収集の詳細については、「[Configure automatic Azure Stack diagnostic log collection](azure-stack-configure-automatic-diagnostic-log-collection.md)」(自動 Azure Stack 診断ログの収集の構成) を参照してください。

## <a name="on-demand-diagnostic-log-collection"></a>オンデマンド診断ログ収集

オンデマンドの収集では、Azure Stack オペレーターが手動で収集をトリガーしたときに、診断ログが Azure Stack から Azure のストレージ BLOB にアップロードされます。
CSS は、CSS が所有しているストレージ BLOB への Shared Access Signature (SAS) URL を提供します。 Azure Stack オペレーターは **[Collect logs now]\(今すぐログを収集する\)** をクリックして、SAS URL を入力できます。 これにより、診断ログは、中間共有を必要とせずに、CSS の BLOB に直接アップロードされます。 

オンデマンドでログを収集する詳細については、[今すぐ Azure Stack 診断ログを収集する](azure-stack-configure-on-demand-diagnostic-log-collection.md)方法に関するページを参照してください。

## <a name="bandwidth-considerations"></a>帯域幅に関する考慮事項

診断ログの収集の平均サイズは、オンデマンドで実行するか自動で実行するかによって異なります。 自動のログ収集の平均サイズは約 2 GB ですが、オンデマンドのログ収集のサイズは、収集される時間数によって異なります。 

次の表に、Azure への制限付きまたは従量制課金接続の環境の場合の考慮事項を示します。

| ネットワーク接続 | 影響 |
|--------------------|--------|
| 低帯域幅/待機時間が長い接続 | ログのアップロードは、完了までに長時間かかります | 
| 共有接続 | アップロードは、ネットワーク接続を共有する他のアプリケーションやユーザーに影響を与える場合があります |
| 従量制課金接続 | 追加のネットワーク使用に対して、ISP からの追加料金が発生する場合があります |

詳細については、「[Best practices for automatic Azure Stack log collection](azure-stack-best-practices-automatic-diagnostic-log-collection.md)」(自動 Azure Stack ログ収集のベストプラクティス) を参照してください。

## <a name="see-also"></a>関連項目

[Azure Stack のログおよび顧客データの処理](https://docs.microsoft.com/azure-stack/operator/azure-stack-data-collection)

[Shared Access Signatures (SAS) の使用](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)

[自動 Azure Stack ログ収集のベストプラクティス](azure-stack-best-practices-automatic-diagnostic-log-collection.md)
