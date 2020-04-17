---
title: Microsoft Azure Stack Hub のヘルプとサポートの概要
description: Microsoft Azure Stack Hub のサポートを受けます。
author: justinha
ms.topic: article
ms.date: 02/26/2020
ms.author: justinha
ms.reviewer: shisab
ms.lastreviewed: 02/26/2020
ms.openlocfilehash: d4d5fbd3979d7f30354cb3716e3277b895ddac78
ms.sourcegitcommit: a630894e5a38666c24e7be350f4691ffce81ab81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "79520616"
---
# <a name="microsoft-azure-stack-hub-help-and-support"></a>Microsoft Azure Stack Hub のヘルプとサポート

Azure Stack Hub のオペレーターは **[ヘルプとサポート]** を使用して診断ログを収集し、トラブルシューティングのために Microsoft に送信することができます。 Azure Stack Hub ポータルの **[ヘルプとサポート]** には、管理者ポータルからアクセスできます。 オペレーターが Azure Stack の詳細を参照し、サポート オプションを確認し、専門家の支援を得るためのリソースが用意されています。  

![管理者ポータルの [ヘルプとサポート] にアクセスする方法を示すスクリーンショット](media/azure-stack-help-and-support/help-and-support.png)

## <a name="help-resources"></a>ヘルプ リソース 

オペレーターは、 **[ヘルプとサポート]** を使用して、Azure Stack Hub の詳細を参照し、サポート オプションを確認し、専門家の支援を得ることができます。 

### <a name="things-to-try-first"></a>最初に試すべきこと

**[ヘルプとサポート]** の先頭には、新しい概念の確認、請求のしくみの理解、使用可能なサポート オプションの確認など、最初に試すことへのリンクがあります。 

![セルフサービスのサポート](media/azure-stack-help-and-support/get-support-tiles.png)

- **ドキュメント**。 [Azure Stack Hub のオペレーター ドキュメント](index.yml)には、仮想マシン、SQL データベース、Web アプリなどの Azure Stack Hub サービスを提供する方法を示す概念、ハウツー トピック、およびチュートリアルが含まれています。 

- **課金の詳細についてはこちらを参照してください**。 [使用量と課金](azure-stack-billing-and-chargeback.md)についてのヒントを得ます。

- **サポート オプション**。 Azure Stack Hub オペレーターは、あらゆる企業のニーズに対応できる幅広い [Azure サポート オプション](https://aka.ms/azstacksupport)から選択できます。 


### <a name="get-expert-help"></a>専門家のサポートを受ける 

統合システムについては、Microsoft と Microsoft の OEM (original equipment manufacturer) ハードウェア パートナーとの間で、統合されたエスカレーションと解決のプロセスをご用意しています。

クラウド サービスに問題がある場合は、Microsoft カスタマー サポート サービス (CSS) を通じてサポートを提供いたします。 管理者ポータルの右上隅の **[ヘルプ]** (疑問符) をクリックし、 **[ヘルプとサポート]** をクリックすると、 **[ヘルプとサポートの概要]** が開き、新しいサポート リクエストを送信できます。 サポート要求を作成するときに、Azure Stack Hub サービスが事前に選択されます。 グローバルな Azure portal を使うのではなく、このエクスペリエンスを使ってチケットを送信することをお客様に強くお勧めします。 

デプロイ、パッチ、更新プログラム、ハードウェア (現場交換可能ユニットを含む) や、ハードウェア ライフサイクル ホストで実行するソフトウェアなどのハードウェア ブランドのソフトウェアに問題がある場合は、はじめに OEM ハードウェア ベンダーにお問い合わせください。 その他の問題については、Microsoft CSS にお問い合わせください。

![統合システムの専門家のサポートを受ける](media/azure-stack-help-and-support/get-support-integrated.png)

ASDK については、[Azure Stack Hub MSDN フォーラム](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)でサポート関連の質問をすることができます。 

管理者ポータルの右上隅の **[ヘルプ]** (疑問符) をクリックし、 **[ヘルプとサポート]** をクリックすると、フォーラムへのリンクが含まれるヘルプとサポートの**概要**が開きます。 MSDN フォーラムは定期的にチェックされています。  
開発キットは評価環境であるため、Microsoft CSS を通した正式なサポートは提供されていません。

また、MSDN フォーラムにアクセスして問題について話し合ったり、オンライン トレーニングを受けて自分のスキルを向上させたりすることもできます。 

![専門家のサポートを受ける](media/azure-stack-help-and-support/get-support-cards.png)

### <a name="get-up-to-speed-with-azure-stack-hub"></a>Azure Stack Hub をすぐに理解する

この一連のチュートリアルは、ASDK と統合システムのどちらを実行しているかに応じてカスタマイズされているので、自分の環境をすぐに理解することができます。 

![サポート利用のチュートリアル](media/azure-stack-help-and-support/get-support-tutorials.png)

## <a name="diagnostic-log-collection"></a>診断ログの収集

Microsoft に診断ログを送信するには、次の2つの方法があります。 

- [ログを事前に送信する](azure-stack-configure-automatic-diagnostic-log-collection-tzl.md):有効にすると、特定の正常性アラートによってログの収集がトリガーされます 
- [今すぐログを送信する](azure-stack-configure-on-demand-diagnostic-log-collection-portal-tzl.md):ログ収集の時間枠として、特定のスライディング ウィンドウを手動で選択できます

![診断ログ収集オプションのスクリーンショット](media/azure-stack-help-and-support/banner-enable-automatic-log-collection.png)


## <a name="next-steps"></a>次のステップ

- [診断ログの収集](azure-stack-diagnostic-log-collection-overview-tzl.md)について学習する
- [クラウド ID を検索する](azure-stack-find-cloud-id.md)方法について学習する
- [Azure Stack Hub のトラブルシューティング](azure-stack-troubleshooting.md)について学習する
