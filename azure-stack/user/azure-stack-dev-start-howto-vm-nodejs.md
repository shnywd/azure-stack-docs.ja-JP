---
title: Azure Stack 内の仮想マシンに Node.js アプリをデプロイする | Microsoft Docs
description: Azure Stack に Node.js アプリをデプロイします。
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: bebf3b349a994379d5f54bd387533b8d4a63ccdd
ms.sourcegitcommit: 41927cb812e6a705d8e414c5f605654da1fc6952
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "64482291"
---
# <a name="how-to-deploy-a-nodejs-web-app-to-a-vm-in-azure-stack"></a>Azure Stack 内の VM に Node.js Web アプリをデプロイする方法

Azure Stack 内にご自分の Node.js Web アプリをホストする VM を作成できます。 この記事では、サーバーを設定し、Node Web アプリをホストするようサーバーを構成して、ご自分のアプリをデプロイする手順を確認します。

Node.js は、Chrome の V8 JavaScript エンジン上に構築された JavaScript ランタイムです。 非同期イベント ドリブン JavaScript ランタイムとして Node はスケーラブルなネットワーク アプリケーションを作成できるよう設計されています。 Node プログラミング言語の詳細および Node の他のリソースについては、[Nodejs.org](https://nodejs.org) を参照してください。

## <a name="create-a-vm"></a>VM の作成

1. Azure Stack 内でご自分の VM を設定します。 「[Web アプリをホストする Linux VM を Azure Stack にデプロイする](azure-stack-dev-start-howto-deploy-linux.md)」の手順に従ってください。

2. VM ネットワーク ブレードで、次のポートにアクセスできることを確認します。

    | Port | Protocol | 説明 |
    | --- | --- | --- |
    | 80 | HTTP | ハイパー テキスト転送プロトコル (HTTP) は、分散した協調的なハイパーメディア情報システム向けのアプリケーション プロトコルです。 クライアントは、ご使用の VM のパブリック IP と DNS 名のどちらかを使用してご自分の Web アプリに接続します。 |
    | 443 | HTTPS | ハイパー テキスト転送プロトコル セキュア (HTTPS) は、ハイパー テキスト転送プロトコル (HTTP) を拡張したものです。 コンピューター ネットワーク経由のセキュリティで保護された通信に使用されます。 クライアントは、ご使用の VM のパブリック IP と DNS 名のどちらかを使用してご自分の Web アプリに接続します。 |
    | 22 | SSH | Secure Shell (SSH) は、セキュリティで保護されていないネットワーク経由でネットワーク サービスを安全に使用するための暗号化ネットワーク プロトコルです。 SSH クライアントとのこの接続を使用して、VM を構成し、アプリをデプロイします。 |
    | 3389 | RDP | 省略可能。 リモート デスクトップ プロトコルは、リモート デスクトップ接続を介して、マシンのグラフィック ユーザー インターフェイスを使用できるようにします。   |
    | 1337 | カスタム | ポート 1337 は、Node.js によって使用されます。 運用サーバーでは、80 と 443 を介してトラフィックをルーティングします。 |

## <a name="install-node"></a>Node をインストールする

1. SSH クライアントを使用して VM に接続します。 手順については、「[PuTTy を使用して SSH 経由で接続する](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty)」を参照してください。
1. ご自分の VM 上の Bash プロンプトで、次のコマンドを入力します。

    ```bash  
      sudo apt install nodejs-legacy
    ```

2. NPM をインストールします。 [NPM](https://www.npmjs.com/) は、Node.js パッケージ、つまりモジュール用のパッケージ マネージャーです。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
       go version
    ```

3. GIT をインストールします。 [Git](https://git-scm.com) は、広域分散型のリビジョン コントロールおよびソース コード管理 (SCM) システムです。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
       sudo apt-get -y install git
    ```

3. インストールを検証します。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
       node -v
    ```

## <a name="deploy-and-run-the-app"></a>アプリのデプロイと実行

1. VM 上でご自分の Git リポジトリを設定します。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
       git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
    
       cd nodejs-docs-hello-world
        npm start
    ```

2. アプリを起動します。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
       sudo node app.js
    ```

3.  次に、ご自分の新しいサーバーに移動すると、実行中のご自分の Web アプリケーションを確認できます。

    ```HTTP  
       http://yourhostname.cloudapp.net:1337
    ```

## <a name="next-steps"></a>次の手順

- [Azure Stack 向けの開発](azure-stack-dev-start.md)方法の詳細を確認する
- [IaaS としての Azure Stack 向けの一般的なデプロイ](azure-stack-dev-start-deploy-app.md)を確認する