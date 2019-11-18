---
title: Azure から Marketplace の項目をダウンロードして Azure Stack に発行する | Microsoft Docs
description: Azure から Marketplace の項目をダウンロードして Azure Stack に発行する方法について説明します。
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: sethm
ms.reviewer: ihcherie
ms.lastreviewed: 12/10/2018
ms.openlocfilehash: 095744322937a34dffd680b886fd4b06ca65d7d6
ms.sourcegitcommit: 20d1c0ab3892e9c4c71d5b039457f1e15b1c84c7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2019
ms.locfileid: "73618277"
---
# <a name="download-existing-marketplace-items-from-azure-and-publish-to-azure-stack"></a>Azure から既存の Marketplace の項目をダウンロードして Azure Stack に発行する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

クラウド オペレーターは、Azure Marketplace から項目をダウンロードし、Azure Stack で使用可能にすることができます。 選択できる項目は、Azure Marketplace の精選された一覧にある項目です。これらの項目は、Azure Stack で動作するように、事前にテストされ、サポートされています。 一覧には新しいアイテムが頻繁に追加されるので、定期的に新しいコンテンツを確認してください。

Azure Marketplace に接続するには、次の 2 つのシナリオがあります。

- **接続されているシナリオ** - Azure Stack 環境がインターネットに接続されている必要があります。 Azure Stack ポータルを使用して、項目を検索し、ダウンロードします。
- **接続されていないか、部分的に接続されているシナリオ** - Marketplace 項目をダウンロードするために、マーケットプレース シンジケーション ツールを使用してインターネットにアクセスする必要があります。 その後、ダウンロードしたファイルを、接続されていない Azure Stack インストールに転送します。 このシナリオでは、PowerShell を使用します。

ダウンロードできる Marketplace 項目の完全な一覧については、[Azure Stack 用の Azure Marketplace 項目](azure-stack-marketplace-azure-items.md)に関するページを参照してください。 Azure Stack Marketplace の最近の追加、削除、更新の一覧については、「[Azure Stack Marketplace の変更](azure-stack-marketplace-changes.md)」の記事を参照してください。

## <a name="connected-scenario"></a>接続されているシナリオ

Azure Stack がインターネットに接続している場合は、管理者ポータルを使用して Marketplace 項目をダウンロードすることができます。

### <a name="prerequisites"></a>前提条件

Azure Stack デプロイは、インターネットに接続し、[Azure に登録](azure-stack-registration.md)されている必要があります。

### <a name="use-the-portal-to-download-marketplace-items"></a>ポータルを使用して Marketplace 項目をダウンロードする
  
1. Azure Stack 管理者ポータルにサインインします。

2. Marketplace 項目をダウンロードする前に、使用可能なストレージ領域を確認してください。 後で、ダウンロードする項目を選択するときに、ダウンロード サイズと使用可能なストレージ容量を比較することができます。 容量が限られている場合は、[空き領域の管理](azure-stack-manage-storage-shares.md#manage-available-space)のためのオプションを検討してください。

    空き領域を確認するには、 **[Region Management]\(リージョン管理\)** で対象リージョンを選択し、 **[リソース プロバイダー]**  >  **[ストレージ]** の順に移動します。

    ![Azure Stack 管理者ポータルでのストレージ領域の確認](media/azure-stack-download-azure-marketplace-item/storage.png)

3. Azure Stack Marketplace を開き、Azure に接続します。 そのためには、 **[Marketplace management]\(Marketplace 管理\)** サービスを選択し、 **[Marketplace の項目]** を選択して、 **[Add from Azure]\(Azure から追加\)** を選択します。

    ![Azure から Marketplace の項目を追加する](media/azure-stack-download-azure-marketplace-item/marketplace.png)

4. Azure Marketplace からダウンロードできる項目の一覧がポータルに表示されます。 名前、パブリッシャーおよび/または製品の種類で製品をフィルターできます。 各項目をクリックして、その説明と、ダウンロード サイズなどの追加情報を表示することもできます。

    ![Azure Marketplace の項目のリスト ](media/azure-stack-download-azure-marketplace-item/image03.PNG)

5. 項目を選択し、 **[ダウンロード]** を選択します。 ダウンロードの時間は項目によって異なります。

    ![Azure Marketplace 項目のダウンロード](media/azure-stack-download-azure-marketplace-item/image04.png)

    ダウンロードが完了したら、Azure Stack オペレーターか、ユーザーとして、その新しい Marketplace 項目をデプロイできます。

6. ダウンロードした項目をデプロイするには、 **[+ リソースの作成]** を選択し、カテゴリで新しいマーケットプレース項目を検索します。 次に、デプロイ処理を開始する項目を選択します。 処理は、Marketplace 項目ごとに異なります。

## <a name="disconnected-or-a-partially-connected-scenario"></a>接続されていないか、部分的に接続されているシナリオ

Azure Stack が切断モードである場合は、PowerShell と "*マーケットプレース シンジケーション ツール*" を使用して、インターネットに接続しているマシンに Marketplace 項目をダウンロードします。 その後、Azure Stack 環境に項目を転送します。 接続されていない環境では、Azure Stack ポータルを使用して Marketplace 項目をダウンロードすることはできません。

マーケットプレース シンジケーション ツールは、接続されているシナリオでも使用することができます。

このシナリオは次の 2 つに分けられます。

- **パート 1:** Azure Marketplace からダウンロードする。 インターネットにアクセスできるコンピューターで PowerShell を構成し、シンジケーション ツールをダウンロードして、Azure Marketplace から項目をダウンロードします。  
- **パート 2:** Azure Stack Marketplace にアップロードして発行する。 ダウンロードしたファイルを Azure Stack 環境に移動し、Azure Stack にインポートして、Azure Stack Marketplace に発行します。  

### <a name="prerequisites"></a>前提条件

- 接続された環境 (Azure Stack である必要はありません)。 Azure から製品の一覧をその詳細と共に取得し、すべてをローカルにダウンロードするには、接続が必要です。 これが完了すると、手順の残りの部分はインターネット接続を必要としません。 これにより、切断された環境で使用するために以前にダウンロードした項目のカタログが作成されます。

- 切断された環境に接続し、必要なすべてのアーティファクトを転送するための USB キーまたは外部ドライブ。

- 次の前提条件を持つ、切断された Azure Stack 環境:
  - Azure Stack のデプロイは、[Azure に登録されている](azure-stack-registration.md)必要があります。
  - インターネットに接続できるコンピューターには、**Azure Stack PowerShell モジュール バージョン 1.2.11** 以降が存在する必要があります。 まだ存在しない場合は、[Azure Stack 固有の PowerShell モジュールをインストール](azure-stack-powershell-install.md)してください。
  - ダウンロードした Marketplace 項目のインポートを有効にするには、[Azure Stack オペレーター用の PowerShell 環境](azure-stack-powershell-configure-admin.md)を構成する必要があります。
  - [Azure Stack ツール](https://github.com/Azure/AzureStack-Tools)の GitHub リポジトリを複製します。

- パブリックにアクセスできるコンテナー (ストレージ BLOB) がある Azure Stack の[ストレージ アカウント](azure-stack-manage-storage-accounts.md)を持っている必要があります。 コンテナーは、Marketplace 項目のギャラリー ファイルのための一時的なストレージとして使用します。 ストレージ アカウントとコンテナーに慣れていない場合は、Azure ドキュメントの [Azure portal での BLOB の操作](/azure/storage/blobs/storage-quickstart-blobs-portal)に関するページを参照してください。

- マーケットプレース シンジケーション ツールは、最初の手順でダウンロードされます。

- ダウンロードのパフォーマンスを最適化するために [AzCopy](/azure/storage/common/storage-use-azcopy) をインストールできますが、これは必須ではありません。

登録の完了後、[Marketplace management]\(Marketplace 管理\) ブレードに表示される次のメッセージは、切断されたユース ケースには関係ないため無視できます。

[![未登録メッセージ](media/azure-stack-download-azure-marketplace-item/toolsmsgsm.png "未登録メッセージ")](media/azure-stack-download-azure-marketplace-item/toolsmsg.png#lightbox)

### <a name="use-the-marketplace-syndication-tool-to-download-marketplace-items"></a>マーケットプレース シンジケーション ツールを使用して Marketplace 項目をダウンロードする

> [!IMPORTANT]
> 切断されたシナリオで Marketplace 項目をダウンロードするたびに、必ず Marketplace シンジケーション ツールをダウンロードしてください。 このスクリプトは頻繁に変更されるため、各ダウンロードに対して最新バージョンを使用する必要があります。

1. インターネットに接続されているコンピューターで、PowerShell コンソールを管理者として開きます。

2. Azure Stack の登録に使用した Azure アカウントを追加します。 アカウントを追加するには、PowerShell でパラメーターなしで `Add-AzureRmAccount` を実行します。 Azure アカウントの資格情報の入力を求めるメッセージが表示されます。アカウントの構成によっては、2 要素認証を使用する必要があります。

   [!include[Remove Account](../../includes/remove-account.md)]

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

5. 次のコマンドを実行して、シンジケーション モジュールをインポートし、ツールを起動します。 `Destination folder path` を、Azure Marketplace からダウンロードしたファイルを保存する場所に置き換えます。

   ```powershell  
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes"
   ```

   `Export-AzSOfflineMarketplaceItem` には、クラウド環境を指定する追加の `-cloud` フラグがあることに留意してください。 既定では **azurecloud** です。

6. ツールを実行すると、次の図のような画面が表示され、使用可能な Azure Marketplace 項目の一覧が示されます。

   [![Azure Marketplace アイテムのポップアップ](media/azure-stack-download-azure-marketplace-item/image05.png "Azure Marketplace の項目")](media/azure-stack-download-azure-marketplace-item/image05.png#lightbox)

7. Azure Storage ツールがインストールされていない場合は、次のメッセージが表示されます。 これらのツールをインストールするには、[AzCopy ](/azure/storage/common/storage-use-azcopy#download-azcopy) をダウンロードしてください。

   ![Storage ツール](media/azure-stack-download-azure-marketplace-item/vmnew1.png)

8. ダウンロードする項目を選択し、**バージョン**を書き留めておきます (**Ctrl** キーを押しながら、複数のイメージを選択することができます)。 次の手順で項目をインポートするときに、"*バージョン*" を参照します。

   また、 **[Add criteria]\(条件の追加\)** オプションを使用して、イメージの一覧をフィルター処理することもできます。

9. **[OK]** を選択し、法律条項を確認して同意します。

10. ダウンロードにかかる時間は、項目のサイズによって異なります。 ダウンロードが完了したら、その項目をスクリプトで指定したフォルダーで使用できます。 ダウンロードには、VHD ファイル (仮想マシンの場合)、または .zip ファイル (仮想マシン拡張機能の場合) が含まれています。 また、 *.azpkg* 形式 (実体は .zip ファイル) のギャラリー パッケージが含まれていることもあります。

11. ダウンロードに失敗した場合は、次の PowerShell コマンドレットを再実行してやり直すことができます。

    ```powershell
    Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes"
    ```

    やり直す前に、ダウンロードが失敗した製品のフォルダーを削除してください。 たとえば、ダウンロード スクリプトを `D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1` にダウンロードするときに失敗した場合は、`D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1` フォルダーを削除した後、コマンドレットを再実行します。

### <a name="import-the-download-and-publish-to-azure-stack-marketplace-using-powershell"></a>PowerShell を使用したダウンロードのインポートと Azure Stack Marketplace への発行

1. ローカルに[ダウンロード済みの](#use-the-marketplace-syndication-tool-to-download-marketplace-items)ファイルを移動して、お使いの Azure Stack 環境で利用できるようにする必要があります。 マーケットプレース シンジケーション ツールを使用してインポート操作を実行する必要があるので、このツールも Azure Stack 環境で利用できるようにする必要があります。

   次の図に、フォルダー構造の例を示します。 ダウンロードしたマーケットプレースのすべての項目は `D:\downloadfolder` に含まれます。 各サブフォルダーは、製品 ID で指定された Marketplace 項目です (例: `microsoft.custom-script-linux-arm-2.0.3`)。 各サブ フォルダーの内容は、Marketplace 項目のダウンロードされたコンテンツです。

   [![Marketplace ダウンロード ディレクトリ構造](media/azure-stack-download-azure-marketplace-item/mp1sm.png "Marketplace ダウンロード ディレクトリ構造")](media/azure-stack-download-azure-marketplace-item/mp1.png#lightbox)

2. [こちらの記事](azure-stack-powershell-configure-admin.md)の指示に従って、Azure Stack Operator PowerShell セッションを構成します。

3. 次のスクリプトを実行して、シンジケーション モジュールをインポートした後、マーケットプレース シンジケーション ツールを起動します。

   ```powershell
   $credential = Get-Credential -Message "Enter the azure stack operator credential:"
   Import-AzSOfflineMarketplaceItem -origin "marketplace content folder" -AzsCredential $credential
   ```

   `-origin` パラメーターには、ダウンロードしたすべての製品を含む最上位フォルダーを指定します。たとえば、`"D:\downloadfolder"` です。

   `-AzsCredential` パラメーターは省略可能です。 これは、有効期限が切れている場合にアクセス トークンを更新するために使用されます。 `-AzsCredential` パラメーターが指定されていないときにトークンの有効期限が切れている場合は、オペレーターの資格情報の入力を求めるメッセージが表示されます。

    > [!NOTE]  
    > AD FS でサポートされるのは、ユーザー ID を使用した対話型認証のみです。 資格情報オブジェクトが必要な場合は、サービス プリンシパル (SPN) を使用する必要があります。 Azure Stack と AD FS で ID 管理サービスとしてサービス プリンシパルを設定する方法の詳細については、[AD FS のサービス プリンシパルの管理](azure-stack-create-service-principals.md#manage-an-ad-fs-service-principal)に関する記事を参照してください。

4. スクリプトが正常に完了すると、Azure Stack Marketplace で項目を利用できるようになります。

## <a name="next-steps"></a>次の手順

- [カスタム仮想マシン イメージを追加する](azure-stack-add-vm-image.md)
- [カスタム Marketplace 項目の作成と発行](azure-stack-create-and-publish-marketplace-item.md)
