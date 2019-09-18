---
title: Azure Stack 更新プログラムのインストール | Microsoft Docs
description: Azure Stack 更新プログラムをインストールする方法について説明します。
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
ms.author: mabriggs
ms.lastreviewed: 09/10/2019
ms.reviewer: ppacent
ms.openlocfilehash: 2153a91bcf833386ef5884a9fd4e7c0fc759c01b
ms.sourcegitcommit: dc633e862d49412a963daee481226c1543287e5e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70863061"
---
# <a name="install-azure-stack-updates"></a>Azure Stack 更新プログラムのインストール

*適用対象:Azure Stack 統合システム*

更新プログラム パッケージをインストールするには、Azure Stack の **[更新]** ブレードを使用します。 この記事では、更新プロセスの更新、監視、およびトラブルシューティングの手順について説明します。 [更新] ブレードを使用して、更新情報の表示、更新プログラムのインストール、更新の進行状況の監視、更新履歴の確認、および現在の Azure Stack と OEM パッケージ バージョンの表示を行います。

管理ポータルから更新プログラムを管理したり、ダッシュボードの **[更新プログラム]** セクションで次のことを行ったりできます。

- 現在のバージョンなどの重要な情報を表示します。
- 更新プログラムをインストールし、進行状況を監視します。
- 以前にインストールされた更新プログラムの更新履歴を確認します。
- クラウドの現在の OEM パッケージ バージョンを表示します。

## <a name="determine-the-current-version"></a>現在のバージョンの判断

現在のバージョンの Azure Stack は **[更新]** ブレードで表示できます。 開くには:

1.  Azure Stack 管理者ポータルを開きます。

2.  **[ダッシュボード]** を選択します。 **[更新]** ブレードに現在のバージョンが表示されます。

    ![既定のダッシュボードの [更新] タイル](./media/azure-stack-update-apply/image1.png)

    たとえば、このイメージではバージョンは 1.1903.0.35 です。

## <a name="install-updates-and-monitor-progress"></a>更新プログラムのインストールと進行状況の監視

1. Azure Stack 管理ポータルを開きます。

2. **[ダッシュボード]** を選択します。 **[Update]\(更新\)** を選択します。

3. インストールする使用可能な更新プログラムを選択します。 **[使用可能]** とマークされた更新プログラムがない場合は、[更新プログラム パッケージを準備する](azure-stack-update-prepare-package.md)必要があります

4. **[今すぐ更新]** を選択します。

    ![Azure Stack 更新実行の詳細](./media/azure-stack-update-apply/image2.png)

5. Azure Stack のさまざまなサブシステムを介して更新プロセスが反復処理されるときに詳細な状態を表示できます。 サブシステムの例として、物理ホスト、Service Fabric、インフラストラクチャ仮想マシン、管理者とユーザーの両方のポータルを提供するサービスなどがあります。 更新プロセスを通じて、更新リソース プロバイダーにより更新に関するその他の詳細 (成功した手順数、進行中の数など) が報告されます。

6. 更新実行の詳細ブレードから **[ダウンロードの概要]** を選択して、詳細ログをダウンロードします。

    更新プログラムの実行中に問題が発生した場合は、[特権エンドポイント](https://docs.microsoft.com/azure-stack/operator/azure-stack-privileged-endpoint)を使用して、Azure Stack 更新プログラムの実行の進行状況を監視することができます。 また、特権エンドポイントを使用して、Azure Stack ポータルが利用できなくなった場合に、障害が発生した更新プログラムの実行を、前回成功した手順から再開することもできます。 手順については、「[Monitor updates in Azure Stack using PowerShell](azure-stack-update-monitor.md)」 (PowerShell を使用して Azure Stack の更新を監視する) を参照してください。

    ![Azure Stack 更新実行の詳細](./media/azure-stack-update-apply/image3.png)

7. 完了したら、更新リソース プロバイダーにより、**成功**の確認が提供され、更新プロセスが終了したことと、それにかかった時間が示されます。 そこから、フィルターを使用して、すべての更新、利用可能な更新、またはインストールされた更新に関する情報を表示できます。

    ![azure-stack-update-apply](./media/azure-stack-update-apply/image4.png)

    更新が失敗した場合、 **[更新]** ブレードで **[要注意]** と報告されます。 **[詳細ログのダウンロード]** オプションを使用して、更新失敗時の大まかな状態を取得します。 Azure Stack のログ収集は、診断とトラブルシューティングで役立ちます。

## <a name="review-update-history"></a>更新履歴を確認する

1. 管理ポータルを開きます。

2. **[ダッシュボード]** を選択します。 **[Update]\(更新\)** を選択します。

3. **[更新の履歴]** を選択します。

    ![Azure Stack の更新履歴](./media/azure-stack-update-apply/image7.png)

## <a name="next-steps"></a>次の手順

-   [Azure Stack での更新プログラムの管理概要](https://docs.microsoft.com/azure-stack/operator/azure-stack-updates)  
-   [Azure Stack サービス ポリシー](https://docs.microsoft.com/azure-stack/operator/azure-stack-servicing-policy)  
