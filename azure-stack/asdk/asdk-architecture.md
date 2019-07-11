---
title: Azure Stack Development Kit のアーキテクチャ | Microsoft Docs
description: Azure Stack Development Kit (ASDK) のアーキテクチャについて説明します。
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 06/28/2019
ms.openlocfilehash: 4cd20ed4c76b062ea2fcfdf3d314ef2bcdecaff0
ms.sourcegitcommit: eccbd0098ef652919f357ef6dba62b68abde1090
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/01/2019
ms.locfileid: "67492343"
---
# <a name="microsoft-azure-stack-development-kit-architecture"></a>Microsoft Azure Stack Development Kit のアーキテクチャ
Azure Stack Development Kit (ASDK) は、1 台のホスト コンピューター上で実行される Azure Stack の単一ノード デプロイです。 Azure Stack に NAT および VPN 機能を提供するために、ホスト コンピューターにエッジ ルーティング コンポーネントがインストールされます。 Azure Stack インフラストラクチャ ロールは、物理ホスト コンピューターの Hyper-V レイヤーで実行されます。


## <a name="virtual-machine-roles"></a>仮想マシンのロール
ASDK は、開発キットのホスト コンピューターにホストされている次の VM を使ってサービスを提供します。

| Name | 説明 |
| ----- | ----- |
| **AzS-ACS01** | Azure Stack ストレージ サービス。|
| **AzS-ADFS01** | Active Directory フェデレーション サービス (ADFS)。  |
| **AzS-CA01** | Azure Stack ロール サービス用の証明機関サービス。|
| **AzS-DC01** | Microsoft Azure Stack 用の Active Directory、DNS、DHCP サービス。|
| **AzS-ERCS01** | 緊急回復コンソールの VM。 |
| **AzS-GWY01** | テナント ネットワーク用の VPN サイト間接続などのエッジ ゲートウェイ サービス。|
| **AzS-NC01** | ネットワーク コントローラー。Azure Stack ネットワーク サービスを管理します。  |
| **AzS-SLB01** | テナントおよび Azure Stack インフラストラクチャ サービス用の、Azure Stack の負荷分散マルチプレクサー サービス。  |
| **AzS-SQL01** | Azure Stack インフラストラクチャ ロール用の内部データ ストア。  |
| **AzS-WAS01** | Azure Stack 管理ポータルと Azure Resource Manager のサービス。|
| **AzS-WASP01**| Azure Stack ユーザー (テナント) ポータルと Azure Resource Manager のサービス。|
| **AzS-XRP01** | Microsoft Azure Stack 用のインフラストラクチャ管理コントローラー。計算、ネットワーク、ストレージのリソースプロバイダーなどがあります。|
| **AzS-SRNG01** | Azure Stack のログ コレクション サービスをホストする Ring VM のサポート。 |

## <a name="next-steps"></a>次の手順
[基本的な ASDK 管理タスクについて学習する](asdk-admin-basics.md)
