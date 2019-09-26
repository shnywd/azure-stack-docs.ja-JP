---
title: Azure Stack の Linux に AKS エンジンをインストールする | Microsoft Docs
description: お使いの Azure Stack の Linux コンピューターを使用して、Kubernetes クラスターをデプロイおよび管理するために AKS エンジンをホストする方法について説明します。
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
ms.date: 09/14/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 09/14/2019
ms.openlocfilehash: 7e2ac217ead86502513b7914a102a029f2472a40
ms.sourcegitcommit: 09d14eb77a43fd585e7e6be93c32fa427770adb6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019475"
---
# <a name="install-the-aks-engine-on-linux-in-azure-stack"></a>Azure Stack の Linux に AKS エンジンをインストールする

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

お使いの Azure Stack の Linux コンピューターを使用して、Kubernetes クラスターをデプロイおよび管理するために AKS エンジンをホストできます。 この記事では、お使いの接続および切断されている両 Azure Stack インスタンスのクラスターを管理するためのクライアント VM の準備、インストールの確認、ASDK でのクライアント VM の設定について説明します。

## <a name="prepare-the-client-vm"></a>クライアント VM の準備

AKS エンジンとは、お使いの Kubernetes クラスターをデプロイおよび管理するために使用するコマンドライン ツールです。 このエンジンは、お使いの Azure Stack のコンピューターで実行できます。 このコンピューターから AKS エンジンを実行して、お使いのクラスターの実行に必要な IaaS リソースとソフトウェアをデプロイします。 その後、このエンジンを実行しているコンピューターを使用して、お使いのクラスターで管理タスクを実行できます。

お使いのクライアント コンピューターを選択する場合には、次の点を考慮してください。

1. 障害が発生した場合にクライアント コンピューターを回復可能にする必要があるか。
2. クライアント コンピューターにどのように接続するかと、コンピューターがどのようにご自分のクラスターと通信するか。

## <a name="install-in-a-connected-environment"></a>接続されている環境へのインストール

クライアント VM をインストールし、インターネットに接続されている Azure Stack 上のお使いの Kubernetes クラスターを管理できます。

1. お使いの Azure Stack に Linux VM を作成します。 手順については、「[クイック スタート:Azure Stack ポータルを使用して Linux サーバー VM を作成する](https://docs.microsoft.com/azure-stack/user/azure-stack-quick-linux-portal)」を参照してください。
2. お使いの VM に接続します。
3. [サポート対象の Kubernetes バージョン](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#supported-kubernetes-versions)の表で、その AKS エンジンのバージョンを確認します。 この AKS ベースのエンジンは、ご自分の Azure Stack の Marketplace で入手できるようになっている必要があります。 コマンドを実行する場合、`--version v0.39.0` のようにバージョンを指定します。 バージョンを指定しない場合、ご自分の Marketplace にはない可能性がある最新のバージョンがコマンドでインストールされます。
4. 次のコマンドを実行します。

    ```bash  
        curl -o get-akse.sh https://raw.githubusercontent.com/Azure/aks-engine/master/scripts/get-akse.sh
        chmod 700 get-akse.sh
        ./get-akse.sh --version v0.XX.X
    ```

    > [!Note]  
    > インストール方法が失敗する場合は、[切断されている環境](#install-in-a-disconnected-environment)の手順を試すか、別のパッケージ マネージャーである [GoFish ](azure-stack-kubernetes-aks-engine-troubleshoot.md#try-gofish)を試してください。

## <a name="install-in-a-disconnected-environment"></a>切断されている環境へのインストール

インターネットから切断されている Azure Stack 上のお使いの Kubernetes クラスターは、クライアント VM をインストールして管理できます。

1.  インターネットにアクセスできるコンピューターから、GitHub [Azure/aks-engine](https://github.com/Azure/aks-engine/releases/latest) に移動します。 `aks-engine-v0.xx.x-linux-amd64.tar.gz` など、Linux コンピューターのアーカイブ (*.tar.gz) をダウンロードします。

2.  お使いの Azure Stack インスタンスにストレージ アカウントを作成し、AKS エンジン バイナリを使用してアーカイブ ファイル (*.tar.gz) をアップロードします。 Azure Storage Explorer の使用方法については、[Azure Stack と Azure Storage Explorer](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-connect-se) に関するページを参照してください。

3. お使いの Azure Stack に Linux VM を作成します。 手順については、「[クイック スタート:Azure Stack ポータルを使用して Linux サーバー VM を作成する](https://docs.microsoft.com/azure-stack/user/azure-stack-quick-linux-portal)」を参照してください。

3.  アーカイブ ファイル (* tar.gz) をアップロードした Azure Stack ストレージ アカウントの blob URL から、お使いの管理 VM にファイルをダウンロードします。 アーカイブをディレクトリ `/usr/local/bin` に抽出します。

4. お使いの VM に接続します。

5.  次のコマンドを実行します。

    ```bash  
    curl -o aks-engine-v0.xx.x-linux-amd64.tar.gz <httpurl/aks-engine-v0.xx.x-linux-amd64.tar.gz>
    tar xvzf aks-engine-v0.xx.x-linux-amd64.tar.gz -C /usr/local/bin
    ```

## <a name="verify-the-installation"></a>インストールの確認

お使いのクライアント VM がセットアップされたら、AKS エンジンがインストールされていることを確認します。

1. お使いのクライアント VM に接続します。
2. 次のコマンドを実行します。

    ```bash  
    aks-engine version
    ```

ご自分のクライアント VM に AKS エンジンがインストールされていることを確認できない場合は、[AKS エンジンのインストールのトラブルシューティング](azure-stack-kubernetes-aks-engine-troubleshoot.md)に関するページを参照してください。


## <a name="asdk-installation"></a>ASDK のインストール

ASDK の AKS エンジン用のクライアント VM を実行する場合には、証明書を追加する必要があります。

ご自分の Azure Resource Manager エンドポイントが自己署名証明書を使用する ASDK を使用している場合、コンピューターの信頼された証明書ストアにこの証明書を明示的に追加する必要があります。 ASDK のルート証明書は、ASDK にデプロイするすべての VM にあります。 たとえば、Ubuntu VM では、このディレクトリは `/var/lib/waagent/Certificates.pem` にあります。 

次のコマンドを使用して証明書ファイルをコピーします。

```bash
sudo cp /var/lib/waagent/Certificates.pem /usr/local/share/ca-certificates/azurestackca.crt

sudo update-ca-certificates
```

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [AKS エンジンを使用して Azure Stack に Kubernetes クラスターをデプロイする](azure-stack-kubernetes-aks-engine-deploy-cluster.md)