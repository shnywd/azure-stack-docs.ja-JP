---
title: Azure Stack のためのキャパシティ プランニングの概要 | Microsoft Docs
description: Azure Stack デプロイのための容量計画について説明します。
services: azure-stack
documentationcenter: ''
author: prchint
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 05/31/2019
ms.openlocfilehash: 0d2610d2082973f4ab255027cc299b5858660c58
ms.sourcegitcommit: 80775f5c5235147ae730dfc7e896675a9a79cdbe
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/03/2019
ms.locfileid: "66461078"
---
# <a name="overview-of-azure-stack-capacity-planning"></a>Azure Stack のキャパシティ プランニングの概要

Azure Stack ソリューションを評価するときに、Azure Stack クラウドのキャパシティ全体に直接影響するハードウェア構成を選択することができます。 CPU、メモリ密度、ストレージ構成、全体的なソリューションの規模 (つまり、サーバー数) といった従来の選択肢があります。 以前の仮想化ソリューションとは異なり、これらのコンポーネントを単純に計算して使用可能なキャパシティを決定することはできません。 その 1 つ目の理由は、Azure Stack が、ソリューション自体の中でインフラストラクチャまたは管理コンポーネントをホストするように設計されているためです。 2 つ目の理由は、ソリューションの容量の一部が、回復性をサポートするために予約され、テナントのワークロードの中断を最小限に抑える方法で、ソリューションのソフトウェアが更新されることです。 

> [!IMPORTANT]
> このキャパシティ プランニングの情報と [Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner) は Azure Stack の計画と構成を決定するための始点です。 これは、独自の調査や分析に代わるものとしては利用できません。 Microsoft は、ここで提供される情報に対して、明示または黙示を問わず、一切の表明または保証を行わないものとします。
 
Azure Stack ソリューションは、コンピューティングおよびストレージのハイパーコンバージド クラスターとして構築されています。 この収束により、"*スケール ユニット*" と呼ばれる、クラスター内でのハードウェア容量の共有が可能になります。 Azure Stack では、スケール ユニットにより、リソースの可用性とスケーラビリティが提供されます。 スケール ユニットは、"*ホスト*" と呼ばれる、Azure Stack サーバーのセットから構成されます。 このインフラストラクチャ ソフトウェアは一連の VM 内でホストされ、テナント VM と同じ物理サーバーを共有します。 その後、すべての Azure Stack VM はスケール ユニットの Windows Server クラスタリング テクノロジと個々の Hyper-V インスタンスによって管理されます。 スケール ユニットにより、Azure Stack の習得と管理が簡素化されます。 また、スケール ユニットにより、Azure Stack 全体でのすべてのサービス (テナントとインフラストラクチャ) の移動とスケーラビリティも実現されます。 

以下のトピックでは、各コンポーネントについて詳しく説明しています。

- [Azure Stack コンピューティング](azure-stack-capacity-planning-compute.md)
- [Azure Stack ストレージ](azure-stack-capacity-planning-storage.md)
- [Azure Stack Capacity Planner](azure-stack-capacity-planner.md)
