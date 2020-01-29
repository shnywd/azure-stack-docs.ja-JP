---
title: PowerShell を使用した Azure Stack Hub での更新の監視 | Microsoft Docs
description: PowerShell を使用して Azure Stack Hub で更新を監視する方法を説明します
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/22/2020
ms.author: mabrigg
ms.lastreviewed: 08/23/2019
ms.reviewer: ppacent
ms.openlocfilehash: 2ef7537e7d4733e1cd99592e8962665deb258f0e
ms.sourcegitcommit: a1abc27a31f04b703666de02ab39ffdc79a632f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2020
ms.locfileid: "76535691"
---
# <a name="monitor-updates-in-azure-stack-hub-using-powershell"></a>PowerShell を使用した Azure Stack Hub での更新の監視

Azure Stack Hub 管理エンドポイントを使用して、更新の監視と管理を行うことができます。 それらには、PowerShell でアクセスできます。 Azure Stack Hub で PowerShell を使用してセットアップする手順については、「[PowerShell for Azure Stack Hub をインストールする](azure-stack-powershell-install.md)」を参照してください。

次の PowerShell コマンドレットを使用して更新を管理することもできます。

| コマンドレット | [説明] |
|------------------------------------------------------|-------------|
| [Get-AzsUpdate](https://docs.microsoft.com/powershell/module/azs.update.admin/Get-AzsUpdate?view=azurestackps-1.8.0) | 使用可能な更新プログラムの一覧を取得します。 |
| [Get-AzsUpdateLocation](https://docs.microsoft.com/powershell/module/azs.update.admin/Get-AzsUpdateLocation?view=azurestackps-1.8.0)| 更新プログラムの場所の一覧を取得します。 |
| [Get-AzsUpdateRun](https://docs.microsoft.com/powershell/module/azs.update.admin/Get-AzsUpdateRun?view=azurestackps-1.8.0) | 更新プログラムの実行の一覧を取得します。  |
| [Install-AzsUpdate](https://docs.microsoft.com/powershell/module/azs.update.admin/Install-AzsUpdate?view=azurestackps-1.8.0) | 更新プログラムの場所にある特定の更新プログラムを適用します。 |
| [Resume-AzsUpdateRun](https://docs.microsoft.com/powershell/module/azs.update.admin/Resume-AzsUpdateRun?view=azurestackps-1.8.0) | 以前に開始され、失敗した更新実行を再開します。 |

## <a name="get-a-list-of-update-runs"></a>更新実行の一覧を取得する

更新実行の一覧を取得するコマンド:

```powershell
Get-AzsUpdateRun -UpdateName Microsoft1.0.180302.1
```

## <a name="resume-a-failed-update-operation"></a>失敗した更新操作を再開する

更新が失敗した場合は、次のコマンドを実行して、失敗した場所から更新実行を再開できます。

```powershell
Get-AzsUpdateRun -Name 5173e9f4-3040-494f-b7a7-738a6331d55c -UpdateName Microsoft1.0.180305.1 | Resume-AzsUpdateRun
```

## <a name="next-steps"></a>次のステップ

-   [Azure Stack Hub での更新プログラム管理](https://docs.microsoft.com/azure-stack/operator/azure-stack-updates)
