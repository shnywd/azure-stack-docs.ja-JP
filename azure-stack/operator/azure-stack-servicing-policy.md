---
title: Azure Stack サービス ポリシー | Microsoft Docs
description: Azure Stack サービス ポリシーの詳細と、サポートを受けられる状態に統合システムを維持する方法を説明します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2019
ms.author: sethm
ms.reviewer: harik
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: 1022ab056157ea1a9bc925d3992a99bd0b395a35
ms.sourcegitcommit: 71d7990a2b21576c44bb2aea13ae2026e9510c55
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/30/2019
ms.locfileid: "70188088"
---
- **OEM ハードウェア ベンダー提供の更新プログラム**。 Azure Stack ハードウェア パートナーには、ハードウェアに関連するファームウェアおよびドライバーの更新プログラム パッケージについて、ガイドを含めエンド ツー エンドでサービスを提供し続ける責任があります。 また、Azure Stack ハードウェア パートナーは、すべてのソフトウェアとハードウェアのハードウェア ライフサイクル ホストに関するガイドを所有して管理します。 OEM ハードウェア ベンダーは、自社のダウンロード サイトでこれらの更新プログラム パッケージを管理します。

## <a name="update-package-release-cadence"></a>更新プログラム パッケージのリリース周期

Microsoft は、毎月ソフトウェア更新プログラム パッケージをリリースします。 ただし、1 か月に複数の更新プログラムをリリースすることや、1 つも更新プログラムをリリースしないこともあります。 OEM ハードウェア ベンダーは、必要に応じて随時更新プログラムをリリースします。

更新を計画し、更新プログラムを管理する方法と、現在のバージョンを確認する方法については、[更新プログラムの管理の概要](azure-stack-updates.md)にあるドキュメントをご確認ください。

更新プログラムのダウンロード方法など、特定の更新プログラムに関する情報については、リリース ノートを参照してください。

- [Azure Stack 1908 更新プログラム](azure-stack-release-notes-1908.md)
- [Azure Stack 1907 更新プログラム](azure-stack-release-notes-1907.md)
- [Azure Stack 1906 更新プログラム](azure-stack-release-notes-1906.md)
- [Azure Stack 1905 更新プログラム](azure-stack-release-notes-1905.md)

## <a name="hotfixes"></a>修正プログラム

Microsoft は、随時、Azure Stack に関する予防可能な、または緊急の問題に対応するため、修正プログラムを提供します。  各修正プログラムは、問題、原因、および解決方法の詳細が記載された Microsoft のナレッジ ベースの記事に対応してリリースされます。

*適用対象:Azure Stack 統合システム*

この記事では、Azure Stack 統合システムのサービス ポリシーについて、サポートを受けられる状態にシステムを維持する必要があることについて、およびサポートを得る方法について説明します。

## <a name="keep-your-system-under-support"></a>システムがサポートされる状態を維持する

Azure Stack インスタンスのサポートされる状態を維持するには、インスタンスで、リリースされた最新バージョンの更新プログラムを実行するか、またはそれより 2 つ前の更新プログラムのバージョンのいずれかを実行する必要があります。

修正プログラムはメジャー バージョンの更新プログラムとは見なされません。 お使いの Azure Stack インスタンスが *2 つ前の更新プログラム*より遅れている場合、コンプライアンスを順守していないとみなされます。 サポートを受けるためには、少なくともサポートされる最低限のバージョンまで更新する必要があります。

たとえば、最新バージョンの更新プログラムが 1904 の場合、その前の 2 つの更新プログラム パッケージは 1903 と 1902 であるため、1902 と 1903 は両方ともサポートされます。 ただし、1901 はサポート外です。 今後 1、2 か月のうちにリリースがないときはこのポリシーが適用されます。 たとえば、最新のリリースが 1807 で 1806 のリリースがなかった場合、その前の 2 つの更新プログラム パッケージである 1805 と 1804 がサポート対象となります。

Microsoft ソフトウェア更新プログラム パッケージは累積的ではないため、前提として前のバージョンの更新パッケージか修正プログラムが必要です。 1 つ以上の更新プログラムを先送りにする場合は、最新のバージョンを取得する際に全体のランタイムを考慮してください。

## <a name="get-support"></a>サポートを受ける

Azure Stack では、Azure と同じサポート プロセスが適用されます。 企業のお客様は、[Azure サポート リクエストを作成する方法](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)で説明されているプロセスに従ってください。 クラウド サービス プロバイダー (CSP) のお客様の場合は、担当の CSP までサポートについてお問い合わせください。 詳細については、「[Azure サポートに関する FAQ](https://azure.microsoft.com/support/faq/)」を参照してください。

## <a name="next-steps"></a>次の手順

- [Azure Stack での更新の管理](https://docs.microsoft.com/azure-stack/operator/azure-stack-updates)
