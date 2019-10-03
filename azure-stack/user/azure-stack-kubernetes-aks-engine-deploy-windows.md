---
title: Azure Stack の Windows に AKS エンジンをデプロイする | Microsoft Docs
description: Azure Stack の Windows マシンを使用して、Kubernetes クラスターをデプロイおよび管理するために AKS エンジンをホストする方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na (Kubernetes)
ms.devlang: nav
ms.topic: article
ms.date: 09/27/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 09/27/2019
ms.openlocfilehash: bd5d1d473bd72b65df3377fb7b2de79b0aa6c7b0
ms.sourcegitcommit: 036d4b22a8076ca9ba5c667a451c544d88f8bb94
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2019
ms.locfileid: "71681811"
---
# <a name="install-the-aks-engine-on-windows-in-azure-stack"></a>Azure Stack の Windows に AKS エンジンをインストールする

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure Stack の Windows マシンを使用して、Kubernetes クラスターをデプロイおよび管理するために AKS エンジンをホストできます。 この記事では、Azure Stack の接続されているインスタンスと接続されていないインスタンスの両方についてクラスターを管理するためのクライアント VM の準備、インストールの確認、ASDK でのクライアント VM の設定について説明します。

## <a name="prepare-the-client-vm"></a>クライアント VM の準備

AKS エンジンとは、お使いの Kubernetes クラスターをデプロイおよび管理するために使用するコマンドライン ツールです。 このエンジンは、お使いの Azure Stack のコンピューターで実行できます。 このコンピューターから AKS エンジンを実行して、お使いのクラスターの実行に必要な IaaS リソースとソフトウェアをデプロイします。 その後、このエンジンを実行しているコンピューターを使用して、お使いのクラスターで管理タスクを実行できます。

お使いのクライアント コンピューターを選択する場合には、次の点を考慮してください。

1. 障害が発生した場合にクライアント コンピューターを回復可能にする必要があるか。
3. クライアント コンピューターにどのように接続するかと、コンピューターがどのようにご自分のクラスターと通信するか。

## <a name="install-in-a-connected-environment"></a>接続されている環境へのインストール

クライアント VM をインストールし、インターネットに接続されている Azure Stack 上の Kubernetes クラスターを管理できます。

1. Azure Stack で Windows VM を作成します。 手順については、「[クイック スタート:Azure Stack ポータルを使用して Windows サーバー VM を作成する](https://docs.microsoft.com/azure-stack/user/azure-stack-quick-windows-portal)」を参照してください。
2. VM に接続します。
3. [PowerShell の手順に従って Chocolatey をインストールします](https://chocolatey.org/install#install-with-powershellexe)。 

    Chocolaty の Web サイトによると:Chocolatey は、Windows のパッケージ マネージャーです。例えば、Windows 用の apt-get や yum などです。 必要なアプリケーションとツールを迅速にインストールするための分散型フレームワークとして設計されています。 現在 PowerShell を使用している NuGet インフラストラクチャ上に構築されており、ディストリビューションからドア、err、コンピューターへのパッケージの配信に重点を置いています。
4. [サポート対象の Kubernetes バージョン](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#supported-kubernetes-versions)の表で、AKS エンジンのバージョンを確認します。 この AKS ベースのエンジンは、ご自分の Azure Stack の Marketplace で入手できるようになっている必要があります。 コマンドを実行するときに、バージョン `--version v0.40.1` を指定する必要があります。 バージョンを指定しないと、このコマンドによって最新バージョンがインストールされ、最新バージョンに必要な VHD イメージがご自分の Marketplace にはない可能性があります。
5. 管理者特権でのプロンプトで次のコマンドを実行し、バージョン番号を含めます。

    ```PowerShell  
        choco install aks-engine --version v0.40.1 -y
    ```

> [!Note]  
> このインストール方法が失敗する場合は、[切断されている環境](#install-in-a-disconnected-environment)の手順を試すか、別のパッケージ マネージャーである [GoFish](azure-stack-kubernetes-aks-engine-troubleshoot.md#try-gofish) を試してください。

## <a name="install-in-a-disconnected-environment"></a>切断されている環境へのインストール

インターネットから切断されている Azure Stack 上のお使いの Kubernetes クラスターは、クライアント VM をインストールして管理できます。

1.  インターネットにアクセスできるマシンから、GitHub [Azure/aks-engine](https://github.com/Azure/aks-engine/releases/latest) に移動します。 `aks-engine-v0.38.8-windows-amd64.tar.gz` など、Windows マシンのアーカイブ (*.tar.gz) をダウンロードします。

2.  Azure Stack インスタンスにストレージ アカウントを作成し、AKS エンジン バイナリを使用してアーカイブ ファイル (*.tar.gz) をアップロードします。 Azure Storage Explorer の使用方法については、[Azure Stack と Azure Storage Explorer](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-connect-se) に関するページを参照してください。

3. Azure Stack で Windows VM を作成します。 手順については、「[クイック スタート:Azure Stack ポータルを使用して Windows サーバー VM を作成する](https://docs.microsoft.com/azure-stack/user/azure-stack-quick-windows-portal)」を参照してください。

4.  アーカイブ ファイル (* tar.gz) をアップロードした Azure Stack ストレージ アカウントの blob URL から、お使いの管理 VM にファイルをダウンロードします。 コマンド プロンプトから、アクセス権のあるディレクトリにアーカイブを抽出します。

5. VM に接続します。

6. [PowerShell の手順に従って Chocolatey をインストールします](https://chocolatey.org/install#install-with-powershellexe)。 

7.  管理者特権でのプロンプトで、次のコマンドを実行します。 適切なバージョン番号を含めます。

    ```PowerShell  
        choco install aks-engine --version v0.40.1 -y
    ```

## <a name="verify-the-installation"></a>インストールの確認

お使いのクライアント VM がセットアップされたら、AKS エンジンがインストールされていることを確認します。

1. お使いのクライアント VM に接続します。
2. 次のコマンドを実行します。

    ```PowerShell  
    aks-engine version
    ```

クライアント VM に AKS エンジンがインストールされていることを確認できない場合は、[AKS エンジンのインストールのトラブルシューティング](azure-stack-kubernetes-aks-engine-troubleshoot.md)に関するページを参照してください。


## <a name="asdk-installation"></a>ASDK のインストール

ASDK の外部のマシン上の ASDK で AKS エンジン用のクライアント VM を実行する場合には、証明書を追加する必要があります。 ASDK 環境内で Windows VM を使用している場合、マシンでは既に ASDK 証明書が信頼されています。 クライアント マシンが ASDK の外部にある場合は、ASDK から証明書を抽出し、Windows マシンに追加する必要があります。

ASDK を使用している場合、Azure Resource Manager エンドポイントで自己署名証明書が使用されているため、この証明書をマシンの信頼された証明書ストアに明示的に追加する必要があります。 ASDK のルート証明書は、ASDK にデプロイするすべての VM にあります。

1. CA ルート証明書をエクスポートします。 手順については、「[Azure Stack の CA ルート証明書をエクスポートする](https://docs.microsoft.com/azure-stack/user/azure-stack-version-profiles-azurecli2#export-the-azure-stack-ca-root-certificate)」を参照してください。
2. Azure Stack の CA ルート証明書を信頼します。 手順については、「[Azure Stack の CA ルート証明書を信頼する](https://docs.microsoft.com/azure-stack/user/azure-stack-version-profiles-azurecli2#trust-the-azure-stack-ca-root-certificate)」を参照してください。

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [AKS エンジンを使用して Azure Stack に Kubernetes クラスターをデプロイする](azure-stack-kubernetes-aks-engine-deploy-cluster.md)
