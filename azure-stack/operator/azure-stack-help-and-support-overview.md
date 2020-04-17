---
title: Microsoft Azure Stack Hub のヘルプとサポートの概要
description: Microsoft Azure Stack Hub のサポートを受けます。
author: justinha
ms.topic: article
ms.date: 07/24/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 07/24/2019
ms.openlocfilehash: 9423411f92845e149a2cb3427597ab2aa541b5da
ms.sourcegitcommit: a630894e5a38666c24e7be350f4691ffce81ab81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "77699764"
---
# <a name="microsoft-azure-stack-hub-help-and-support"></a>Microsoft Azure Stack Hub のヘルプとサポート

Azure Stack Hub ポータルの **[ヘルプとサポート]** には、オペレーターが Azure Stack Hub の詳細を参照し、サポート オプションを確認し、専門家の支援を得るためのリソースが用意されています。 1907 リリース以降、オペレーターはヘルプとサポートを使用して、トラブルシューティングのための診断ログを収集することもできます。  

## <a name="help-resources"></a>ヘルプ リソース 

オペレーターは、 **[ヘルプとサポート]** を使用して、Azure Stack Hub の詳細を参照し、サポート オプションを確認し、専門家の支援を得ることもできます。 

### <a name="things-to-try-first"></a>最初に試すべきこと

**[ヘルプとサポート]** の先頭には、新しい概念の確認、請求のしくみの理解、使用可能なサポート オプションの確認など、最初に試すことへのリンクがあります。 

![セルフサービスのサポート](media/azure-stack-help-and-support/get-support-tiles.png)

- **ドキュメント**。 [Azure Stack Hub のオペレーター ドキュメント](index.yml)には、仮想マシン、SQL データベース、Web アプリなどの Azure Stack Hub サービスを提供する方法を示す概念、ハウツー トピック、およびチュートリアルが含まれています。 

- **課金の詳細についてはこちらを参照してください**。 [使用量と課金](azure-stack-billing-and-chargeback.md)についてのヒントを得ます。

- **サポート オプション**。 Azure Stack Hub オペレーターは、あらゆる企業のニーズに対応できる幅広い [Azure サポート オプション](https://aka.ms/azstacksupport)から選択できます。 

### <a name="get-expert-help"></a>専門家のサポートを受ける 

統合システムについては、Microsoft と Microsoft の OEM (original equipment manufacturer) ハードウェア パートナーとの間で、統合されたエスカレーションと解決のプロセスをご用意しています。

クラウド サービスに問題がある場合は、Microsoft カスタマー サポート サービス (CSS) を通じてサポートを提供いたします。 管理者ポータルの右上隅の **[ヘルプ]** (疑問符) をクリックし、 **[ヘルプとサポート]** をクリックすると、 **[ヘルプとサポートの概要]** が開き、新しいサポート リクエストを送信できます。 サポート要求を作成するときに、Azure Stack Hub サービスが事前に選択されます。 お客様には、パブリック Azure portal を使うのではなく、このエクスペリエンスを使ってチケットを送信することを強くお勧めします。 

デプロイ、パッチ、更新プログラム、ハードウェア (現場交換可能ユニットを含む) や、ハードウェア ライフサイクル ホストで実行するソフトウェアなどのハードウェア ブランドのソフトウェアに問題がある場合は、はじめに OEM ハードウェア ベンダーにお問い合わせください。 その他の問題については、Microsoft CSS にお問い合わせください。

![統合システムの専門家のサポートを受ける](media/azure-stack-help-and-support/get-support-integrated.png)

ASDK については、[Azure Stack Hub MSDN フォーラム](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)でサポート関連の質問をすることができます。 

管理者ポータルの右上隅の **[ヘルプ]** (疑問符) をクリックし、 **[ヘルプとサポート]** をクリックすると、フォーラムへのリンクが含まれるヘルプとサポートの**概要**が開きます。 MSDN フォーラムは定期的にチェックされています。  
開発キットは評価環境であるため、Microsoft CSS を通した正式なサポートは提供されていません。

![ASDK の専門家のサポートを受ける](media/azure-stack-help-and-support/get-support-asdk.png)

また、MSDN フォーラムにアクセスして問題について話し合ったり、オンライン トレーニングを受けて自分のスキルを向上させたりすることもできます。 

![専門家のサポートを受ける](media/azure-stack-help-and-support/get-support-cards.png)

### <a name="get-up-to-speed-with-azure-stack-hub"></a>Azure Stack Hub をすぐに理解する

この一連のチュートリアルは、ASDK と統合システムのどちらを実行しているかに応じてカスタマイズされているので、自分の環境をすぐに理解することができます。 

![サポート利用のチュートリアル](media/azure-stack-help-and-support/get-support-tutorials.png)

## <a name="diagnostic-log-collection"></a>診断ログの収集

1907 リリース以降、**ヘルプとサポート**にはログを収集するために 2 つの新しい方法があります。

- **自動収集**:有効にすると、特定の正常性アラートによってログの収集がトリガーされます 
- **Collect logs now (今すぐログを収集する)** :過去 7 日間の 1 時間から 4 時間のスライディング ウィンドウを選択できます

![診断ログ収集オプションのスクリーンショット](media/azure-stack-automatic-log-collection/azure-stack-log-collection-overview.png)

統合システムでは、Microsoft カスタマー サポート サービス (CSS) と診断ログを共有できます。 Azure Stack Development Kit (ASDK) は評価環境であるため、CSS ではサポートされません。 詳細については、[Azure Stack Hub 診断ログの収集の概要](azure-stack-diagnostic-log-collection-overview.md)に関するページを参照してください。



## <a name="help-and-support-for-earlier-releases-azure-stack-hub-pre-1905"></a>以前のリリースの Azure Stack Hub (1905 より前) に関するヘルプとサポート

前の Azure Stack Hub のリリースにも、[Azure Stack Hub のオペレーター ドキュメント](https://aka.ms/adminportaldocs)にリダイレクトされる **[ヘルプとサポート]** へのリンクがあります。

![サポート利用のチュートリアル](media/azure-stack-help-and-support/get-support-previous.png)

クラウド サービスに問題がある場合は、Microsoft カスタマー サポート サービス (CSS) を通じてサポートを提供いたします。 管理者ポータルの右上隅の **[ヘルプ]** (疑問符) をクリックし、 **[ヘルプとサポート]** をクリックしてから、 **[新しいサポート リクエスト]** をクリックして、CSS で新しいサポート リクエストを直接送信できます。

統合システムの場合は、Microsoft と OEM パートナーとの間に、調整されたエスカレーションと解決のプロセスがあります。 クラウド サービスに問題がある場合は、Microsoft CSS を通じてサポートが提供されます。 

デプロイ、パッチ、更新プログラム、ハードウェア (現場交換可能ユニットを含む) や、ハードウェア ライフサイクル ホストで実行するソフトウェアなどのハードウェア ブランドのソフトウェアに問題がある場合は、はじめに OEM ハードウェア ベンダーにお問い合わせください。 その他の問題については、Microsoft CSS にお問い合わせください。

開発キットについては、[Azure Stack Hub MSDN フォーラム](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)でサポート関連の質問をすることができます。 管理者ポータルの右上隅の **[ヘルプ]** (疑問符) をクリックし、 **[新しいサポート要求]** をクリックして、Azure Stack Hub コミュニティの専門家からのヘルプを得られます。
開発キットは評価環境であるため、Microsoft CSS を通した正式なサポートは提供されていません。

## <a name="next-steps"></a>次のステップ

- [Azure Stack Hub のトラブルシューティング](azure-stack-troubleshooting.md)を学習する