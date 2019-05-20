---
title: Azure Stack で SSH 公開キーを使用する方法 | Microsoft Docs
description: SSH 公開キーの使用方法
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/25/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 0f4133052022963e1ed0457dd479a00bcc5bb749
ms.sourcegitcommit: 0d8ccf2a32b08ab9bcbe13d54c7c3dce2379757f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "64490058"
---
# <a name="how-to-use-an-ssh-public-key"></a>SSH 公開キーの使用方法

ご使用の開発用マシンや、ご自分の Web アプリをホストしている Azure Stack 内のサーバー VM からオープン SSH 接続を使用するには、SSH 公開キーと秘密キーの組の作成が必要になる場合があります。 この記事では、ご自分のキーを入手し、そのキーを使用してご利用のサーバーに接続する手順を説明します。 SSH クライアントを使用して Linux サーバー上の Bash プロンプトを取得することや、SFTP クライアントを使用してサーバーとの間でファイルを移動することができます。

## <a name="create-an-ssh-public-key-on-windows"></a>Windows 上で SSH 公開キーを作成する

このセクションでは、ご使用の Azure Stack 上の Linux マシンへのセキュリティで保護された接続を作成するときに使用する SSH 公開キーと秘密キーの組を PuTTY key generator を使用して作成します。 PuTTY は、Windows と Unix プラットフォーム向けの SSH と Telnet の無料の実装で、`xterm` ターミナル エミュレーターが付属しています。

1. [ご使用のマシン用の PuTTY をダウンロードしてインストールします。](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

1. PuTTY key generator を開きます。

1. **[Parameters]\(パラメーター\)** を **[RSA]** に設定します。

1. 生成するキーのビット数を `2048` に設定します。  

    ![PuTTY を使用して SSH 公開キーを生成する](media/azure-stack-dev-start-howto-ssh-public-key/001-putty-key-gen-start.png)

1. **[Generate] \(生成)** を選択します。 **[Key]\(キー\)** 領域の空白領域の上でカーソルを動かしてある程度のランダムさを生成します。

1. **キーのパスフレーズ**を追加し、**[Confirm]\(確認\)** ボックスでそれを確認します。 ご自分のパスフレーズをメモします。
    ![PuTTY を使用して SSH 公開キーを生成する](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-key-gen-result.png)

1. **[Save the public key]\(公開鍵の保存\)** を選択し、ご自分がアクセスできる場所に保存します。

1. **[Save private key]\(秘密鍵の保存\)** を選択し、ご自分がアクセスできる場所に保存します。公開キーと組になっていることを忘れないでください。

ご自分の公開キーは、保存したテキスト ファイル内に格納されます。 それを開くと、次のようなテキストが格納されています。

```text  
---- BEGIN SSH2 PUBLIC KEY ----
Comment: "rsa-key-20190330"
THISISANEXAMPLEDONOTUSE AAAAB3NzaC1yc2EAAAABJQAAAQEAthW2CinpqhXq
9uSa8/lSH7tLelMXnFljSrJIcpxp3MlHlYVbjHHoKfpvQek8DwKdOUcFIEzuStfT
Z8eUI1s5ZXkACudML68qQT8R0cmcFBGNY20K9ZMz/kZkCEbN80DJ+UnWgjdXKLvD
Dwl9aQwNc7W/WCuZtWPazee95PzAShPefGZ87Jp0OCxKaGYZ7UXMrCethwfVumvU
aj+aPsSThXncgVQUhSf/1IoRtnGOiZoktVvt0TIlhxDrHKHU/aZueaFXYqpxDLIs
BvpmONCSR3YnyUtgWV27N6zC7U1OBdmv7TN6M7g01uOYQKI/GQ==
---- END SSH2 PUBLIC KEY ----
```

公開キーを使用する場合は、アプリケーションがキーを要求したときに、テキスト ボックスのコンテキスト全体をコピーし、値として貼り付けます。

<!-- 
## Create an SSH public key on Linux

ToDo: I need to write this section.

-->
## <a name="connect-with-ssh-using-putty"></a>PuTTY を使用して SSH で接続する

PuTTY をインストールした場合は、key generator と SSH クライアントの両方があります。 SSH クライアントの PuTTY を開き、ご自分の接続値と SSH キーを構成します。ご使用の Azure Stack と同じネットワーク上で作業している場合は、ご自分の VM に接続します。

接続する前に、次が必要となります。
- PuTTY
- 認証の種類として SSH 公開キーを利用するご使用の Azure Stack 内の Linux マシンの IP アドレスとユーザー名。
- マシン用にポート 22 を開く必要があります。
- マシンを作成するときに使用した公開 SSH キー。
- ご使用の Azure Stack と同じネットワーク上にある、PuTTY を実行しているご自分のクライアント マシン。

### <a name="connect-via-ssh-with-putty"></a>PuTTY を使用して SSH 経由で接続する

1. PuTTY を開きます。

    ![PuTTY を使用して接続する](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-connect.png)

2. マシンのユーザー名とパブリック IP アドレスを追加します。 たとえば、**[Host Name]\(ホスト名\)** に「`username@192.XXX.XXX.XX`」と入力します。 
3. **[Port]\(ポート\)** が `22` に設定され、**[Connection type]\(接続タイプ\)** が `SSH` に設定されていることを確認します。
4. **[Category]\(カテゴリ\)** ツリー内の **[SSH]** > **[Auth]\(認証\)** を展開します。

    ![SSH 秘密キー](media/azure-stack-dev-start-howto-ssh-public-key/002-putty-set-private-key.png)

5. **[Browse]\(参照\)** を選択し、ご自分の公開キーと秘密キーの組の秘密キー ファイル (filename.ppk) を見つけます。
6. [Category]\(カテゴリ\) ツリー内の [Session]\(セッション\) を選択します。

    ![SSH 公開キーと秘密キー](media/azure-stack-dev-start-howto-ssh-public-key/003-puTTY-save-session.png)

7. **[Saved Sessions]\(保存されたセッション\)** の下にセッションの名前を入力し、**[Save]\(保存\)** を選択します。
8. セッションの名前、**[Load]\(読込\)** の順に選択します。
9. **[Open (開く)]** を選択します。 SSH セッションが開きます。

## <a name="connect-with-sftp-with-filezilla"></a>FileZilla を使用して SFTP で接続する

ご自分の Linux マシンとの間でファイルを移動するために、SFTP をサポートする FTP クライアントとして Filezilla を使用できます。 FileZilla は Windows 10、Linux、および macOS 上で実行されます。 FileZilla クライアントでは、FTP だけでなく、FTP over TLS (FTPS) および SFTP もサポートされます。 これは、GNU General Public License の条件の下で無料で配布されているオープンソース ソフトウェアです。

### <a name="set-your-connection"></a>ご自分の接続を設定する

1. [FileZilla をダウンロードしてインストールします](https://filezilla-project.org/download.php)。
1. FileZilla を開きます。
1. **[File]\(ファイル\)** > **[Site Manager]\(サイト マネージャー\)** の順に選択します。

    ![SSH 公開キーと秘密キー](media/azure-stack-dev-start-howto-ssh-public-key/005-filezilla-file-manager.png)

1. **[Protocol]\(プロトコル\)** で **[SFTP - SSH File Transfer Protocol]\(SFTP - SSH ファイル転送プロトコル\)** を選択します。
1. **[Host]\(ホスト\)** ボックスにご自分のマシンのパブリック IP アドレスを追加します。
1. **[sign in Type]\(サインインの種類\)** で **[Normal]\(通常\)** を選択します。
1. ご自分のユーザー名とパスワードを追加します。
1. **[OK]** を選択します。
1. **[Edit]\(編集\)** > **[Settings]\(設定\)** の順に選択します。

    ![SSH 公開キーと秘密キー](media/azure-stack-dev-start-howto-ssh-public-key/006-filezilla-add-private-key.png)

1. **[Select page]\(ページを選択\)** ツリーの **[Connection]\(接続\)** を展開します。 **[SFTP]** を選択します。
1. **[Add key file]\(鍵ファイルを追加\)** を選択し、ご自分の秘密キー ファイル (filename.ppk など) を追加します。
1. **[OK]** を選択します。

### <a name="open-your-connection"></a>ご自分の接続を開く

1. FileZilla を開きます。
1. **[File]\(ファイル\)** > **[Site Manager]\(サイト マネージャー\)** の順に選択します。
1. ご自分のサイトの名前を選択し、**[Connect]\(接続\)** を選択します。

## <a name="next-steps"></a>次の手順

[Azure Stack 向けの開発](azure-stack-dev-start.md)方法の詳細を確認する