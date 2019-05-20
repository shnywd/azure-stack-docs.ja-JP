---
title: Azure Stack 内の仮想マシンに Python Web アプリをデプロイする方法 | Microsoft Docs
description: Azure Stack 内の仮想マシンに Python Web アプリをデプロイします。
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: b1a588dd084962c095a534f6569333b34e694bc6
ms.sourcegitcommit: 41927cb812e6a705d8e414c5f605654da1fc6952
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "64482255"
---
# <a name="how-to-deploy-a-python-web-app-to-a-vm-in-azure-stack"></a>Azure Stack 内の VM に Python Web アプリをデプロイする方法

Azure Stack 内にご自分の Python Web アプリをホストする VM を作成できます。 この記事では、サーバーを設定し、Python Web アプリをホストするようサーバーを構成して、アプリをデプロイする手順を確認します。

Python は、インタープリター型の高度な汎用プログラミング言語です。 Guido van Rossum によって作成され、1991 年に初めてリリースされた Python は特に有意の空白を使用したコードの読みやすさを重視する設計理念を掲げています。 小規模と大規模のどちらでもわかりやすいプログラミングを実現するコンストラクトを提供しています。 Python プログラミング言語の詳細および Python の他のリソースについては、[Python.org](https://www.python.org) を参照してください。

この記事では、Ngnix サーバー上の仮想環境内で Flask を実行している Python 3.x を使用します。

## <a name="create-a-vm"></a>VM の作成

1. Azure Stack 内でご自分の VM を設定します。 「[Web アプリをホストする Linux VM を Azure Stack にデプロイする](azure-stack-dev-start-howto-deploy-linux.md)」の手順に従ってください。

2. VM ネットワーク ブレードで、次のポートにアクセスできることを確認します。

    | Port | Protocol | 説明 |
    | --- | --- | --- |
    | 80 | HTTP | ハイパー テキスト転送プロトコル (HTTP) は、分散した協調的なハイパーメディア情報システム向けのアプリケーション プロトコルです。 クライアントは、ご使用の VM のパブリック IP と DNS 名のどちらかを使用してご自分の Web アプリに接続します。 |
    | 443 | HTTPS | ハイパー テキスト転送プロトコル セキュア (HTTPS) は、ハイパー テキスト転送プロトコル (HTTP) を拡張したものです。 コンピューター ネットワーク経由のセキュリティで保護された通信に使用されます。 クライアントは、ご使用の VM のパブリック IP と DNS 名のどちらかを使用してご自分の Web アプリに接続します。 |
    | 22 | SSH | Secure Shell (SSH) は、セキュリティで保護されていないネットワーク経由でネットワーク サービスを安全に使用するための暗号化ネットワーク プロトコルです。 SSH クライアントとのこの接続を使用して、VM を構成し、アプリをデプロイします。 |
    | 3389 | RDP | 省略可能。 リモート デスクトップ プロトコルは、リモート デスクトップ接続を介して、マシンのグラフィック ユーザー インターフェイスを使用できるようにします。   |
    | 5000、8000 | カスタム | ポート 5000、8000 は、開発時に Flask Web フレームワークによって使用されます。 運用サーバーでは、80 と 443 を介してトラフィックをルーティングします。 |

## <a name="install-python"></a>Python のインストール

1. SSH クライアントを使用して VM に接続します。 手順については、「[PuTTy を使用して SSH 経由で接続する](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty)」を参照してください。
2. ご自分の VM 上の Bash プロンプトで、次のコマンドを入力します。

    ```bash  
    sudo apt-get -y install python3 python3-venv python3-dev
    ```

3. インストールを検証します。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
        python -version
    ```


3. Nginx をインストールします。 [Nginx](https://www.nginx.com/resources/wiki/) は、リバース プロキシ、ロード バランサー、メール プロキシ、および HTTP キャッシュとしても使用できる Web サーバーです。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
       sudo apt-get -y install nginx git
    ```

4. GIT をインストールします。 [Git](https://git-scm.com) は、広域分散型のリビジョン コントロールおよびソース コード管理 (SCM) システムです。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
       sudo apt-get -y install git
    ```

## <a name="deploy-and-run-the-app"></a>アプリのデプロイと実行

1. VM 上でご自分の Git リポジトリを設定します。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
       git clone https://github.com/mattbriggs/flask-hello-world.git
    
       cd flask-hello-world
    ```

2. 仮想環境を作成し、パッケージの依存関係をすべて設定します。  SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    
    export FLASK_APP=application.py
    # export FLASK_DEBUG=1 
    flask run -h 0.0.0.0
    ```

3.  次に、ご自分の新しいサーバーに移動すると、実行中のご自分の Web アプリケーションを確認できます。

    ```HTTP  
       http://yourhostname.cloudapp.net:5000
    ```

## <a name="update-your-server"></a>ご自分のサーバーを更新する

1. SSH セッション内でご自分の VM に接続します。 `CTRL+C` キーを押して、サーバーを停止します。
2. 次のコマンドを入力します。

    ```bash  
    deactivate
    open the git repo
    git pull
    ```

3. 仮想環境をアクティブ化し、アプリを起動します。

    ```bash  
    source venv/bin/activate
    export FLASK_APP=application.py
    flask run -h 0.0.0.0
    ```

## <a name="next-steps"></a>次の手順

- [Azure Stack 向けの開発](azure-stack-dev-start.md)方法の詳細を確認する
- [IaaS としての Azure Stack 向けの一般的なデプロイ](azure-stack-dev-start-deploy-app.md)を確認する
