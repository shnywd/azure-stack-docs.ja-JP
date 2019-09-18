---
title: Azure Stack 1908 リリース ノート | Microsoft Docs
description: 新機能、既知の問題、更新プログラムをダウンロードする場所など、Azure Stack 統合システムの 1908 更新プログラムについて説明します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2019
ms.author: sethm
ms.reviewer: prchint
ms.lastreviewed: 08/30/2019
monikerRange: azs-1908
ms.openlocfilehash: ad059e09bbd544e4709a237ee0c51cafc2a734b4
ms.sourcegitcommit: 9438b559ea314cc8c19fd710e1b2a6ceb15e50c3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912648"
---
# <a name="azure-stack-1908-update"></a>Azure Stack 1908 更新プログラム

*適用対象:Azure Stack 統合システム*

この記事では、1908 更新プログラム パッケージの内容について説明します。 この更新プログラムには、このリリースの Azure Stack に対する新機能、機能強化、および修正が含まれています。

> [!IMPORTANT]  
> 更新プログラム パッケージは、Azure Stack 統合システム専用です。 Azure Stack Development Kit にこの更新プログラム パッケージは適用しないでください。

## <a name="previous-release-notes"></a>以前のリリース ノート

1908 リリース以降、以前のバージョンのリリース ノートは、左側の目次に表示されなくなりました。 以前のバージョンのリリース ノートにアクセスするには、別の記事 ([Azure Stack の概要](azure-stack-overview.md)など) を選択し、左側の目次の上部にあるバージョン セレクターから 1905、1906、1907、または 1908 を選択します。 以前のバージョンのリリース ノートについては、「[アーカイブされたリリース ノート](#archived-release-notes)」を参照してください。

## <a name="build-reference"></a>ビルドのリファレンス

Azure Stack 1908 更新プログラムのビルド番号は **1.1908.0.20** です。

### <a name="update-type"></a>更新の種類

1908 では、Azure Stack が実行される基になるオペレーティング システムが Windows Server 2019 に更新されています。 これにより、核となる基本的な機能強化だけでなく、近い将来に Azure Stack に機能を追加する機能も使用可能になります。

Azure Stack 1908 更新プログラムのビルドの種類は**完全**です。 そのため、1908 更新プログラムは、1906 や 1907 のような高速更新プログラムよりもランタイムが長くなります。 完全な更新プログラムの正確なランタイムは、Azure Stack インスタンスに含まれているノード数、テナントのワークロードごとにシステムで使用される容量、システムのネットワーク接続 (インターネットに接続されている場合)、システムのハードウェア構成によって異なります。 内部テストでは、1908 更新プログラムのランタイムが次のように予測されました。4 ノード - 42 時間、8 ノード - 50 時間、12 ノード - 60 時間、16 ノード - 70 時間。 更新プログラムのランタイムがこの予測値よりも長くなることは一般的ではなく、更新が失敗した場合を除き、Azure Stack オペレーターによるアクションは不要です。

更新プログラムのビルドの種類については、「[Azure Stack での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。

- 更新プログラムの正確な実行時間は一般に、ご使用のシステムでテナント ワークロードによって使用されている容量、システム ネットワーク接続 (インターネットに接続されている場合)、およびシステム ハードウェアの構成に左右されます。
- 実行時間が予測よりも長くなることは一般的ではなく、更新が失敗した場合を除き、Azure Stack オペレーターによるアクションは不要です。
- このおおよその実行時間は、1908 更新プログラムに固有であり、他の Azure Stack 更新プログラムと比較することはできません。

<!-- ## What's in this update -->

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>新機能

<!-- What's new, also net new experiences and features. -->

- 1908 では、Azure Stack が実行される基になるオペレーティング システムが Windows Server 2019 に更新されていることに注意してください。 これにより、核となる基本的な機能強化だけでなく、近い将来に Azure Stack に機能を追加する機能も使用可能になります。
- Azure Stack インフラストラクチャのすべてのコンポーネントが FIPS 140-2 モードで動作するようになりました。
- Azure Stack オペレーターは、ポータル ユーザー データを削除できるようになりました。 詳細については、「[Clear portal user data from Azure Stack](azure-stack-portal-clear.md)」 (Azure Stack からポータル ユーザー データをクリアする) を参照してください。

### <a name="improvements"></a>機能強化

<!-- Changes and product improvements with tangible customer-facing value. -->
- 物理ノードのハードウェアのトラステッド プラットフォーム モジュール (TPM) にシークレットを保持するために、Azure Stack の保存データの暗号化が向上しました。

### <a name="changes"></a>変更点

- ハードウェア プロバイダーは Azure Stack バージョン 1908 と同時に OEM 拡張機能パッケージ 2.1 以降をリリースします。 Azure Stack バージョン 1908 には OEM 拡張機能パッケージ 2.1 以降が前提条件です。 OEM 拡張機能パッケージ 2.1 以降をダウンロードする方法の詳細については、システムのハードウェア プロバイダーに問い合わせてください。また、[OEM 更新プログラム](azure-stack-update-oem.md#oem-contact-information)の記事を参照してください。  

### <a name="fixes"></a>修正

- 今後の Azure Stack OEM 更新プログラムとの互換性、およびユーザーのユーザー イメージを使用した VM デプロイに関する問題が修正されました。 この問題は 1907 で見つかり、修正プログラム [KB4517473](https://support.microsoft.com/en-us/help/4517473/azure-stack-hotfix-1-1907-12-44) で修正されました  
- OEM ファームウェア更新プログラムに関する問題が修正され、Fabric リングの正常性についての Test-AzureStack での誤診断に関する問題が修正されました。 この問題は 1907 で見つかり、修正プログラム [KB4515310](https://support.microsoft.com/en-us/help/4515310/azure-stack-hotfix-1-1907-7-35) で修正されました
- OEM ファームウェア更新プロセスに関する問題を修正しました。 この問題は 1907 で見つかり、修正プログラム [KB4515650](https://support.microsoft.com/en-us/help/4515650/azure-stack-hotfix-1-1907-8-37) で修正されました


<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

## <a name="security-updates"></a>セキュリティ更新プログラム

Azure Stack のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack security updates](azure-stack-release-notes-security-updates.md)」 (Azure Stack のセキュリティ更新プログラム) をご覧ください。

## <a name="update-planning"></a>計画の更新

更新プログラムを適用する前に、必ず次の情報を確認してください。

- [既知の問題](azure-stack-release-notes-known-issues-1908.md)
- [セキュリティ更新プログラム](azure-stack-release-notes-security-updates.md)
- [更新プログラム適用前後のアクティビティのチェックリスト](azure-stack-release-notes-checklist.md)

## <a name="download-the-update"></a>更新プログラムをダウンロードする

Azure Stack 1908 更新プログラム パッケージは、[Azure Stack ダウンロード ページ](https://aka.ms/azurestackupdatedownload)からダウンロードできます。

## <a name="hotfixes"></a>修正プログラム

Azure Stack では、修正プログラムが定期的にリリースされます。 Azure Stack を 1908 に更新する前に、必ず 1907 用の最新の Azure Stack 修正プログラムをインストールしてください。

Azure Stack 修正プログラムを適用できるのは Azure Stack 統合システムのみです。ASDK には修正プログラムをインストールしないでください。

### <a name="prerequisites-before-applying-the-1908-update"></a>前提条件:1908 更新プログラムを適用する前

Azure Stack の 1908 リリースは、次の修正プログラムが適用された 1907 リリースに適用する必要があります。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack 修正プログラム 1.1907.12.44](https://support.microsoft.com/help/4517473)

Azure Stack 1908 更新プログラムには、システムのハードウェア プロバイダーからの **Azure Stack OEM バージョン 2.1 以降**が必要です。 OEM 更新プログラムには、Azure Stack システム ハードウェアのドライバーとファームウェアの更新プログラムが含まれています。 OEM 更新プログラムの適用の詳細については、「[Azure Stack に OEM (相手先ブランド供給) 更新プログラムを適用する](azure-stack-update-oem.md)」を参照してください

### <a name="after-successfully-applying-the-1908-update"></a>1908 更新プログラムの適用に成功した後

この更新プログラムをインストールした後、適用可能な修正プログラムがあればインストールします。 詳細については、[サービス ポリシー](azure-stack-servicing-policy.md)に関する記事を参照してください。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- 1908 用の修正プログラムはありません。

## <a name="automatic-update-notifications"></a>自動更新通知

インフラストラクチャ ネットワークからインターネットにアクセスできるシステムでは、オペレーター ポータルに "**利用可能な更新プログラムがあります**" というメッセージが表示されます。 インターネットにアクセスできないシステムでは、対応する .xml を含む .zip ファイルをダウンロードしてインポートできます。

> [!TIP]  
> 以下の *RSS* または *Atom* フィードに登録して、Azure Stack 修正プログラムの最新情報を入手してください。
>
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="archived-release-notes"></a>アーカイブされたリリース ノート

[以前のバージョンの Azure Stack のリリース ノートは TechNet ギャラリー](https://aka.ms/azsarchivedrelnotes)で確認できます。 これらのアーカイブされたリリース ノートは参照のみを目的に提供されており、これらのバージョンのサポートを意味しているわけではありません。 Azure Stack のサポートについては、「[Azure Stack サービス ポリシー](azure-stack-servicing-policy.md)」をご覧ください。 さらにサポートが必要な場合は、Microsoft カスタマー サポート サービスにお問い合わせください。

## <a name="next-steps"></a>次の手順

- Azure Stack での更新プログラム管理の概要については、「[Azure Stack での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。  
- Azure Stack に更新プログラムを適用する方法については、「[Azure Stack で更新を適用する](azure-stack-apply-updates.md)」を参照してください。
- Azure Stack 統合システムのサービス ポリシーについて、およびサポートを受けられる状態にシステムを維持するために必要な作業について確認するには、「[Azure Stack サービス ポリシー](azure-stack-servicing-policy.md)」を参照してください。  
- 特権エンドポイント (PEP) を使用して更新プログラムを監視および再開するには、「[特権エンドポイントを使用して Azure Stack での更新プログラムをモニターする](azure-stack-monitor-update.md)」をご覧ください。
