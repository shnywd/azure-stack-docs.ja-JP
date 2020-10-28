---
title: Azure Stack - Modular Data Center 拡張ストレージのファームウェアの更新
description: この記事では、Modular Data Center BLOB ストレージの拡張ストレージでファームウェアの更新プログラムを実行する方法について説明します。
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/14/2020
ms.author: justinha
ms.reviewer: karlt
ms.lastreviewed: 10/14/2020
ms.openlocfilehash: 23430fd05254b1509677bf2d7ddedab75c7c2355
ms.sourcegitcommit: e4e2cc6a68f02c3e856f58ca5ee51b3313c7ff8f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183459"
---
# <a name="updating-onefs-and-firmware-for-the-datacenter-extended-storage"></a>データセンター拡張ストレージの OneFS とファームウェアの更新

この記事では、OneFS とデータセンター拡張ストレージ ハードウェアのファームウェアを更新するためのガイダンスを提供します。 

拡張ストレージ ハードウェアの統合と展開の最後の手順は、ハードウェアが正常であり、構成の準備ができていることを検証する手順です。 手順については、[拡張ストレージの正常性の確認](extended-storage-health-checks.md)に関するページを参照してください。

## <a name="update-onefs"></a>OneFS を更新する

OneFS を更新して、ファームウェアを更新する前に最新のオペレーティング システムのパッチが適用されるようにします。 

1. このパッチをインストールできる十分な空き容量がクラスターにあることを確認します。
1. クラスター内の任意のノードで SSH 接続を開き、ルート アカウントを使用してログインします。

   >[!NOTE]
   >パッチをコンプライアンス モード クラスターにインストールする場合は、compadmin アカウントを使用してログインします。

1. 8\.2.2_GA-RUP_2020-07_PSP-40.tgz ファイルをクラスター上の /ifs/data/Isilon_Support ディレクトリにコピーします。
1. 次のコマンドを実行して、/ifs/data/Isilon_Support ディレクトリに移動します。 

   ```
   /ifs/data/Isilon_Support
   pwd
   ```

1. パッチ ファイルを抽出するために、次のコマンドを実行します。

   ```
   -zxvf 8.2.2_GA-RUP_2020-07_PSP-40.tgz
   ```

1. このパッチをインストールするために、次のコマンドを実行します。
   >[!WARNING]
   >次のコマンドを実行すると、クラスター内のすべてのノードが同時に再起動されます。 パッチをインストールしてから、各ノードを連続して再起動するには、`-simultaneous` パラメーターを省略します。 詳細については、README のインストールと削除の影響に関するセクションを参照してください。

   ```
   isi upgrade patches install 8.2.2_GA-RUP_2020-07_PSP-40.pkg --simultaneous
   ```

1. このパッチがインストールされていることを確認するには、次のコマンドを実行します。

   ```
   isi upgrade patches list
   ```

1. インストールされているパッケージの一覧に 8.2.2_GA-RUP_2020-07_PSP-40 があり、状態が "Installed"(インストール済み) に設定されていることを確認します。

## <a name="update-firmware-on-nodes"></a>ノード上のファームウェアを更新する

   1. 現在のドライブのファームウェアと Drive Support [パッケージのリリース](https://aka.ms/currentextstoragerelease)を確認します。
   1. ローカル コンピューターにファイルをダウンロードします。  
   1. ドライブとノードのファームウェア パッケージを Isilon ファイル共有にアップロードし、ファイルを次のそれぞれのディレクトリに配置します。
    
        * Drive_Support_*.tgz: /ifs/data/Isilon_Support/dsp/
        * IsiFw_Package_*.tar: /ifs/data 

## <a name="verify-disk-drive-firmware"></a>ディスク ドライブのファームウェアを確認する

最初に Drive Support の更新と確認を実行します。 これにより、ディスク ドライブのファームウェアが、提供されたパッケージを使用した最新の状態かどうかが確認されます。 そうでない場合は、パッケージを使用して必要な更新を実行できます。

   1. アクティブな ssh セッションから、以下を実行します。

       ```
       cd /ifs/data/Isilon_Support/dsp/
       ```

   1. ls コマンドを実行して、Drive_Support_*.tgz がこのディレクトリ内にあることを確認します。
   1. 次のコマンドを実行して、.tgz ファイルを展開します。 ファイル名が完全であることを確認し、* をパッケージの実際のバージョン番号に置き換えます。
       ```
       tar -zxvf Drive_Support_v1.*.tgz
       ```
       たとえば、パッケージのバージョンが Drive_Support_v1.33.tgz の場合は、次のコマンドを実行します。
       ```
       tar -zxvf Drive_Support_v1.33.tgz
       ```
   1. これで次の 3 つのファイルに展開されます。
    
        * Drive_Support_v1.33.tar
        * Drive_Support_v1.33.tar.md5
        * Drive_Support_v1.33.tar.sha256
   1. 次のコマンドを実行して、Drive Support ソフトウェアをインストールまたは確認します。
       ```
       isi_dsp_install Drive_Support_v1.*.tar
       ```
       たとえば、パッケージのバージョンが Drive_Support_v1.33.tgz の場合は、次のコマンドを実行します。
       ```
       isi_dsp_install Drive_Support_v1.33.tar
       ```
   1. このコマンドによって、チェックが実行され、Drive Support パッケージのインストールが要求されます。 パッケージが必要な場合はインストールされます。それ以外の場合は、インストール済みなので Drive Support ソフトウェアの以降の更新プログラムはスキップされることが示されます。 v1.33 の出力例を次に示します。
    
        ```
        Running local CHECKS for patch Drive_Support_v1.33
        INSTALL operation has been requested for patch Drive_Support_v1.33
        Installed Drive_Support_v1.33
        DELETE operation has been requested for patch
        Drive_Support_v1.33
        Removed Drive_Support_v1.33 package files
        DSP Install Succeeded, 170.605088949 sec elapsed
        ```
        
   1. 最終的な確認は、次のコマンドを実行することで実行できます。 "Desired" (必要) 列の "-" 以外は、ドライブを更新する必要があることを示します。
       ```
       isi devices drive firmware list --node-lnn all
       ```
   1. 更新が必要なドライブがある場合は、次のコマンドを使用してディスク ファームウェアの更新を実行する必要があります。
       ```
       isi devices drive firmware update start all --node-lnn all
       ```
   1. このプロセスは、次のコマンドを使用して監視できます。
       ```
       isi devices drive firmware update list
       ```
   1. 完了すると、次のコマンドを使用してドライブの正常性を確認できます。
       ```
       isi devices drive list --node-lnn all
       ```
## <a name="install-node-firmware"></a>ノードのファームウェアをインストールする

Drive Support の更新プログラムを完了し、確認したら、IsiFw_Package_*.tar を利用してノードのファームウェアをインストールできます。

   1. アクティブな ssh セッションから、以下を実行します。
        ```
        cd /ifs/data/
        ```
   1. ls コマンドを実行して、IsiFw_Package_*.tar がこのディレクトリ内にあることを確認します。
   1. 次のコマンドを実行して、ノード全体にファームウェアをステージングし、インストールします。 ファイル名が完全であることを確認し、* をパッケージの実際のバージョン番号に置き換えます。
       ```
       isi upgrade patches install IsiFw_Package_v*.tar --rolling=false
       ```
       たとえば、バージョン番号が v10.3.3 の場合は、次のコマンドを実行します。
       ```
       isi upgrade patches install IsiFw_Package_v10.3.3.tar --rolling=false
       ```
   1. 実行が完了すると、確認として次のメッセージが送信されます。
       ```
        Requested install of patch IsiFw_Package_v10.3.3.
       ```
   1. 次のコマンドを使用して、ステージとインストールの状態を確認します。
       ```
       isi upgrade patches list
       ```
   1. プロセスが完了すると、"Status" (状態) 列が更新され、次の状態が表示されます。次の出力例に示すように、"Pending" (保留中)、"Unknown" (不明)、"Installing" (インストール中)、"Installed" (インストール済み) です。
        
       ![ファームウェア更新の状態](media/extended-storage-firmware-updates/firmware-update-status.png)
        
   1. IsiFw_Package*.tar のステージングとインストールが完了したら、次のコマンドを使用して、ノード全体で更新されたファームウェアのデプロイを開始できます。
        ```
        isi upgrade cluster firmware start --no-verify
        ```
   1. これにより、次の確認が求められます。「yes」と入力して続行します。
       ```
       You are about to start a Rolling Firmware UPGRADE, are you sure? (yes/[no]):
       ```
        
       > [!NOTE]
       > ノードのファームウェア更新には数時間かかる場合があり、このプロセス中はノードがオフラインになります。 これらすべてのコマンドの実行はノード 1 から実行され、これが更新される最初のノードであるため、接続はドロップされます。 OneFS ポータルについても同じことが言えます。5 から 10 分待ってから再接続して状態を確認してください (必要に応じて、更新されていない別の管理インターフェイスを介した接続も可能です)。
   1. アクティブな管理インターフェイスまたは接続から、次のコマンドを使用して、このファームウェアの更新プログラムの進行時の状態を追跡できます。
       ```
       isi upgrade cluster nodes firmware progress list
       ```
       または、OneFS ポータルを使用して、[Cluster Management]\(クラスター管理\) - [Patches and firmware]\(パッチとファームウェア\) で次の操作を行います。
        
       ![クラスターの管理](media/extended-storage-firmware-updates/cluster-management.png)
        
       完了すると、状態は **"Unknown" (不明)** から **"Installed" (インストール済み)** に変わります。

## <a name="remove-firmware-packages"></a>ファームウェア パッケージを削除する

次のコマンドを使用して、ドライブとノードに完全にインストールされたファームウェア パッケージを削除します。 ファイル名が完全であることを確認し、v* を実際のパッケージ バージョンに置き換えます。
   ```
   isi upgrade patches uninstall IsiFw_Package_ v*.tar --rolling=false
   ```
   たとえば、パッケージのバージョンが IsiFw_Package_ v10.3.1.tar の場合、次のコマンドを実行します。
   ```
   isi upgrade patches uninstall IsiFw_Package_ v10.3.1.tar --rolling=false
   ```
アクティブな ssh セッションから、以下を実行します。
   ```
   cd /ifs/data/
   rm -f Drive_Support_v*.tgz
   rm -f Drive_Support_v*.tar
   rm -f Drive_Support_v*.tar.md5
   rm -f Drive_Support_v*.tar.sha256
   ```
   次に例を示します。
   ```
   cd /ifs/data/
   rm -f Drive_Support_v10.3.1.tgz
   rm -f Drive_Support_v10.3.1.tar
   rm -f Drive_Support_v10.3.1.tar.md5
   rm -f Drive_Support_v10.3.1.tar.sha256
   ```


        
<!-- ## Contact Dell EMC for support -->

問題が発生した場合は、Microsoft サポートにお問い合わせください。

## <a name="next-steps"></a>次の手順

- [拡張ストレージの正常性チェック](extended-storage-health-checks.md)
