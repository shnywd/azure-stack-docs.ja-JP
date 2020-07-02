---
title: Azure Stack Hub 診断ログを今すぐ送信する
description: 管理者ポータルまたは PowerShell スクリプトを使用して Azure Stack Hub 内で診断ログをオンデマンドで収集する方法について説明します。
author: justinha
ms.topic: article
ms.date: 03/30/2020
ms.author: justinha
ms.reviewer: shisab
ms.lastreviewed: 03/30/2020
ms.openlocfilehash: 2c741440deb92be81497b34a7e485d4628501100
ms.sourcegitcommit: b2b0fe629d840ca8d5b6353a90f1fcb392a73bd5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2020
ms.locfileid: "85377180"
---
# <a name="send-azure-stack-hub-diagnostic-logs-now"></a>Azure Stack Hub 診断ログを今すぐ送信する

::: moniker range=">= azs-2002"

Azure Stack オペレーターは、管理者ポータルまたは PowerShell を使用して、サポートを要求する前に Microsoft サポートに診断ログをオンデマンドで送信することができます。 Azure Stack Hub が Azure に接続されている場合は、管理者ポータルの **[Send logs now]\(今すぐログを送信する\)** オプションをお勧めします。これは、この方法がログを Microsoft に直接送信する最も簡単な方法であるためです。 ポータルが使用できない場合、オペレーターは代わりに [Send-AzureStackDiagnosticLog を使用してログをすぐに送信する](azure-stack-configure-on-demand-diagnostic-log-collection-powershell-tzl.md)必要があります。 

インターネットに接続していない場合、またはローカルでのみログを保存する場合は、[Get-AzureStackLog](azure-stack-get-azurestacklog.md) メソッドを使用してログを送信します。 次のフローチャートは、それぞれの場合に診断ログを送信するために使用するオプションを示しています。 

![Microsoft にログを今すぐ送信する方法を示すフローチャート](media/azure-stack-help-and-support/send-logs-now-flowchart.png)

>[!NOTE]
>オンデマンドのログ収集の代わりとして、[診断ログを事前に収集する](azure-stack-configure-automatic-diagnostic-log-collection-tzl.md)ことによって、トラブルシューティングのプロセスを合理化できます。 システム正常性状態を調査する必要がある場合は、Microsoft サポートでケースを開く前にログが分析のために自動的にアップロードされます。 事前ログ収集が有効になっている場合、 **[ヘルプとサポート]** に、ログ収集が進行中であるタイミングが表示されます。 事前ログ収集の進行中に、 **[Send logs now]\(今すぐログを送信する\)** をクリックして特定の時間からログを収集した場合、事前ログ収集の完了後にオンデマンドの収集が開始されます。

ログ収集の開始時刻と終了時刻を指定し、 **[Collect and Upload]\(収集してアップロード\)** をクリックします。 

![今すぐログを送信するオプションのスクリーンショット](media/azure-stack-help-and-support/send-logs-now.png)


::: moniker-end
::: moniker range="<= azs-1910"
## <a name="use-help-and-support-to-collect-diagnostic-logs-on-demand"></a>ヘルプとサポートを使用して必要に応じて診断ログを収集する

問題のトラブルシューティングを行うために、Microsoft サポートは、Azure Stack Hub オペレーターに、前週の特定の時間枠に対して、オンデマンドで診断ログを収集するように要求する場合があります。 その場合、Microsoft サポートはオペレーターにコレクションをアップロードするための SAS URL を提供します。 
Microsoft サポートからの SAS URL を使用して、オンデマンドのログ収集を構成するには、次の手順に従います。

1. **[Help and Support Overview]\(ヘルプとサポートの概要\)** を開き、 **[Collect logs now]\(今すぐログを収集する\)** をクリックします。 
1. 過去 7 日間の 1 時間から 4 時間のスライディング ウィンドウを選択します。 
1. ローカル タイム ゾーンを選択します。
1. Microsoft サポートから提供された SAS URL を入力します。

   ![オンデマンドのログ収集のスクリーンショット](media/azure-stack-automatic-log-collection/collect-logs-now.png)

>[!NOTE]
>自動診断ログ収集が有効になっている場合、 **[ヘルプとサポート]** に、ログ収集が進行中であるタイミングが表示されます。 自動ログ収集の進行中に、 **[Collect logs now]\(今すぐログを収集する\)** をクリックして特定の時間からログを収集した場合、自動ログ収集の完了後にオンデマンドの収集が開始されます。 


::: moniker-end


## <a name="next-steps"></a>次のステップ

[特権エンドポイント (PEP) を使用して Azure Stack Hub 診断ログを送信する](azure-stack-configure-on-demand-diagnostic-log-collection-powershell-tzl.md)

