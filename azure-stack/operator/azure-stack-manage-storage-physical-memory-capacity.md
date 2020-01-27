---
title: Azure Stack Hub の物理メモリ容量を管理する | Microsoft Docs
description: Azure Stack Hub の物理メモリと容量を監視および管理する方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 84518E90-75E1-4037-8D4E-497EAC72AAA1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 1/22/2020
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 24fdcfc4b28a1c5fb9ebb2d7facb774ec176de05
ms.sourcegitcommit: a1abc27a31f04b703666de02ab39ffdc79a632f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2020
ms.locfileid: "76535146"
---
# <a name="manage-physical-memory-capacity-in-azure-stack-hub"></a>Azure Stack Hub の物理メモリ容量を管理する

Azure Stack Hub で使用可能な合計メモリ容量を増やすために、メモリを増設できます。 Azure Stack Hub では、物理サーバーは "*スケール ユニット ノード*" と呼ばれることもあります。 単一のスケール ユニットに属しているすべてのスケール ユニット ノードには、同量のメモリが確保されている必要があります。

> [!note]  
> 先に進む前に、ご利用のハードウェア製造元のドキュメントを参照し、その製造元が、物理メモリのアップグレードをサポートしているかどうかを確かめてください。 ご利用の OEM ハードウェア ベンダーのサポート契約で、物理サーバー ラックの配置とデバイス ファームウェアの更新がベンダーに義務付けられている場合があります。

次のフロー図は、各スケール ユニット ノードにメモリを増設するための一般的なプロセスを示しています。

![個々のスケール ユニット ノードにメモリを増設するプロセス](media/azure-stack-manage-storage-physical-capacity/process-to-add-memory-to-scale-unit.png)

## <a name="add-memory-to-an-existing-node"></a>既存のノードへのメモリの増設
次の手順は、メモリを増設するプロセスの概要を示しています。

> [!Warning]
> OEM 提供のマニュアルを参照せずにこれらの手順を実行しないでください。
> 
> [!Warning]
> メモリのローリング アップグレードはサポートされていないため、スケール ユニット全体をシャットダウンしてください。

1. 記事「[Azure Stack Hub の開始および停止](azure-stack-start-and-stop.md)」に記載されている手順に従って、Azure Stack Hub を停止します。
2. ハードウェア製造元のドキュメントを参照して、各物理コンピューターのメモリをアップグレードします。
3. 記事「[Azure Stack Hub の開始および停止](azure-stack-start-and-stop.md)」に記載されている手順に従って、Azure Stack Hub を開始します。

## <a name="next-steps"></a>次のステップ

 - Azure Stack Hub のストレージ アカウントを管理する方法については、[Azure Stack Hub でのストレージ アカウントの管理](azure-stack-manage-storage-accounts.md)に関する記事を参照してください。
 - Azure Stack Hub のデプロイのストレージ容量を監視および管理する方法については、「[Azure Stack Hub のストレージ容量を管理する](azure-stack-manage-storage-shares.md)」を参照してください。
