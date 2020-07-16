---
title: Azure Stack Hub の MySQL リソース プロバイダーを更新する
description: Azure Stack Hub の Azure Stack Hub MySQL リソース プロバイダーを更新する方法について説明します。
author: bryanla
ms.topic: article
ms.date: 1/22/2020
ms.author: bryanla
ms.reviewer: xiaofmao
ms.lastreviewed: 01/11/2020
ms.openlocfilehash: 909927d82953cb21c8665afcb45c82131675c2e4
ms.sourcegitcommit: c1f48c19c8a9c438fd22298bc570c12a9b19bb45
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/16/2020
ms.locfileid: "86410578"
---
# <a name="update-the-mysql-resource-provider-in-azure-stack-hub"></a>Azure Stack Hub の MySQL リソース プロバイダーを更新する

> [!IMPORTANT]
> リソース プロバイダーを更新する前に、新しい機能、修正、およびデプロイに影響を与える可能性のある既知の問題に関する詳細については、リリース ノートを確認してください。 リリース ノートには、リソース プロバイダーに必要な Azure Stack Hub の最小バージョンも指定されています。

Azure Stack Hub のビルドの更新時に、新しい MySQL リソース プロバイダー アダプターがリリースされる場合があります。 既存のアダプターが動作し続けている場合でも、できるだけ早く最新のビルドに更新することをお勧めします。

MySQL リソース プロバイダー バージョン 1.1.33.0 リリース以降の更新プログラムは累積的であり、バージョン 1.1.24.0 以降から開始する限り、リリースされた順序でインストールする必要はありません。 たとえば、バージョン 1.1.24.0 の MySQL リソース プロバイダーを実行している場合、最初にバージョン 1.1.30.0 をインストールしなくても、バージョン 1.1.33.0 以降にアップグレードできます。 使用可能なリソース プロバイダーのバージョンと、それがサポートされる Azure Stack Hub のバージョンを確認するには、[リソース プロバイダーのデプロイの前提条件](./azure-stack-mysql-resource-provider-deploy.md#prerequisites)に関する記事で、バージョンの一覧を参照してください。

リソース プロバイダーの更新には **UpdateMySQLProvider.ps1** スクリプトを使用します。 プロセスは、この記事のリソース プロバイダーのデプロイに関するセクションに記述されている、リソース プロバイダーをインストールするプロセスと類似しています。 スクリプトはリソース プロバイダーのダウンロードに含まれています。 

## <a name="update-script-processes"></a>更新スクリプトのプロセス

**UpdateMySQLProvider.ps1** スクリプトは、最新のリソース プロバイダーのコードを使って新しい仮想マシン (VM) を作成し、古い VM から新しい VM に設定を移行します。 移行される設定には、データベースとホスティング サーバーの情報や、必要な DNS レコードが含まれます。

>[!NOTE]
>Marketplace の管理から最新の Windows Server 2016 Core のイメージをダウンロードすることをお勧めします。 更新プログラムをインストールする必要がある場合は、ローカルの依存関係のパスに MSU パッケージを **1 つ**配置できます。 この場所に複数の MSU ファイルがある場合、スクリプトは失敗します。

スクリプトでは、DeployMySqlProvider.ps1 スクリプト用に記述されているものと同じ引数の使用が必要になります。 証明書も同様に指定します。  


## <a name="update-script-parameters"></a>更新スクリプトのパラメーター 
**UpdateMySQLProvider.ps1** PowerShell スクリプトを実行するときに、コマンド ラインから以下のパラメーターを指定します。 必須パラメーターの指定がない場合、またはいずれかのパラメーターの検証が失敗した場合は、指定することを求められます。

| パラメーター名 | 説明 | コメントまたは既定値 | 
| --- | --- | --- | 
| **CloudAdminCredential** | 特権エンドポイントへのアクセスに必要な、クラウド管理者の資格情報。 | _必須_ | 
| **AzCredential** | Azure Stack Hub サービス管理者アカウントの資格情報。 Azure Stack Hub のデプロイに使用した資格情報と同じものを使用します。 | _必須_ | 
| **VMLocalCredential** |SQL リソース プロバイダー VM のローカル管理者アカウントの資格情報。 | _必須_ | 
| **PrivilegedEndpoint** | 特権エンドポイントの IP アドレスまたは DNS 名。 |  _必須_ | 
| **AzureEnvironment** | Azure Stack Hub のデプロイに使用するサービス管理者アカウントの Azure 環境。 Azure AD のデプロイでのみ必須です。 サポートされている環境名は **AzureCloud**、**AzureUSGovernment**、または中国の Azure AD を使用している場合は **AzureChinaCloud** です。 | AzureCloud |
| **DependencyFilesLocalPath** | 証明書 .pfx ファイルはこのディレクトリにも配置する必要があります。 | _省略可能_ (マルチノードでは _必須_) | 
| **DefaultSSLCertificatePassword** | .pfx 証明書のパスワード。 | _必須_ | 
| **MaxRetryCount** | エラーが 発生した場合に各操作を再試行する回数。| 2 | 
| **RetryDuration** | 再試行間のタイムアウト間隔 (秒単位)。 | 120 | 
| **アンインストール** | リソース プロバイダーと関連付けられているすべてのリソースを削除します (以下のメモを参照してください)。 | いいえ | 
| **DebugMode** | 障害発生時に自動クリーンアップが行われないようにします。 | いいえ | 
| **AcceptLicense** | GPL ライセンスに同意するためのプロンプトをスキップします。  (https://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | | 

## <a name="update-script-example"></a>更新スクリプトの例

> [!NOTE] 
> 更新プロセスは、統合システムにのみに適用されます。

MySQL リソース プロバイダーのバージョン 1.1.33.0 以前のバージョンを更新する場合は、PowerShell で特定のバージョンの AzureRm.BootStrapper と Azure Stack Hub モジュールをインストールする必要があります。 MySQL リソース プロバイダーをバージョン 1.1.47.0 に更新する場合は、デプロイ スクリプトが自動的にダウンロードされ、C:\Program Files\SqlMySqlPsh へのパスに必要な PowerShell モジュールがインストールされます。

```powershell 
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
# Note that this might not be the most currently available version of Azure Stack Hub PowerShell.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force
Install-Module -Name AzureStack -RequiredVersion 1.6.0
```

> [!NOTE]
> 接続が切断された場合のシナリオでは、必要な PowerShell モジュールのダウンロードと手動でのリポジトリ登録が前提条件として必要です。 詳細については、[MySQL リソース プロバイダーのデプロイ](azure-stack-mysql-resource-provider-deploy.md)に関する記事を参照してください。

管理者特権の PowerShell コンソールから実行できる *UpdateMySQLProvider.ps1* スクリプトを次に示します。 変数情報とパスワードは、必要に応じて変更してください。

```powershell 
# Use the NetBIOS name for the Azure Stack Hub domain. On the Azure Stack Hub SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack" 

# For integrated systems, use the IP address of one of the ERCS VMs.
$privilegedEndpoint = "AzS-ERCS01" 

# Provide the Azure environment used for deploying Azure Stack Hub. Required only for Azure AD deployments. Supported environment names are AzureCloud, AzureUSGovernment, or AzureChinaCloud. 
$AzureEnvironment = "<EnvironmentName>"

# Point to the directory where the resource provider installation files were extracted. 
$tempDir = 'C:\TEMP\MYSQLRP' 

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com" 
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass) 
 
# Set credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass) 
 
# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass) 

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 

# For version 1.1.47.0, the PowerShell modules used by the RP deployment are placed in C:\Program Files\SqlMySqlPsh
# The deployment script adds this path to the system $env:PSModulePath to ensure correct modules are used.
$rpModulePath = Join-Path -Path $env:ProgramFiles -ChildPath 'SqlMySqlPsh'
$env:PSModulePath = $env:PSModulePath + ";" + $rpModulePath 

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
.$tempDir\UpdateMySQLProvider.ps1 -AzCredential $AdminCreds ` 
-VMLocalCredential $vmLocalAdminCreds ` 
-CloudAdminCredential $cloudAdminCreds ` 
-PrivilegedEndpoint $privilegedEndpoint ` 
-AzureEnvironment $AzureEnvironment ` 
-DefaultSSLCertificatePassword $PfxPass ` 
-DependencyFilesLocalPath $tempDir\cert ` 
-AcceptLicense 
```  

リソース プロバイダーの更新スクリプトが終了したら、現在の PowerShell セッションを終了します。

## <a name="next-steps"></a>次のステップ
[MySQL リソース プロバイダーの維持](azure-stack-mysql-resource-provider-maintain.md)
