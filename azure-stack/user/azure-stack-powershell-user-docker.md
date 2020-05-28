---
title: Docker を使用して Azure Stack Hub 内で PowerShell を実行する
description: Docker を使用して Azure Stack Hub 内で PowerShell を実行する
author: mattbriggs
ms.topic: article
ms.date: 5/27/2020
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 07/09/2019
ms.openlocfilehash: 9d761114908919ef1b98436997c7225ba1544ae3
ms.sourcegitcommit: cad40ae88212cc72f40c84a1c88143ea0abb65ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/27/2020
ms.locfileid: "84111831"
---
# <a name="use-docker-to-run-powershell-in-azure-stack-hub"></a>Docker を使用して Azure Stack Hub 内で PowerShell を実行する

この記事では、Docker を使用して、Windows ベースのコンテナーを作成します。そのコンテナー上で、さまざまなインターフェイスの操作に必要な PowerShell のバージョンを実行します。 Docker では、Windows ベースのコンテナーを使用する必要があります。

## <a name="docker-prerequisites"></a>Docker の前提条件

1. [Docker](https://docs.docker.com/install/) をインストールします。

1. PowerShell や Bash などのコマンドライン ツールで、次を入力します。

    ```bash
        Docker --version
    ```

1. Windows 10 を必要とする Windows コンテナーを使って Docker を実行する必要があります。 Docker を実行しているときに、Windows コンテナーに切り替えます。

1. Azure Stack Hub と同じドメインに参加しているマシンから Docker を実行します。 Azure Stack Development Kit (ASDK) を使用している場合は、[お使いのリモート マシン上に VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-hub-with-vpn) をインストールする必要があります。

## <a name="set-up-a-service-principal-for-using-powershell"></a>PowerShell を使用してサービス プリンシパルを設定する

PowerShell を使用して Azure Stack Hub 内のリソースにアクセスするには、お使いの Azure Active Directory (Azure AD) テナントのサービス プリンシパルが必要になります。 ユーザーのロールベースのアクセス制御 (RBAC) を使用してアクセス許可を委任します。

1. サービス プリンシパルを設定するには、[サービス プリンシパルを作成することによるアプリケーションへの Azure Stack Hub リソースに対するアクセスの付与](azure-stack-create-service-principals.md)に関するページの手順に従います。

2. 後で使用するためにアプリケーション ID、シークレット、お使いのテナント ID をメモします。

## <a name="docker---azure-stack-hub-api-profiles-module"></a>Docker - Azure Stack Hub API プロファイル モジュール

Dockerfile は、Windows PowerShell 5.1 がインストールされている Microsoft イメージ *microsoft/windowsservercore* を開きます。 その後、そのファイルは、NuGet と Azure Stack Hub PowerShell モジュールを読み込み、Azure Stack Hub ツールからツールをダウンロードします。

1. ZIP ファイルとして [azure-stack-powershell リポジトリをダウンロードする](https://github.com/Azure-Samples/azure-stack-hub-powershell-in-docker.git)か、そのリポジトリを複製します。

2. ターミナルからリポジトリ フォルダーを開きます。

3. お使いのリポジトリでコマンド ライン インターフェイスを開き、次のコマンドを入力します。

    ```bash  
    docker build --tag azure-stack-powershell .
    ```

4. イメージがビルドされたら、次を入力して、対話型コンテナーを開始します。

    ```bash  
        docker run -it azure-stack-powershell powershell
    ```

5. コマンドレットに対するシェルの準備が整いました。

    ```bash
    Windows PowerShell
    Copyright (C) 2016 Microsoft Corporation. All rights reserved.

    PS C:\>
    ```

6. サービス プリンシパルを使用して、お使いの Azure Stack Hub インスタンスに接続します。 現在、Docker で PowerShell プロンプトを使用しています。 

    ```powershell
    $passwd = ConvertTo-SecureString <Secret> -AsPlainText -Force
    $pscredential = New-Object System.Management.Automation.PSCredential('<ApplicationID>', $passwd)
    Connect-AzureRmAccount -ServicePrincipal -Credential $pscredential -TenantId <TenantID>
    ```

   PowerShell からアカウント オブジェクトが返されます。

    ```powershell  
    Account    SubscriptionName    TenantId    Environment
    -------    ----------------    --------    -----------
    <AccountID>    <SubName>       <TenantID>  AzureCloud
    ```

7. Azure Stack Hub でリソース グループを作成して、接続をテストします。

    ```powershell  
    New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
    ```

## <a name="next-steps"></a>次のステップ

-  [Azure Stack Hub の Azure Stack Hub PowerShell](azure-stack-powershell-overview.md) の概要をお読みください。
- Azure Stack Hub の [PowerShell の API プロファイル](azure-stack-version-profiles.md)についてお読みください。
- [Azure Stack Hub PowerShell](../operator/azure-stack-powershell-install.md) をインストールします。
- クラウドの一貫性のための [Azure Resource Manager テンプレート](azure-stack-develop-templates.md)の作成についてお読みください。
