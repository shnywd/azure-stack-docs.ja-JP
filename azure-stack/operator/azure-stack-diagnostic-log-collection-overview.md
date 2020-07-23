---
title: Azure Stack Hub での診断ログの収集
description: Azure Stack Hub の [ヘルプとサポート] における診断ログ収集について説明します。
author: justinha
ms.topic: article
ms.date: 02/26/2020
ms.author: justinha
ms.reviewer: shisab
ms.lastreviewed: 02/26/2020
ms.openlocfilehash: f7d9335e612387a780e002a2fe3d070436a10c5a
ms.sourcegitcommit: e9a1dfa871e525f1d6d2b355b4bbc9bae11720d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/20/2020
ms.locfileid: "86488979"
---
# <a name="diagnostic-log-collection-in-azure-stack-hub"></a>Azure Stack Hub での診断ログの収集

::: moniker range=">= azs-2002"

Azure Stack Hub は、相互にやり取りする Windows コンポーネントとオンプレミスの Azure サービスからなる大規模なコレクションです。 これらのすべてのコンポーネントとサービスは、独自のログ セットを生成します。 Microsoft サポートが問題を効率的に診断できるように、診断ログ収集のシームレスなエクスペリエンスが提供されています。

**[ヘルプとサポート]**   の診断ログ収集によって、オペレーターは診断ログを迅速に収集し、Microsoft サポートと共有できます。これは PowerShell を必要としない簡単なユーザー インターフェイスです。 他のインフラストラクチャ サービスが停止している場合でも、ログは収集されます。  

このログ収集方法を使用し、管理者ポータルまたは **[ヘルプとサポート]** ブレードが使用できない場合のみ[特権エンドポイント (PEP) を使用する](azure-stack-get-azurestacklog.md)ことをお勧めします。

>[!NOTE]
>診断ログ収集を使用するには、Azure Stack Hub が登録済みで、インターネットに接続できる必要があります。 Azure Stack Hub が登録済みでない場合は、[特権エンドポイント (PEP) を使用](azure-stack-get-azurestacklog.md)してログを共有します。

![Azure Stack Hub での診断ログ収集オプション](media/azure-stack-help-and-support/banner-enable-automatic-log-collection.png)

## <a name="collection-options-and-data-handling"></a>収集オプションとデータの処理

診断ログ収集機能には、ログを送信するための 2 つのオプションが用意されています。 次の表では、各オプションと、各ケースでのデータの処理方法について説明します。

### <a name="send-logs-proactively"></a>ログを事前に送信する

[事前ログ収集](./azure-stack-configure-automatic-diagnostic-log-collection.md?view=azs-2002)では、お客様がサポート ケースを開く前に Microsoft にログを送信できるように、診断ログの収集が合理化および簡素化されます。 診断ログは、分析のために Azure Stack Hub から事前にアップロードされます。 これらのログは、[システム正常性アラート](./azure-stack-configure-automatic-diagnostic-log-collection.md?view=azs-2002#proactive-diagnostic-log-collection-alerts)が発生した場合にのみ収集され、サポート ケースのコンテキストで Microsoft サポートによってのみアクセスされます。

#### <a name="how-the-data-is-handled"></a>データの処理方法

お客様は、Azure Stack Hub のシステム正常性アラートのみに基づいて、Microsoft による定期的な自動ログ収集に同意します。 また、Microsoft によって管理および制御されている Azure ストレージ アカウントに、これらのログをアップロードして保持することを承認し、同意します。

このデータはシステム正常性アラートをトラブルシューティングするためにのみ使用され、お客様の同意なしに、マーケティング、広告、その他の商業目的で使用されることはありません。 このデータは最大 90 日間保持でき、Microsoft が収集したすべてのデータは、[標準的なプライバシーに関する声明](https://privacy.microsoft.com/)に従って処理されます。

お客様の同意を得て以前に収集したすべてのデータは、お客様の許可の取り消しによる影響を受けません。

**事前ログ収集**を使用して収集されたログは、Microsoft によって管理および制御されている Azure ストレージ アカウントにアップロードされます。 これらのログは、サポート ケースのコンテキスト、および Azure Stack Hub の正常性を向上させるために、Microsoft によってアクセスされる場合があります。

### <a name="send-logs-now"></a>今すぐログを送信する

[[Send logs now]\(今すぐログを送信する\)](./azure-stack-configure-on-demand-diagnostic-log-collection-portal.md?view=azs-2002) は、お客様が通常はサポートケースを開く前に収集を開始したときのみ、診断ログが Azure Stack Hub からアップロードされる手動オプションです。

Azure Stack オペレーターは、管理者ポータルまたは PowerShell を使用し、Microsoft サポートに診断ログをオンデマンドで送信できます。 Azure Stack Hub が Azure に接続されている場合は、[管理者ポータルの [Send logs now]\(今すぐログを送信する\)](./azure-stack-configure-on-demand-diagnostic-log-collection-portal.md?view=azs-2002) の使用をお勧めします。これは、この方法がログを Microsoft に直接送信する最も簡単な方法であるためです。 ポータルが使用できない場合、オペレーターは代わりに [PowerShell を使用してログをすぐに送信する](./azure-stack-configure-on-demand-diagnostic-log-collection-powershell.md?view=azs-2002)必要があります。

インターネットに接続していない場合、またはローカルでのみログを保存する場合は、[Get-AzureStackLog](azure-stack-get-azurestacklog.md) メソッドを使用してログを送信します。 次のフローチャートは、それぞれの場合に診断ログを送信するために使用するオプションを示しています。

![Microsoft にログを今すぐ送信する方法を示すフローチャート](media/azure-stack-help-and-support/send-logs-now-flowchart.png)

#### <a name="how-the-data-is-handled"></a>データの処理方法

Azure Stack Hub から診断ログ収集を開始することで、これらのログをアップロードし、Microsoft によって管理および制御されている Azure ストレージア カウントに保持することを承認し、同意します。 Microsoft サポートは、ログ収集のために顧客とやり取りすることなく、サポート ケースを使用して、これらのログにすぐにアクセスできます。

このデータはシステム正常性アラートをトラブルシューティングするためにのみ使用され、お客様の同意なしに、マーケティング、広告、その他の商業目的で使用されることはありません。 このデータは最大 90 日間保持でき、Microsoft が収集したすべてのデータは、[標準的なプライバシーに関する声明](https://privacy.microsoft.com/)に従って処理されます。

**[Send logs now]\(今すぐログを送信する\)** を使用して収集されたログは、Microsoft が管理および制御するストレージにアップロードされます。 これらのログは、Azure Stack Hub の正常性を向上させるために、サポート ケースのコンテキストで Microsoft によってアクセスされます。

## <a name="bandwidth-considerations"></a>帯域幅に関する考慮事項

診断ログの収集の平均サイズは、事前実行するか手動で実行するかによって異なります。 **事前ログ収集** の平均サイズは約 2 GB です。 **[Send logs now]\(今すぐログを送信する\)** の収集サイズは、収集される時間の長さによって異なります。

次の表に、Azure への制限付きまたは従量制課金接続の環境の場合の考慮事項を示します。

| ネットワーク接続 | 影響 |
|----|---|
| 低帯域幅/待機時間が長い接続 | ログのアップロードは、完了までに長時間かかります。 |
| 共有接続 | アップロードは、ネットワーク接続を共有する他のアプリやユーザーに影響を与える場合があります。 |
| 従量制課金接続 | 追加のネットワーク使用に対して、ISP からの追加料金が発生する場合があります。 |

::: moniker-end
::: moniker range="<= azs-1910"

## <a name="collecting-logs-from-multiple-azure-stack-hub-systems"></a>複数の Azure Stack Hub システムからのログの収集

ログを収集する Azure Stack Hub スケールユニットごとに 1 つの BLOB コンテナーを設定します。 BLOB コンテナーを構成する方法の詳細については、「[自動 Azure Stack Hub 診断ログ収集の構成](./azure-stack-configure-automatic-diagnostic-log-collection.md?view=azs-2002)」を参照してください。 ベスト プラクティスとして、単一の BLOB コンテナー内の同じ Azure Stack Hub スケール ユニットからの診断ログのみを保存します。

## <a name="retention-policy"></a>Retention ポリシー

Azure Blob Storage [ライフサイクル管理ルール](/azure/storage/blobs/storage-lifecycle-management-concepts)を作成して、ログの保持ポリシーを管理します。 診断ログは 30 日間保持することをお勧めします。 Azure Storage でライフサイクル管理ルールを作成するには、Azure portal にサインインし、 **[ストレージ アカウント]** 、BLOB コンテナー、 **[Blob service]** の **[ライフサイクル管理]** の順にクリックします。

![Azure portal でのライフサイクル管理](media/azure-stack-automatic-log-collection/blob-storage-lifecycle-management.png)

## <a name="sas-token-expiration"></a>SAS トークンの有効期限

SAS URL の有効期限を 2 年に設定します。 ストレージ アカウント キーを更新する場合は、必ず SAS URL を再生成してください。 SAS トークンは、ベストプラクティスに従って管理する必要があります。 詳細については、「[SAS を使用する際のベスト プラクティス](/azure/storage/common/storage-dotnet-shared-access-signature-part-1#best-practices-when-using-sas)」を参照してください。

## <a name="bandwidth-consumption"></a>帯域幅の消費

診断ログ収集の平均サイズは、ログ収集がオンデマンドであるか自動であるかによって異なります。

オンデマンドのログ収集の場合、ログ収集のサイズは、収集される時間数によって異なります。 過去 7 日間の 1 時間から 4 時間のスライディング ウィンドウを選択できます。

自動診断ログ収集が有効になっている場合、サービスによって重大なアラートが監視されます。 重大なアラートが発生し、約 30 分間持続すると、サービスによって適切なログが収集され、アップロードされます。 このログ収集のサイズは、平均で約 2 GB です。 修正プログラムや更新のエラーが発生した場合、自動ログ収集は、重大なアラートが発生し、約 30 分間持続している場合のみ開始されます。 [修正プログラムと更新の監視に関するガイダンス](azure-stack-updates.md)に従うことをお勧めします。 アラートの監視、ログ収集、およびアップロードは、ユーザーに対して透過的です。

正常なシステムでは、ログはまったく収集されません。 異常なシステムでは、ログ収集が 1 日に 2、3 回実行される場合がありますが、通常は 1 回だけ実行されます。 多くて、最悪のシナリオで 1 日に最大 10 回実行される可能性があります。  

次の表は、Azure への接続が制限されている環境で、自動ログ収集を有効にする影響を考慮する場合に役立つ可能性があります。

| ネットワーク接続 | 影響 |
|---|---|
| 低帯域幅/待機時間が長い接続 | ログのアップロードは、完了までに長時間かかります。 | 
| 共有接続 | アップロードは、ネットワーク接続を共有する他のアプリやユーザーに影響を与える場合があります。 |
| 従量制課金接続 | 追加のネットワーク使用に対して、ISP からの追加料金が発生する場合があります。 |

## <a name="managing-costs"></a>コストの管理

Azure [BLOB ストレージの料金](https://azure.microsoft.com/pricing/details/storage/blobs/)は、毎月保存されるデータの量と、データの冗長性などのその他の要因によって異なります。 既存のストレージ アカウントがない場合は、Azure portal にサインインし、 **[ストレージ アカウント]** を選択し、手順に従って、[Azure BLOB コンテナーの SAS URL を作成](./azure-stack-configure-automatic-diagnostic-log-collection.md?view=azs-2002)します。

ベスト プラクティスとして、Azure BLOB ストレージ [ライフサイクル管理ポリシー](/azure/storage/blobs/storage-lifecycle-management-concepts)を作成して、継続的なストレージ コストを最小限に抑えます。 ストレージ アカウントを設定する方法の詳細については、「[自動 Azure Stack Hub 診断ログ収集の構成](./azure-stack-configure-automatic-diagnostic-log-collection.md?view=azs-2002)」を参照してください

::: moniker-end

## <a name="see-also"></a>関連項目

[Azure Stack Hub でのログおよび顧客データの処理](./azure-stack-data-collection.md)
