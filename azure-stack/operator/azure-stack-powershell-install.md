---
title: PowerShell for Azure Stack のインストール | Microsoft Docs
description: PowerShell for Azure Stack をインストールする方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 09/18/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 09/18/2019
ms.openlocfilehash: 79d7a0adfc7d869f600e864264716b34d6452213
ms.sourcegitcommit: c46d913ebfa4cb6c775c5117ac5c9e87d032a271
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2019
ms.locfileid: "71101138"
---
# <a name="install-powershell-for-azure-stack"></a>PowerShell for Azure Stack をインストールする

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure PowerShell には、Azure Stack リソースの管理に Azure Resource Manager モデルを使う一連のコマンドレットが用意されています。

クラウドで動作させるには、Azure Stack と互換のある PowerShell モジュールをインストールする必要があります。 Azure Stack では、グローバル Azure で使われている新しい **AzureAZ** モジュールではなく、**AzureRM** モジュールが使われます。 さらに、*API プロファイル*を使って、Azure Stack リソース プロバイダーと互換性のあるエンドポイントを指定する必要もあります。

API のプロファイルは、Azure と Azure Stack の間のバージョンの違いを管理するための方法を提供します。 API バージョンのプロファイルは、特定の API バージョンを持つ一連の Azure Resource Manager PowerShell モジュールです。 各クラウド プラットフォームでは、一連の API バージョンのプロファイルがサポートされています。 たとえば、Azure Stack では**2019-03-01-hybrid** などの特定のプロファイル バージョンがサポートされます。 プロファイルをインストールすると、指定されたプロファイルに対応する Azure Resource Manager PowerShell モジュールがインストールされます。

Azure Stack と互換のある PowerShell モジュールのインストールは、インターネットに接続されたシナリオ、部分的に接続されたシナリオ、接続が切断されたシナリオのいずれでも可能です。 この記事では、これらのシナリオの詳細な手順について説明します。

## <a name="1-verify-your-prerequisites"></a>1.前提条件を確認する

Azure Stack と PowerShell を開始する前に、次の前提条件が必要です。

- **PowerShell バージョン 5.0** <br>
バージョンを確認するには、 **$PSVersionTable.PSVersion** を実行して、**メジャー** バージョンを比較します。 PowerShell 5.0 を使用していない場合は、[Windows PowerShell のインストール](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell)に関するページに従ってください。

  > [!Note]
  > PowerShell 5.0 には、Windows コンピューターが必要です。

- **PowerShell の実行 (管理者特権でのコマンド プロンプト)** 。

- **PowerShell ギャラリーへのアクセス** <br>
  [PowerShell ギャラリー](https://www.powershellgallery.com)へのアクセスが必要になります。 ギャラリーとは、PowerShell コンテンツのための中央のリポジトリです。 **PowerShellGet** モジュールには、PowerShell 成果物の検出、インストール、更新、および発行を行うためのコマンドレットが含まれています。 これらの成果物の例としては、モジュール、DSC リソース、ロール機能、PowerShell ギャラリーおよびその他のプライベート リポジトリからのスクリプトなどがあります。 切断されたシナリオで PowerShell を使用している場合は、インターネットに接続しているマシンからリソースを取得し、切断されたマシンがアクセスできる場所に格納する必要があります。

## <a name="2-validate-the-powershell-gallery-accessibility"></a>2.PowerShell ギャラリーにアクセスできるか検証する

PSGallery がリポジトリとして登録されているかどうかを検証します。

> [!Note]  
> この手順では、インターネットへのアクセスが必要です。

管理者特権の PowerShell プロンプトを開き、次のコマンドレットを実行します。

```powershell
Import-Module -Name PowerShellGet -ErrorAction Stop
Import-Module -Name PackageManagement -ErrorAction Stop
Get-PSRepository -Name "PSGallery"
```

リポジトリが登録されていない場合は、管理者特権の PowerShell セッションを開き、次のコマンドを実行します。

```powershell
Register-PSRepository -Default
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

## <a name="3-uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>手順 3.既存のバージョンの Azure Stack PowerShell モジュールのアンインストール

必要なバージョンをインストールする前に、必ず以前にインストールした Azure Stack AzureRM PowerShell モジュールをアンインストールしてください。 モジュールをアンインストールするには、次の 2 つの方法のいずれかを使用します。

1. 既存の AzureRM PowerShell モジュールをアンインストールするには、アクティブな PowerShell セッションをすべて終了し、次のコマンドレットを実行します。

    ```powershell
    Get-Module -Name Azs.* -ListAvailable | Uninstall-Module -Force -Verbose
    Get-Module -Name Azure* -ListAvailable | Uninstall-Module -Force -Verbose
    ```

    「モジュールが既に使用されています」などのエラーが発生した場合には、そのモジュールを使用している PowerShell セッションを終了してから、上のスクリプトをもう一度実行してください。

2. `C:\Program Files\WindowsPowerShell\Modules` フォルダーおよび `C:\Users\{yourusername}\Documents\WindowsPowerShell\Modules` フォルダーから、`Azure` または `Azs.` で始まるすべてのフォルダーを削除します。 これらのフォルダーを削除すると、既存の PowerShell モジュールがすべて削除されます。

## <a name="4-connected-install-powershell-for-azure-stack-with-internet-connectivity"></a>4.接続済みの場合: インターネット接続を使用して PowerShell for Azure Stack をインストールする

必要な API バージョン プロファイルおよび Azure Stack PowerShell モジュールは、実行している Azure Stack のバージョンによって異なります。

### <a name="install-azure-stack-powershell"></a>Azure Stack PowerShell のインストール

次の PowerShell スクリプトを実行して、これらのモジュールを開発用ワークステーションにインストールします。

- Azure Stack 1904 以降の場合:

    ```powershell  
    # Install the AzureRM.BootStrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRM.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2019-03-01-hybrid -Force
    Install-Module -Name AzureStack -RequiredVersion 1.7.2
    ```
  
- Azure Stack バージョン 1903 以前では、以下の 2 つのモジュールのみをインストールします。

    ```powershell  
    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.

    Install-Module -Name AzureRM -RequiredVersion 2.4.0
    Install-Module -Name AzureStack -RequiredVersion 1.7.1
    ```

    > [!Note]  
    > - Azure Stack モジュール バージョン 1.7.1 は破壊的変更を伴うリリースです。 Azure Stack 1.6.0 から移行するには、[移行ガイド](https://aka.ms/azspshmigration171)を参照してください。
    > - AzureRM モジュール バージョン 2.4.0 には、コマンドレット Remove-AzureRmStorageAccount について破壊的変更が存在します。 このコマンドレットでは、確認なしでストレージ アカウントを削除する際に `-Force` パラメーターを指定する必要があります。
    > - Azure Stack バージョン 1901 以降のモジュールをインストールするために、**AzureRM.BootStrapper** をインストールする必要はありません。
    > - Azure Stack バージョン 1901 以降で上記の AzureRM モジュールの使用に加えて 2018-03-01-hybrid プロファイルをインストールしないでください。

### <a name="confirm-the-installation-of-powershell"></a>PowerShell のインストールを確認する

次のコマンドを実行してインストールを確認します。

```powershell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

インストールに成功した場合、出力に AzureRM および AzureStack モジュールが表示されます。

## <a name="5-disconnected-install-powershell-without-an-internet-connection"></a>5.切断状態の場合: インターネット接続なしで PowerShell をインストールする

接続が切断されたシナリオでは、まずインターネット接続が確立されたマシンに PowerShell モジュールをダウンロードします。 次に、それらを Azure Stack Development Kit (ASDK) に転送してインストールします。

インターネット接続が確立されたコンピューターにサインインし、Azure Stack のバージョンに応じて次のスクリプトを使用して、Azure Resource Manager および AzureStack パッケージをダウンロードします。

インストールには 4 つの手順があります:

1. 接続されているマシンに Azure Stack PowerShell をインストールします。
2. 追加のストレージ機能を有効にします。
3. 接続が切断されたワークステーションに PowerShell パッケージを転送します。
4. 切断されたワークステーション上で NuGet プロバイダーを手動でブートストラップします。
5. PowerShell のインストールを確認します。

### <a name="install-azure-stack-powershell"></a>Azure Stack PowerShell のインストール

- Azure Stack 1904 以降。

    ```powershell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.5.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.7.2
    ```

- Azure Stack 1903 以前。

    ```powershell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.4.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.7.1
    ```

    > [!Note]  
    > Azure Stack モジュール バージョン 1.7.1 は破壊的変更です。 AzureStack 1.6.0 から移行するには、[移行ガイド](https://github.com/Azure/azure-powershell/tree/AzureRM/documentation/migration-guides/Stack)を参照してください。

    > [!NOTE]
    > インターネット接続のないマシンでは、次のコマンドレットを実行して利用統計情報の収集を無効にすることをお勧めします。 利用統計情報の収集を無効にしないと、コマンドレットのパフォーマンスが低下することがあります。 これは、インターネット接続のないマシンのみ該当します。
    > ```powershell
    > Disable-AzureRmDataCollection
    > ```

### <a name="add-your-packages-to-your-workstation"></a>パッケージをワークステーションに追加する

1. ダウンロードしたパッケージを USB デバイスにコピーします。

2. 接続が切断されたワークステーションにサインインし、パッケージを USB デバイスからワークステーション上の場所にコピーします。

3. 切断されたワークステーション上で NuGet プロバイダーを手動でブートストラップします。 手順については、「[インターネットに接続されていないマシンで NuGet プロバイダーを手動でブートストラップする](https://docs.microsoft.com/powershell/gallery/how-to/getting-support/bootstrapping-nuget#manually-bootstrapping-the-nuget-provider-on-a-machine-that-is-not-connected-to-the-internet)」をご覧ください。

4. この場所を既定のレポジトリとして登録し、このレポジトリから AzureRM および AzureStack モジュールをインストールします。

   ```powershell
   # requires -Version 5
   # requires -RunAsAdministrator
   # requires -Module PowerShellGet
   # requires -Module PackageManagement

   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation -InstallationPolicy Trusted

   Install-Module -Name AzureRM -Repository $RepoName

   Install-Module -Name AzureStack -Repository $RepoName
   ```

### <a name="confirm-the-installation-of-powershell"></a>PowerShell のインストールを確認する

次のコマンドを実行してインストールを確認します。

```powershell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

## <a name="6-configure-powershell-to-use-a-proxy-server"></a>6.プロキシ サーバーを使用するように PowerShell を構成する

インターネットへのアクセスにプロキシ サーバーを必要とするシナリオでは、まず既存のプロキシ サーバーを使用するように PowerShell を構成します。

1. 管理者特権の PowerShell プロンプトを開きます。
2. 次のコマンドを実行します。

   ```powershell
   #To use Windows credentials for proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

   #Alternatively, to prompt for separate credentials that can be used for #proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
   ```

## <a name="next-steps"></a>次の手順

- [GitHub からの Azure Stack ツールのダウンロード](azure-stack-powershell-download.md)
- [Azure Stack ユーザーの PowerShell 環境の構成](../user/azure-stack-powershell-configure-user.md)
- [Azure Stack オペレーターの PowerShell 環境の構成](azure-stack-powershell-configure-admin.md)
- [Azure Stack での API バージョン プロファイルの管理](../user/azure-stack-version-profiles.md)
