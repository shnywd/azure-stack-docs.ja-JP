---
title: Visual Studio Code 内で Azure Account 拡張機能を使用して Azure Stack に接続する | Microsoft Docs
description: 開発者として Visual Studio Code 内で Azure Account 拡張機能を使用して Azure Stack に接続します
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 11/11/2019
ms.openlocfilehash: 58fecc801312d9da1a2311e4a51114cfbf245e9d
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955750"
---
# <a name="connect-to-azure-stack-using-azure-account-extension-in-visual-studio-code"></a>Visual Studio Code 内で Azure Account 拡張機能を使用して Azure Stack に接続する

この記事では、Azure Account 拡張機能を使用して Azure Stack に接続する方法について説明します。 ご自分の Visual Studio Code (VS Code) の設定を更新する必要があります。

VS Code は、Web とクラウドのアプリケーションのビルドとデバッグを行うための軽量エディターです。 ASP.NET Core、Python、NodeJS、Go などの開発者が VS Code を使用しています。 Azure Account 拡張機能を利用すると、追加の Azure 拡張機能に対するサブスクリプション フィルターと Azure シングル サインインを使用することができます。 この拡張機能により、VS Code 統合ターミナル内で Azure Cloud Shell を使用できるようになります。 この拡張機能を使用すると、ID マネージャーに Azure AD (Azure AD) と Active Directory フェデレーション サービス (AD FS) の両方を使用して、お使いの Azure Stack サブスクリプションに接続できます。 Azure Stack にサインインし、お使いのサブスクリプションを選択して、Cloud Shell 内で新しいコマンド ラインを開くことができます。 

> [!Note]  
> この記事の手順は、Active Directory フェデレーション サービス (AD FS) 環境に使用できます。 AD FS の資格情報とエンドポイントを使用します。

## <a name="pre-requisites-for-the-azure-account-extension"></a>Azure Account 拡張機能の前提条件

1. Azure Stack 環境 1904 ビルド以降
2. [Visual Studio Code](https://code.visualstudio.com/)
3. [Azure Account 拡張機能](https://github.com/Microsoft/vscode-azure-account)
4. [Azure Stack サブスクリプション](https://azure.microsoft.com/overview/azure-stack/)

## <a name="steps-to-connect-to-azure-stack"></a>Azure Stack に接続する手順

1. GitHub の Azure Stack ツールから **Identity** スクリプトを実行します。

    - スクリプトを実行する前に、PowerShell がインストールされ、お使いの環境用に構成されている必要があります。 手順については、「[PowerShell for Azure Stack をインストールする](../operator/azure-stack-powershell-install.md)」を参照してください。

    - **Identity** スクリプトの手順とスクリプトについては、「[AzureStack-Tools/Identity](https://github.com/Azure/AzureStack-Tools/tree/master/Identity)」を参照してください。

    - 同じセッションで、次のように実行します。

    ```powershell  
    Update-AzsHomeDirectoryTenant -AdminResourceManagerEndpoint $adminResourceManagerEndpoint `
    -DirectoryTenantName $homeDirectoryTenantName -Verbose
    Register-AzsWithMyDirectoryTenant -TenantResourceManagerEndpoint $tenantARMEndpoint `
    -DirectoryTenantName $guestDirectoryTenantName
    ```

2. VS Code を開きます。

3. 左隅にある **[拡張機能]** を選択します。

4. 検索ボックスに「 `Azure Account`」と入力します。

5. **[Azure Account]** を選択し、 **[インストール]** を選択します。

      ![Azure Stack Visual Studio Code](media/azure-stack-dev-start-vscode-azure/image1.png)

6. VS Code を再起動して、拡張機能を読み込みます。

7. お使いの Azure Stack 内の Azure Resource Manager に接続するためのメタデータを取得します。 
    
    Microsoft Azure Resource Manager は、Azure リソースのデプロイ、管理、監視を行うことができる管理フレームワークです。
    - Azure Stack Development Kit (ASDK) の Resource Manager URL: `https://management.local.azurestack.external/` 
    - 統合システムの Resource Manager URL: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`
    - メタデータにアクセスするために、次のテキストを実際の URL に追加します。`<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`

    たとえば、お使いの Azure Resource Manager エンドポイントのメタデータを取得する URL は次のようになります。`https://management.local.azurestack.external/metadata/endpoints?api-version=1.0`

    返された JSON をメモします。 `loginEndpoint` と `audiences` プロパティの値が必要になります。

8. **Ctrl + Shift + P** キーを押し、 **[Preferences:Open User Settings (JSON)]\(基本設定: ユーザー設定 (JSON) を開く\)** を選択します。

9. コード エディター内で、お使いの環境の値を使って次の JSON スニペットを更新し、スニペットを設定ブロック内に貼り付けます。

    - 値:

        | パラメーター | 説明 |
        | --- | --- |
        | `tenant-ID` | Azure Stack の[テナント ID](../operator/azure-stack-identity-overview.md) の値。 |
        | `activeDirectoryEndpointUrl` | これは loginEndpoint プロパティからの URL です。 |
        | `activeDirectoryResourceId` | これは、audiences プロパティからの URL です。
        | `resourceManagerEndpointUrl` | これは Azure Stack の Azure Resource Manager のルート URL です。 | 

    - JSON スニペット:

      ```JSON  
      "azure.tenant": "tenant-ID",
      "azure.ppe": {
          "activeDirectoryEndpointUrl": "Login endpoint",
          "activeDirectoryResourceId": "This is the URL from the audiences property.",
          "resourceManagerEndpointUrl": "Aure Resource Management Endpoint",
      },
      "azure.cloud": "AzurePPE"
      ```

10. ユーザー設定を保存し、**Ctrl + Shift + P** キーをもう一度使用します。 **[Azure:Sign in to Azure Cloud]\(Azure: Azure クラウドへのサインイン\)** を選択します。 ターゲットの一覧に新しいオプション **[AzurePPE]** が表示されます。

11. **[AzurePPE]** を選択します。 お使いのブラウザーに認証ページが読み込まれます。 お使いのエンドポイントにサインインします。

12. お使いの Azure Stack サブスクリプションに正常にログインしたことをテストするために、**Ctrl + Shift + P** キーを使用し、 **[Azure:Select Subscription]\(Azure: サブスクリプションの選択\)** を選択し、ご自分のサブスクリプションが使用できるかどうかを確認します。

## <a name="commands"></a>command

| Azure: Sign In (Azure: サインイン) | Azure サブスクリプションにサインインします。 |
| --- | --- |
| Azure: Sign In with Device Code (Azure: デバイス コードを使用したサインイン) | デバイス コードを使用して、お使いの Azure サブスクリプションにサインインします。 セットアップで、サインイン コマンドが動作しないデバイス コードを使用します。 |
| Azure: Sign in to Azure Cloud (Azure: Azure クラウドへのサインイン) | いずれかのソブリン クラウドでお使いの Azure サブスクリプションにサインインします。 |
| Azure: サインアウト | お使いの Azure サブスクリプションからサインアウトします。 |
| Azure: [サブスクリプション] を選択します。 | 使用するサブスクリプションのセットを選択します。 この拡張機能では、フィルター処理されたサブスクリプション内のリソースのみが表示されます。 |
| Azure: アカウントの作成 | Azure アカウントを持っていない場合、今すぐ[サインアップ](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-azure-account&mktingSource=vscode-azure-account)し、\$200 の無料クレジットを手に入れることができます。 |
| Azure: Open Bash in Cloud Shell (Azure: Cloud Shell 内で Bash を開く) | Cloud Shell 内で Bash を実行している新しいターミナルを開きます。 |
| Azure: Open PowerShell in Cloud Shell (Azure: Cloud Shell 内で PowerShell を開く) | Cloud Shell 内で PowerShell を実行している新しいターミナルを開きます。 |
| Azure: Upload to Cloud Shell (Azure: Cloud Shell へのアップロード) | お使いの Cloud Shell ストレージ アカウントにファイルをアップロードします。 |

## <a name="next-steps"></a>次の手順

[Azure Stack 内で開発環境を設定する](azure-stack-dev-start.md)
