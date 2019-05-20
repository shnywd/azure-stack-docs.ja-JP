---
title: Azure Stack 内の仮想マシンに GO アプリをデプロイする方法 | Microsoft Docs
description: Azure Stack 内の VM に GO Web アプリをデプロイする方法
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: c0cef076522e77a6d0fdafbd8848d5e9bb8a90a8
ms.sourcegitcommit: 41927cb812e6a705d8e414c5f605654da1fc6952
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "64482285"
---
# <a name="how-to-deploy-a-go-web-app-to-a-vm-in-azure-stack"></a>Azure Stack 内の VM に GO Web アプリをデプロイする方法

Azure Stack 内にご自分の Go Web アプリをホストする VM を作成できます。 この記事では、サーバーを設定し、GO Web アプリをホストするようサーバーを構成して、ご自分のアプリをデプロイする手順を確認します。

Go は、表現力豊かなうえに、簡潔で、わかりやすく、効率的です。 そのコンカレンシー メカニズムにより、マルチコアとネットワークに接続されたマシンを最大限に活用するプログラムを簡単に記述できるだけでなく、その型システムにより、柔軟なモジュール型のプログラム構築が可能になっています。 Go プログラミング言語の詳細および GO の他のリソースについては、[Golang.org](https://golang.org) を参照してください。

## <a name="create-a-vm"></a>VM の作成

1. Azure Stack 内でご自分の VM を設定します。 「[Web アプリをホストする Linux VM を Azure Stack にデプロイする](azure-stack-dev-start-howto-deploy-linux.md)」の手順に従ってください。

2. VM ネットワーク ブレードで、次のポートにアクセスできることを確認します。

    | Port | Protocol | 説明 |
    | --- | --- | --- |
    | 80 | HTTP | ハイパー テキスト転送プロトコル (HTTP) は、分散した協調的なハイパーメディア情報システム向けのアプリケーション プロトコルです。 クライアントは、ご使用の VM のパブリック IP と DNS 名のどちらかを使用してご自分の Web アプリに接続します。 |
    | 443 | HTTPS | ハイパー テキスト転送プロトコル セキュア (HTTPS) は、ハイパー テキスト転送プロトコル (HTTP) を拡張したものです。 コンピューター ネットワーク経由のセキュリティで保護された通信に使用されます。 クライアントは、ご使用の VM のパブリック IP と DNS 名のどちらかを使用してご自分の Web アプリに接続します。 |
    | 22 | SSH | Secure Shell (SSH) は、セキュリティで保護されていないネットワーク経由でネットワーク サービスを安全に使用するための暗号化ネットワーク プロトコルです。 SSH クライアントとのこの接続を使用して、VM を構成し、アプリをデプロイします。 |
    | 3389 | RDP | 省略可能。 リモート デスクトップ プロトコルは、リモート デスクトップ接続を介して、マシンのグラフィック ユーザー インターフェイスを使用できるようにします。   |
    | 3000 | カスタム | ポート 3000 は、開発時に GO Web フレームワークによって使用されます。 運用サーバーでは、80 と 443 を介してトラフィックをルーティングします。 |

## <a name="install-go"></a>GO をインストールする

1. SSH クライアントを使用して VM に接続します。 手順については、「[PuTTy を使用して SSH 経由で接続する](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty)」を参照してください。
1. ご自分の VM 上の Bash プロンプトで、次のコマンドを入力します。

    ```bash  
    wget https://dl.google.com/go/go1.10.linux-amd64.tar.gz
    sudo tar -xvf go1.10.linux-amd64.tar.gz
    sudo mv go /usr/local
    ```

2. ご自分の VM 上に GO 環境を設定します。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
    export GOROOT=/usr/local/go
    export GOPATH=$HOME/Projects/ADMFactory/Golang
    export PATH=$GOPATH/bin:$GOROOT/bin:$PATH

    vi ~/.profile
    ```

3. インストールを検証します。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
        go version
    ```

3. GIT をインストールします。 [Git](https://git-scm.com) は、広域分散型のリビジョン コントロールおよびソース コード管理 (SCM) システムです。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
       sudo apt-get -y install git
    ```

## <a name="deploy-and-run-the-app"></a>アプリのデプロイと実行

1. VM 上でご自分の Git リポジトリを設定します。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
       git clone https://github.com/appleboy/go-hello
    
       cd go-hello
       go get -d
    ```

2. アプリを起動します。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
       go run hello-world.go
    ```

3.  次に、ご自分の新しいサーバーに移動すると、実行中のご自分の Web アプリケーションを確認できます。

    ```HTTP  
       http://yourhostname.cloudapp.net:3000
    ```

## <a name="next-steps"></a>次の手順

- [Azure Stack 向けの開発](azure-stack-dev-start.md)方法の詳細を確認する
- [IaaS としての Azure Stack 向けの一般的なデプロイ](azure-stack-dev-start-deploy-app.md)を確認する