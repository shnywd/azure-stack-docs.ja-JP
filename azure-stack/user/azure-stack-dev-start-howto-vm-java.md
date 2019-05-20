---
title: Azure Stack 内の仮想マシンに Java WAR をデプロイする | Microsoft Docs
description: Azure Stack 内の仮想マシンに Java WAR をデプロイします。
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: e788be6315078fccee020fefe6ad79a20485c382
ms.sourcegitcommit: 41927cb812e6a705d8e414c5f605654da1fc6952
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "64482387"
---
# <a name="how-to-deploy-a-java-web-app-to-a-vm-in-azure-stack"></a>Azure Stack 内の VM に Java Web アプリをデプロイする方法

Azure Stack 内にご自分の Python Web アプリをホストする VM を作成できます。 この記事では、サーバーを設定し、Python Web アプリをホストするようサーバーを構成して、アプリをデプロイする手順を確認します。

Java では、クラスをベースとしたオブジェクト指向のコンカレントな汎用コンピューター プログラミング言語で、実装の依存関係ができるだけ少なくなるよう設計されています。 アプリケーション開発者が "一度作成すれば、どこでも実行できる" ようになっています。これは、コンパイルされた Java コードは、Java をサポートするすべてのプラットフォーム上で実行でき、再コンパイルを必要としないことを意味します。 Java プログラミング言語の詳細および Java の他のリソースについては、[Java.com](https://www.java.com) を参照してください。

この記事では、Azure Stack 内の Linux VM 上に Apache Tomcat サーバーをインストールして構成し、そのサーバーに Java Web アプリケーション リソース (WAR) ファイルを読み込む方法を説明します。 WAR ファイルは、JAR ファイル、JavaServer Pages、Java サーブレット、Java クラス、XML ファイル、タグ ライブラリ、静的 Web ページ (HTML および関連ファイル)、および一緒に Web アプリケーションを構成するその他のリソースのコレクションを配布するために使用されます。

Apache Tomcat (Tomcat サーバーと呼ばれることも多い) は Apache Software Foundation によって開発されたオープンソース Java サーブレット コンテナーです。 Tomcat は、Java サーブレット、JavaServer Pages、Java EL、および WebSocket を含む複数の Java EE 仕様を実装し、Java コードを実行できる "純粋な Java" HTTP Web サーバー環境を提供します。

## <a name="create-a-vm"></a>VM の作成

1. Azure Stack 内でご自分の VM を設定します。 「[Web アプリをホストする Linux VM を Azure Stack にデプロイする](azure-stack-dev-start-howto-deploy-linux.md)」の手順に従ってください。

2. VM ネットワーク ブレードで、次のポートにアクセスできることを確認します。

    | Port | Protocol | 説明 |
    | --- | --- | --- |
    | 80 | HTTP | ハイパー テキスト転送プロトコル (HTTP) は、分散した協調的なハイパーメディア情報システム向けのアプリケーション プロトコルです。 クライアントは、ご使用の VM のパブリック IP と DNS 名のどちらかを使用してご自分の Web アプリに接続します。 |
    | 443 | HTTPS | ハイパー テキスト転送プロトコル セキュア (HTTPS) は、ハイパー テキスト転送プロトコル (HTTP) を拡張したものです。 コンピューター ネットワーク経由のセキュリティで保護された通信に使用されます。 クライアントは、ご使用の VM のパブリック IP と DNS 名のどちらかを使用してご自分の Web アプリに接続します。 |
    | 22 | SSH | Secure Shell (SSH) は、セキュリティで保護されていないネットワーク経由でネットワーク サービスを安全に使用するための暗号化ネットワーク プロトコルです。 SSH クライアントとのこの接続を使用して、VM を構成し、アプリをデプロイします。 |
    | 3389 | RDP | 省略可能。 リモート デスクトップ プロトコルは、リモート デスクトップ接続を介して、マシンのグラフィック ユーザー インターフェイスを使用できるようにします。   |
    | 8080 | カスタム | Apache Tomcat サービスの既定のポートは 8080 です。 運用サーバーでは、80 と 443 を介してトラフィックをルーティングします。 |

## <a name="install-java"></a>Java のインストール

1. SSH クライアントを使用して VM に接続します。 手順については、「[PuTTy を使用して SSH 経由で接続する](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty)」を参照してください。
2. ご自分の VM 上の Bash プロンプトで、次のコマンドを入力します。

    ```bash  
        sudo apt-get install default-jdk
    ```

3. インストールを検証します。 SSH セッション内でご自分の VM に接続したままで、次のコマンドを入力します。

    ```bash  
        java -version
    ```

## <a name="install-and-configure-tomcat"></a>Tomcat をインストールして構成する

1. SSH クライアントを使用して VM に接続します。 手順については、「[PuTTy を使用して SSH 経由で接続する](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty)」を参照してください。

2. Tomcat ユーザーを作成します。
    - 1 つ目に、新しい Tomcat グループを作成します。
        ```bash  
            sudo groupadd tomcat
        ```
     
    - 2 つ目に、新しい Tomcat ユーザーを作成し、ホーム ディレクトリに `/opt/tomcat` (ここに Tomcat をインストールします) を、シェルに `/bin/false` を指定して (そのため、このアカウントにだれもログインできなくなります) このユーザーを tomcat グループのメンバーにします。
        ```bash  
            sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
        ```

3. Tomcat をインストールします。
    - 1 つ目に、Tomcat 8 のダウンロード ページ ([http://tomcat.apache.org/download-80.cgi](http://tomcat.apache.org/download-80.cgi)) から Tomcat 8 の最新バージョンの tar の URL を入手します。
    - 2 つ目に、curl とそのリンクを使用して最新バージョンをダウンロードします。

        ```bash  
            cd /tmp 
            curl -O <URL for the tar for the latest version of Tomcat 8>
        ```

    - 3 つ目に、Tomcat を `/opt/tomcat` ディレクトリにインストールします。 次のコマンドを使用して、ディレクトリを作成し、アーカイブを抽出します。

        ```bash  
            sudo mkdir /opt/tomcat
            sudo tar xzvf apache-tomcat-8*tar.gz -C /opt/tomcat --strip-components=1
            sudo chown -R tomcat webapps/ work/ temp/ logs/
        ```

4. Tomcat のアクセス許可を更新します。

    ```bash  
        sudo chgrp -R tomcat /opt/tomcat
        sudo chmod -R g+r conf
        sudo chmod g+x conf
    ```

5. `systemd` サービス ファイルを作成します。 これにより、Tomcat をサービスとして実行できます。

    - Java がインストールされている場所を Tomcat に認識させる必要があります。 このパスは一般に **JAVA_HOME** と呼ばれます。 次を実行して、その場所を調べます。

        ```bash  
            sudo update-java-alternatives -l
        ```

        次のようなものが生成されます。

        ```Text  
            Output
            java-1.8.0-openjdk-amd64       1081       /usr/lib/jvm/java-1.8.0-openjdk-amd64
        ```

        **JAVA_HOME** 変数の値は、出力からパスを取り出し、`/jre` を追加することによって作成できます。 たとえば、上記の例を使用すると、`/usr/lib/jvm/java-1.8.0-openjdk-amd64/jre` となります。

    - ご自分のサーバーからの値を使用して、systemd サービス ファイルを作成します。

        ```bash  
            sudo nano /etc/systemd/system/tomcat.service
        ```

    - ご自分のサービス ファイルに次の内容を貼り付けます。 必要に応じて **JAVA_HOME** の値を変更して、ご使用のシステム上で見つけた値と一致させます。 CATALINA_OPTS で指定するメモリ割り当ての設定を変更することもできます。

        ```Text  
            [Unit]
            Description=Apache Tomcat Web Application Container
            After=network.target
            
            [Service]
            Type=forking
            
            Environment=JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64/jre
            Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
            Environment=CATALINA_HOME=/opt/tomcat
            Environment=CATALINA_BASE=/opt/tomcat
            Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
            Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'
            
            ExecStart=/opt/tomcat/bin/startup.sh
            ExecStop=/opt/tomcat/bin/shutdown.sh
        
            User=tomcat
            Group=tomcat
            UMask=0007
            RestartSec=10
            Restart=always
            
            [Install]
            WantedBy=multi-user.target
        ```

    - ファイルを保存して閉じます。
    - systemd デーモンを再読み込みして、ご自分のサービス ファイルを認識させます。

        ```bash  
            sudo systemctl daemon-reload
        ```

    - Tomcat サービスを開始します。 

        ```bash  
            sudo systemctl start tomcat
        ```

    - 次を入力して、エラーが発生することなく開始されたことを確認します。

        ```bash  
            ssudo systemctl status tomcat
        ```

6. Tomcat サーバーを確認します。 Tomcat では、一般的な要求の受け入れにポート 8080 を使用します。 次のコマンドを実行して、そのポートへのトラフィックを許可します。

    ```bash  
        sudo ufw allow 8080
    ```

    ご自分の Azure Stack VM 用の**インバウンド ポートの規則**を追加していない場合、ここで追加します。 詳細については、「[VM の作成](#create-a-vm)」を参照してください。

7. ご使用の Azure Stack と同じネットワーク内でブラウザーを開き、ご自分のサーバーを開きます。`yourmachine.local.cloudapp.azurestack.external:8080`

    ![Azure Stack VM 上の Apache Tomcat](media/azure-stack-dev-start-howto-vm-java/apache-tomcat.png)

    ご自分のサーバー上の Apache Tomcat ページが読み込まれます。 次に、Server Status、Manager App、Host Manager にアクセスできるようサーバーを構成します。

8. サービス ファイルを有効にして、ご自分のサーバーを再起動したときに Tomcat が自動的に開始されるようにします。

    ```bash  
        sudo systemctl enable tomcat
    ```

9. Web 管理インターフェイスにアクセスできるよう Tomcat サーバーを構成します。 `tomcat-users.xml` を編集し、ロールとユーザーを定義してサインインできるようにします。 `manager-gui` と `admin-gui` にアクセスするユーザーを定義します。

    ```bash  
        sudo nano /opt/tomcat/conf/tomcat-users.xml
    ```

    - 次のコードを `<tomcat-users>` セクションに追加します。

    ```XML  
        <role rolename="tomcat"/>
        <user username="<username>" password="<password>" roles="tomcat,manager-gui,admin-gui"/>
    ```

    - たとえば、最終的なファイルは次のようになります。

    ```XML  
        <tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
         <role rolename="tomcat"/>
        <user username="tomcatuser" password="changemepassword" roles="tomcat,manager-gui,admin-gui"/>
        </tomcat-users>
    ```

    - ファイルを保存して閉じます。


10. Tomcat は **Manager** と **Host Manager** アプリへのアクセスをサーバーからの接続に制限します。 Azure Stack 内の VM 上に Tomcat をインストールするため、この制限を削除する必要があります。 適切な `context.xml` ファイルを編集して、これらのアプリに対する IP アドレスの制限を変更します。

    - Manager アプリの `context.xml` を更新します。

        ```bash  
            sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
        ```

    - IP アドレスの制限をコメント アウトしてすべての接続を許可するか、Tomcat への接続に使用するマシンの IP アドレスを追加します。

        ```XML  
        <Context antiResourceLocking="false" privileged="true" >
          <!--<Valve className="org.apache.catalina.valves.RemoteAddrValve"
                 allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />-->
        </Context>
        ```

    - ファイルを保存して閉じます。

    - Host Manager アプリの `context.xml` を同じように更新します。

        ```bash  
            sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml
        ```

    - ファイルを保存して閉じます。

11. Tomcat サービスを再起動して、この変更に合わせてサーバーを更新します。

    ```bash  
        sudo systemctl restart tomcat
    ```

12. ご使用の Azure Stack と同じネットワーク内でブラウザーを開き、ご自分のサーバーを開きます。`yourmachine.local.cloudapp.azurestack.external:8080`

    - [Server Status]\(サーバーの状態\) を選択して、Tomcat サーバーの状態を確認し、アクセス権があることを確認します。
    - ご自分の Tomcat の資格情報を使用してサインインします。

![Azure Stack VM 上の Apache Tomcat](media/azure-stack-dev-start-howto-vm-java/apache-tomcat-management-app.png)

## <a name="create-an-app"></a>アプリを作成する

Tomcat に展開する WAR を作成する必要があります。 ご自分の環境を確認したいだけの場合は、Apache TomCat サイト内に War の例があります ([サンプル アプリ](https://tomcat.apache.org/tomcat-6.0-doc/appdev/sample/))。

Azure 内での Java アプリの開発に関するガイダンスについては、「[Azure で Java アプリを構築、デプロイする](https://azure.microsoft.com/develop/java/)」を参照してください。

## <a name="deploy-and-run-the-app"></a>アプリのデプロイと実行

1. SSH クライアントを使用して VM に接続します。 手順については、「[PuTTy を使用して SSH 経由で接続する](azure-stack-dev-start-howto-ssh-public-key.md#connect-via-ssh-with-putty)」を参照してください。
1. Tomcat サービスを停止して、ご自分のアプリ パッケージに合わせてサーバーを更新します。

    ```bash  
        sudo systemctl stop tomcat
    ```

2.  tomcat グループにご使用の FTP ユーザーを追加して、webapps フォルダーに書き込めるようにします。 ご使用の FTP ユーザーとは、Azure Stack 内に VM を作成するときに定義したユーザーです。

    ```bash  
        sudo usermod -a -G tomcat <VM-user>
    ```

3. FileZilla を使用してご自分の VM に接続して、webapps フォルダーをクリアし、新規または更新済みの WAR を読み込みます。 FileZila を使用する手順については、「[FileZilla を使用して SFTP で接続する](azure-stack-dev-start-howto-ssh-public-key.md#connect-with-sftp-with-filezilla)」を参照してください。
    - `TOMCAT_HOME/webapps` をクリアします。
    - ご自分の WAR を ` TOMCAT_HOME/webapps` (例: `/opt/tomcat/webapps/`) に追加します。

4.  Tomcat は自動的に、アプリケーションを展開して配置します。 先ほど作成した DNS 名を使用して確認できます。 例: 

    ```HTTP  
       http://yourmachine.local.cloudapp.azurestack.external:8080/sample

## Next steps

- Learn more about how to [Develop for Azure Stack](azure-stack-dev-start.md)
- Learn about [common deployments for Azure Stack as IaaS](azure-stack-dev-start-deploy-app.md).