---
title: Azure Stack Hub 診断ログを今すぐ送信する
description: 管理者ポータルまたは PowerShell スクリプトを使用して Azure Stack Hub 内で診断ログをオンデマンドで収集する方法について説明します。
author: justinha
ms.topic: article
ms.date: 03/30/2020
ms.author: justinha
ms.reviewer: shisab
ms.lastreviewed: 03/30/2020
ms.openlocfilehash: 2a46c031be648eedf42e204496ed2a123c926dd9
ms.sourcegitcommit: b65952127f39c263b162aad990e4d5b265570a7f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2020
ms.locfileid: "80402724"
---
# <a name="send-azure-stack-hub-diagnostic-logs-now"></a>Azure Stack Hub 診断ログを今すぐ送信する

Azure Stack オペレーターは、管理者ポータルまたは PowerShell を使用して、サポートを要求する前に Microsoft カスタマー サポート サービス (CSS) に診断ログをオンデマンドで送信することができます。 Azure Stack Hub が Azure に接続されている場合は、管理者ポータルの **[Send logs now]\(今すぐログを送信する\)** オプションをお勧めします。これは、この方法がログを Microsoft に直接送信する最も簡単な方法であるためです。 ポータルが使用できない場合、オペレーターは代わりに [Send-AzureStackDiagnosticLog を使用してログをすぐに送信する](azure-stack-configure-on-demand-diagnostic-log-collection-powershell-tzl.md)必要があります。 

インターネットに接続していない場合、またはローカルでのみログを保存する場合は、[Get-AzureStackLog](azure-stack-get-azurestacklog.md) メソッドを使用してログを送信します。 次のフローチャートは、それぞれの場合に診断ログを送信するために使用するオプションを示しています。 

![Microsoft にログを今すぐ送信する方法を示すフローチャート](media/azure-stack-help-and-support/send-logs-now-flowchart.png)

>[!NOTE]
>オンデマンドのログ収集の代わりとして、[診断ログを事前に収集する](azure-stack-configure-automatic-diagnostic-log-collection-tzl.md)ことによって、トラブルシューティングのプロセスを合理化できます。 システム正常性状態を調査する必要がある場合は、CSS でケースを開く前にログが分析のために自動的にアップロードされます。 事前ログ収集が有効になっている場合、 **[ヘルプとサポート]** に、ログ収集が進行中であるタイミングが表示されます。 事前ログ収集の進行中に、 **[Send logs now]\(今すぐログを送信する\)** をクリックして特定の時間からログを収集した場合、事前ログ収集の完了後にオンデマンドの収集が開始されます。

ログ収集の開始時刻と終了時刻を指定し、 **[Collect and Upload]\(収集してアップロード\)** をクリックします。 

![今すぐログを送信するオプションのスクリーンショット](media/azure-stack-help-and-support/send-logs-now.png)

## <a name="next-steps"></a>次のステップ

[特権エンドポイント (PEP) を使用して Azure Stack Hub 診断ログを送信する](azure-stack-configure-on-demand-diagnostic-log-collection-powershell-tzl.md)
