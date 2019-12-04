---
title: Azure Stack の DNS フォワーダーを更新する | Microsoft Docs
description: Azure Stack の DNS フォワーダーを更新する方法について説明します。
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
ms.openlocfilehash: 22e49f28dee6b4aa97b9e84cf52950dd678450e4
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2019
ms.locfileid: "74308244"
---
# <a name="update-the-dns-forwarder-in-azure-stack"></a>Azure Stack の DNS フォワーダーを更新する

Azure Stack インフラストラクチャで外部名を解決するには、少なくとも 1 つの到達可能な DNS フォワーダーが必要です。 Azure Stack のデプロイには、DNS フォワーダーを用意する必要があります。 その入力が Azure Stack の内部 DNS サーバーでフォワーダーとして使用され、認証、Marketplace 管理、使用などのサービスの外部名の解決が可能になります。

DNS は、変更可能な重要なデータセンター インフラストラクチャ サービスであり、変更した場合は Azure Stack を更新する必要があります。

この記事では、特権エンドポイント (PEP) を使用して Azure Stack の DNS フォワーダーを更新する方法について説明します。 2 つの信頼できる DNS フォワーダー IP アドレスを使用することをお勧めします。

1. [特権エンドポイント](azure-stack-privileged-endpoint.md)にアクセスします。 サポート チケットを開いて特権エンドポイントのロックを解除する必要はないことに注意してください。

2. 次のコマンドを実行して、現在構成されている DNS フォワーダーを確認します。 別の方法として、管理ポータルのリージョンのプロパティを使用することもできます。

   ```powershell
   Get-AzsDnsForwarder
   ```

3. 次のコマンドを実行して、新しい DNS フォワーダーを使用するように Azure Stack を更新します。

   ```powershell
    Set-AzsDnsForwarder -IPAddress "IPAddress 1","IPAddress 2"
   ```

4. エラーが発生した場合は、コマンドの出力を確認してください。

## <a name="next-steps"></a>次の手順

[ファイアウォールの統合](azure-stack-firewall.md)
