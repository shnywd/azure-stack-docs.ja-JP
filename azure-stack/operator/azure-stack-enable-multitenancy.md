---
title: Azure Stack でのマルチテナントの構成 | Microsoft Docs
description: Azure Stack 上で複数の Azure Active Directory テナントを有効および無効にする方法について説明します。
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2019
ms.author: patricka
ms.reviewer: bryanr
ms.lastreviewed: 06/10/2019
ms.openlocfilehash: 168565a47a7c3511111fbae565e80dbfe0e3c606
ms.sourcegitcommit: 451cfaa24b349393f36ae9d646d4d311a14dd1fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72019377"
---
# <a name="configure-multi-tenancy-in-azure-stack"></a>Azure Stack でのマルチテナントの構成

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

複数の Azure Active Directory (Azure AD) テナントのユーザーをサポートして、彼らが Azure Stack でのサービスを使用できるように、Azure Stack を構成することができます。 ここでは、次のシナリオを例に説明します。

- あなたは、Azure Stack がインストールされている contoso.onmicrosoft.com のサービス管理者です。
- メアリーは、ゲスト ユーザーがいる fabrikam.onmicrosoft.com のディレクトリ管理者です。
- メアリーの会社では、あなたの会社から IaaS および PaaS サービスを受けており、ゲスト ディレクトリ (fabrikam.onmicrosoft.com) のユーザーが、contoso.onmicrosoft.com にある Azure Stack のリソースにサインインして使用できるようにする必要があります。

このガイドでは、このシナリオに基づいて、Azure Stack でマルチテナントを構成するために必要な手順を説明します。 このシナリオでは、Fabrikam のユーザーが Contoso の Azure Stack デプロイのサービスにサインインして使用できるようにするための手順を、あなたとメアリーがそれぞれ完了する必要があります。

## <a name="enable-multi-tenancy"></a>マルチテナントの有効化

Azure Stack でマルチテナントを構成する前に、いくつかの前提条件を考慮する必要があります。
  
 - Azure Stack がインストールされているディレクトリ (Contoso) とゲスト ディレクトリ (Fabrikam) の両方に対して、あなたとメアリーが連携して管理上の手順を実行する必要があります。
 - Azure Stack 用 PowerShell の[インストール](azure-stack-powershell-install.md)と[構成](azure-stack-powershell-configure-admin.md)が済んでいることを確認してください。
 - [Azure Stack Tools をダウンロード](azure-stack-powershell-download.md)して、Connect モジュールと Identity モジュールをインポートします。

    ```powershell
    Import-Module .\Connect\AzureStack.Connect.psm1
    Import-Module .\Identity\AzureStack.Identity.psm1
    ```

### <a name="configure-azure-stack-directory"></a>Azure Stack ディレクトリの構成

このセクションでは、Fabrikam の Azure AD ディレクトリのテナントからのサインインを許可するように Azure Stack を構成します。

ゲスト ディレクトリ テナント (Fabrikam) のユーザーとサービス プリンシパルを受け入れるように Azure Resource Manager を構成して、ゲスト ディレクトリ テナントを Azure Stack にオンボードします。

contoso.onmicrosoft.com のサービス管理者は、次のコマンドを実行します。

```powershell  
## The following Azure Resource Manager endpoint is for the ASDK. If you're in a multinode environment, contact your operator or service provider to get the endpoint.
$adminARMEndpoint = "https://adminmanagement.local.azurestack.external"

## Replace the value below with the Azure Stack directory
$azureStackDirectoryTenant = "contoso.onmicrosoft.com"

## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantToBeOnboarded = "fabrikam.onmicrosoft.com"

## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
$ResourceGroupName = "system.local"

## Replace the value below with the region location of the resource group.
$location = "local"

# Subscription Name
$SubscriptionName = "Default Provider Subscription"

Register-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
 -DirectoryTenantName $azureStackDirectoryTenant `
 -GuestDirectoryTenantName $guestDirectoryTenantToBeOnboarded `
 -Location $location `
 -ResourceGroupName $ResourceGroupName `
 -SubscriptionName $SubscriptionName
```

### <a name="configure-guest-directory"></a>ゲスト ディレクトリの構成

Azure Stack オペレーターが Azure Stack で使用される Fabrikam ディレクトリを有効にすると、メアリーは Azure Stack を Fabrikam のディレクトリ テナントに登録する必要があります。

#### <a name="registering-azure-stack-with-the-guest-directory"></a>ゲスト ディレクトリへの Azure Stack の登録

メアリー (Fabrikam のディレクトリ管理者) は、ゲスト ディレクトリ fabrikam.onmicrosoft.com で次のコマンドを実行します。

```powershell
## The following Azure Resource Manager endpoint is for the ASDK. If you're in a multinode environment, contact your operator or service provider to get the endpoint.
$tenantARMEndpoint = "https://management.local.azurestack.external"
    
## Replace the value below with the guest tenant directory.
$guestDirectoryTenantName = "fabrikam.onmicrosoft.com"

Register-AzSWithMyDirectoryTenant `
 -TenantResourceManagerEndpoint $tenantARMEndpoint `
 -DirectoryTenantName $guestDirectoryTenantName `
 -Verbose
```

> [!IMPORTANT]
> Azure Stack 管理者が今後新しいサービスや更新プログラムをインストールした場合には、このスクリプトをもう一度実行する必要が生じることがあります。
>
> ディレクトリ内の Azure Stack アプリの状態を確認するために、このスクリプトはいつでも再実行できます。
>
> VM をマネージド ディスク内に作成する (1808 更新プログラムで導入) 際に問題が起きた場合には、このスクリプトの再実行を求める新しい**ディスク リソース プロバイダー**が追加されました。

### <a name="direct-users-to-sign-in"></a>ユーザーをサインインに誘導する

あなたとメアリーの両方が Fabrikam ディレクトリのオンボード手順を完了したので、メアリーは Fabrikam のユーザーをサインインに誘導することができます。 Fabrikam ユーザー (fabrikam.onmicrosoft.com のサフィックスを持つユーザー) は、https\://portal.local.azurestack.external にアクセスしてサインインします。

メアリーは、Fabrikam ディレクトリの[外部プリンシパル](/azure/role-based-access-control/rbac-and-directory-admin-roles) (fabrikam.onmicrosoft.com のサフィックスを持たない Fabrikam ディレクトリ内のユーザー) はすべて、https\://portal.local.azurestack.external/fabrikam.onmicrosoft.com を使用してサインインするように誘導します。 この URL を使用しない場合、これらのユーザーは自分の既定のディレクトリ (Fabrikam) に送信され、管理者が同意していないことを示すエラーを受信します。

## <a name="disable-multi-tenancy"></a>マルチテナントの無効化

Azure Stack 内に複数のテナントを持つ必要がなくなった場合は、次の手順を実行してマルチ テナントを無効にできます。

1. ゲスト ディレクトリの管理者 (このシナリオではメアリー) として、*Unregister-AzsWithMyDirectoryTenant* を実行します。 コマンドレットでは、新しいディレクトリからすべての Azure Stack アプリをアンインストールします。

    ``` PowerShell
    ## The following Azure Resource Manager endpoint is for the ASDK. If you're in a multinode environment, contact your operator or service provider to get the endpoint.
    $tenantARMEndpoint = "https://management.local.azurestack.external"
        
    ## Replace the value below with the guest tenant directory.
    $guestDirectoryTenantName = "fabrikam.onmicrosoft.com"
    
    Unregister-AzsWithMyDirectoryTenant `
     -TenantResourceManagerEndpoint $tenantARMEndpoint `
     -DirectoryTenantName $guestDirectoryTenantName `
     -Verbose 
    ```

2. Azure Stack のサービス管理者 (このシナリオではあなた) として、*Unregister-AzSGuestDirectoryTenant* を実行します。

    ``` PowerShell
    ## The following Azure Resource Manager endpoint is for the ASDK. If you're in a multinode environment, contact your operator or service provider to get the endpoint.
    $adminARMEndpoint = "https://adminmanagement.local.azurestack.external"
    
    ## Replace the value below with the Azure Stack directory
    $azureStackDirectoryTenant = "contoso.onmicrosoft.com"
    
    ## Replace the value below with the guest tenant directory. 
    $guestDirectoryTenantToBeDecommissioned = "fabrikam.onmicrosoft.com"
    
    ## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
    $ResourceGroupName = "system.local"
    
    Unregister-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
     -DirectoryTenantName $azureStackDirectoryTenant `
     -GuestDirectoryTenantName $guestDirectoryTenantToBeDecommissioned `
     -ResourceGroupName $ResourceGroupName
    ```

    > [!WARNING]
    > マルチ テナントを無効にする手順は、正しい順序で実行する必要があります。 手順 2 を先に実行した場合、手順 1 は失敗します。

## <a name="next-steps"></a>次の手順

- [委任されたプロバイダーの管理](azure-stack-delegated-provider.md)
- [Azure Stack の主要概念](azure-stack-overview.md)
- [クラウド ソリューション プロバイダーとして Azure Stack の使用状況と課金を管理する](azure-stack-add-manage-billing-as-a-csp.md)
- [Azure Stack に使用量と課金のためのテナントを追加する](azure-stack-csp-howto-register-tenants.md)
