---
title: Docker を使用した PowerShell for Azure Stack の実行 | Microsoft Docs
description: Docker を使用して Azure Stack 上で PowerShell を実行する
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: article
ms.date: 04/25/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 0eacd2c5a058bca68e86f2d34df8d3cc91987c1c
ms.sourcegitcommit: 85c3acd316fd61b4e94c991a9cd68aa97702073b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/01/2019
ms.locfileid: "64985556"
---
# <a name="use-docker-to-run-powershell"></a>Docker を使用して PowerShell を実行する

Docker を使用すると、Windows ベースのコンテナーを作成して、さまざまなインターフェイスの操作に必要な PowerShell の特定のバージョンを実行できます。 Docker の Windows ベースのコンテナーを使用する必要があります。

## <a name="docker-prerequisites"></a>Docker の前提条件

1. [Docker](https://docs.docker.com/install/) をインストールします。
2. Powershell や Bash などのコマンドラインを開き、次のように入力します。

    ```bash
        Docker -version
    ```

3. Windows10 を必要とする Windows コンテナーを使って Docker を実行する必要があります。 Docker を実行しているときに、Windows コンテナーに切り替えます。

4. Azure Stack と同じドメインに参加しているマシンから Docker を実行する必要があります。 ASDK を使用している場合は、[お使いのリモート マシンに VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) をインストールする必要があります。

## <a name="service-principals-for-using-powershell"></a>PowerShell を使用するためのサービス プリンシパル

PowerShell を使用して Azure Stack のリソースにアクセスするには、Azure AD テナントのサービス プリンシパルが必要になります。 ユーザーのロール ベースのアクセス制御を使用してアクセス許可を委任します。

1. 「[サービス プリンシパルを作成してアプリケーションに Azure Stack リソースへのアクセスを付与する](azure-stack-create-service-principals.md)」記事の手順に従って、プリンシパルを設定します。
2. アプリケーション ID、シークレット、お使いのテナント ID をメモします。

## <a name="docker---azure-stack-api-profiles-module"></a>Docker - Azure Stack API プロファイル モジュール

Dockerfile は、Windows PowerShell 5.1 がインストールされている Microsoft イメージ microsoft/windowsservercore を開きます。 その後、そのファイルは、Azure Stack PowerShell モジュールである NuGet を読み込み、Azure Stack ツールからツールをダウンロードします。

1. このリポジトリを ZIP としてダウンロードするか、リポジトリを複製します。  
[https://github.com/mattbriggs/azure-stack-powershell](https://github.com/mattbriggs/azure-stack-powershell)

2. ターミナルからリポジトリ フォルダーを開きます。

3. リポジトリでコマンドライン インターフェイスを開き、次のように入力します。

    ```bash  
    docker build --tag azure-stack-powershell .
    ```

4. イメージがビルドされると、対話型コンテナーを開始できます。 型: 

    ```bash  
        docker run -it azure-stack-powershell powershell
    ```

5. コマンドレットに対するシェルの準備が整いました。

    ```bash
    Windows PowerShell
    Copyright (C) 2016 Microsoft Corporation. All rights reserved.

    PS C:\>
    ```

6. サービス プリンシパルを使ってご使用の Azure Stack に接続します。 現在、Docker で PowerShell プロンプトを使用しています。 

    ```Powershell
    $passwd = ConvertTo-SecureString <Secret> -AsPlainText -Force
    $pscredential = New-Object System.Management.Automation.PSCredential('<ApplicationID>', $passwd)
    Connect-AzureRmAccount -ServicePrincipal -Credential $pscredential -TenantId <TenantID>
    ```

   PowerShell からアカウント オブジェクトが返されます。

    ```PowerShell  
    Account    SubscriptionName    TenantId    Environment
    -------    ----------------    --------    -----------
    <AccountID>    <SubName>       <TenantID>  AzureCloud
    ```

7. Azure Stack でリソース グループを作成して、接続をテストします。

    ```PowerShell  
    New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
    ```

## <a name="next-steps"></a>次の手順

-  [Azure Stack の Azure Stack PowerShell](azure-stack-powershell-overview.md) の概要に関するページをお読みください。
- Azure Stack の [PowerShell の API プロファイル](azure-stack-version-profiles.md)に関するページについてお読みください。
- [Azure Stack PowerShell のインストール](../operator/azure-stack-powershell-install.md)を行います。
- クラウドの一貫性のための [Azure Resource Manager テンプレート](azure-stack-develop-templates.md)の作成についてお読みください。