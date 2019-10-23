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
ms.openlocfilehash: cc432538715c1c990a9efe6473b33303deb78734
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/11/2019
ms.locfileid: "72280554"
---
# <a name="configure-the-time-server-for-azure-stack"></a>Azure Stack のタイム サーバーを構成する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*  

Azure Stack でのタイム サーバーを更新するには、特権エンドポイント (PEP) を使用できます。 2 つ以上の NTP サーバーの IP アドレスに解決されるホスト名を使用します。

Azure Stack では、ネットワーク タイム プロトコル (NTP) を使用して、インターネット上のタイム サーバーに接続します。 NTP サーバーからは、正確なシステム時刻が提供されます。 時刻は、Azure Stack の物理ネットワーク スイッチ、ハードウェア ライフサイクル ホスト、インフラストラクチャ サービス、および仮想マシン間で使用されます。 クロックが同期されていない場合、Azure Stack では、ネットワークと認証に関する重大な問題が発生する可能性があります。 ログ ファイル、ドキュメント、およびその他のファイルが、不正確なタイムスタンプによって作成される可能性があります。

時刻を同期するために、Azure Stack には少なくとも 1 つのタイム サーバー (NTP) が必要です。 Azure Stack をデプロイするときに、NTP サーバーのアドレスを指定します。 時刻は、重要なデータセンター インフラストラクチャ サービスです。 サービスが変更された場合は、時刻を更新する必要があります。

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
