---
title: Azure Stack Hub ネットワークの概要
description: Azure Stack Hub ネットワークについて
author: mattbriggs
ms.topic: conceptual
ms.date: 11/16/2020
ms.author: mabrigg
ms.reviewer: scottnap
ms.lastreviewed: 01/14/2020
ms.openlocfilehash: 2cb8ede31a91ba05102c55591a4942f512bce0c8
ms.sourcegitcommit: c89d8aa6d07d7aec002b58bd07a7976203aa760b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "94674474"
---
# <a name="introduction-to-azure-stack-hub-networking"></a>Azure Stack Hub ネットワークの概要

Azure Stack Hub には、単独でまたは組み合わせて使用できる、さまざまな種類のネットワーク機能が用意されています。

- **Azure Stack Hub リソースの間の接続**  
    クラウド内の安全でプライベートな仮想ネットワークで Azure のリソースを接続します。
- **インターネット接続**  
    インターネットを介して双方向で Azure Stack Hub リソースと通信します。
- **オンプレミスの接続**  
    インターネット上の仮想プライベート ネットワーク (VPN) を介して、または Azure Stack Hub への専用接続を介して、オンプレミスのネットワークを Azure Stack Hub リソースに接続します。 
    > [!IMPORTANT]
    > オンプレミスのリソースにアクセスするには、VPN またはパブリック IP 接続を作成する必要があります。
- **負荷分散とトラフィックの方向**  
    同じ場所のサーバー間でトラフィックを負荷分散したり、異なる場所のサーバーにトラフィックを誘導したりします。
- **Security**  
    ネットワーク サブネット間や個々の VM 間のネットワーク トラフィックをフィルタリングします。
- **ルーティング**  
    Azure Stack Hub リソースとオンプレミス リソースとの間で既定のルーティングまたは完全制御のルーティングを使用します。
- **管理の容易性**  
    Azure Stack Hub のネットワーク リソースを監視したり管理したりします。
- **デプロイ ツールと構成ツール**  
    Web ベースのポータルまたはクロスプラットフォームのコマンド ライン ツールを使って、ネットワーク リソースのデプロイと構成を行います。


## <a name="next-steps"></a>次の手順

* [Azure Stack Hub ネットワークに関する考慮事項](azure-stack-network-differences.md)