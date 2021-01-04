---
title: ハードウェア ライフサイクル ホストを操作するために必要な知識
description: ハードウェア ライフサイクル ホストを操作するために必要な知識について説明します。
author: myoungerman
ms.topic: how-to
ms.date: 11/13/2020
ms.author: v-myoung
ms.reviewer: ''
ms.lastreviewed: ''
ms.openlocfilehash: f4dc52cff6f04cdfdafedd845ccf40843687e7a3
ms.sourcegitcommit: 3bd42be22e626564b62e560dc037aed4d462011f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2020
ms.locfileid: "97392850"
---
# <a name="required-knowledge-for-working-with-the-hardware-lifecycle-host"></a>ハードウェア ライフサイクル ホストを操作するために必要な知識

FRU の手順を完了するには、次の概念とガイドに習熟し、アクセスできる必要があります。

## <a name="hardware-lifecycle-host"></a>ハードウェア ライフサイクル ホスト

ハードウェア ライフサイクル ホスト (HLH) は、Azure Stack Hub ラックの上部に搭載された物理的な管理サーバーです。 このホストにアクセスするには、次の 3 つの方法のいずれかを使用して接続します。

* Direct (クラッシュ カート)
* iDRAC (サービス ポート)
* iDRAC (IP アクセス)

データ センター内にいる場合は、VGA および USB の各ポートを使用して HLH に直接接続することができます。 たとえば、クラッシュ カートに接続するとします。

データ センター内にいる場合は、マイクロ USB ケーブルを使用して、自分のノート PC を iDRAC 9 サービス ポートに接続します。 詳細については、「直接 USB 接続経由で iDRAC インターフェイスにアクセスする」を参照してください。

顧客と協力して、その管理ネットワークおよび管理ワークステーションから iDRAC IP を介して HLH に接続してください。

> [!NOTE]
> HLH iDRAC に直接接続できるのは、あらかじめスイッチ ACL に追加されているネットワークからのみです。

## <a name="credentials"></a>資格情報

次の資格情報を取得するには、顧客と協力してください。

* HLH
* administrator
* iDRAC アカウント (省略可能)

完全な管理者権限を持つ Windows アカウント。

クラッシュ カートを使用してサーバーに直接接続していない場合、仮想 KVM にアクセスするには iDRAC アカウントの資格情報が必要です。

## <a name="dell-emc-poweredge-r640-installation-and-service-manual"></a>Dell EMC PowerEdge R640 設置およびサービス マニュアル

関連するハードウェアを物理的に交換する方法の詳細については、「[Dell](https://www.dell.com/support/manuals/us/en/04/poweredge-r640/per640_ism_pub/dell-emc-poweredge-r640-overview?guid=guid-f39be9ba-158c-45e3-b8b1-f07bb750d6d4)
 [EMC PowerEdge R640 設置およびサービス マニュアル](https://www.dell.com/support/manuals/us/en/04/poweredge-r640/per640_ism_pub/dell-emc-poweredge-r640-overview?guid=guid-f39be9ba-158c-45e3-b8b1-f07bb750d6d4)」の PowerEdge R640 FRU の交換手順を参照してください。
「[システム コンポーネントの取り付けと取り外し](https://www.dell.com/support/manuals/us/en/04/poweredge-r640/per640_ism_pub/installing-and-removing-system-components?guid=guid-5a5943c4-fe26-4faa-a10c-2afa4c1993ff&lang=en-us)」セクションを参照してください。

## <a name="microsoft-azure-stack-hub-tactical-cloud-appliance-service-manual"></a>Microsoft Azure Stack Hub Tactical Cloud Appliance サービス マニュアル

Microsoft Azure Stack Hub Tactical Cloud Appliance サービス マニュアルには、Tracewell Tactical Pods から Hardware Lifecycle Host を取り外すための手順が記載されています。

## <a name="dell-emc-poweredge-idrac"></a>Dell EMC PowerEdge iDRAC

Dell EMC PowerEdge iDRAC Web インターフェイスをナビゲートして使用する方法について理解しておく必要があります。

