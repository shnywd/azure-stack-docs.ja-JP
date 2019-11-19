---
title: Azure Stack Marketplace に Commvault を追加する | Microsoft Docs
description: Azure Stack Marketplace に Commvault を追加する方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/28/2019
ms.openlocfilehash: 787453550e9a8ed69aa9dfda0a03ea5e57c49d7a
ms.sourcegitcommit: ca358ea5c91a0441e1d33f540f6dbb5b4d3c92c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2019
ms.locfileid: "73802317"
---
# <a name="add-commvault-to-the-azure-stack-marketplace"></a>Azure Stack Marketplace に Commvault を追加する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

この記事では、Commvault Live Sync を使用して、別の Azure Stack スケール ユニットにある復旧 VM を更新する手順について説明します。 ユーザー向けのバックアップとレプリケーションのソリューションとして、Commvault をダウンロードして提供することができます。 

## <a name="notes-for-commvault"></a>Commvault に関する注意

- ユーザーは、ソース Azure Stack サブスクリプションの VM にバックアップとレプリケーションのソフトウェアをインストールする必要があります。 Azure Site Recovery と Azure Backup では、バックアップおよび回復イメージを格納する Stack 以外の場所を用意できます。 どちらも、次の場所から Azure Stack にインストールするソフトウェア イメージをダウンロードする前に、Azure に Recovery Services Vault を作成する必要があります。[Azure Backup Server](https://go.microsoft.com/fwLink/?LinkId=626082&clcid=0x0409) と [Azure Site Recovery](https://aka.ms/unifiedinstaller_eus)。  
    
- サード パーティ ソフトウェアのライセンスが必要になる場合があります (選択した場合)。
- ユーザーはバックアップおよびレプリケーション ホスト上の仮想ネットワーク ゲートウェイ (VPN) またはパブリック IP を介してソースとターゲットを接続する際に、支援が必要になる場合があります。
- Azure Cloud サブスクリプションまたは回復ターゲット Azure Stack のサブスクリプションをターゲットにします。
- 回復ターゲット Azure Stack のリソース グループと BLOB ストレージ アカウントをターゲットにします。
- 一部のソリューションでは、移行元サーバーから変更を受信するために、24 時間 365 日実行する必要がある仮想マシンをターゲット サブスクリプションに作成する必要があります。 「[Commvault を使用した Azure Stack での VM のバックアップ](../user/azure-stack-network-howto-backup-commvault.md)」では、Commvault Live Sync によって、初期構成中にターゲット復旧 VM が作成され、レプリケーション サイクル中に変更の適用が必要になるまでアイドル状態 (実行中ではなく、課金対象ではない) が維持されます。


## <a name="get-commvault-for-your-marketplace"></a>Marketplace 用に Commvault を取得する

1. Azure Stack 管理ポータルを開きます。
2. **[Marketplace Management]\(Marketplace の管理\)**  >  **[Add from Azure]\(Azure から追加\)** の順に選択します。

    ![Azure Stack 用の Commvault](./media/azure-stack-network-offer-backup-commvault/get-commvault-for-marketplace.png)

3. 「 `commvault` 」を入力します。
4. **[Commvault Trial]\(Commvault 試用版\)** を選択します。 次に、 **[ダウンロード]** を選択します。


## <a name="next-steps"></a>次の手順

[Commvault を使用して Azure Stack で VM をバックアップする](../user/azure-stack-network-howto-backup-commvault.md)

[Azure Stack でのサービスの提供の概要](service-plan-offer-subscription-overview.md)
