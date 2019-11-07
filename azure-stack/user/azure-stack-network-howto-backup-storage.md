---
title: Azure Stack でストレージ アカウントをバックアップする方法 | Microsoft Docs
description: Azure Stack でストレージ アカウントをバックアップする方法について説明します。
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 10/19/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/19/2019
ms.openlocfilehash: 3f6ed5604bd2d32d9d030ceb4b5f67567362cc34
ms.sourcegitcommit: 4d7611d81da6f2f8ef50adab3c09f9122a75bc9d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73145837"
---
# <a name="back-up-your-storage-accounts-on-azure-stack"></a>Azure Stack でストレージ アカウントをバックアップする

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

この記事では、Azure Stack 上での Azure Storage アカウント内のストレージ アカウントの保護と復旧について説明します。

## <a name="elements-of-the-solution"></a>ソリューションの要素

このセクションでは、ソリューションの全体的な構造と主要部分について説明します。

![Azure Stack Storage のバックアップ](./media/azure-stack-network-howto-backup-storage/azure-stack-storage-backup.png)

### <a name="application-layer"></a>アプリケーション レイヤー

別の Azure Stack スケール ユニット上のストレージ アカウント間でデータをレプリケートするには、複数の [PUT BLOB](https://docs.microsoft.com/rest/api/storageservices/put-blob) または [Put Block](https://docs.microsoft.com/rest/api/storageservices/put-block) 操作を発行してオブジェクトを複数の場所に書き込みます。 または、プライマリ アカウントへの Put 操作が完了した後、アプリケーションから [Copy BLOB](https://docs.microsoft.com/rest/api/storageservices/copy-blob) 操作を発行し、別のスケール ユニットでホストされているストレージ アカウントに BLOB をコピーできます。

### <a name="scheduled-copy-task"></a>スケジュールされたコピー タスク

AzCopy は、ローカル ファイル システム、Azure Cloud Storage、Azure Stack Storage、および s3 からデータをコピーするために利用できる優れたツールです。 現在、AzCopy では、2 つの Azure Stack Storage アカウント間でデータをコピーすることはできません。 ソース Azure Stack Storage アカウントからターゲット Azure Stack Storage アカウントにオブジェクトをコピーするには、中継ローカル ファイルシステムが必要です。

詳細については、「[Azure Stack Storage でのデータ転送ツールの使用](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-transfer?view=azs-1908#azcopy)」の記事の「AzCopy」を参照してください。

### <a name="azure-stack-source"></a>Azure Stack (ソース)

これは、バックアップするストレージ アカウント データのソースです。

ソース ストレージ アカウントの URL と SAS トークンが必要になります。 ストレージ アカウントを使用する手順については、「[Azure Stack ストレージの開発ツールの概要](azure-stack-storage-dev.md)」を参照してください。

### <a name="azure-stack-target"></a>Azure Stack (ターゲット)

これは、バックアップするアカウント データを格納するターゲットです。 ターゲット Azure Stack インスタンスは、ターゲット Azure Stack とは別の場所にある必要があります。 また、ソースはターゲットに接続できる必要があります。

ソース ストレージ アカウントの URL と SAS トークンが必要になります。 ストレージ アカウントを使用する手順については、「[Azure Stack ストレージの開発ツールの概要](azure-stack-storage-dev.md)」を参照してください。

### <a name="intermediary-local-filesystem"></a>中継ローカル ファイルシステム

AzCopy を実行し、ソースからコピーしてターゲットの Azure Stack に書き込むときにデータを格納するための場所が必要です。 これは、ソース Azure Stack 内の中継サーバーです。

Linux または Windows サーバーを中継サーバーとして作成できます。 サーバーには、ソース ストレージ アカウント コンテナー内のすべてのオブジェクトを格納できる十分な領域が必要です。
- Linux サーバーを設定する手順については、「[Azure Stack ポータルを使用して Linux サーバー VM を作成する](azure-stack-quick-linux-portal.md)」を参照してください。  
- Windows Server を設定する手順については、「[Azure Stack ポータルを使用して Windows サーバー VM を作成する](azure-stack-quick-windows-portal.md)」を参照してください。  

Windows Server を設定したら、[Azure Stack PowerShell](https://docs.microsoft.com/azure-stack/operator/azure-stack-powershell-install?toc=https%3A%2F%2Fdocs.microsoft.com%2FFazure-stack%2Fuser%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2FFazure-stack%2Fbreadcrumb%2Ftoc.json) と [Azure Stack ツール](https://docs.microsoft.com/azure-stack/operator/azure-stack-powershell-download?toc=https%3A%2F%2Fdocs.microsoft.com%2FFazure-stack%2Fuser%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2FFazure-stack%2Fbreadcrumb%2Ftoc.json)をインストールする必要があります。

## <a name="set-up-backup-for-storage-accounts"></a>ストレージ アカウントのバックアップを設定する

1. ソースとターゲットのストレージ アカウントの BLOB エンドポイントを取得します。

    ![Azure Stack Storage のバックアップ](./media/azure-stack-network-howto-backup-storage/back-up-step1.png)

2. ソースとターゲットのストレージ アカウントの SAS トークンを作成し、記録します。

    ![Azure Stack Storage のバックアップ](./media/azure-stack-network-howto-backup-storage/back-up-step2.png)

3. 中継サーバーに [AzCopy](https://github.com/Azure/azure-storage-azcopy) をインストールし、Azure Stack Storage アカウントの API バージョンをアカウントに設定します。

    - Windows サーバーの場合:

    ```PowerShell  
    set AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09 PowerShell use: $env:AZCOPY_DEFAULT_SERVICE_API_VERSION="2017-11-09"
    ```

    - Linux (Ubuntu) サーバーの場合:

    ```bash  
    export AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09
    ```

4. 中継サーバー上でスクリプトを作成します。 実際の**ストレージ アカウント**、**SAS キー**、および **ローカル ディレクトリのパス**を使用して、このコマンドを更新します。 スクリプトを実行して、**ソース** ストレージ アカウントからデータを増分コピーします。

    ```
    azcopy sync "https:/<storagaccount>/<container>?<SAS Key>" "C:\\myFolder" --recursive=true --delete-destination=true
    ```

5.  **ストレージ アカウント**、**SAS キー**、および**ローカル ディレクトリのパスを入力します。  これを使用して、**ターゲット** ストレージ アカウントにデータを増分コピーします
    
    ```
    azcopy sync "C:\\myFolder" "https:// <storagaccount>/<container>?<SAS Key>" --recursive=true --delete-destination=true
    ```

6.  Cron または Windows のタスク スケジューラを使用して、ソース Azure Stack Storage アカウントから中継サーバー上のローカル ストレージへのコピーをスケジュールします。 次に、中継サーバーのローカル ストレージからターゲット Azure Stack Storage アカウントにコピーします。

    このソリューションで実現できる RPO は、/MO パラメーター値と、ソース アカウントと中継サーバー間および中継サーバーとターゲット アカウント間のネットワーク帯域幅によって決まります。

    - Linux (Ubuntu) サーバーの場合:

    ```bash  
    schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\\&lt;script name>.bat
    ```

    | パラメーター | Note | 
    | ---- | ---- |
    | /SC | 分単位のスケジュールを使用します。 |
    | /MO | *XX* 分の間隔。 |
    | /TN | タスク名。 |
    | /TR | `script.bat` ファイルへのパス |


    - Windows サーバーの場合:

    Windows タスク スケジュールの使用の詳細については、「[開発者向けのタスク スケジューラ](https://docs.microsoft.com/windows/win32/taskschd/task-scheduler-start-page)」を参照してください。
    

## <a name="use-your-storage-account-in-a-disaster"></a>災害時にストレージ アカウントを使用する

各 Azure Stack Storage アカウントには、`https://krsource.blob.east.asicdc.com/` など、Azure Stack のリージョン名から派生した一意の DNS 名があります。 この DNS 名の書き込みと読み取りを行うアプリケーションでは、災害時にターゲット アカウント (`https://krtarget.blob.west.asicdc.com/` など) を使用する必要がある場合に、ストレージ アカウントの DNS 名の変更に対応する必要があります。

アプリケーションの接続文字列は、オブジェクトの再配置を考慮して、災害が宣言された後に変更することができます。または、ソース ストレージ アカウントとターゲット ストレージ アカウントをフロントエンド処理するロード バランサーの前で CNAME レコードを使用する場合、手動フェールオーバー アルゴリズムを使用してロード バランサーを構成し、管理者がターゲットを宣言できるようにします。

SAS が AAD または AD FS ではなくアプリケーションで使用されている場合、上記の方法は機能しません。ターゲット ストレージ アカウント URL とターゲット ストレージ アカウント用に生成された SAS キーを使用して、アプリケーション接続文字列を更新する必要があります。

## <a name="next-steps"></a>次の手順

「[Azure Stack ストレージの開発ツールの概要](azure-stack-storage-dev.md)」を参照してください