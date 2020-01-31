---
title: Azure Stack Hub Marketplace に Commvault を追加する
description: Azure Stack Hub Marketplace に Commvault を追加する方法について説明します。
author: mattbriggs
ms.topic: article
ms.date: 10/28/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/28/2019
ms.openlocfilehash: d2d12e032e639cb9b272affb4beed3a3a439b2cb
ms.sourcegitcommit: fd5d217d3a8adeec2f04b74d4728e709a4a95790
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76881713"
---
# <a name="add-commvault-to-the-azure-stack-hub-marketplace"></a>Azure Stack Hub Marketplace に Commvault を追加する

この記事では、Commvault Live Sync を使用して、別の Azure Stack Hub スケール ユニットにある復旧 VM を更新する手順について説明します。 ユーザー向けのバックアップとレプリケーションのソリューションとして、Commvault をダウンロードして提供することができます。 

## <a name="notes-for-commvault"></a>Commvault に関する注意

- ユーザーは、ソース Azure Stack Hub サブスクリプションの VM にバックアップとレプリケーションのソフトウェアをインストールする必要があります。 Azure Site Recovery と Azure Backup では、バックアップおよび回復イメージを格納する Stack 以外の場所を用意できます。 どちらも、次の場所から Azure Stack Hub にインストールするソフトウェア イメージをダウンロードする前に、Azure に Recovery Services Vault を作成する必要があります。[Azure Backup Server](https://go.microsoft.com/fwLink/?LinkId=626082&clcid=0x0409) と [Azure Site Recovery](https://aka.ms/unifiedinstaller_eus)。  
    
- サード パーティ ソフトウェアのライセンスが必要になる場合があります (選択した場合)。
- ユーザーはバックアップおよびレプリケーション ホスト上の仮想ネットワーク ゲートウェイ (VPN) またはパブリック IP を介してソースとターゲットを接続する際に、支援が必要になる場合があります。
- Azure Cloud サブスクリプションまたは回復ターゲット Azure Stack Hub のサブスクリプションをターゲットにします。
- 回復ターゲット Azure Stack Hub のリソース グループと BLOB ストレージ アカウントをターゲットにします。
- 一部のソリューションでは、移行元サーバーから変更を受信するために、24 時間 365 日実行する必要がある仮想マシンをターゲット サブスクリプションに作成する必要があります。 「[Commvault を使用して Azure Stack Hub で VM をバックアップする](../user/azure-stack-network-howto-backup-commvault.md)」では、Commvault Live Sync によって、初期構成中にターゲット復旧 VM が作成され、レプリケーション サイクル中に変更の適用が必要になるまでアイドル状態 (実行中ではなく、課金対象ではない) が維持されます。


## <a name="get-commvault-for-your-marketplace"></a>Marketplace 用に Commvault を取得する

1. Azure Stack Hub 管理ポータルを開きます。
2. **[Marketplace Management]\(Marketplace の管理\)**  >  **[Add from Azure]\(Azure から追加\)** の順に選択します。

    ![Azure Stack Hub 用の Commvault](./media/azure-stack-network-offer-backup-commvault/get-commvault-for-marketplace.png)

3. 「`commvault`」と入力します。
4. **[Commvault Trial]\(Commvault 試用版\)** を選択します。 次に、 **[ダウンロード]** を選択します。


## <a name="next-steps"></a>次のステップ

[Commvault を使用して Azure Stack Hub で VM をバックアップする](../user/azure-stack-network-howto-backup-commvault.md)

[Azure Stack Hub でのサービスの提供の概要](service-plan-offer-subscription-overview.md)
