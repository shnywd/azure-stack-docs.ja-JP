---
title: Azure Stack Hub で PowerShell を使用してテンプレートをデプロイする
description: Azure Stack Hub で PowerShell を使用してテンプレートをデプロイします。
author: mattbriggs
ms.topic: article
ms.date: 5/27/2020
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 09/23/2019
ms.openlocfilehash: a5bd582cd93a95f662a8acc2094e6a62a7ecdf50
ms.sourcegitcommit: 695f56237826fce7f5b81319c379c9e2c38f0b88
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2020
ms.locfileid: "94546907"
---
# <a name="deploy-a-template-using-powershell-in-azure-stack-hub"></a>Azure Stack Hub で PowerShell を使用してテンプレートをデプロイする

PowerShell を使用して、Azure Resource Manager テンプレートを Azure Stack Hub にデプロイできます。 この記事では、PowerShell を使用してテンプレートをデプロイする方法について説明します。

## <a name="run-az-powershell-cmdlets"></a>Az PowerShell コマンドレットを実行する

この例では、**Az** PowerShell コマンドレットと GitHub に格納されているテンプレートを使用します。 テンプレートにより、Windows Server 2012 R2 Datacenter の仮想マシンが作成されます。

>[!NOTE]
> この例を試す前に、Azure Stack Hub ユーザー用に [PowerShell を構成している](azure-stack-powershell-configure-user.md)ことを確認します。

1. [AzureStack-QuickStart-Templates リポジトリ](https://aka.ms/AzureStackGitHub)を参照して、**101-simple-windows-vm** テンプレートを見つけます。 テンプレートを `C:\templates\azuredeploy-101-simple-windows-vm.json` に保存します。
2. 管理者特権の PowerShell コマンド プロンプトを開きます。
3. 次のスクリプト内の `username` と `password` を自分のユーザー名とパスワードに置き換えてから、スクリプトを実行します。

    ```powershell
    # Set deployment variables
    $myNum = "001" # Modify this per deployment
    $RGName = "myRG$myNum"
    $myLocation = "yourregion" # local for the ASDK

    # Create resource group for template deployment
    New-AzResourceGroup -Name $RGName -Location $myLocation

    # Deploy simple IaaS template
    New-AzResourceGroupDeployment `
        -Name myDeployment$myNum `
        -ResourceGroupName $RGName `
        -TemplateUri <path>\AzureStack-QuickStart-Templates\101-vm-windows-create\azuredeploy.json `
        -AdminUsername <username> `
        -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force)
    ```

    >[!IMPORTANT]
    > このスクリプトを実行するたびに、`$myNum` パラメーターの値をインクリメントし、デプロイの上書きを防ぎます。

4. Azure Stack Hub ポータルを開き、 **[参照]** 、 **[仮想マシン]** の順に選択して、新しい仮想マシン (**myDeployment001**) を探します。

## <a name="cancel-a-running-template-deployment"></a>実行中のテンプレートのデプロイのキャンセル

実行中のテンプレートのデプロイをキャンセルするには、`Stop-AzResourceGroupDeployment` PowerShell コマンドレットを使用します。

## <a name="next-steps"></a>次のステップ

- [Visual Studio でテンプレートをデプロイする](azure-stack-deploy-template-visual-studio.md)
