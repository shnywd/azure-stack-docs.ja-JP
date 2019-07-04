---
title: Azure Stack のクラウド サービス プロバイダー向けの使用量レポート インフラストラクチャ | Microsoft Docs
description: Azure Stack には、クラウド サービス プロバイダー (CSP) がサービスを提供するテナントの使用状況を追跡して Azure に転送するために必要なインフラストラクチャが含まれています。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 05/09/2019
ms.openlocfilehash: 0b4ff1e6c76bedc4618bfa527b0045d7bfce41af
ms.sourcegitcommit: bcaad8b7db2ea596018d973cb29283d8c6daebfb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2019
ms.locfileid: "67419486"
---
# <a name="usage-reporting-infrastructure-for-cloud-service-providers"></a>クラウド サービス プロバイダー向けの使用量レポート インフラストラクチャ

Azure Stack には、発生した使用量を追跡して Azure に転送にするために必要なインフラストラクチャが含まれています。 Azure では、Azure Commerce が使用量データを処理し、適切な Azure サブスクリプションに使用量を課金します。 これは、グローバル Azure クラウドで使用状況の追跡を監視する場合と同じ方法で行われます。

一部の概念はグローバル Azure と Azure Stack の間で一貫性があります。 Azure Stack には、Azure サブスクリプションと同等の役割を果たすローカル サブスクリプションがあります。 ローカル サブスクリプションは、ローカルでのみ有効です。 ローカル サブスクリプションは、使用量が Azure に転送されるときに、Azure サブスクリプションにマップされます。

Azure Stack には、ローカル使用量メーターがあります。 ローカルの使用量は、Azure Commerce で使用されるメーターにマップされます。 ただし、メーター ID は異なります。 ローカルでは、Microsoft が請求に使用しているものを上回る数のメーターをご利用いただけます。

Azure Stack と Azure でのサービスの課金方法にはいくつか違いがあります。 たとえば Azure Stack では、Azure とは異なり、VM は時間あたりの仮想コアのみに基づき、すべての VM シリーズで同じレートです。 これは、グローバル Azure ではハードウェアの違いが価格の違いに反映されるためです。 Azure Stack では、お客様がハードウェアを提供するため、異なる VM クラスに異なるレートを請求する理由がありません。

Commerce で使用される Azure Stack メーターとその価格については、パートナー センターで Azure サービスの場合と同じ方法で確認できます。

1. パートナー センターで、 **[ダッシュボード] メニュー**に移動し、 **[販売]** を選択してから、 **[料金とプラン]** を選択します。
2. **[使用量ベースのサービス]** で、 **[最新]** を選択します。
3. **Azure in Global CSP price list (グローバル CSP 価格表での Azure)** スプレッドシートを開きます。
4. **リージョン = Azure Stack** でフィルターします。

## <a name="terms-used-for-billing-and-usage"></a>請求と使用量に使用される用語

次の用語と概念は、Azure Stack での使用量と請求に使用されます。

| 用語 | 定義 |
| --- | --- |
| 直接 CSP パートナー | 直接クラウド サービス プロバイダー (CSP) パートナーは、Azure と Azure Stack の使用量について Microsoft から直接請求書を受け取り、顧客に直接請求します。 |
| 間接 CSP | 間接リセラーは、間接プロバイダー (ディストリビューターとも呼ばれます) と連携します。 リセラーはエンド カスタマーを募集します。間接プロバイダーは、Microsoft と請求関係を保持し、顧客への請求を管理し、製品サポートのような追加サービスを提供します。 |
| エンド カスタマー | エンド カスタマーは、Azure Stack で実行されるアプリケーションやその他のワークロードを所有する企業および政府機関です。 |

## <a name="next-steps"></a>次の手順

- CSP プログラムの詳細については、「[クラウド ソリューション](https://partner.microsoft.com/solutions/microsoft-cloud-solutions)」を参照してください。
- Azure Stack からリソース使用量情報を取得する方法ついて詳しくは、「[Azure Stack での使用量と請求](azure-stack-billing-and-chargeback.md)」をご覧ください。
