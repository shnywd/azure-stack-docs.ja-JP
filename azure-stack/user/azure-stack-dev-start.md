---
title: Azure Stack 内で開発環境を設定する | Microsoft Docs
description: Azure Stack 向けのアプリケーションの開発を開始します。
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 07/30/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 07/30/2019
ms.openlocfilehash: 404f6b889f921e547f8c44c7b4219b232819c322
ms.sourcegitcommit: 7961fda0bfcdd3db8cf94a8c405b5c23a23643af
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/29/2019
ms.locfileid: "68616463"
---
# <a name="set-up-a-development-environment-in-azure-stack"></a>Azure Stack 内で開発環境を設定する 

Windows 10、Linux、または macOS のワークステーションを使用して Azure Stack 向けのアプリケーションを開発できます。 この記事では、次のことについて説明します。 

- Azure Stack 内でご自分のアプリを実行するさまざまなコンテキスト。 
- Windows 10、Linux、または macOS のワークステーションを設定するための手順。 
- Azure Stack 内でリソースを作成してアプリにデプロイするための手順。 

## <a name="azure-stack-context-and-your-code"></a>Azure Stack のコンテキストとご自分のコード 

Azure Stack 内で多くのタスクを実行するスクリプトおよびアプリを記述できます。 ただし、次の 3 つのモードにスコープを制限することをお勧めします。 

1. 1 つ目のモードでは、Azure Resource Manager テンプレートを使用して Azure Stack 内にリソースをプロビジョニングするアプリを作成できます。 たとえば、仮想ネットワークと、ご自分のアプリをホストする VM を作成する Azure Resource Manager テンプレートを構築するスクリプトを記述できます。 

2. 2 つ目のモードでは、REST API と、ご自分のコードで作成した REST クライアントを使用して、エンドポイントを直接操作します。 このモードでは、API に要求を送信することによって仮想ネットワークと VM を作成するスクリプトを記述します。 

3. 3 つ目のモードでは、ご自分のコードを使用して、Azure Stack 内でホストされるアプリを作成できます。 ご自分のアプリをホストするインフラストラクチャを Azure Stack 内に作成したら、ご自分のアプリをインフラストラクチャにデプロイします。 通常、環境を準備してから、そこへご自分のアプリをデプロイします。 

###  <a name="infrastructure-as-a-service-and-platform-as-a-service"></a>サービスとしてのインフラストラクチャとサービスとしてのプラットフォーム 

クラウド プラットフォーム製品として、Azure Stack は次の両方をサポートしています。 

- サービスとしてのインフラストラクチャ (IaaS) 
- サービスとしてのプラットフォーム (PaaS) 

IaaS と PaaS はどちらも、ご使用になる開発用マシンの設定方法を示します。 

IaaS は、ネットワーク機器、ネットワーク、およびサーバーに由来するデータセンターの各パーツの仮想化です。 Web サーバーをホストしている VM にアプリをデプロイする場合は、IaaS モデルの中で作業していることになります。 このモデルでは、Azure Stack が仮想機器を管理し、アプリは仮想サーバー上に配置されます。 Azure Stack リソース プロバイダーでは、ネットワーク コンポーネントと仮想サーバーがサポートされます。 

PaaS ではインフラストラクチャ レイヤーが抽象化されるため、アプリをエンドポイントにデプロイすると、そこでアプリが実行されます。 PaaS モデルでは、コンテナーを使用してアプリをホストした後、そのコンテナーを実行するサービスにコンテナー化されたアプリをデプロイすることができます。 また、アプリを実行するサービスに直接アプリをプッシュすることもできます。 Azure Stack を使用して、Azure App Service と Kubernetes を実行できます。 

### <a name="azure-stack-resource-manager"></a>Azure Stack Resource Manager 

前述の 3 つのモードと PaaS または IaaS は、Azure Stack バージョンの Azure Resource Manager によって有効になります。 この管理フレームワークを使用して、Azure Stack リソースをデプロイ、管理、監視できます。 1 回の操作で複数のリソースをグループとして操作できます。 Azure Stack Resource Manager の使用の詳細については、「[Azure Stack での API バージョンのプロファイルの管理](azure-stack-version-profiles.md)」を参照してください。 

### <a name="azure-stack-sdks"></a>Azure Stack SDK 

Azure Stack では、Azure Stack バージョンの Azure Resource Manager を使用します。 任意のコードを使用して Azure Stack Resource Manager を操作できるように、次のようなさまざまな SDK が用意されています。 

- [.NET/C#](azure-stack-version-profiles-net.md)
- [Java](azure-stack-version-profiles-java.md)
- [Go](azure-stack-version-profiles-go.md)
- [Ruby](azure-stack-version-profiles-ruby.md)
- [Python](azure-stack-version-profiles-python.md)
- [Node.JS](azure-stack-version-profile-nodejs.md)

## <a name="before-you-start"></a>開始する前に 

環境の設定を始める前に、次のものが必要です。 

- Azure Stack ユーザー ポータルへのアクセス。 
- ご使用のテナントの名前。 
- ID マネージャーとして、Azure Active Directory (Azure AD) と Active Directory フェデレーション サービス (AD FS) のどちらを使用しているかを確認すること。 

Azure Stack について質問がある場合は、お客様のクラウド オペレーターに問い合わせてください。 

## <a name="windows-10"></a>Windows 10 

Windows 10 マシンを使用している場合は、PowerShell 5.0 と Visual Studio を使用できます。 また、Azure Stack Development Kit (ASDK) を使用している場合は、VPN 接続を使用してお使いの環境に接続できます。 

### <a name="set-up-your-tools"></a>ご使用になるツールを設定する 

1. PowerShell の設定を行います。 手順については、[Azure Stack PowerShell のインストール](../operator/azure-stack-powershell-install.md)に関するページを参照してください。 

2. Azure Stack ツールをダウンロードします。 手順については、「[GitHub からの Azure Stack ツールのダウンロード](../operator/azure-stack-powershell-download.md)」を参照してください。 

3. ASDK を使用する場合は、インストールし、[Azure Stack への VPN 接続](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn)を構成します。 

4. Azure CLI をインストールして構成します。 手順については、「[Azure Stack での Azure CLI による API バージョンのプロファイルの使用](azure-stack-version-profiles-azurecli2.md)」を参照してください。 

5. Azure Storage Explorer をインストールして構成します。 Storage Explorer は、Azure Stack ストレージのデータを操作できるスタンドアロンのアプリです。 手順については、[Azure Stack サブスクリプションまたはストレージ アカウントへの Storage Explorer の接続](azure-stack-storage-connect-se.md)に関するページを参照してください。 

### <a name="install-your-integrated-development-environment"></a>ご使用になる統合開発環境をインストールする 

1. ご使用になるコード ベースや好みに応じて、統合開発環境 (IDE) をインストールします。 

     - Visual Studio Code (Python、Go、NodeJS)。 [code.visualstudio.com](https://code.visualstudio.com/Download) からご使用のマシンに Visual Studio Code をダウンロードします。 
     - Visual Studio (.NET/C#)。 [visualstudio.microsoft.com](https://visualstudio.microsoft.com/vs/community/) から Visual Studio Community Edition をダウンロードします。 
     - Eclipse (Java)。 [eclipse.org](https://www.eclipse.org/downloads/) から Eclipse をダウンロードします。 

2. ご自分のコード用の SDK をインストールします。 

     - [.NET/C#](azure-stack-version-profiles-net.md) 
     - [Java](azure-stack-version-profiles-java.md) 
     - [Go](azure-stack-version-profiles-go.md) 
     - [Ruby](azure-stack-version-profiles-python.md) 
     - [Python](azure-stack-version-profiles-python.md) 

## <a name="linux"></a>Linux 

Linux マシンを使用している場合は、Azure CLI と Visual Studio Code、またはお好みの統合開発環境を使用できます。 

> [!Note]   
> Linux マシンと ASDK を使用する場合、ご使用のリモート マシンが ASDK と同じネットワーク内にある必要があります。 仮想プライベート ネットワーク接続を使用して接続することはできません。 

### <a name="set-up-your-tools"></a>ご使用になるツールを設定する 

1. Azure CLI をインストールして構成します。 手順については、「[Azure Stack での Azure CLI による API バージョンのプロファイルの使用](azure-stack-version-profiles-azurecli2.md)」を参照してください。 

2. Azure Storage Explorer をインストールして構成します。 Storage Explorer は、Azure Stack ストレージのデータを操作できるスタンドアロンのアプリです。 手順については、[Azure Stack サブスクリプションまたはストレージ アカウントへの Storage Explorer の接続](azure-stack-storage-connect-se.md)に関するページを参照してください。 

### <a name="install-your-integrated-development-environment"></a>ご使用になる統合開発環境をインストールする 

1. ご使用になるコード ベースや好みに応じて、統合開発環境 (IDE) をインストールします。 

     - Visual Studio Code (Python、Go、NodeJS)。 [code.visualstudio.com](https://code.visualstudio.com/Download) からご使用のマシンに Visual Studio Code をダウンロードします。 
     - Visual Studio (.NET/C#)。 [visualstudio.microsoft.com](https://visualstudio.microsoft.com/vs/community/) から Visual Studio Community Edition をダウンロードします。 
     - Eclipse (Java)。 [eclipse.org](https://www.eclipse.org/downloads/) から Eclipse をダウンロードします。 

2. ご自分のコード用の SDK をインストールします。 

     - [.NET/C#](azure-stack-version-profiles-net.md) 
     - [Java](azure-stack-version-profiles-java.md) 
     - [Go](azure-stack-version-profiles-go.md) 
     - [Ruby](azure-stack-version-profiles-python.md) 
     - [Python](azure-stack-version-profiles-python.md) 

## <a name="macos"></a>macOS 

macOS マシンでは、Azure CLI と Visual Studio Code、またはお好みの統合開発環境を使用できます。 

> [!Note]   
> macOS マシンと ASDK を使用する場合、ご使用のリモート マシンが ASDK と同じネットワーク内にある必要があります。 仮想プライベート ネットワーク接続を使用して接続することはできません。 

### <a name="set-up-your-tools"></a>ご使用になるツールを設定する 

1. Azure CLI をインストールして構成します。 手順については、「[Azure Stack での Azure CLI による API バージョンのプロファイルの使用](azure-stack-version-profiles-azurecli2.md)」を参照してください。 

2. Azure Storage Explorer をインストールして構成します。 Storage Explorer は、Azure Stack ストレージのデータを操作できるスタンドアロンのアプリです。 手順については、[Azure Stack サブスクリプションまたはストレージ アカウントへの Storage Explorer の接続](azure-stack-storage-connect-se.md)に関するページを参照してください。 

### <a name="install-your-integrated-development-environment"></a>ご使用になる統合開発環境をインストールする 

1. ご使用になるコード ベースや好みに応じて、統合開発環境 (IDE) をインストールします。 

     - Visual Studio Code (Python、Go、NodeJS)。 [code.visualstudio.com](https://code.visualstudio.com/Download) からご使用のマシンに Visual Studio Code をダウンロードします。 
     - Visual Studio (.NET/C#)。 [visualstudio.microsoft.com](https://visualstudio.microsoft.com/vs/community/) から Visual Studio Community Edition をダウンロードします。 
     - Eclipse (Java)。 [eclipse.org](https://www.eclipse.org/downloads/) から Eclipse をダウンロードします。 

2. ご自分のコード用の SDK をインストールします。 

     - [.NET/C#](azure-stack-version-profiles-net.md) 
     - [Java](azure-stack-version-profiles-java.md) 
     - [Go](azure-stack-version-profiles-go.md)
     - [Ruby](azure-stack-version-profiles-python.md) 
     - [Python](azure-stack-version-profiles-python.md) 

## <a name="next-steps"></a>次の手順 

アプリを Azure Stack 内のリソースにデプロイするには、「[Azure Stack 向けの一般的なデプロイ](azure-stack-dev-start-deploy-app.md)」を参照してください。
