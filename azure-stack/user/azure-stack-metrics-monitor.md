---
title: Azure Stack の監視データの使用 | Microsoft Docs
description: Azure Stack からの監視データを使用するためのオプションについて説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2019
ms.author: mabrigg
ms.lastreviewed: 12/01/2018
ms.openlocfilehash: 64935022f8ace33bd4350ab4ef5421636ff3991e
ms.sourcegitcommit: b95983e6e954e772ca5267304cfe6a0dab1cfcab
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2019
ms.locfileid: "68418511"
---
# <a name="how-to-consume-monitoring-data-from-azure-stack"></a>Azure Stack の監視データの使用方法

*適用対象: Azure Stack 統合システム*

グローバル Azure の Azure Monitor と同様に、Azure Monitor パイプラインを使用して、監視データを 1 か所で確認します。 ただし、グローバル Azure で使用できるすべての監視データを Azure Stack で使用できるわけではありません。 この記事では、Azure Stack で監視データを使用するさまざまな方法の概要について説明します。
 
## <a name="options-for-data-consumption"></a>データの使用のオプション

| データ型 | Category | サポートされているサービス | アクセスの方法 |
|-------------------------------------------------------------|----------|------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Azure Monitor プラットフォーム レベルのメトリック | メトリック | [Azure Stack 上の Azure Monitor でサポートされるメトリック](azure-stack-metrics-supported.md) | REST API |
| ゲスト OS メトリック (Perf カウントなど) の計算 | メトリック | Windows VM と Linux VM | ストレージ テーブルまたは BLOB:<br>Windows または Linux の Azure Diagnostics <br>イベント ハブ:<br>Windows の Azure Diagnostics |
| Storage のメトリック | メトリック | Azure Storage | ストレージ テーブル:<br>Storage Analytics |
| アクティビティ ログ | events | すべての Azure サービス | REST API:<br>Azure Monitor イベント API |
| ゲスト OS ログ (IIS、ETW、syslog など) の計算 | events | Windows VM と Linux VM | ストレージ テーブルまたは BLOB:<br>Windows または Linux の Azure Diagnostics <br>イベント ハブ:<br>Windows の Azure Diagnostics |
| ストレージ ログ | events | Azure Storage | ストレージ テーブル:<br>Storage Analytics |

## <a name="next-steps"></a>次の手順

[Azure Stack 上の Azure Monitor](azure-stack-metrics-azure-data.md) について学習します。
