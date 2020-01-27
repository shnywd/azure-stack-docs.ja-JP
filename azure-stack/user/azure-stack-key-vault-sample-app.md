---
title: Azure Stack Hub でアプリに Key Vault に格納されているシークレットへのアクセスを許可する | Microsoft Docs
description: Azure Stack Hub で Key Vault からキーとシークレットを取得するサンプル アプリを実行する方法を説明します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3748b719-e269-4b48-8d7d-d75a84b0e1e5
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: sethm
ms.lastreviewed: 04/08/2019
ms.openlocfilehash: f73af34f3069b0fe550c8266253dce32efcb6b90
ms.sourcegitcommit: d450dcf5ab9e2b22b8145319dca7098065af563b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75883628"
---
# <a name="allow-apps-to-access-azure-stack-hub-key-vault-secrets"></a>Azure Stack Hub でアプリに Key Vault に格納されているシークレットへのアクセスを許可する

この記事の手順に従って、Azure Stack Hub のキー コンテナーからキーとシークレットを取得するサンプル アプリ **HelloKeyVault** を実行します。

## <a name="prerequisites"></a>前提条件

[Azure Stack Development Kit](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp) から、または [VPN 経由で接続](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn)している場合は Windows ベースの外部クライアントから、次の前提条件をインストールできます。

* [Azure Stack Hub と互換性のある Azure PowerShell モジュール](../operator/azure-stack-powershell-install.md)をインストールします。
* [Azure Stack Hub の操作に必要なツール](../operator/azure-stack-powershell-download.md)をダウンロードします。

## <a name="create-a-key-vault-and-register-an-app"></a>Key Vault の作成とアプリの登録

サンプル アプリケーションを準備するには:

* Azure Stack Hub でキー コンテナーを作成します。
* Azure Active Directory (Azure AD) でアプリを登録します。

Azure portal または PowerShell を使用して、サンプル アプリを準備します。

> [!NOTE]
> 既定では、この PowerShell スクリプトによって、Active Directory に新しいアプリが作成されます。 ただし、既存のアプリケーションのいずれかを登録できます。

次のスクリプトを実行する前に、必ず `aadTenantName` 変数と `applicationPassword` 変数の値を指定します。 `applicationPassword` の値を指定しないと、このスクリプトによりランダムなパスワードが生成されます。

```powershell
$vaultName           = 'myVault'
$resourceGroupName   = 'myResourceGroup'
$applicationName     = 'myApp'
$location            = 'local'

# Password for the application. If not specified, this script generates a random password during app creation.
$applicationPassword = ''

# Function to generate a random password for the application.
Function GenerateSymmetricKey()
{
    $key = New-Object byte[](32)
    $rng = [System.Security.Cryptography.RNGCryptoServiceProvider]::Create()
    $rng.GetBytes($key)
    return [System.Convert]::ToBase64String($key)
}

Write-Host 'Please log into your Azure Stack Hub user environment' -foregroundcolor Green

$tenantARM = "https://management.local.azurestack.external"
$aadTenantName = "FILL THIS IN WITH YOUR AAD TENANT NAME. FOR EXAMPLE: myazurestack.onmicrosoft.com"

# Configure the Azure Stack Hub operator's PowerShell environment.
Add-AzureRMEnvironment `
  -Name "AzureStackUser" `
  -ArmEndpoint $tenantARM

$TenantID = Get-AzsDirectoryTenantId `
  -AADTenantName $aadTenantName `
  -EnvironmentName AzureStackUser

# Sign in to the user portal.
Add-AzureRmAccount `
  -EnvironmentName "AzureStackUser" `
  -TenantId $TenantID `

$now = [System.DateTime]::Now
$oneYearFromNow = $now.AddYears(1)

$applicationPassword = GenerateSymmetricKey

# Create a new Azure AD application.
$identifierUri = [string]::Format("http://localhost:8080/{0}",[Guid]::NewGuid().ToString("N"))
$homePage = "https://contoso.com"

Write-Host "Creating a new AAD Application"
$ADApp = New-AzureRmADApplication `
  -DisplayName $applicationName `
  -HomePage $homePage `
  -IdentifierUris $identifierUri `
  -StartDate $now `
  -EndDate $oneYearFromNow `
  -Password $applicationPassword

Write-Host "Creating a new AAD service principal"
$servicePrincipal = New-AzureRmADServicePrincipal `
  -ApplicationId $ADApp.ApplicationId

# Create a new resource group and a key vault in that resource group.
New-AzureRmResourceGroup `
  -Name $resourceGroupName `
  -Location $location

Write-Host "Creating vault $vaultName"
$vault = New-AzureRmKeyVault -VaultName $vaultName `
  -ResourceGroupName $resourceGroupName `
  -Sku standard `
  -Location $location

# Specify full privileges to the vault for the application.
Write-Host "Setting access policy"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName `
  -ObjectId $servicePrincipal.Id `
  -PermissionsToKeys all `
  -PermissionsToSecrets all

Write-Host "Paste the following settings into the app.config file for the HelloKeyVault project:"
'<add key="VaultUrl" value="' + $vault.VaultUri + '"/>'
'<add key="AuthClientId" value="' + $servicePrincipal.ApplicationId + '"/>'
'<add key="AuthClientSecret" value="' + $applicationPassword + '"/>'
Write-Host
```

次の画像は、キー コンテナーの作成に使用されたスクリプトからの出力を示しています。

![キー コンテナーとアクセス キー](media/azure-stack-key-vault-sample-app/settingsoutput.png)

上のスクリプトによって返される **VaultUrl**、**AuthClientId**、**AuthClientSecret** の値をメモしておきます。 これらの値は、**HelloKeyVault** アプリケーションを実行するために使用します。

## <a name="download-and-configure-the-sample-application"></a>サンプル アプリケーションのダウンロードと構成

「[Azure Key Vault client samples (Azure Key Vault クライアントのサンプル)](https://www.microsoft.com/download/details.aspx?id=45343)」ページから、キー コンテナーのサンプルをダウンロードします。 .zip ファイルの内容を自分の開発ワークステーションに抽出します。 samples フォルダー内には 2 つのアプリがあります。 この記事では、**HelloKeyVault** を使用します。

**HelloKeyVault** サンプルを読み込むには:

1. **Microsoft.Azure.KeyVault.Samples** > **samples** > **HelloKeyVault** フォルダーを参照します。
2. Visual Studio で**HelloKeyVault** アプリを開きます。

### <a name="configure-the-sample-application"></a>サンプル アプリケーションを構成する

Visual Studio で次の操作を行います。

1. HelloKeyVault\App.config ファイルを開き、`<appSettings>` 要素を見つけます。
2. Key Vault の作成時に返された値を使用して、**VaultUrl**、**AuthClientId**、**AuthClientSecret** を更新します。 既定では、App.config ファイルには `AuthCertThumbprint` のプレースホルダーが含まれます。 このプレースホルダーを `AuthClientSecret` に置き換えます。

   ![アプリケーション設定](media/azure-stack-key-vault-sample-app/appconfig.png)

3. ソリューションをリビルドします。

## <a name="run-the-app"></a>アプリを実行する

**HelloKeyVault** を実行すると、このアプリは Azure AD にサインインし、`AuthClientSecret` トークンを使用して、Azure Stack Hub の Key Vault に対する認証を行います。

**HelloKeyVault** サンプルを使用して、以下の操作を行います。

* キーおよびシークレットで作成、暗号化、ラップ、削除などの基本操作を行います。
* `encrypt`、`decrypt` などのパラメーターを **HelloKeyVault** に渡して、指定した変更をキー コンテナーに適用します。

## <a name="next-steps"></a>次のステップ

* [Key Vault パスワードを使用して VM をデプロイする](azure-stack-key-vault-deploy-vm-with-secret.md)
* [Key Vault 証明書を使って VM をデプロイする](azure-stack-key-vault-push-secret-into-vm.md)
