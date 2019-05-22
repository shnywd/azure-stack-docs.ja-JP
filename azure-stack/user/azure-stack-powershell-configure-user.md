---
title: ユーザーとして PowerShell を使用して Azure Stack に接続する | Microsoft Docs
description: PowerShell を使用して Azure Stack に接続する手順。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/24/2019
ms.openlocfilehash: 855d7c03f4a18c4409d36b8ac5fd702c8549e413
ms.sourcegitcommit: 85c3acd316fd61b4e94c991a9cd68aa97702073b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/01/2019
ms.locfileid: "64986244"
---
# <a name="connect-to-azure-stack-with-powershell-as-a-user"></a>ユーザーとして PowerShell を使用して Azure Stack に接続する

*適用対象: Azure Stack 統合システムと Azure Stack Development Kit*

PowerShell を使用して Azure Stack に接続できます。 Azure Stack リソースを管理するには、PowerShell を使用して接続する必要があります。 たとえば、PowerShell を使用し、オファーをサブスクライブしたり、仮想マシンを作成したり、Azure Resource Manager テンプレートをデプロイしたりできます。

設定の方法：
  - 要件を満たしていることをご確認ください。
  - Azure Active Directory (Azure AD) または Active Directory フェデレーション サービス (AD FS) と接続します。 
  - リソース プロバイダーを登録します。
  - 接続をテストします。

## <a name="prerequisites-to-connect-using-powershell"></a>PowerShell を使用して接続するための前提条件

次の前提条件は[開発キット](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp)から構成するか、[VPN 経由で接続](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn)している場合は Windows ベースの外部クライアントから構成します。

* [Azure Stack と互換性のある Azure PowerShell モジュール](../operator/azure-stack-powershell-install.md)をインストールします。
* [Azure Stack を操作するために必要なツール](../operator/azure-stack-powershell-download.md)をダウンロードします。

次のスクリプト変数を自分の Azure Stack 構成の値に必ず変更してください。

- **Azure AD テナントの名前**  
  Azure Stack の管理に使用する Azure AD テナントの名前 (yourdirectory.onmicrosoft.com など)。
- **Azure Resource Manager エンドポイント**  
  Azure Stack 開発キットの場合、この値は https://management.local.azurestack.external に設定されます。 Azure Stack 統合システムのこの値を取得するには、サービス プロバイダーにお問い合わせください。

## <a name="connect-with-azure-ad"></a>Azure AD との接続

```powershell  
    Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint "https://management.local.azurestack.external"
    # Set your tenant name
    $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackUser").ActiveDirectoryAuthority.TrimEnd('/')
    $AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com"
    $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackUser" -TenantId $TenantId
```

## <a name="connect-with-ad-fs"></a>AD FS を使用した接続

  ```powershell  
  # Register an Azure Resource Manager environment that targets your Azure Stack instance
  Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint "https://management.local.azurestack.external"

  # Sign in to your environment
  Login-AzureRmAccount -EnvironmentName "AzureStackUser"
  ```

## <a name="register-resource-providers"></a>リソース プロバイダーを登録する

ポータル経由でリソースがデプロイされていない新しいユーザー サブスクリプションの場合、リソース プロバイダーが自動登録されません。 次のスクリプトを実行し、リソース プロバイダーを明示的に登録できます。

```powershell  
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider
    }
```

## <a name="test-the-connectivity"></a>接続のテスト

すべてが整ったら、PowerShell を使って接続をテストし、Azure Stack でリソースを作成します。 テストとして、アプリケーションのリソース グループを作成し、仮想マシンを追加します。 次のコマンドを実行し、"MyResourceGroup" という名前のリソース グループを作成します。

```powershell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>次の手順

- [Azure Stack のテンプレートの開発](azure-stack-develop-templates.md)
- [PowerShell を使用したテンプレートのデプロイ](azure-stack-deploy-template-powershell.md)
- [Azure Stack PowerShell Module リファレンス](https://docs.microsoft.com/powershell/azure/azure-stack/overview)
- クラウド オペレーター環境用に PowerShell を設定する場合、[Azure Stack オペレーターの PowerShell 環境の構成](../operator/azure-stack-powershell-configure-admin.md)に関する記事をご覧ください。
