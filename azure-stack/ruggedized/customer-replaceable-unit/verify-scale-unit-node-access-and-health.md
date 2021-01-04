---
title: スケール ユニット ノードのアクセスと正常性の確認
description: スケール ユニット ノードのアクセスと正常性の確認方法について説明します
author: myoungerman
ms.topic: how-to
ms.date: 11/13/2020
ms.author: v-myoung
ms.reviewer: ''
ms.lastreviewed: ''
ms.openlocfilehash: cec29fed89fa02f6a5142f9eb179d8fc8437f37d
ms.sourcegitcommit: 3bd42be22e626564b62e560dc037aed4d462011f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2020
ms.locfileid: "97392962"
---
# <a name="verifying-scale-unit-node-access-and-health"></a>スケール ユニット ノードのアクセスと正常性の確認



特権アクセス ワークステーションにログインし、管理者ポータルを起動して、システム正常性を確認し、特権エンドポイントの IP アドレスを取得して、ドレインまたは再開する必要があるノードを特定します。

1.  リモート デスクトップを使用して、特権アクセス ワークステーションに接続します。

2.  Azure Stack Hub 管理者ポータルにアクセスします。

    顧客から取得した資格情報を使用して、Azure Stack Hub 管理者ポータルにログインします。
        
3.  特権エンドポイントの IP アドレスを取得します。


    **[Region Management]\(リージョン管理\)** タイルで、 **[プロパティ]** を選択します。 ペインの一番下までスクロールし、 **[Privileged endpoint IP addresses]\(特権エンドポイントの IP アドレス\)** フィールドで IP アドレスを見つけます。 これらは、この手順で後ほど必要になる場合や、問題が発生した場合にサポートで必要になることもあるため、メモしておきます。

    [![](media/image-18-inline.png)](media/image-18-expanded.png)
    
4.  現在のアラートを確認します。

    **[Region Management]\(リージョン管理\)** で、 **[アラート]** を選択して、現在のアラートを確認します。 予期しないアラートが存在する場合は、それらをクリアできるかまたは無視しても問題ないのかを Dell Technologie サポートに確認してください。
    
    [![](media/image-19-inline.png)](media/image-19-expanded.png)
    
5.  スケール ユニット ノードを特定します。

    サービス タグのみが提供され、Azure Stack Hub 管理ポータルから問題が発生しているノードを特定できない場合 (つまり、ノードの電源状態が既に停止している場合) は、次の手順を使用して、スケール ユニット ノードをサービス タグに関連付けます。
    
    1.  **[Region Management]\(リージョン管理\)** で、 **[スケール ユニット]** を選択し、クラスター **s-cluster** を選択します。 **[ノード]** を選択します。
    
    1.  ノード サービス タグを取得するには、 **[BMC]** IP アドレス リンクを選択します。これにより、サーバーの iDRAC Web インターフェイスが新しいタブまたはウィンドウに表示されます。

        [![](media/image-20-inline.png)](media/image-20-expanded.png) 
    
    1.  iDRAC インターフェイスにログインし、 **[システム情報]** ペインでノードのサービス タグを確認します。
    
    1.  すべてのノードに対してこの手順を繰り返し、これらのサービス タグと予定されているハードウェアの交換を関連付けて、処理する必要があるノードを特定します。

        [![](media/image-21-inline.png)](media/image-21-expanded.png)
    
