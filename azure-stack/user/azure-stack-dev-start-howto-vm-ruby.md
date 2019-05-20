---
title: Azure Stack 内の仮想マシンに Ruby アプリをデプロイする | Microsoft Docs
description: Azure Stack 内の仮想マシンに Ruby アプリをデプロイします。
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 7744d1adcdcb1dde53c6ef887498a9a3978f4513
ms.sourcegitcommit: 41927cb812e6a705d8e414c5f605654da1fc6952
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "64482297"
---
# <a name="how-to-deploy-a-ruby-web-app-to-a-vm-in-azure-stack"></a>Azure Stack 内の VM に Ruby Web アプリをデプロイする方法

Azure Stack 内にご自分の Ruby Web アプリをホストする VM を作成できます。 この記事では、サーバーを設定し、Ruby Web アプリをホストするようサーバーを構成して、ご自分のアプリをデプロイする手順を確認します。

Ruby は、慎重にバランスを取った言語です。 その作成者のまつもとゆきひろ "Matz" は、自分のお気に入りの言語 (Perl、Smalltalk、Eiffel、Ada、Lisp) の一部を組み合わせて、関数型プログラミングと命令型プログラミングのバランスを取った新しい言語を作成しました。 Ruby プログラミング言語の詳細および Python の他のリソースについては、[Ruby-lang.org](https://www.ruby-lang.org) を参照してください。

この記事では、Ruby および Ruby on Rails Web フレームワークを使用します。

## <a name="create-a-vm"></a>VM の作成

1. Azure Stack 内でご自分の VM を設定します。 「[Web アプリをホストする Linux VM を Azure Stack にデプロイする](azure-stack-dev-start-howto-deploy-linux.md)」の手順に従ってください。

2. VM ネットワーク ブレードで、次のポートにアクセスできることを確認します。

    | Port | Protocol | 説明 |
    | --- | --- | --- |
    | 80 | HTTP | ハイパー テキスト転送プロトコル (HTTP) は、分散した協調的なハイパーメディア情報システム向けのアプリケーション プロトコルです。 クライアントは、ご使用の VM のパブリック IP と DNS 名のどちらかを使用してご自分の Web アプリに接続します。 |
    | 443 | HTTPS | ハイパー テキスト転送プロトコル セキュア (HTTPS) は、ハイパー テキスト転送プロトコル (HTTP) を拡張したものです。 コンピューター ネットワーク経由のセキュリティで保護された通信に使用されます。 クライアントは、ご使用の VM のパブリック IP と DNS 名のどちらかを使用してご自分の Web アプリに接続します。 |
    | 22 | SSH | Secure Shell (SSH) は、セキュリティで保護されていないネットワーク経由でネットワーク サービスを安全に使用するための暗号化ネットワーク プロトコルです。 SSH クライアントとのこの接続を使用して、VM を構成し、アプリをデプロイします。 |
    | 3389 | RDP | 省略可能。 リモート デスクトップ プロトコルは、リモート デスクトップ接続を介して、マシンのグラフィック ユーザー インターフェイスを使用できるようにします。   |
    | 3000 | カスタム | ポート 3000 は、開発時に Ruby on Rails Web フレームワークによって使用されます。 運用サーバーでは、80 と 443 を介してトラフィックをルーティングします。 |

## <a name="install-ruby"></a>Ruby のインストール

1. SSH クライアントを使用して VM に接続します。 手順については、「[PuTTy を使用して SSH 経由で接続する](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty)」を参照してください。
1. PPA リポジトリをインストールします。 ご自分の VM 上の Bash プロンプトで、次のコマンドを入力します。

    ```bash  
    sudo apt -y install software-properties-common
    sudo apt-add-repository ppa:brightbox/ruby-ng

    sudo apt update
    ```

2. Ruby および Ruby Rails をご自分の VM にインストールします。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
    sudo apt install ruby
    gem install rails -v 4.2.6
    ```

3. Ruby Rails の依存関係をインストールします。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
    sudo apt-get install make
    sudo apt-get install gcc
    sudo apt-get install sqlite3
    sudo apt-get install nodejs
    sudo gem install sqlite
    sudo gem install bundler
    ```

    > [!Note]  
    > インストール中に、繰り返し `sudo gem install bundler` を実行することが必要な場合があります。 依存関係のインストールを試行して失敗した場合は、エラー ログを確認し、問題を解決してください。

4. インストールを検証します。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
        ruby -v
    ```

3. GIT をインストールします。 [Git](https://git-scm.com) は、広域分散型のリビジョン コントロールおよびソース コード管理 (SCM) システムです。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
       sudo apt-get -y install git
    ```

## <a name="create-and-run-an-app"></a>アプリの作成と実行

1. SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash
        rails new myapp
        cd myapp
        rails server -b 0.0.0.0 -p 3000
    ```

2.  次に、ご自分の新しいサーバーに移動すると、実行中のご自分の Web アプリケーションを確認できます。

    ```HTTP  
       http://yourhostname.cloudapp.net:3000
    ```

## <a name="next-steps"></a>次の手順

- [Azure Stack 向けの開発](azure-stack-dev-start.md)方法の詳細を確認する
- [IaaS としての Azure Stack 向けの一般的なデプロイ](azure-stack-dev-start-deploy-app.md)を確認する