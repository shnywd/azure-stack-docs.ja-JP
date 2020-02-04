---
title: Azure Stack Hub に OEM (相手先ブランド供給) 更新プログラムを適用します。
description: Azure Stack Hub に OEM (相手先ブランド供給) 更新プログラムを適用する方法について説明します。
author: mattbriggs
ms.topic: article
ms.date: 1/22/2020
ms.author: mabrigg
ms.lastreviewed: 09/10/2019
ms.reviewer: ppacent
ms.openlocfilehash: 52848a4da872800e34d6b43e6a3d2f834fb15125
ms.sourcegitcommit: fd5d217d3a8adeec2f04b74d4728e709a4a95790
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76880707"
---
# <a name="azure-stack-hub-servicing-policy"></a>Azure Stack Hub サービス ポリシー

この記事では、Azure Stack Hub 統合システムのサービス ポリシー、サポートを受けられる状態にシステムを維持するために行う必要がある事柄、およびサポートを受ける方法について説明します。

## <a name="keep-your-system-under-support"></a>システムがサポートされる状態を維持する

サポートを継続して受けるには、お使いの Azure Stack Hub の更新プログラムを最新に維持する必要があります。

サポートされる状態に Azure Stack Hub システムを維持するには、インスタンスで、リリースされた最新バージョンの更新プログラムを実行するか、それより 2 つ前の更新プログラムのバージョンのいずれかを実行する必要があります。

修正プログラムはメジャー バージョンの更新プログラムとは見なされません。 ご利用の Azure Stack Hub システムが *2 つ前の更新プログラム*より古い場合、コンプライアンスに対応していないとみなされます。 サポートを受けるためには、少なくともサポートされる最低限のバージョンまで更新する必要があります。

たとえば、最新バージョンの更新プログラムが 1904 の場合、その前の 2 つの更新プログラム パッケージは 1903 と 1902 であるため、1902 と 1903 は両方ともサポートされます。 ただし、1901 はサポート外です。 今後 1、2 か月のうちにリリースがないときはこのポリシーが適用されます。 たとえば、最新のリリースが 1807 で 1806 のリリースがなかった場合、その前の 2 つの更新プログラム パッケージである 1805 と 1804 がサポート対象となります。

Microsoft ソフトウェア更新プログラム パッケージは累積的ではないため、前提として前のバージョンの更新パッケージか修正プログラムが必要です。 1 つ以上の更新プログラムを先送りにする場合は、最新のバージョンを取得する際に全体のランタイムを考慮してください。

## <a name="get-support"></a>サポートを受ける

Azure Stack Hub には、Azure と同じサポート プロセスが適用されます。 企業のお客様は、[Azure サポート要求を作成する方法](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)で説明されているプロセスに従ってください。 クラウド ソリューション プロバイダー (CSP) のお客様の場合は、担当の CSP までサポートについてお問い合わせください。 詳細については、「[Azure サポートに関する FAQ](https://azure.microsoft.com/support/faq/)」を参照してください。

## <a name="next-steps"></a>次のステップ

[Azure Stack Hub での更新プログラム管理](azure-stack-updates.md)
