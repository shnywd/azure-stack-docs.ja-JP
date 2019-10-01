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
ms.openlocfilehash: 76c6c21c2a728004d2a33c04a02b905ec674b972
ms.sourcegitcommit: d967cf8cae320fa09f1e97eeb888e3db5b6e7972
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301246"
---
# <a name="troubleshooting-patch-and-update-issues-for-azure-stack"></a>Azure Stack の修正プログラムと更新プログラムに関する問題のトラブルシューティング

*適用対象:Azure Stack 統合システム*

この記事のガイダンスを使用することで、Azure Stack を更新するときに発生する問題を解決できます。

## <a name="preparationfailed"></a>PreparationFailed

**適用先**: この問題は、サポートされているすべてのリリースに適用されます。

**原因**:Azure Stack の更新プログラムをインストールしようとしたときに、更新が失敗して、状態が `PreparationFailed` に変更される場合があります。 これは、更新リソース プロバイダー (URP) が、処理のためにストレージ コンテナーから内部インフラストラクチャ共有にファイルを正しく転送できないことが原因です。

**対応策**: バージョン 1901 (1.1901.0.95) 以降、この問題は、 **[今すぐ更新]** ( **[再開]** ではない) をもう一度クリックすることで回避できるようになりました。 それにより、URP は前回の試行のファイルをクリーンアップして、ダウンロードを再度開始します。 問題が解決しない場合は、[更新プログラムのインストール](azure-stack-apply-updates.md?#install-updates-and-monitor-progress)に関するセクションに従って、更新プログラム パッケージを手動でアップロードすることをお勧めします。

**発生頻度**: 一般

## <a name="next-steps"></a>次の手順

- [Azure Stack を更新する](azure-stack-updates.md)  
- [Microsoft Azure Stack のヘルプとサポート](azure-stack-help-and-support-overview.md)
