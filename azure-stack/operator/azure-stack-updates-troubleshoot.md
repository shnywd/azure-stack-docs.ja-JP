---
title: Azure Stack で更新プログラムをトラブルシューティングする | Microsoft Docs
description: Azure Stack ができるだけ迅速に運用可能な状態に戻ることができるように、Azure Stack オペレーターが更新プログラムに関する問題を解決する方法について説明します。
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
ms.openlocfilehash: d5606e7904fe311a54d792a18e5d4029c709b33c
ms.sourcegitcommit: 0866555e0ed240a65595052899ef1b836dd07fbc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2019
ms.locfileid: "72257742"
---
# <a name="troubleshooting-patch-and-update-issues-for-azure-stack"></a>Azure Stack の修正プログラムと更新プログラムに関する問題のトラブルシューティング

*適用対象:Azure Stack 統合システム*

この記事のガイダンスを使用することで、Azure Stack を更新するときに発生する問題を解決できます。

## <a name="preparationfailed"></a>PreparationFailed

**適用先**: この問題は、サポートされているすべてのリリースに適用されます。

**原因**:Azure Stack の更新プログラムをインストールしようとしたときに、更新が失敗して、状態が `PreparationFailed` に変更される場合があります。 インターネットに接続されたシステムの場合、これは通常、インターネット接続が脆弱であるために、更新プログラム パッケージが適切にダウンロードされないことを示します。 

**対応策**: **[今すぐインストール]** をもう一度クリックすることで、この問題を回避できます。 問題が解決しない場合は、[更新プログラムのインストール](azure-stack-apply-updates.md?#install-updates-and-monitor-progress)に関するセクションに従って、更新プログラム パッケージを手動でアップロードすることをお勧めします。

**発生頻度**: 一般

## <a name="next-steps"></a>次の手順

- [Azure Stack を更新する](azure-stack-updates.md)  
- [Microsoft Azure Stack のヘルプとサポート](azure-stack-help-and-support-overview.md)
