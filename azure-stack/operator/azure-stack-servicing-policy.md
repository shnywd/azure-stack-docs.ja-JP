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
ms.date: 09/06/2019
ms.author: sethm
ms.reviewer: harik
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: dbfce7138d76892c7f66e2cf6f33883668e55395
ms.sourcegitcommit: 5703255b4647ff0ebec23658a3f5c25d67f076a2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70749954"
---
# <a name="azure-stack-servicing-policy"></a>Azure Stack サービス ポリシー

この記事では、Azure Stack 統合システムのサービス ポリシーについて、およびサポートを受けられる状態にシステムを維持する必要があることについて説明します。

## <a name="download-update-packages-for-integrated-systems"></a>統合システムの更新プログラム パッケージをダウンロードする

Microsoft では、特定の問題に対処する修正プログラム パッケージだけでなく、完全な月例更新プログラム パッケージをリリースしています。

月例更新プログラム パッケージは、セキュリティで保護された Azure エンドポイントでホストされます。 これらは、[Azure Stack 更新プログラム ダウンローダー ツール](https://aka.ms/azurestackupdatedownload)を使用して手動でダウンロードすることができます。 スケール ユニットが接続されている場合、更新プログラムは、"**更新プログラムが利用可能です**" と自動的に管理者ポータルに表示されます。 月例更新プログラム パッケージは、各リリースで詳細にドキュメント化されます。 各リリースの詳細については、この記事の「[更新プログラム パッケージのリリース周期](#update-package-release-cadence)」セクションの任意のリリースをクリックしてください。

修正プログラム パッケージは、セキュリティで保護された同じ Azure エンドポイントでホストされます。 各修正プログラム KB 記事の埋め込みリンク (たとえば、[Azure Stack 修正プログラム 1.1809.12.114](https://support.microsoft.com/help/4481548/azure-stack-hotfix-1-1809-12-114)) を使用して、それらをダウンロードできます。 完全な月例更新プログラム パッケージと同様に、Azure Stack オペレーターは、「[Azure Stack で更新を適用する](azure-stack-apply-updates.md)」の手順を使用して、.xml、.bin、および .exe ファイルをダウンロードしてインポートできます。 スケール ユニットが接続されている Azure Stack オペレーターに対しては、修正プログラムが "**更新プログラムが利用可能です**" というメッセージと共に自動的に管理者ポータルに表示されます。

スケール ユニットが接続されておらず、各修正プログラムのリリースについて通知を受け取る場合は、各リリースに記載されている [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss) または [ATOM](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom) フィードを購読します。

## <a name="update-package-types"></a>更新プログラム パッケージの種類

統合システムの更新プログラム パッケージには、次の 2 つの種類があります。

- **Microsoft ソフトウェア更新プログラム**。 Microsoft には、Microsoft ソフトウェア更新プログラム パッケージについて、エンド ツー エンドでサービスを提供し続ける責任があります。 これらのパッケージには、最新の Windows Server のセキュリティ更新プログラム、セキュリティ以外の更新プログラム、Azure Stack の機能の更新プログラムを含めることができます。 これらの更新プログラム パッケージは Microsoft から直接ダウンロードできます。

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

*適用対象:Azure Stack 統合システム*

Microsoft は、随時、Azure Stack に関する予防可能な、または緊急の問題に対応するため、修正プログラムを提供します。  各修正プログラムは、問題、原因、および解決方法の詳細が記載された Microsoft のナレッジ ベースの記事に対応してリリースされます。

修正プログラムは、Azure Stack の通常の完全版の更新プログラム パッケージと同様にインストールできます。 ただし、完全な更新プログラムとは異なり、修正プログラムは数分でインストールできます。 修正プログラムをインストールする際に、Azure Stack オペレーターによるメンテナンス期間の設定をお勧めします。 修正プログラムは、修正プログラムが適用されているかを簡単に判別できるように、お使いの Azure Stack クラウドのバージョンを更新します。 まだサポート対象である Azure Stack のバージョンごとに個別の修正プログラムが提供されています。 特定のイテレーションの各修正プログラムは重複しており、同じバージョンの以前の更新プログラムが含まれています。 特定の修正プログラムの該当性については、対応するナレッジ ベースの記事を参照してください。 前のセクションのリリース ノートのリンクを参照してください。

現在利用可能な修正プログラムの詳細については、その更新のリリースノートを参照してください。

- [Azure Stack 1908 修正プログラム](azure-stack-release-notes-1908.md#hotfixes)
- [Azure Stack 1907 修正プログラム](azure-stack-release-notes-1907.md#hotfixes)
- [Azure Stack 1906 修正プログラム](azure-stack-release-notes-1906.md#hotfixes)
- [Azure Stack 1905 修正プログラム](azure-stack-release-notes-1905.md#hotfixes)

## <a name="keep-your-system-under-support"></a>システムがサポートされる状態を維持する

Azure Stack インスタンスのサポートされる状態を維持するには、インスタンスで、リリースされた最新バージョンの更新プログラムを実行するか、またはそれより 2 つ前の更新プログラムのバージョンのいずれかを実行する必要があります。

修正プログラムはメジャー バージョンの更新プログラムとは見なされません。 お使いの Azure Stack インスタンスが *2 つ前の更新プログラム*より遅れている場合、コンプライアンスを順守していないとみなされます。 サポートを受けるためには、少なくともサポートされる最低限のバージョンまで更新する必要があります。

たとえば、最新バージョンの更新プログラムが 1904 の場合、その前の 2 つの更新プログラム パッケージは 1903 と 1902 であるため、1902 と 1903 は両方ともサポートされます。 ただし、1901 はサポート外です。 今後 1、2 か月のうちにリリースがないときはこのポリシーが適用されます。 たとえば、最新のリリースが 1807 で 1806 のリリースがなかった場合、その前の 2 つの更新プログラム パッケージである 1805 と 1804 がサポート対象となります。

Microsoft ソフトウェア更新プログラム パッケージは累積的ではないため、前提として前のバージョンの更新パッケージか修正プログラムが必要です。 1 つ以上の更新プログラムを先送りにする場合は、最新のバージョンを取得する際に全体のランタイムを考慮してください。

## <a name="get-support"></a>サポートを受ける

Azure Stack では、Azure と同じサポート プロセスが適用されます。 企業のお客様は、[Azure サポート リクエストを作成する方法](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)で説明されているプロセスに従ってください。 クラウド サービス プロバイダー (CSP) のお客様の場合は、担当の CSP までサポートについてお問い合わせください。 詳細については、「[Azure サポートに関する FAQ](https://azure.microsoft.com/support/faq/)」を参照してください。

## <a name="next-steps"></a>次の手順

- [Azure Stack での更新の管理](azure-stack-updates.md)
