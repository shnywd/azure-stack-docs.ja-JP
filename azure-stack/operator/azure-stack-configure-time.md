---
title: Azure Stack のタイム サーバーを構成する | Microsoft Docs
description: Azure Stack のタイム サーバーを構成する方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 10/10/2019
ms.openlocfilehash: a70eaaf46988524f5323052a3f2ca90f5b7719e1
ms.sourcegitcommit: 5c92a669007ab4aaffe4484f1d8836a40340dde1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2019
ms.locfileid: "73636804"
---
# <a name="configure-the-time-server-for-azure-stack"></a>Azure Stack のタイム サーバーを構成する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*  

Azure Stack でのタイム サーバーを更新するには、特権エンドポイント (PEP) を使用できます。 2 つ以上の NTP サーバーの IP アドレスに解決されるホスト名を使用します。

Azure Stack では、ネットワーク タイム プロトコル (NTP) を使用して、インターネット上のタイム サーバーに接続します。 NTP サーバーからは、正確なシステム時刻が提供されます。 時刻は、Azure Stack の物理ネットワーク スイッチ、ハードウェア ライフサイクル ホスト、インフラストラクチャ サービス、および仮想マシン間で使用されます。 クロックが同期されていない場合、Azure Stack では、ネットワークと認証に関する重大な問題が発生する可能性があります。 ログ ファイル、ドキュメント、およびその他のファイルが、不正確なタイムスタンプによって作成される可能性があります。

Azure Stack で時刻を同期するには、タイム サーバー (NTP) を 1 つ指定する必要があります。 Azure Stack をデプロイするときに、NTP サーバーのアドレスを指定します。 時刻は、重要なデータセンター インフラストラクチャ サービスです。 サービスが変更された場合は、時刻を更新する必要があります。

> [!NOTE]
> Azure Stack では、時刻を同期できるタイム サーバー (NTP) は 1 つだけです。 Azure Stack で時刻を同期する NTP を複数指定することはできません。

## <a name="configure-time"></a>時刻の構成

1. [PEP に接続します](azure-stack-privileged-endpoint.md)。 
    > [!Note]  
    > サポート チケットを開いて、特権エンドポイントのロックを解除する必要はありません。

2. 次のコマンドを実行して、現在構成されている NTP サーバーを確認します。

    ```PowerShell
    Get-AzsTimeSource
    ```

3. 次のコマンドを実行して、新しい NTP サーバーを使用するように Azure Stack を更新し、時刻を直ちに同期します。

    > [!Note]  
    > この手順では、物理スイッチ上のタイム サーバーを更新しません

    ```PowerShell
    Set-AzsTimeSource -TimeServer NEWTIMESERVERIP -resync
    ```

4. エラーが発生した場合は、コマンドの出力を確認してください。


## <a name="next-steps"></a>次の手順

[対応状況レポートを表示する](azure-stack-validation-report.md)  
[Azure Stack の統合に関する一般的な考慮事項](azure-stack-datacenter-integration.md)  
