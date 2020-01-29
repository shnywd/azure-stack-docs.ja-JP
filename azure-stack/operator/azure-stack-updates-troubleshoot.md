---
title: Azure Stack Hub で更新プログラムをトラブルシューティングする | Microsoft Docs
description: Azure Stack Hub ができるだけ迅速に運用可能な状態に戻ることができるように、Azure Stack Hub オペレーターが更新プログラムに関する問題を解決する方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2019
ms.author: mabrigg
ms.lastreviewed: 09/23/2019
ms.reviewer: ppacent
ms.openlocfilehash: 94c6b6882c3b2f88c9815ab535fbef8b23811007
ms.sourcegitcommit: 320eddb281a36d066ec80d67b103efad7d4f33c8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2020
ms.locfileid: "76145769"
---
# <a name="best-practices-for-troubleshooting-azure-stack-hub-patch-and-update-issues"></a>Azure Stack Hub の修正プログラムと更新プログラムに関する問題のトラブルシューティングのベスト プラクティス

この記事では、Azure Stack Hub の修正プログラムと更新プログラムの問題に関するトラブルシューティングを行うためのベスト プラクティスの概要に加え、修正プログラムと更新プログラムのよくある問題の修復について説明します。


Azure Stack Hub の修正プログラムと更新プログラムのプロセスは、オペレーターが更新プログラム パッケージを一貫した合理的な方法で適用できるように設計されています。 まれに、修正プログラムや更新プログラムのプロセス中に問題が発生することがあります。 修正プログラムや更新プログラムのプロセス中に問題が発生した場合は、以下の手順を実行することをお勧めします。

0. **前提条件**:[更新プログラムのアクティビティのチェックリスト](release-notes-checklist.md)に従っていること、および[自動ログ収集を構成済み](azure-stack-configure-automatic-diagnostic-log-collection.md)であることを確認してください。
1. 更新が失敗したときに作成されたエラー アラートの修復手順に従います。
2. [Azure Stack Hub の修正プログラムと更新プログラムに関する一般的な問題](https://docs.microsoft.com/azure-stack/operator/azure-stack-updates-troubleshoot#Common-azure-stack-hub-patch-and-update-issues)を確認し、起きた問題が示されている場合は推奨されている操作を行います。
3. 上記の手順で問題を解決できない場合は、[Azure Stack Hub のサポート チケット](azure-stack-help-and-support-overview.md)を作成します。 問題が発生した期間に[収集されたログ](https://docs.microsoft.com/azure-stack/operator/azure-stack-configure-on-demand-diagnostic-log-collection)があることを確認してください。

## <a name="common-azure-stack-hub-patch-and-update-issues"></a>Azure Stack Hub の修正プログラムと更新プログラムに関する一般的な問題

*適用対象:Azure Stack Hub 統合システム*

### <a name="preparationfailed"></a>PreparationFailed

**適用先**: この問題は、サポートされているすべてのリリースに適用されます。

**原因**:Azure Stack Hub の更新プログラムをインストールしようとしたときに、更新が失敗して、状態が `PreparationFailed` に変更される場合があります。 インターネットに接続されたシステムの場合、これは通常、インターネット接続が脆弱であるために、更新プログラム パッケージが適切にダウンロードされないことを示します。 

**対応策**: **[今すぐインストール]** をもう一度クリックすることで、この問題を回避できます。 問題が解決しない場合は、[更新プログラムのインストール](azure-stack-apply-updates.md?#install-updates-and-monitor-progress)に関するセクションに従って、更新プログラム パッケージを手動でアップロードすることをお勧めします。

**発生頻度**: 共通

## <a name="next-steps"></a>次のステップ

- [Azure Stack Hub の更新](azure-stack-updates.md)  
- [Microsoft Azure Stack Hub のヘルプとサポート](azure-stack-help-and-support-overview.md)
