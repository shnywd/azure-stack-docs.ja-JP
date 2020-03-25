---
title: Azure Stack Hub の診断ログの収集の概要
description: ログのオンデマンドおよび事前収集など、Azure Stack Hub の [ヘルプとサポート] の診断ログの収集について説明します。
author: justinha
ms.topic: article
ms.date: 02/26/2020
ms.author: justinha
ms.reviewer: shisab
ms.lastreviewed: 02/26/2020
ms.openlocfilehash: 8f97ecd20e7ef8db69033268baf96060e1315751
ms.sourcegitcommit: 53efd12bf453378b6a4224949b60d6e90003063b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/18/2020
ms.locfileid: "79520634"
---
# <a name="overview-of-azure-stack-hub-diagnostic-log-collection"></a>Azure Stack Hub の診断ログの収集の概要 

Azure Stack Hub は、相互にやり取りする Windows コンポーネントとオンプレミスの Azure サービスからなる大規模なコレクションです。 これらのすべてのコンポーネントとサービスは、独自のログ セットを生成します。 Microsoft カスタマーサポート サービス (CSS) が問題を効率的に診断できるように、診断ログ収集のシームレスなエクスペリエンスが提供されています。 

[ヘルプとサポート] の診断ログ収集によって、オペレーターは診断ログを迅速に収集し、Microsoft カスタマー サポート サービス (CSS) と共有できます。これは PowerShell を必要としない簡単なユーザーインターフェイスです。 他のインフラストラクチャ サービスが停止している場合でも、ログは収集されます。  
 
このログ収集方法を使用し、管理者ポータルまたは [ヘルプとサポート] ブレードが使用できない場合のみ [特権エンドポイント (PEP)](azure-stack-get-azurestacklog.md) を使用することをお勧めします。 

>[!NOTE]
>診断ログ収集を使用するには、Azure Stack Hub が登録済みで、インターネットに接続できる必要があります。 Azure Stack Hub が登録済みでない場合は、[特権エンドポイント (PEP)](azure-stack-get-azurestacklog.md) を使用してログを共有します。 

![診断ログ収集オプションのスクリーンショット](media/azure-stack-help-and-support/banner-enable-automatic-log-collection.png)

## <a name="collection-options-and-data-handling"></a>収集オプションとデータの処理

診断ログ収集機能には、ログを送信するための 2 つのオプションが用意されています。 次の表では、各オプションと、各ケースでのデータの処理方法について説明します。 

### <a name="send-logs-proactively"></a>ログを事前に送信する

[事前ログ収集](azure-stack-configure-automatic-diagnostic-log-collection-tzl.md)では、お客様がサポート ケースを開く前に Microsoft にログを送信できるように、診断ログの収集が合理化および簡素化されます。 診断ログは、分析のために Azure Stack Hub から事前にアップロードされます。 これらのログは、[システム正常性アラート](azure-stack-configure-automatic-diagnostic-log-collection-tzl.md#proactive-diagnostic-log-collection-alerts)が発生した場合にのみ収集され、サポート ケースのコンテキストで CSS によってのみアクセスされます。


#### <a name="how-the-data-is-handled"></a>データの処理方法

お客様は、Azure Stack Hub のシステム正常性アラートのみに基づいて、Microsoft による定期的な自動ログ収集に同意します。 また、Microsoft によって管理および制御されている Azure ストレージ アカウントに、これらのログをアップロードして保持することを承認し、同意します。 

このデータはシステム正常性アラートをトラブルシューティングする目的でのみ使用され、お客様の同意なしに、マーケティング、広告、その他の商用目的で使用されることはありません。 このデータは最大 90 日間保持でき、Microsoft が収集したすべてのデータは、[標準的なプライバシーに関する声明](https://privacy.microsoft.com/)に従って処理されます。

お客様の同意を得て以前に収集したすべてのデータは、お客様の許可の取り消しによる影響を受けません。

**事前ログ収集**を使用して収集されたログは、Microsoft によって管理および制御されている Azure ストレージ アカウントにアップロードされます。 これらのログは、Azure Stack Hub の正常性を向上させるために、サポート ケースのコンテキストで Microsoft によってアクセスされる場合があります。

### <a name="send-logs-now"></a>今すぐログを送信する

[[Send logs now]\(今すぐログを送信する\)](azure-stack-configure-on-demand-diagnostic-log-collection-portal-tzl.md) は、お客様が通常はサポートケースを開く前に収集を開始したときのみ、診断ログが Azure Stack Hub からアップロードされる手動オプションです。 

Azure Stack オペレーターは、管理者ポータルまたは PowerShell を使用して、Microsoft カスタマー サポート サービス (CSS) に診断ログをオンデマンドで送信することができます。 Azure Stack Hub が Azure に接続されている場合は、[管理者ポータルの [Send logs now]\(今すぐログを送信する\)](azure-stack-configure-on-demand-diagnostic-log-collection-portal-tzl.md) の使用をお勧めします。これは、この方法がログを Microsoft に直接送信する最も簡単な方法であるためです。 ポータルが使用できない場合、オペレーターは代わりに [PowerShell を使用してログをすぐに送信する](azure-stack-configure-on-demand-diagnostic-log-collection-powershell-tzl.md)必要があります。 

インターネットに接続していない場合、またはローカルでのみログを保存する場合は、[Get-AzureStackLog](azure-stack-get-azurestacklog.md) メソッドを使用してログを送信します。 次のフローチャートは、それぞれの場合に診断ログを送信するために使用するオプションを示しています。 

![Microsoft にログを今すぐ送信する方法を示すフローチャート](media/azure-stack-help-and-support/send-logs-now-flowchart.png)

#### <a name="how-the-data-is-handled"></a>データの処理方法

Azure Stack Hub から診断ログ収集を開始することで、これらのログをアップロードし、Microsoft によって管理および制御されている Azure ストレージア カウントに保持することを承認し、同意します。 Microsoft CSS は、ログ収集のために顧客とやり取りすることなく、サポート ケースを使用して、これらのログにすぐにアクセスできます。 

このデータはシステム正常性アラートをトラブルシューティングする目的でのみ使用され、お客様の同意なしに、マーケティング、広告、その他の商用目的で使用されることはありません。 このデータは最大 90 日間保持でき、Microsoft が収集したすべてのデータは、[標準的なプライバシーに関する声明](https://privacy.microsoft.com/)に従って処理されます。 

[Send logs now]\(今すぐログを送信する\) を使用して収集されたログは、Microsoft が管理および制御するストレージにアップロードされます。 これらのログは、Azure Stack Hub の正常性を向上させるために、サポート ケースのコンテキストで Microsoft によってアクセスされます。 

## <a name="bandwidth-considerations"></a>帯域幅に関する考慮事項

診断ログの収集の平均サイズは、事前実行するか手動で実行するかによって異なります。 **事前ログ収集** の平均サイズは約 2 GB です。 **[Send logs now]\(今すぐログを送信する\)** の収集サイズは、収集される時間の長さによって異なります。

次の表に、Azure への制限付きまたは従量制課金接続の環境の場合の考慮事項を示します。


| ネットワーク接続 | 影響 |
|--------------------|--------|
| 低帯域幅/待機時間が長い接続 | ログのアップロードは、完了までに長時間かかります | 
| 共有接続 | アップロードは、ネットワーク接続を共有する他のアプリケーションやユーザーに影響を与える場合があります |
| 従量制課金接続 | 追加のネットワーク使用に対して、ISP からの追加料金が発生する場合があります | 

## <a name="see-also"></a>関連項目

[Azure Stack Hub でのログおよび顧客データの処理](https://docs.microsoft.com/azure-stack/operator/azure-stack-data-collection)

