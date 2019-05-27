---
title: Azure Stack で仮想マシンに Node.js アプリをデプロイする | Microsoft Docs
description: Azure Stack に Node.js アプリをデプロイします。
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 4fcf76b8f4950fa7ca919d57281c5662b31e96f6
ms.sourcegitcommit: 05a16552569fae342896b6300514c656c1df3c4e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2019
ms.locfileid: "65838296"
---
# <a name="deploy-a-nodejs-web-app-to-a-vm-in-azure-stack"></a>Azure Stack で VM に Node.js Web アプリをデプロイする

Azure Stack で Node.js Web アプリをホストする仮想マシン (VM) を作成することができます。 この記事では、サーバーを設定し、Node.js Web アプリをホストするようにサーバーを構成してから、アプリを Azure Stack にデプロイします。

## <a name="create-a-vm"></a>VM の作成

1. 「[Web アプリをホストする Linux VM を Azure Stack にデプロイする](azure-stack-dev-start-howto-deploy-linux.md)」の手順に従って、Azure Stack で VM を設定します。

2. VM ネットワーク ウィンドウで、次のポートにアクセスできることを確認します。

    | Port | Protocol | 説明 |
    | --- | --- | --- |
    | 80 | HTTP | ハイパーテキスト転送プロトコル (HTTP) は、サーバーからの Web ページの配信に使用されるプロトコルです。 クライアントは、DNS 名または IP アドレスを使用して HTTP 経由で接続されます。 |
    | 443 | HTTPS | ハイパーテキスト転送プロトコル セキュア (HTTPS) は、セキュリティ証明書を要求し、情報の暗号化された転送を許可する、セキュリティで保護されたバージョンの HTTP です。 |
    | 22 | SSH | Secure Shell (SSH) は、セキュリティで保護された通信のための暗号化されたネットワーク プロトコルです。 SSH クライアントとのこの接続を使用して、VM を構成し、アプリをデプロイします。 |
    | 3389 | RDP | 省略可能。 リモート デスクトップ プロトコル (RDP) では、リモート デスクトップ接続を介して、ご利用のマシンでグラフィック ユーザー インターフェイスを使用できるようにします。   |
    | 1337 | カスタム | Node.js で使用されるポート。 運用サーバーでは、80 と 443 を介してトラフィックをルーティングします。 |

## <a name="install-node"></a>Node をインストールする

1. SSH クライアントを使用して VM に接続します。 手順については、[PuTTY を使用する SSH 経由での接続](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-ssh-by-using-putty)に関する記述を参照してください。

1. ご自分の VM 上の bash プロンプトで、次のコマンドを入力します。

    ```bash  
      sudo apt install nodejs-legacy
    ```

2. [NPM をインストール](https://www.npmjs.com/)します。これは、Node.js パッケージ、つまり、モジュール用のパッケージ マネージャーです。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
       go version
    ```

3. [Git をインストール](https://git-scm.com) します。これは、広域分散型のバージョン管理およびソース コード管理 (SCM) システムです。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

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

3. ご自分の新しいサーバーに移動します。 実行中のご自分の Web アプリケーションを確認できます。

    ```HTTP  
       http://yourhostname.cloudapp.net:1337
    ```

## <a name="next-steps"></a>次の手順

- [Azure Stack 向けの開発](azure-stack-dev-start.md)方法について、さらに学習する。
- [IaaS としての Azure Stack 向けの一般的なデプロイ](azure-stack-dev-start-deploy-app.md)を確認する
- Node プログラミング言語の詳細および Node の他のリソースについては、[Nodejs.org](https://nodejs.org) を参照してください。
