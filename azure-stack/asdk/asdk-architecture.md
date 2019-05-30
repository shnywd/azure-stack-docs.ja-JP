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
ms.date: 01/21/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: efb77c054bbd3e0c1f46a6bb10aef34ae043216c
ms.sourcegitcommit: 797dbacd1c6b8479d8c9189a939a13709228d816
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2019
ms.locfileid: "66267054"
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


## <a name="next-steps"></a>次の手順
[基本的な ASDK 管理タスクについて学習する](asdk-admin-basics.md)
