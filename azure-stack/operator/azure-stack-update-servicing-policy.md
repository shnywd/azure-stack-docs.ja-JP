---
title: Azure Stack に OEM (相手先ブランド供給) 更新プログラムを適用する | Microsoft Docs
description: Azure Stack に OEM (相手先ブランド供給) 更新プログラムを適用する方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2019
ms.author: mabrigg
ms.lastreviewed: 09/10/2019
ms.reviewer: ppacent
ms.openlocfilehash: 70fe7c8cdb43f55c8e013dcca27dcde870b1cc9a
ms.sourcegitcommit: dc633e862d49412a963daee481226c1543287e5e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70862969"
---
# <a name="azure-stack-servicing-policy"></a>Azure Stack サービス ポリシー

*適用対象:Azure Stack 統合システム*

この記事では、Azure Stack 統合システムのサービス ポリシーについて、サポートを受けられる状態にシステムを維持する必要があることについて、およびサポートを得る方法について説明します。

## <a name="keep-your-system-under-support"></a>システムがサポートされる状態を維持する

サポートを継続して受けるには、Azure Stack の更新プログラムを最新に維持する必要があります。

Azure Stack インスタンスのサポートされる状態を維持するには、インスタンスで、リリースされた最新バージョンの更新プログラムを実行するか、またはそれより 2 つ前の更新プログラムのバージョンのいずれかを実行する必要があります。

修正プログラムはメジャー バージョンの更新プログラムとは見なされません。 お使いの Azure Stack インスタンスが *2 つ前の更新プログラム*より遅れている場合、コンプライアンスを順守していないとみなされます。 サポートを受けるためには、少なくともサポートされる最低限のバージョンまで更新する必要があります。

たとえば、最新バージョンの更新プログラムが 1904 の場合、その前の 2 つの更新プログラム パッケージは 1903 と 1902 であるため、1902 と 1903 は両方ともサポートされます。 ただし、1901 はサポート外です。 今後 1、2 か月のうちにリリースがないときはこのポリシーが適用されます。 たとえば、最新のリリースが 1807 で 1806 のリリースがなかった場合、その前の 2 つの更新プログラム パッケージである 1805 と 1804 がサポート対象となります。

Microsoft ソフトウェア更新プログラム パッケージは累積的ではないため、前提として前のバージョンの更新パッケージか修正プログラムが必要です。 1 つ以上の更新プログラムを先送りにする場合は、最新のバージョンを取得する際に全体のランタイムを考慮してください。

## <a name="get-support"></a>サポートを受ける

Azure Stack では、Azure と同じサポート プロセスが適用されます。 企業のお客様は、[Azure サポート リクエストを作成する方法](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)で説明されているプロセスに従ってください。 クラウド サービス プロバイダー (CSP) のお客様の場合は、担当の CSP までサポートについてお問い合わせください。 詳細については、「[Azure サポートに関する FAQ](https://azure.microsoft.com/support/faq/)」を参照してください。

# <a name="next-steps"></a>次の手順

[Azure Stack での更新の管理](azure-stack-updates.md)
