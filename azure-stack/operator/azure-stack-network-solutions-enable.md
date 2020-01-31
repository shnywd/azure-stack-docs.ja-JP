---
title: Fortinet FortiGate を使用して Azure Stack Hub にネットワーク ソリューションを提供する
description: Fortinet FortiGate を使用して Azure Stack Hub でのネットワーク ソリューションを有効にする方法について説明します
author: mattbriggs
ms.topic: how-to
ms.date: 1/22/2020
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 09/30/2019
ms.openlocfilehash: 87122d755ef7065747fc2df214ed270f92c4e49e
ms.sourcegitcommit: fd5d217d3a8adeec2f04b74d4728e709a4a95790
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76881731"
---
# <a name="offer-a-network-solution-in-azure-stack-hub-with-fortinet-fortigate"></a>Fortinet FortiGate を使用して Azure Stack Hub にネットワーク ソリューションを提供する

Azure Stack Hub Marketplace に FortiGate 次世代ファイアウォールを追加できます。 FortiGate を使用すると、ユーザーは、Azure Stack Hub と VNET ピアリングに対して、仮想プライベート ネットワーク (VPN) などのネットワーク ソリューションを作成できます。 ネットワーク仮想アプライアンス (NVA) では、境界ネットワークから他のネットワークまたはサブネットへのネットワーク トラフィックのフローが制御されます。 

Azure Marketplace での FortiGate の詳細については、「[Fortinet FortiGate の次世代ファイアウォールの単一の VM ソリューション](https://azuremarketplace.microsoft.com/marketplace/apps/fortinet.fortinet-FortiGate-singlevm)」を参照してください。

## <a name="download-the-required-azure-stack-hub-marketplace-items"></a>必須の Azure Stack Hub Marketplace アイテムをダウンロードする

1.  Azure Stack Hub 管理者ポータルを開きます。

2.  **[Marketplace Management]\(Marketplace 管理\)** を選択して、 **[+ Azure から追加]** を選択します。

3. 検索ボックスに「`Forti`」と入力して、 **[ダウンロード]** を選択し、次のアイテムの利用可能な最新バージョンを取得します。 
    - Fortinet FortiGate - Azure BYOL 用の VM
    - FortiGate NGFW - 単一の VM のデプロイ (BYOL)

    ![Azure Stack Hub の FortiGate Fortinet](./media/azure-stack-network-solutions-enable/azure-stack-marketplace-FortiGate-fortinet.png)

2.  Marketplace のアイテムの状態が **[ダウンロード済み]** になるまで待機します。 アイテムのダウンロードには、数分かかる可能性があります。

    ![Azure Stack Hub の FortiGate Fortinet](./media/azure-stack-network-solutions-enable/image4.png)

## <a name="next-steps"></a>次のステップ

[FortiGate NVA を使用して Azure Stack Hub の VPN を設定する](../user/azure-stack-network-howto-vnet-to-onprem.md)  
[ピアリングを通じて 2 つの VNET を接続する方法](../user/azure-stack-network-howto-vnet-to-vnet.md)  
[Fortinet FortiGate NVA を使用して VNET 間の接続を確立する方法](../user/azure-stack-network-howto-vnet-to-vnet-stacks.md)  
