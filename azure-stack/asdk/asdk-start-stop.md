---
title: ASDK を起動および停止する | Microsoft Docs
description: Azure Stack Development Kit (ASDK) を起動および停止する方法について説明します。
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2019
ms.author: justinha
ms.reviewer: misainat
ms.lastreviewed: 07/18/2019
ms.openlocfilehash: e32fc6c58232e21eb6e7c087c2d95623209bc28d
ms.sourcegitcommit: 7817d61fa34ac4f6410ce6f8ac11d292e1ad807c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/02/2019
ms.locfileid: "74689848"
---
# <a name="start-and-stop-the-asdk"></a>ASDK を起動および停止する
ASDK ホスト コンピューターを単純に再起動することはお勧めしません。 代わりに、この記事の手順に従い、正しくシャット ダウンして ASDK サービスを再起動してください。

## <a name="stop-azure-stack"></a>Azure Stack の停止 
次の PowerShell コマンドを使用し、Azure Stack サービス、および ASDK ホスト コンピューターを正しくシャット ダウンします。

1. ASDK ホスト コンピューターで AzureStack\AzureStackAdmin としてログインします。
2. PowerShell を (PowerShell ISE ではなく) 管理者として開きます。
3. 次のコマンドを実行し、特権エンドポイント (PEP) セッションを確立します。 

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. 次に、PEP セッションでは **Stop AzureStack**コマンドレットを使用して、Azure Stack サービスを停止し、ASDK ホスト コンピューターをシャット ダウンします。

   ```powershell
   Stop-AzureStack
   ```
5. PowerShell の出力を確認し、ASDK ホスト コンピューターがシャット ダウンする前にすべての Azure Stack サービスが正常にシャット ダウンされたことを確認します。 シャットダウンのプロセスには数分かかります。

## <a name="start-azure-stack"></a>Azure Stack の開始 
ASDK サービスは、ホスト コンピューターが起動したときに自動的に開始する必要があります。 しかし、ASDK インフラストラクチャ サービスの起動時間は、ASDK ホスト コンピューターのハードウェア構成のパフォーマンスによって変わります。 すべてのサービスが正常に再起動するまでに、場合によっては数時間がかかることもあります。

ASDK のシャット ダウン方法に関係なく、次のステップを実行して、ホスト コンピューターの電源投入後に、すべての Azure Stack サービスが起動し完全に動作していることを確認する必要があります。 

1. ASDK ホスト コンピューターの電源を投入します。 
2. ASDK ホスト コンピューターで AzureStack\AzureStackAdmin としてログインします。
3. PowerShell を (PowerShell ISE ではなく) 管理者として開きます。
4. 次のコマンドを実行し、特権エンドポイント (PEP) セッションを確立します。

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
5. 次に、PEP セッションでは、次のコマンドを実行し Azure Stack サービスのスタートアップの状態を確認します。

   ```powershell
   Get-ActionStatus Start-AzureStack
   ```
6. 出力を確認して Azure Stack サービスが正常に再起動したことを確認してください。

Microsoft Azure Stack サービスの正しいシャットダウンと再起動について、推奨される手順の詳細を見るには、[Azure Stack の開始および停止](../operator/azure-stack-start-and-stop.md)をご覧ください。

## <a name="troubleshoot-startup-and-shutdown"></a>スタートアップおよびシャットダウンのトラブルシューティング 
ASDK ホスト コンピューターの電源投入後 2 時間以内に Azure Stack サービスが正常に起動しない場合は、次の手順に従います。

1. ASDK ホスト コンピューターで AzureStack\AzureStackAdmin としてログインします。
2. PowerShell を (PowerShell ISE ではなく) 管理者として開きます。
3. 次のコマンドを実行し、特権エンドポイント (PEP) セッションを確立します。

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. 次に、PEP セッションでは、次のコマンドを実行し Azure Stack サービスのスタートアップの状態を確認します。

   ```powershell
   Test-AzureStack
   ```
5. 出力を確認しエラーを解決します。 詳細については、[Azure Stack の検証テストの実行](../operator/azure-stack-diagnostic-test.md)に関するページをご覧ください。
6. PEP セッション内で **Start-AzureStack**コマンドレットを実行してから Azure Stack サービスを再起動します。

   ```powershell
   Start-AzureStack
   ```

**Start-AzureStack**の実行が失敗した場合、[Azure Stack サポート フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurestack)で ASDK トラブルシューティングのサポートを得てください。 

## <a name="next-steps"></a>次の手順 
Azure Stack 診断ツールと問題のログ記録については、「[Azure Stack の診断ツール](../operator/azure-stack-configure-on-demand-diagnostic-log-collection.md#use-the-privileged-endpoint-pep-to-collect-diagnostic-logs)」をご覧ください。
