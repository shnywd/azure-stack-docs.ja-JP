---
title: Azure Stack Hub サービス ポリシー
titleSuffix: Azure Stack Hub
description: Azure Stack Hub サービス ポリシーの詳細と、サポートを受けられる状態に統合システムを維持する方法を説明します。
author: sethmanheim
ms.topic: article
ms.date: 06/09/2020
ms.author: sethm
ms.reviewer: niy
ms.lastreviewed: 03/18/2020
ms.openlocfilehash: 38fb04f0b3dae557eee18271146312c57347bd89
ms.sourcegitcommit: e9a1dfa871e525f1d6d2b355b4bbc9bae11720d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/20/2020
ms.locfileid: "86487857"
---
# <a name="azure-stack-hub-servicing-policy"></a>Azure Stack Hub サービス ポリシー

この記事では、Azure Stack Hub 統合システムのサービス ポリシーについて、およびサポートを受けられる状態にシステムを維持するために必要なことについて説明します。

## <a name="download-update-packages-for-integrated-systems"></a>統合システムの更新プログラム パッケージをダウンロードする

Microsoft では、特定の問題に対処する修正プログラム パッケージだけでなく、完全な月例更新プログラム パッケージをリリースしています。

月例更新プログラム パッケージは、セキュリティで保護された Azure エンドポイントでホストされます。 これらは、[Azure Stack Hub 更新プログラム ダウンローダー ツール](https://aka.ms/azurestackupdatedownload)を使用して、手動でダウンロードできます。 スケール ユニットが接続されている場合、更新プログラムは、"**更新プログラムが利用可能です**" と自動的に管理者ポータルに表示されます。 月例更新プログラム パッケージは、各リリースで詳細にドキュメント化されます。 各リリースの詳細については、この記事の「[更新プログラム パッケージのリリース周期](#update-package-release-cadence)」セクションの任意のリリースをクリックしてください。

修正プログラム パッケージは、セキュリティで保護された同じ Azure エンドポイントでホストされます。 これらは、各修正プログラムの KB 記事 (たとえば、[Azure Stack Hub 修正プログラム 1.1809.12.114](https://support.microsoft.com/help/4481548/azure-stack-hotfix-1-1809-12-114)) の埋め込みリンクを使用してダウンロードできます。 毎月の完全版更新プログラム パッケージと同様に、Azure Stack Hub オペレーターは、[Azure Stack Hub での更新プログラムの適用](azure-stack-apply-updates.md)に関する記事に記載されている手順を使用して、.xml、.bin、および .exe ファイルのダウンロードとインポートを行うことができます。 スケール ユニットが接続されている Azure Stack Hub オペレーターに対しては、修正プログラムが "**更新プログラムが利用可能です**" というメッセージと共に、自動的に管理者ポータルに表示されます。

スケール ユニットが接続されておらず、各修正プログラムのリリースについて通知を受け取る場合は、各リリースに記載されている [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss) または [ATOM](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom) フィードを購読します。

## <a name="update-package-types"></a>更新プログラム パッケージの種類

統合システムの更新プログラム パッケージには、次の 2 つの種類があります。

- **Microsoft ソフトウェア更新プログラム**。 Microsoft には、Microsoft ソフトウェア更新プログラム パッケージについて、エンド ツー エンドでサービスを提供し続ける責任があります。 これらのパッケージには、最新の Windows Server のセキュリティ更新プログラム、セキュリティ以外の更新プログラム、Azure Stack Hub の機能更新プログラムが含まれています。 これらの更新プログラム パッケージは Microsoft から直接ダウンロードできます。

- **OEM ハードウェア ベンダー提供の更新プログラム**。 Azure Stack Hub ハードウェア パートナーには、ハードウェアに関連するファームウェアおよびドライバーの更新プログラム パッケージについて、ガイドを含めエンド ツー エンドでサービスを提供し続ける責任があります。 また、Azure Stack Hub ハードウェア パートナーは、すべてのソフトウェアとハードウェアのハードウェア ライフサイクル ホストに関するガイドを所有して管理します。 OEM ハードウェア ベンダーは、自社のダウンロード サイトでこれらの更新プログラム パッケージを管理します。

## <a name="update-package-release-cadence"></a>更新プログラム パッケージのリリース周期

Microsoft は、毎月ソフトウェア更新プログラム パッケージをリリースします。 ただし、1 か月に複数の更新プログラムをリリースすることや、1 つも更新プログラムをリリースしないこともあります。 OEM ハードウェア ベンダーは、必要に応じて随時更新プログラムをリリースします。

更新を計画し、更新プログラムを管理する方法と、現在のバージョンを確認する方法については、[更新プログラムの管理の概要](azure-stack-updates.md)にあるドキュメントをご確認ください。

更新プログラムのダウンロード方法など、特定の更新プログラムに関する情報については、リリース ノートを参照してください。

- [Azure Stack Hub 2002 更新プログラム](./release-notes.md?view=azs-2002)
- [Azure Stack Hub 1910 更新プログラム](./release-notes.md?view=azs-1910)
- [Azure Stack Hub 1908 更新プログラム](./release-notes.md?view=azs-1908)
- [Azure Stack Hub 1907 更新プログラム](./release-notes.md?view=azs-1907)

## <a name="hotfixes"></a>修正プログラム

Microsoft は、予防可能または緊急な場合の多い特定の問題に対応するため、Azure Stack Hub の修正プログラムを随時提供します。 各修正プログラムは、その修正プログラムで対処された問題の詳細が記載された Microsoft ナレッジ ベースの該当記事と共にリリースされます。

修正プログラムは、Azure Stack Hub の通常の完全版更新プログラム パッケージと同様に、ダウンロードおよびインストールできます。 ただし、完全な更新プログラムとは異なり、修正プログラムは数分でインストールできます。 修正プログラムをインストールする際に、Azure Stack Hub オペレーターがメンテナンス期間を設定することをお勧めします。 修正プログラムでは、修正プログラムが適用されているかを簡単に判別できるように、お使いの Azure Stack Hub クラウドのバージョンが更新されます。 まだサポート対象となっている Azure Stack Hub のバージョンごとに、個別の修正プログラムが提供されています。 **特定のイテレーションの各修正プログラムは重複しており、同じバージョンの以前の修正プログラムが含まれています。** 特定の修正プログラムの該当性については、対応するナレッジ ベースの記事を参照してください。 前のセクションのリリース ノートのリンクを参照してください。

現在利用可能な修正プログラムの詳細については、その更新のリリースノートを参照してください。

- [Azure Stack Hub 2002 修正プログラム](./release-notes.md?view=azs-2002#hotfixes)
- [Azure Stack Hub 1910 修正プログラム](./release-notes.md?view=azs-1910#hotfixes-1)
- [Azure Stack Hub 1908 修正プログラム](./release-notes.md?view=azs-1908#hotfixes-2)
- [Azure Stack Hub 1907 修正プログラム](./release-notes.md?view=azs-1907#hotfixes-3)

## <a name="keep-your-system-under-support"></a>システムがサポートされる状態を維持する

::: moniker range="azs-2002"

> [!IMPORTANT]  
> Azure Stack Hub 2002 更新プログラムでは、Microsoft は Azure Stack Hub サポート ポリシー ステートメントを一時的に延長しています。 Microsoft は現在、COVID-19 に対応中の、Azure Stack Hub システムとその更新方法と管理方法に関する重要な意思決定を行っている可能性のある世界中のお客様と連携して、お客様のデータセンターのビジネス運用を正常に継続するための取り組みを行っています。 Microsoft では、お客様をサポートするために、3 つ前までの更新プログラムのバージョンを含めるように一時的なサポート ポリシー変更の延長を行っています。 その結果、新しくリリースされた 2002 更新プログラムと、3 つ前までの更新プログラムのバージョン (1910、1908、1907 など) のすべてがサポートされます。

::: moniker-end

サポートを受けられる状態に Azure Stack Hub インスタンスを維持するためには、インスタンスで、リリースされた最新バージョンの更新プログラムを実行するか、またはその前の 2 つの更新プログラムのバージョンのいずれかを実行する必要があります。

修正プログラムはメジャー バージョンの更新プログラムとは見なされません。 お使いの Azure Stack Hub インスタンスが "*2 つ前の更新プログラム*" より古い場合、対応していないと見なされます。 サポートを受けるためには、少なくともサポートされる最低限のバージョンまで更新する必要があります。

たとえば、最新バージョンの更新プログラムが 1904 の場合、その前の 2 つの更新プログラム パッケージは 1903 と 1902 であるため、1902 と 1903 は両方ともサポートされます。 ただし、1901 はサポート外です。 今後 1、2 か月のうちにリリースがないときはこのポリシーが適用されます。 たとえば、最新のリリースが 1807 で 1806 のリリースがなかった場合、その前の 2 つの更新プログラム パッケージである 1805 と 1804 がサポート対象となります。

Microsoft ソフトウェア更新プログラム パッケージは累積的ではないため、前提として前のバージョンの更新パッケージか修正プログラムが必要です。 1 つ以上の更新プログラムを先送りにする場合は、最新のバージョンを取得する際に全体のランタイムを考慮してください。

### <a name="resource-provider-version-support"></a>リソース プロバイダーのバージョンのサポート

Azure Stack Hub リソース プロバイダーでは、サポートされているバージョンの Azure Stack Hub と互換性がある、特定のリソース プロバイダーの最新リリース バージョンのみがサポートされています。これは、まだサポート有効期限内の古いバージョンの Azure Stack Hub を使用している場合でも同様です。

リソース プロバイダーの互換性の詳細については、その特定のリソース プロバイダーのリリース ノートを参照してください。

## <a name="get-support"></a>サポートを受ける

Azure Stack Hub には、Azure と同じサポート プロセスが適用されます。 企業のお客様は、[Azure サポート要求を作成する方法](/azure/azure-supportability/how-to-create-azure-support-request)で説明されているプロセスに従ってください。 クラウド ソリューション プロバイダー (CSP) のお客様の場合は、担当の CSP までサポートについてお問い合わせください。 詳細については、「[Azure サポートに関する FAQ](https://azure.microsoft.com/support/faq/)」を参照してください。

更新プログラムに関する問題のトラブルシューティングのヘルプについては、「[Azure Stack Hub の修正プログラムと更新プログラムに関する問題のトラブルシューティングのベスト プラクティス](azure-stack-troubleshooting.md)」を参照してください。

## <a name="next-steps"></a>次のステップ

- [Azure Stack Hub での更新プログラム管理](azure-stack-updates.md)
- [Azure Stack Hub の修正プログラムと更新プログラムに関する問題のトラブルシューティングのベスト プラクティス](azure-stack-troubleshooting.md)
