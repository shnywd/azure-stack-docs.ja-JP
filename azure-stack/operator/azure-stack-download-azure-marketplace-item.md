---
title: Azure から Marketplace の項目をダウンロードして Azure Stack Hub に発行する | Microsoft Docs
description: Azure から Marketplace の項目をダウンロードして Azure Stack Hub に発行する方法について学習します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/23/2019
ms.author: sethm
ms.reviewer: avishwan
ms.lastreviewed: 12/23/2018
ms.openlocfilehash: 80cf9d192be07f951ee959c7a83419bb16bd2bbb
ms.sourcegitcommit: d62400454b583249ba5074a5fc375ace0999c412
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "76022955"
---
# <a name="download-marketplace-items-to-azure-stack-hub"></a>Azure Stack Hub に Marketplace の項目をダウンロードする 

クラウド オペレーターは、Azure Marketplace から項目をダウンロードし、Azure Stack Hub 環境を使用して、すべてのユーザーがそれらを利用可能にすることができます。 選択できる項目は、Azure Marketplace の選別された一覧にある項目であり、Azure Stack Hub で動作するように、事前にテストされ、サポートされています。 一覧には新しいアイテムが頻繁に追加されるので、定期的に新しいコンテンツを確認してください。

Marketplace 製品をダウンロードする場合は、次の 2 つのシナリオがあります。

- **接続されているシナリオ**:Azure Stack Hub 環境がインターネットに接続されている必要があります。 Azure Stack Hub 管理者ポータルを使用して項目を見つけ、ダウンロードします。
- **接続されないか、部分的に接続されるシナリオ**:Marketplace の項目をダウンロードには、Marketplace シンジケーション ツールを使用してインターネットにアクセスする必要があります。 その後、ダウンロードしたものを、接続されていない Azure Stack Hub のインストールに転送します。 このシナリオでは、PowerShell を使用します。

ダウンロードできる Marketplace 項目の完全な一覧については、[Azure Stack Hub 用の Azure Marketplace 項目](azure-stack-marketplace-azure-items.md)に関するページを参照してください。 Azure Stack Hub Marketplace の最近の追加、削除、更新の一覧については、「[Azure Stack Hub Marketplace の変更](azure-stack-marketplace-changes.md)」の記事を参照してください。

> [!NOTE]
> カタログは、お使いの Azure Stack Hub システムの接続先クラウドによって異なります。 クラウド環境は、Azure Stack Hub の登録に使用する Azure サブスクリプションによって決まります。

## <a name="connected-scenario"></a>接続されているシナリオ

Azure Stack Hub がインターネットに接続されている場合は、管理者ポータルを使用して Marketplace 項目をダウンロードできます。

### <a name="prerequisites"></a>前提条件

Azure Stack Hub デプロイは、インターネット接続を備えていて、Azure に登録されている必要があります。

### <a name="use-the-portal-to-download-marketplace-items"></a>ポータルを使用して Marketplace 項目をダウンロードする

1. Azure Stack Hub 管理者ポータルにサインインします。

2. Marketplace 項目をダウンロードする前に、使用可能なストレージ領域を確認してください。 後で、ダウンロードする項目を選択するときに、ダウンロード サイズと使用可能なストレージ容量を比較することができます。 容量が限られている場合は、 [空き領域を管理する](azure-stack-manage-storage-shares.md#manage-available-space)のためのオプションを検討してください。

   空き領域を確認するには、 **[リージョン管理]** で対象リージョンを選択し、 **[リソース プロバイダー]**  > **[ストレージ]** の順に移動します。

   ![Azure Stack Hub 管理者ポータルでのストレージ スペースの確認](media/azure-stack-download-azure-marketplace-item/storage.png)

3. Azure Stack Hub Marketplace を開き、Azure に接続します。 そのためには、 **[Marketplace の管理]**  サービスを選択し、 **[Marketplace の項目]** を選択して、 **[Azure から追加]** を選択します。

   ![Azure から Marketplace の項目を追加する](media/azure-stack-download-azure-marketplace-item/marketplace.png)

4. 各行項目には、現在使用可能なバージョンも表示されます。 Marketplace 項目の複数のバージョンを使用できる場合、 **[バージョン]** 列には **[複数]** が表示されます。 各項目をクリックして、その説明と、ダウンロード サイズなどの追加情報を表示できます。

   ![Azure から追加する](media/azure-stack-download-azure-marketplace-item/add-from-azure1.png)

5. 項目のバージョンが **[複数]** と表示されている場合は、その項目を選択した後表示されるバージョン セレクターのドロップダウンから特定のバージョンを選択できます。

   ![Azure から追加する](media/azure-stack-download-azure-marketplace-item/add-from-azure3.png)

6. 必要な項目を選択し、 **[ダウンロード]** を選択します。 ダウンロード時間はさまざまで、ネットワーク接続によって異なります。 ダウンロードが完了したら、Azure Stack Hub オペレーターか、ユーザーとして、その新しい Marketplace 項目をデプロイできます。

7. ダウンロードした項目をデプロイするには、 **[+ リソースの作成]** を選択してから、新しい Marketplace 項目がないかカテゴリを検索します。 次に、デプロイ処理を開始する項目を選択します。 処理は、Marketplace 項目ごとに異なります。

## <a name="disconnected-or-a-partially-connected-scenario"></a>接続されていないか、部分的に接続されているシナリオ

Azure Stack Hub のインターネット接続が制限されている場合や接続がない場合は、PowerShell と *Marketplace シンジケーション ツール* を使用して、インターネット接続のあるコンピューターに Marketplace 項目をダウンロードできます。 その後、Azure Stack Hub 環境に項目を転送します。 接続されていない環境では、Azure Stack Hub ポータルを使用して Marketplace 項目をダウンロードすることはできません。

マーケットプレース シンジケーション ツールは、接続されているシナリオでも使用することができます。

このシナリオは次の 2 つに分けられます。

- **パート 1**: Marketplace から項目をダウンロードします。 インターネットにアクセスできるコンピューターで PowerShell を構成し、シンジケーション ツールをダウンロードして、Azure Marketplace から項目をダウンロードします。
- **パート 2**: Azure Stack Hub Marketplace にアップロードして発行します。 ダウンロードしたファイルを Azure Stack Hub 環境に移動し、Azure Stack Hub にインポートして、Azure Stack Hub Marketplace に発行します。

### <a name="prerequisites"></a>前提条件

- 接続された環境 (Azure Stack Hub である必要はありません)。 Azure から製品の一覧をその詳細と共に取得し、すべてをローカルにダウンロードするには、接続が必要です。 これが実行されれば、手順の残りの部分ではインターネット接続は不要です。 これにより、切断された環境で使用するために以前にダウンロードした項目のカタログが作成されます。

- 切断された環境に接続し、必要なすべてのアーティファクトを転送するための リムーバブル メディア。

- 以下の前提条件がある、切断された Azure Stack Hub 環境:

  - お使いの Azure Stack Hub のデプロイが Azure に登録されている必要があります。

  - インターネットに接続できるコンピューターに、 **Azure Stack Hub PowerShell モジュール バージョン 1.2.11** 以降が備わっている必要があります。 まだない場合は、 [Azure Stack Hub 固有の PowerShell モジュールをインストール](azure-stack-powershell-install.md)してください。

  - ダウンロードした Marketplace 項目のインポートを有効にするには、 [Azure Stack Hub オペレーター用の PowerShell 環境](azure-stack-powershell-configure-admin.md) を構成する必要があります。

  -  [Azure Stack Hub ツール](https://github.com/Azure/AzureStack-Tools) の GitHub リポジトリをクローンします。

- パブリックにアクセスできるコンテナー (ストレージ BLOB) がある Azure Stack Hub の [ストレージ アカウント](azure-stack-manage-storage-accounts.md) を持っている必要があります。 コンテナーは、Marketplace 項目のギャラリー ファイルのための一時的なストレージとして使用します。 ストレージ アカウントとコンテナーに慣れていない場合は、Azure ドキュメントの [Azure portal での BLOB の操作](/azure/storage/blobs/storage-quickstart-blobs-portal) に関するページを参照してください。

- マーケットプレース シンジケーション ツールは、最初の手順でダウンロードされます。

- ダウンロードのパフォーマンスを最適化するために [AzCopy](/azure/storage/common/storage-use-azcopy) をインストールできますが、これは必須ではありません。

登録の完了後、[Marketplace management]\(Marketplace 管理\) ブレードに表示される次のメッセージは、切断されたユース ケースには関係ないため無視できます。

![Marketplace Management](media/azure-stack-download-azure-marketplace-item/toolsmsg.png)

### <a name="use-the-marketplace-syndication-tool-to-download-marketplace-items"></a>マーケットプレース シンジケーション ツールを使用して Marketplace 項目をダウンロードする

> [!IMPORTANT]
> 切断されたシナリオで Marketplace 項目をダウンロードするたびに、必ず Marketplace シンジケーション ツールをダウンロードしてください。 このスクリプトは頻繁に変更されるため、各ダウンロードに対して最新バージョンを使用する必要があります。

1. インターネットに接続されているコンピューターで、PowerShell コンソールを管理者として開きます。

2. Azure Stack Hub の登録に使用した Azure アカウントを追加します。 アカウントを追加するには、PowerShell で、パラメーターを指定しないで **Add-AzureRmAccount** を実行します。 Azure アカウントの資格情報の入力を求めるメッセージが表示されます。アカウントの構成によっては、2 要素認証を使用する必要があります。

   > [!NOTE]
   > セッションの有効期限が切れた、パスワードが変更された、または単にアカウントを切り替えたい場合は、**Add-AzureRmAccount** を使用してサインインする前に、コマンドレット  **Remove-AzureRmAccount-Scope Process** を実行します。

3. 複数のサブスクリプションがある場合は、次のコマンドを実行して、登録に使用したものを選択します。

   ```powershell  
   Get-AzureRmSubscription -SubscriptionID 'Your Azure Subscription GUID' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. 次のスクリプトを使用して、マーケットプレース シンジケーション ツールの最新バージョンをダウンロードします。

   ```powershell
   # Download the tools archive.
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/master.zip `
     -OutFile master.zip

   # Expand the downloaded files.
   expand-archive master.zip `
     -DestinationPath `
     -Force

   # Change to the tools directory.
   cd .\AzureStack-Tools-master
   ```

5. 次のコマンドを実行し、シンジケーション モジュールをインポートしてからツールを起動します。

   ```powershell  
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1
   ```

6. VM イメージ、拡張機能、ソリューション テンプレートなどの Marketplace 項目をエクスポートするには、次のコマンドを実行します。 対象のフォルダー パスは、Azure Marketplace からダウンロードしたファイルを保存する場所に置き換えます。

   ```powershell
   Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes" -azCopyDownloadThreads "[optional - AzCopy threads number]" -azureContext $AzureContext
   ```

   リソース プロバイダーまたは PaaS サービスをエクスポートするには、次のコマンドを実行します。

   ```powershell
   Export-AzSOfflineResourceProvider -destination "Destination folder path" -azCopyDownloadThreads "AzCopy threads number" -azureContext $AzureContext
   ```

   `-azCopyDownloadThreads` パラメーターは省略可能です。 これは、低帯域幅のネットワークを使用中で、Premium レベルのダウンロードを利用しようとしている場合にのみ使用してください。 このオプションでは、AzCopy における同時操作の数を指定します。 低帯域幅のネットワークを実行中の場合は、小さい数を指定すれば、リソースの競合によって発生する失敗を回避できます。 詳細については、[Azure のこの記事](/previous-versions/azure/storage/storage-use-azcopy#specify-the-number-of-concurrent-operations-to-start)を参照してください。

   `-azureContext` パラメーターも省略可能です。 Azure コンテキストを指定しない場合、コマンドレットでは既定の Azure コンテキストが使用されます。

7. ツールを実行すると、次の画像のような画面が表示され、使用可能な Azure Marketplace 項目の一覧が示されます。

   ![Marketplace アイテム](media/azure-stack-download-azure-marketplace-item/tool1.png)

8. Marketplace 項目のバージョンを複数使用できる場合、 **[バージョン]**  列に **[複数のバージョン]** が表示されます。 項目のバージョンの表示が **[複数のバージョン]** となっている場合は、その項目を選択してから、表示されるバージョン選択ウィンドウから特定のバージョンを選択できます。

9. ダウンロードする項目を選択し、その **バージョン**を書き留めておきます。  **Ctrl** キーを押しながら複数のイメージを選択できます。 次の手順で項目をインポートするときに、その *バージョン* を参照します。

    また、 **[条件の追加]**  オプションを使用してイメージの一覧をフィルター処理することもできます。

10. Azure Storage ツールがインストールされていない場合は、次のメッセージが表示されます。 これらのツールをインストールするには、 [AzCopy](/azure/storage/common/storage-use-azcopy#download-azcopy)をダウンロードします。

    ![Storage ツール](media/azure-stack-download-azure-marketplace-item/vmnew1.png)

11.  **[OK]** を選択し、法律条項を確認して同意します。

12. ダウンロードにかかる時間は、項目のサイズによって異なります。 ダウンロードが完了したら、その項目をスクリプトで指定したフォルダーで使用できます。 ダウンロードには、VHD ファイル (仮想マシンの場合) または .zip ファイル (仮想マシン拡張機能やリソース プロバイダーの場合) が含まれています。  *.azpkg* 形式 (実際は .zip ファイル) のギャラリー パッケージが含まれていることもあります。

13. ダウンロードに失敗した場合は、次の PowerShell コマンドレットを再実行してやり直すことができます。

   ```powershell
   # for Marketplace items
   Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes"

   # for Resource providers
   Export-AzSOfflineResourceProvider -Destination "Destination folder path in quotes"
   ```

   やり直す前に、ダウンロードが失敗した製品のフォルダーを削除してください。 たとえば、 **D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1**にダウンロードするときにダウンロード スクリプトが失敗した場合は、 **D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1** フォルダーを削除してからコマンドレットを再実行します。

### <a name="import-the-download-and-publish-to-azure-stack-hub-marketplace-using-powershell"></a>PowerShell を使用したダウンロードのインポートと Azure Stack Hub Marketplace への発行

1.  [以前にローカルにダウンロードした](#use-the-marketplace-syndication-tool-to-download-marketplace-items) ファイルを、Azure Stack Hub 環境で使用できるように移動する必要があります。 マーケットプレース シンジケーション ツールを使用してインポート操作を実行する必要があるので、このツールも Azure Stack Hub 環境で利用できるようにする必要があります。

   次の画像は、フォルダー構造の例を示しています。 **D:\downloadfolder** に、ダウンロードしたすべての Marketplace 項目が含まれています。 各サブフォルダーは、製品 ID で名前が指定された Marketplace 項目です (例:  **microsoft.custom-script-linux-arm-2.0.3**)。 各サブ フォルダーの内容は、Marketplace 項目のダウンロードされたコンテンツです。

   ![Marketplace ダウンロード ディレクトリ構造](media/azure-stack-download-azure-marketplace-item/mp1.png)

2.  [この記事](azure-stack-powershell-configure-admin.md) の指示に従って、Azure Stack Hub オペレーターの PowerShell セッションを構成します。

3. 次のスクリプトを実行して、シンジケーション モジュールをインポートした後、マーケットプレース シンジケーション ツールを起動します。

   ```powershell
   $credential = Get-Credential -Message "Enter the Azure Stack Hub operator credential:"
   Import-AzSOfflineMarketplaceItem -origin "marketplace content folder" -AzsCredential $credential
   ```

    `-origin` パラメーターには、ダウンロードしたすべての製品を含む最上位フォルダーを指定します。たとえば、 `"D:\downloadfolder"`です。

    `-AzsCredential` パラメーターは省略可能です。 これは、有効期限が切れている場合にアクセス トークンを更新するために使用されます。  `-AzsCredential` パラメーターが指定されておらず、トークンの有効期限が切れている場合は、オペレーターの資格情報の入力を求めるメッセージが表示されます。

   > [!NOTE]
   > AD FS でサポートされるのは、ユーザー ID を使用した対話型認証のみです。 資格情報オブジェクトが必要な場合は、サービス プリンシパル (SPN) を使用する必要があります。 Azure Stack Hub および ID 管理サービスとしての AD FS を使用したサービス プリンシパルの設定の詳細については、「 [AD FS サービス プリンシパルを管理する](azure-stack-create-service-principals.md#manage-an-ad-fs-service-principal)」を参照してください。

4. スクリプトが正常に完了すると、Azure Stack Hub Marketplace で項目を利用できるようになります。
