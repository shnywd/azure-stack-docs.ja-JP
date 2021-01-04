---
title: スケール ユニット ノードのディスク正常性の確認
description: スケール ユニット ノードのディスク正常性を確認する方法について説明します
author: myoungerman
ms.topic: how-to
ms.date: 11/13/2020
ms.author: v-myoung
ms.reviewer: ''
ms.lastreviewed: ''
ms.openlocfilehash: bebff3ff8970a43d0893f057a94ec3dfbf0a7fc9
ms.sourcegitcommit: 3bd42be22e626564b62e560dc037aed4d462011f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2020
ms.locfileid: "97392842"
---
# <a name="verifying-scale-unit-node-disk-health"></a>スケール ユニット ノードのディスク正常性の確認

1.  特権エンドポイント (PEP) に接続します。

    1.  PEP に接続する方法については、「特権アクセス ワークステーションと特権エンドポイント アクセス」を参照してください。

    1.  接続したら、PEP セッション `Enter-PSSession -Session $pep` を入力します。

2.  仮想ディスクの正常性を取得します。

    1.  `Get-VirtualDisk -cimsession "S-Cluster"` を実行して、仮想ディスクの正常性を確認します。

        システムで **OperationalStatus** の **OK** と **HealthStatus** の **Healthy** が返されない場合は、数分待ってからコマンドを再実行します。
        
        ![](media/image-57.png)
        
    1.  `Get-VirtualDisk -cimsession "S-Cluster" | Get-StorageJob` を実行して、実行中のすべての記憶域ジョブが完了していることを確認します。
    
    1.  結果が返されないことを確認します。 **JobState** で示されているようにジョブがまだ実行中の場合、またはすべてのジョブが完了とマークされている場合は、10 分待ってから、同じコマンドをもう一度実行します。 最終的な状態は、ジョブが一覧表示されないことです。
    
    1.  必要な場合は、「[Azure Stack Hub PowerShell を使用した仮想ディスクの修復状態の確認](https://docs.microsoft.com/azure-stack/operator/azure-stack-replace-disk?view=azs-2002&check-the-status-of-virtual-disk-repair-using-azure-stack-hub-powershell)」で記憶域の正常性確認の追加手順を確認できます。
        
