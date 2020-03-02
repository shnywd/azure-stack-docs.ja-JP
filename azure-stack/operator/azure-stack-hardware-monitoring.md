---
title: Azure Stack Hub のハードウェアの正常性を監視する
description: Azure Stack Hub のハードウェア コンポーネントの正常性を監視する方法を学習します。
author: sethmanheim
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: sethm
ms.reviewer: thoroet
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: 4fe942e6a4230d9807780507de8b658a6e0ed4c3
ms.sourcegitcommit: c653efe7cd5177bf61c1a321ba7f38c0cdf90346
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/25/2020
ms.locfileid: "77575124"
---
# <a name="monitor-azure-stack-hub-hardware-components"></a>Azure Stack Hub のハードウェア コンポーネントを監視する

Azure Stack Hub の正常性および監視システムは、記憶域サブシステムの状態を監視し、必要に応じてアラートを生成します。 正常性および監視システムは、次のハードウェア コンポーネントについてもアラートを生成できます。

- システム ファン
- システム温度
- 電源装置
- CPU
- メモリ
- ブート ドライブ

> [!NOTE]
> この機能を有効にする前に、ハードウェアの準備ができていることをハードウェア パートナーと共に検証する必要があります。 ハードウェア パートナーは、BMC でこの機能を有効にするための詳細な手順も提供します。

## <a name="snmp-listener-scenario"></a>SNMP リスナーのシナリオ

SNMP v3 リスナーは、TCP ポート 162 上の 3 つの ERCS インスタンスすべてで実行されています。 Azure Stack Hub リスナーに SNMP トラップを送信するように、ベースボード管理コントローラー (BMC) を構成する必要があります。 リージョンのプロパティ ビューを開くことで、管理ポータルから 3 つの PEP IP を取得できます。

リスナーにトラップを送信するには認証が必要であり、ベース BMC 自体にアクセスするときと同じ資格情報を使用する必要があります。

TCP ポート 162 上の 3 つの ERCS インスタンスのいずれかで SNMP トラップが受信されると、OID が内部的に照合され、アラートが生成されます。 Azure Stack Hub の正常性および監視システムでは、ハードウェア パートナーによって定義された OID のみが受け入れられます。 OID は、Azure Stack Hub で認識されない場合、アラートと一致しません。

問題のあるコンポーネントが交換されると、状態の変化を示すイベントが BMC から SNMP リスナーに送信され、Azure Stack Hub のアラートは自動的にクローズされます。

> [!NOTE]
> ノードまたはマザーボード全体が交換された場合、既存のアラートは自動的にクローズされません。 出荷時の設定へのリセットなどによって BMC の構成が失われた場合にも同じことが当てはまります。

## <a name="next-steps"></a>次のステップ

[ファイアウォールの統合](azure-stack-firewall.md)
