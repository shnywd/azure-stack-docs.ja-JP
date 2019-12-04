---
title: Azure Stack のハードウェアの正常性を監視する | Microsoft Docs
description: Azure Stack のハードウェア コンポーネントの正常性を監視する方法について説明します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: sethm
ms.reviewer: thoroet
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: b41a60aaf45d4b4fbbbf00945a4048dbf9dfb13a
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2019
ms.locfileid: "74308256"
---
# <a name="monitor-azure-stack-hardware-components"></a>Azure Stack のハードウェア コンポーネントを監視する

記憶域サブシステムの状態は、Azure Stack の正常性および監視システムによって既に監視されており、必要に応じてアラートが生成されます。 Azure Stack の 1910 リリースでは、正常性および監視システムによって、次のハードウェア コンポーネントについてもアラートが生成されるようになりました。

- システム ファン
- システム温度
- 電源装置
- CPU
- メモリ
- ブート ドライブ

> [!NOTE]
> この機能を有効にする前に、ハードウェアの準備ができていることをハードウェア パートナーと共に検証する必要があります。 ハードウェア パートナーは、BMC でこの機能を有効にするための詳細な手順も提供します。

## <a name="snmp-listener-scenario"></a>SNMP リスナーのシナリオ

SNMP v3 リスナーは、TCP ポート 162 上の 3 つの ERCS インスタンスすべてで実行されています。 Azure Stack リスナーに SNMP トラップを送信するように、ベースボード管理コントローラー (BMC) を構成する必要があります。 リージョンのプロパティ ビューを開くことで、管理ポータルから 3 つの PEP IP を取得できます。

リスナーにトラップを送信するには認証が必要であり、ベース BMC 自体にアクセスするときと同じ資格情報を使用する必要があります。

TCP ポート 162 上の 3 つの ERCS インスタンスのいずれかで SNMP トラップが受信されると、OID が内部的に照合され、アラートが生成されます。 Azure Stack の正常性および監視システムでは、ハードウェア パートナーによって定義された OID のみが受信されます。 Azure Stack にとって不明な OID の場合、アラートとは一致しません。

障害が発生したコンポーネントが交換されると、状態の変化を示すイベントが BMC から SNMP リスナーに送信され、Azure Stack のアラートは自動的にクローズされます。

> [!NOTE]
> ノードまたはマザーボード全体が交換された場合、既存のアラートは自動的にクローズされません。 出荷時の設定へのリセットなどによって BMC の構成が失われた場合にも同じことが当てはまります。

## <a name="next-steps"></a>次の手順

[ファイアウォールの統合](azure-stack-firewall.md)
