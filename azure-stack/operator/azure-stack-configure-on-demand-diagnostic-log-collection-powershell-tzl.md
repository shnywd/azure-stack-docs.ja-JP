---
title: 特権エンドポイント (PEP) を使用して Azure Stack Hub 診断ログを Azure に送信する
description: 特権エンドポイント (PEP) を使用して Azure Stack Hub 診断ログを Azure に送信する方法について説明します。
author: justinha
ms.topic: article
ms.date: 03/05/2020
ms.author: justinha
ms.reviewer: shisab
ms.lastreviewed: 03/05/2020
ms.openlocfilehash: ca6240cb4f1e54c5e5fbfda79c46697909612063
ms.sourcegitcommit: a630894e5a38666c24e7be350f4691ffce81ab81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "79520562"
---
# <a name="send-azure-stack-hub-diagnostic-logs-to-azure-using-the-privileged-endpoint-pep"></a>特権エンドポイント (PEP) を使用して Azure Stack Hub 診断ログを Azure に送信する

この方法を使用する場合、ターゲット記憶域の場所を指定する必要はありません。 **fromDate** と **toDate** のパラメーターが指定されていない場合、実行時刻から最後の 4 時間が既定値になります。 

必要に応じて **FilterByRole** または **FilterByResourceProvider** パラメーターを指定して、特定のロールまたは付加価値 RP ログのみを含めることができます。 

PEP を使用して統合システムでログを収集するのに実行できるスクリプト例を次に示します。 


```powershell
$ipAddress = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP address. 
 
$password = ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force 
$cred = New-Object -TypeName System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $password) 
 
$session = New-PSSession -ComputerName $ipAddress -ConfigurationName PrivilegedEndpoint -Credential $cred 
 
$fromDate = (Get-Date).AddHours(-8) # Optional. 
$toDate = (Get-Date).AddHours(-2) # Optional. Provide the time that includes the period for your issue 
 
Invoke-Command -Session $session {Send-AzureStackDiagnosticLog -FromDate $using:fromDate -ToDate $using:toDate} 
 
if ($session) { 
    Remove-PSSession -Session $session 
} 
```

## <a name="parameter-considerations"></a>パラメーターに関する考慮事項 

* **FromDate** パラメーターと **ToDate** パラメーターを使用して、特定の期間のログを収集できます。 これらのパラメーターが指定されない場合、既定では過去 4 時間のログが収集されます。

* コンピューター名でログをフィルター処理するには、**FilterByNode** パラメーターを使用します。 次に例を示します。

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByNode azs-xrp01
  ```

* 種類でログをフィルター処理するには、**FilterByLogType** パラメーターを使用します。 File、Share、または WindowsEvent を選択してフィルター処理できます。 次に例を示します。

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByLogType File
  ```

* **FilterByResourceProvider** パラメーターを使用して、付加価値リソース プロバイダー (RP) の診断ログを送信します。 一般的な構文は次のとおりです。
 
  ```powershell
  Send-AzureStackDiagnosticLog -FilterByResourceProvider <<value-add RP name>>
  ```
 
  IoT Hub の診断ログを送信するには: 

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByResourceProvider IotHub
  ```
 
  Event Hub の診断ログを送信するには:

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByResourceProvider eventhub
  ```
 
  Azure Stack Edge の診断ログを送信するには:

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByResourceProvide databoxedge
  ```

* **FilterByRole** パラメーターを使用して、VirtualMachines ロールと BareMetal ロールから診断ログを送信します。

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByRole VirtualMachines,BareMetal
  ```

* ログ ファイルに対する過去 8 時間以内の日付範囲のフィルター処理で、VirtualMachines ロールと BareMetal ロールから診断ログを送信するには:

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)
  ```

* ログ ファイルに対する過去 8 時間から 2 時間以内の日付範囲のフィルター処理で、VirtualMachines ロールと BareMetal ロールから診断ログを送信するには:

  ```powershell
  Send-AzureStackDiagnosticLog -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)
  ```


## <a name="next-steps"></a>次のステップ

[特権エンドポイント (PEP) を使用して Azure Stack Hub 診断ログを送信する](azure-stack-get-azurestacklog.md)
