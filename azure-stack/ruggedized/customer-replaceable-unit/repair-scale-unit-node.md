---
title: スケール ユニット ノードの電源オンと修復
description: スケール ユニット ノードを電源オンにして修復する方法について説明します
author: myoungerman
ms.topic: how-to
ms.date: 11/13/2020
ms.author: v-myoung
ms.reviewer: ''
ms.lastreviewed: ''
ms.openlocfilehash: 255a40bc2439ae1a6995d4ddf89df192d5e57551
ms.sourcegitcommit: 3bd42be22e626564b62e560dc037aed4d462011f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2020
ms.locfileid: "97393018"
---
# <a name="powering-on-and-repairing-a-scale-unit-node"></a>スケール ユニット ノードの電源オンと修復

**手順**

スケール ユニット ノードを修復して運用環境に戻すには、Azure Stack Hub の修復手順を行う必要があります。

> [!NOTE]
> この修復手順の所要時間は約 3 時間です。

1.  **管理ポータル** で、ノードを選択し、 **[修復]** を選択します。

    ![](media/image-52.png)

1.  修復するノードに対応する **BMC IP アドレス** を指定して、 **[修復]** を選択します。

    ![](media/image-53.png)

1.  通知ウィンドウで進行状況を監視します。

    ![](media/image-54.png)
    
    
    > [!NOTE]
    > 修復手順が 3 時間以内に完了しなかった場合、または問題が発生した場合は、Dell Technologies サポートでケースを作成します。ここから、さらなるトラブルシューティングに向けた Microsoft サポートとの連携が行われる可能性があります。
    
    修復処理が完了すると、**実行中の動作状態** に戻ります。
    
