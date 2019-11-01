---
title: Azure Stack を使用した Linux VM の作成 | Microsoft Docs
description: Azure Stack を使用して Linux サーバー VM を作成します。
services: azure-stack
cloud: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: kivenkat
ms.custom: mvc
ms.lastreviewed: 12/03/2018
ms.openlocfilehash: 5c3b8d85f5dca0eeb439ca475d4396848d316366
ms.sourcegitcommit: 0d27456332031ab98ba2277117395ae5ffcbb79f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73047261"
---
# <a name="quickstart-create-a-linux-server-vm-by-using-the-azure-stack-portal"></a>クイック スタート:Azure Stack ポータルを使用して Linux サーバー VM を作成する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure Stack ポータルを使用して、Ubuntu Server 16.04 LTS 仮想マシン (VM) を作成できます。 この記事では、仮想マシンを作成し、使用します。 この記事では、次の方法についても説明します。

* リモート クライアントを使用して VM に接続する。
* NGINX Web サーバーをインストールする。
* リソースをクリーンアップする。

> [!NOTE]  
> この記事の画像は、Azure Stack バージョン 1808 で導入された変更に合わせて更新されています。 バージョン 1808 では、アンマネージド ディスクに加え、"*マネージド ディスク*" の使用のサポートが追加されています。 以前のバージョンを使用している場合、ディスクの選択など、一部のタスクの画像が実際の UI に表示されるものと異なります。  

## <a name="prerequisites"></a>前提条件

* Azure Stack Marketplace 内の Linux イメージ

   Azure Stack Marketplace には、既定では Linux イメージがありません。 必要な Ubuntu Server 16.04 LTS イメージを Azure Stack オペレーターに提供してもらってください。 オペレーターは、「[Azure から Azure Stack に Marketplace の項目をダウンロードする](../operator/azure-stack-download-azure-marketplace-item.md)」の手順を使用できます。

* SSH クライアントへのアクセス

   Azure Stack Development Kit (ASDK) を使用している場合は、Secure Shell (SSH) クライアントにアクセスできない可能性があります。 クライアントが必要な場合は、いくつかのパッケージに SSH クライアントが含まれています。 たとえば、PuTTY には SSH クライアントと SSH キー ジェネレーター (puttygen.exe) が含まれています。 利用できるパッケージの詳細については、[SSH 公開キーの使用方法](azure-stack-dev-start-howto-ssh-public-key.md)に関するページを参照してください。

* このクイック スタートでは、PuTTY を使用して SSH キーを生成し、Linux サーバー VM に接続します。 [PuTTY をダウンロードしてインストールします](https://www.putty.org)。

## <a name="create-an-ssh-key-pair"></a>SSH キー ペアの作成

この記事のすべての手順を完了するには、SSH キーの組が必要です。 既存の SSH キーの組がある場合は、この手順をスキップできます。

SSH キーの組を作成するには:

1. PuTTY のインストール フォルダー (既定の場所は *C:\Program Files\PuTTY*) に移動し、次を実行します。

    `puttygen.exe`

1. **[PuTTY Key Generator]** ウィンドウで、 **[Type of key to generate]\(生成するキーの種類\)** を **[RSA]** に、 **[Number of bits in a generated key]\(生成されるキーのビット数\)** を **[2048]** に設定します。

   ![PuTTY Key Generator の構成](media/azure-stack-quick-linux-portal/Putty01.PNG)

1. **[Generate] \(生成)** を選択します。

1. キーを生成するために、 **[Key]\(キー\)** ボックス内でポインターをランダムに動かします。

1. キーの生成が完了したら、 **[Save public key]\(公開キーを保存する\)** を選択してから、 **[Save private key]\(秘密キーを保存する\)** を選択してキーをファイルに保存します。

   ![PuTTY Key Generator の結果](media/azure-stack-quick-linux-portal/Putty02.PNG)

## <a name="sign-in-to-the-azure-stack-portal"></a>Azure Stack ポータルへのサインイン

Azure Stack ポータルのアドレスは、接続している Azure Stack 製品によって異なります。

* ASDK の場合は、 https://portal.local.azurestack.external に移動します。

* Azure Stack 統合システムの場合は、Azure Stack オペレーターによって提供された URL にアクセスします。

## <a name="create-the-vm"></a>VM の作成

1. Azure Stack ポータルの左上隅にある **[リソースの作成]** を選択します。

1. **[コンピューティング]** 、 **[Ubuntu Server 16.04 LTS]** の順に選択します。
   
   ![Linux サーバーの選択](media/azure-stack-quick-linux-portal/select.png)

1. **作成** を選択します。

1. VM 情報を入力します。 **[認証の種類]** で **[SSH 公開キー]** を選択し、保存した SSH 公開キーを貼り付け、 **[OK]** を選択します。

   > [!NOTE]
   > キーの先頭または末尾の空白を必ず削除してください。

   ![基本パネル - VM の構成](media/azure-stack-quick-linux-portal/linux-01.PNG)

1. VM に **[D1]** を選択します。

   ![サイズ ウィンドウ - VM サイズの選択](media/azure-stack-quick-linux-portal/linux-02.PNG)

1. **[設定]** ページで、既定値を変更します。
   
   Azure Stack バージョン 1808 以降では、**ストレージ**を構成し、"*マネージド ディスク*" の使用を選択できます。 1808 より前のバージョンでは、アンマネージド ディスクのみを使用できます。

   ![マネージド ディスクのストレージを構成する](media/azure-stack-quick-linux-portal/linux-03.PNG)
    
   お使いの構成の準備が整ったら、 **[OK]** を選択して続行します。

1. **[概要]** ページで、 **[OK]** を選択して、VM のデプロイを開始します。  

   ![デプロイ](media/azure-stack-quick-linux-portal/deploy.png)

## <a name="connect-to-the-vm"></a>VM に接続します

1. VM ページで **[接続]** を選択します。 VM に接続するために必要な SSH 接続文字列を見つけることができます。 

1. **[PuTTY Configuration]\(PuTTY 構成\)** ページの **[Category]\(カテゴリ\)** ウィンドウで、下へスクロールして、 **[SSH]** を展開し、 **[Auth]\(認証\)** を選択します。 

   ![VM を接続する](media/azure-stack-quick-linux-portal/putty03.PNG)

1. **[Browse]\(参照\)** を選択し、保存した秘密キー ファイルを選択します。

1. **[Category]\(カテゴリ\)** ウィンドウで、上へスクロールし、 **[Session]\(セッション\)** を選択します。

1. **[Host Name (or IP address)]\(ホスト名 (または IP アドレス)\)** ボックスに、Azure Stack ポータルに表示されている接続文字列を貼り付けます。 この例では、この文字列は *asadmin@192.168.102.34* です。

1. **[Open]\(開く\)** を選択して、VM のセッションを開きます。

   ![Linux セッション](media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-the-nginx-web-server"></a>NGINX Web サーバーのインストール

パッケージ ソースを更新し、VM 上に最新の NGINX パッケージをインストールするために、次の bash コマンドを入力します。

```bash
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

NGINX のインストールが完了したら、SSH セッションを閉じて Azure Stack ポータルの VM の **[概要]** ページを開きます。

## <a name="open-port-80-for-web-traffic"></a>Web トラフィック用にポート 80 を開く

受信トラフィックと送信トラフィックのセキュリティは、ネットワーク セキュリティ グループ (NSG) で確保します。 Azure Stack ポータルから VM が作成されると、SSH 接続用のポート 22 上の受信規則が作成されます。 この VM は Web サーバーをホストするため、ポート 80 上で Web トラフィックを許可するには NSG 規則を作成する必要があります。

1. VM の **[概要]** ページで、**リソース グループ**の名前を選択します。

1. VM の**ネットワーク セキュリティ グループ**を選択します。 NSG は **[種類]** 列を使用して識別できます。

1. 左側のウィンドウの **[設定]** で **[受信セキュリティ規則]** を選択します。

1. **[追加]** を選択します。

1. **[名前]** ボックスに「**http**」と入力します。 

1. **[ポート範囲]** が 80 に設定されていることと、 **[アクション]** が **[許可]** に設定されていることを確認します。

1. **[OK]** を選択します。

## <a name="view-the-welcome-to-nginx-page"></a>[Welcome to nginx]\(nginx へようこそ\) ページを表示する

NGINX がインストールされ、VM 上のポート 80 が開かれたので、その VM のパブリック IP アドレスを使用して Web サーバーにアクセスできます (パブリック IP アドレスは VM の **[概要]** ページに表示されます)。

Web ブラウザーを開いて、*http://\<public IP address>* に移動します。

![NGINX Web サーバーのようこそページ](media/azure-stack-quick-linux-portal/linux-05.PNG)

## <a name="clean-up-resources"></a>リソースのクリーンアップ

不要になったリソースをクリーンアップします。 VM とそのリソースを削除するには、VM ページでリソース グループを選択し、 **[削除]** を選択します。

## <a name="next-steps"></a>次の手順

このクイック スタートでは、Web サーバーがインストールされた基本の Linux サーバー VM をデプロイしました。 Azure Stack VM の詳細については、「[Azure Stack の仮想マシンに関する考慮事項](azure-stack-vm-considerations.md)」に進んでください。
