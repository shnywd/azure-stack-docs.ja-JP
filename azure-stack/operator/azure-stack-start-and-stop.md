---
title: 開始と停止
titleSuffix: Azure Stack
description: Azure Stack を開始および停止する方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 43BF9DCF-F1B7-49B5-ADC5-1DA3AF9668CA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: misainat
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: dbd85b4d7b63edb89cc327fd5b1f5592c24db508
ms.sourcegitcommit: 08d2938006b743b76fba42778db79202d7c3e1c4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2019
ms.locfileid: "74954385"
---
# <a name="start-and-stop-azure-stack"></a>Azure Stack の開始および停止

Azure Stack サービスを正しくシャットダウンして再起動するには、この記事の手順に従ってください。 "*停止*" は、Azure Stack 環境全体を物理的にシャットダウンして電源を切ります。 "*開始*" は、すべてのインフラストラクチャ ロールの電源が入れられ、テナントのリソースがシャットダウン前の電源状態に戻されます。

## <a name="stop-azure-stack"></a>Azure Stack の停止

Azure Stack は次の手順で停止 (シャットダウン) します。

1. 将来のシャットダウンに備えて、Azure Stack 環境のテナント リソースで実行しているすべてのワークロードを準備します。

2. Azure Stack ERCS VM へのネットワーク アクセス権を持つマシンから特権エンドポイント セッション (PEP) を開きます。 手順については、「[Azure Stack での特権エンドポイントの使用](azure-stack-privileged-endpoint.md)」を参照してください。

3. PEP から次を実行します。

    ```powershell
      Stop-AzureStack
    ```

4. Azure Stack のすべての物理ノードの電源がオフになるまで待ちます。

> [!Note]
> 物理ノードの電源の状態は、お使いの Azure Stack ハードウェアを提供した相手先ブランド供給 (OEM) の手順に従って確認できます。

## <a name="start-azure-stack"></a>Azure Stack の開始

Azure Stack は次の手順で開始します。 Azure Stack を停止した方法に関係なくこれらの手順に従います。

1. Azure Stack 環境内の各物理ノードの電源を入れます。 物理ノードの電源投入手順は、お使いの Azure Stack のハードウェアを提供した OEM の手順に従って確認します。

2. Azure Stack インフラストラクチャ サービスが開始されるまで待ちます。 Azure Stack インフラストラクチャ サービスは、開始プロセスの終了に 2 時間かかることがあります。 Azure Stack の開始状態は、[**Get-ActionStatus** コマンドレット](#get-the-startup-status-for-azure-stack)を使って確認できます。

3. すべてのテナント リソースがシャットダウン前の状態に戻ったことを確認します。 ワークロード マネージャーによる起動後に、テナント リソースで実行しているワークロードの再構成が必要な場合があります。

## <a name="get-the-startup-status-for-azure-stack"></a>Azure Stack のスタートアップ状態の取得

Azure Stack のスタートアップ ルーチンのスタートアップは次の手順で取得します。

1. Azure Stack ERCS VM へのネットワーク アクセス権を持つマシンから特権エンドポイント セッションを開きます。

2. PEP から次を実行します。

    ```powershell
      Get-ActionStatus Start-AzureStack
    ```

## <a name="troubleshoot-startup-and-shutdown-of-azure-stack"></a>Azure Stack のスタートアップおよびシャットダウンのトラブルシューティング

Azure Stack 環境の電源投入後 2 時間経ってもインフラストラクチャおよびテナントのサービスが正常に開始されない場合は、次の手順を実行します。

1. Azure Stack ERCS VM へのネットワーク アクセス権を持つマシンから特権エンドポイント セッションを開きます。

2. 次のコマンドを実行します。

    ```powershell
      Test-AzureStack
      ```

3. 出力を確認し、正常性エラーを解決します。 詳細については、[Azure Stack の検証テストの実行](azure-stack-diagnostic-test.md)に関するページをご覧ください。

4. 次のコマンドを実行します。

    ```powershell
      Start-AzureStack
    ```

5. **Start-AzureStack** の実行に失敗した場合は、Microsoft サポートにお問い合わせください。

## <a name="next-steps"></a>次の手順

[Azure Stack 診断ツール](azure-stack-configure-on-demand-diagnostic-log-collection.md#use-the-privileged-endpoint-pep-to-collect-diagnostic-logs)の詳細を確認する
