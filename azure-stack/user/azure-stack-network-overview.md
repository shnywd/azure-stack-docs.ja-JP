---
title: Azure Stack Hub ネットワークの概要 | Microsoft Docs
description: Azure Stack Hub ネットワークについて
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: conceptual
ms.date: 1/22/2020
ms.author: mabrigg
ms.reviewer: scottnap
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 1a0ba98c44c4c28feedbf72e23534603d08abd44
ms.sourcegitcommit: a1abc27a31f04b703666de02ab39ffdc79a632f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2020
ms.locfileid: "76536047"
---
# <a name="introduction-to-azure-stack-hub-networking"></a>Azure Stack Hub ネットワークの概要

Azure Stack Hub には、単独でまたは組み合わせて使用できる、さまざまな種類のネットワーク機能が用意されています。

- **Azure Stack Hub リソースの間の接続**  
    クラウド内の安全でプライベートな仮想ネットワークで Azure のリソースを接続します。
- **インターネット接続**  
    インターネットを介して双方向で Azure Stack Hub リソースと通信します。
- **オンプレミスの接続**  
    インターネット上の仮想プライベート ネットワーク (VPN) を介して、または Azure Stack Hub への専用接続を介して、オンプレミスのネットワークを Azure Stack Hub リソースに接続します。
- **負荷分散とトラフィックの方向**  
    同じ場所のサーバー間でトラフィックを負荷分散したり、異なる場所のサーバーにトラフィックを誘導したりします。
- **セキュリティ**  
    ネットワーク サブネット間や個々の VM 間のネットワーク トラフィックをフィルタリングします。
- **ルーティング**  
    Azure Stack Hub リソースとオンプレミス リソースとの間で既定のルーティングまたは完全制御のルーティングを使用します。
- **管理の容易性**  
    Azure Stack Hub のネットワーク リソースを監視したり管理したりします。
- **デプロイ ツールと構成ツール**  
    Web ベースのポータルまたはクロスプラットフォームのコマンド ライン ツールを使って、ネットワーク リソースのデプロイと構成を行います。


## <a name="next-steps"></a>次のステップ

* [Azure Stack Hub ネットワークに関する考慮事項](azure-stack-network-differences.md)