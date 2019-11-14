---
title: Azure Stack サブスクリプションまたはストレージ アカウントにストレージ エクスプローラーを接続する | Microsoft Docs
description: Azure Stack サブスクリプションにストレージ エクスプローラーを接続する方法について学習します
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 11/11/2019
ms.openlocfilehash: 2fa9998a584bd76c8f9c875d3a2ac082c9ca812b
ms.sourcegitcommit: 102ef41963b5d2d91336c84f2d6af3fdf2ce11c4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73955626"
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription-or-a-storage-account"></a>Azure Stack サブスクリプションまたはストレージ アカウントにストレージ エクスプローラーを接続する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

この記事では、[Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) を使用して Azure Stack サブスクリプションとストレージ アカウントに接続する方法について説明します。 Storage Explorer は、Windows、macOS、および Linux で Azure Stack ストレージのデータを簡単に操作できるようにするスタンドアロン アプリです。

> [!NOTE]  
> Azure Stack ストレージからデータを移動するために利用できるツールは、いくつかあります。 詳細については、「[Data transfer tools for Azure Stack storage](azure-stack-storage-transfer.md)」 (Azure Stack Storage のデータ転送ツール) を参照してください。

まだインストールされていない場合は、[Storage Explorer をダウンロード](https://www.storageexplorer.com/)してインストールします。

Azure Stack サブスクリプションまたはストレージ アカウントに接続したら、[Azure Storage Explorer の記事](/azure/vs-azure-tools-storage-manage-with-storage-explorer)を使用して Azure Stack データを操作できます。 

## <a name="prepare-for-connecting-to-azure-stack"></a>Azure Stack への接続を準備する

Storage Explorer が Azure Stack サブスクリプションにアクセスするには、Azure Stack への直接アクセス、または VPN 接続が必要です。 Azure Stack への VPN 接続を設定する方法の詳細については、[VPN を使用した Azure Stack への接続](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn)に関するページを参照してください。

> [!Note]  
> ASDK では、VPN 経由で ASDK に接続している場合、VPN 設定プロセス中に作成されたルート証明書 (CA.cer) を使用しないでください。  これは DER でエンコードされた証明書であり、Storage Explorer が Azure Stack サブスクリプションを取得することは許可されません。 以下の手順を使用して、Storage Explorer で使用するための Base-64 でエンコードされた証明書をエクスポートします。

接続されていない統合システムおよび ASDK の場合は、内部エンタープライズ証明機関を使用して、Base-64 形式でルート証明書をエクスポートした後、Azure Storage Explorer にインポートすることをお勧めします。  

### <a name="export-and-then-import-the-azure-stack-certificate"></a>Azure Stack の証明書をエクスポートしインポートする

接続されていない統合システムおよび ASDK の場合は、Azure Stack 証明書をエクスポートしてインポートします。 接続されている統合システムの場合は、証明書はパブリック的に署名されており、この手順は必要ありません。

1. Azure Stack に VPN 接続された、Azure Stack のホスト コンピューターまたはローカル コンピューターで `mmc.exe` を開きます。 

2. **[ファイル]** の **[スナップインの追加と削除]** を選択します。 [利用できるスナップイン] で、 **[証明書]** を選択します。 

3. **[コンピューター アカウント]** を選択し、 **[次へ]** を選択します。 **[ローカル コンピューター]** を選択し、 **[完了]** を選択します。

4.  **Console Root\Certificated (Local Computer)\Trusted Root Certification Authorities\Certificates** の直下で **AzureStackSelfSignedRootCert** を探します。

    ![Azure Stack のルート証明書を mmc.exe で読み込む](./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png)

5. 証明書を右クリックし、 **[すべてのタスク]**  >  **[エクスポート]** の順に選択したら、表示される手順に従って **Base-64 encoded X.509 (.CER)** で証明書をエクスポートします。

    エクスポートした証明書は、次の手順で使用します。

6. Storage Explorer を開始します。 **[Azure Storage へ接続]** ダイアログ ボックスが表示されたらそれをキャンセルします。

7. **[編集]** メニューで **[SSL 証明書]** を選択し、次に **[証明書のインポート]** を選択します。 ファイル ピッカー ダイアログ ボックスを使用して、前の手順でエクスポートした証明書を検索し、開きます。

    証明書のインポート後は、Storage Explorer の再起動を求めるメッセージが表示されます。

    ![ストレージ エクスプローラーに証明書をインポートする](./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png)

8. Storage Explorer が再起動したら、 **[編集]** メニューを選択し、 **[Target Azure Stack APIs]\(Azure Stack API を対象にする\)** が選択されているかどうかを確認します。 **[Target Azure Stack]\(Azure Stack を対象にする\)** が選択されていない場合は、選択してから Storage Explorer を再起動して、この変更を反映させます。 この構成は、Azure Stack 環境との互換性を確保するために必要です。

    ![[Target Azure Stack (Azure Stack を対象にする)] が選択されていることを確認する](./media/azure-stack-storage-connect-se/target-azure-stack.png)

## <a name="connect-to-an-azure-stack-subscription-with-azure-ad"></a>Azure AD を使用して Azure Stack サブスクリプションに接続する

Azure Active Directory (Azure AD) アカウントに属している Azure Stack サブスクリプションに Storage Explorer を接続するには、次の手順を使用します。

1. Storage Explorer の左側のウィンドウで、 **[アカウントの管理]** を選択します。  
    サインインしている Microsoft サブスクリプションがすべて表示されます。

2. Azure Stack サブスクリプションに接続するには、 **[アカウントの追加]** を選択します。

    ![Azure Stack アカウントを追加する](./media/azure-stack-storage-connect-se/add-azure-stack-account.png)

3. [Azure Storage へ接続] ダイアログ ボックスの **[Azure 環境]** で、 **[Azure]** 、 **[Azure 中国 21Vianet]** 、 **[Azure ドイツ]** 、 **[Azure 米国政府機関]** 、または **[新しい環境を追加します]** を選択します。 これは、使用されている Azure Stack アカウントによって決まります。 **[サインイン]** を選択し、少なくとも 1 つのアクティブな Azure Stack サブスクリプションに関連付けられた Azure Stack アカウントを使用してサインインします。

    ![Azure Storage へ接続](./media/azure-stack-storage-connect-se/azure-stack-connect-to-storage.png)

4. Azure Stack アカウントでのサインインに成功すると、左側のウィンドウに、そのアカウントに関連付けられている Azure Stack サブスクリプションが表示されます。 操作する Azure Stack サブスクリプションを選択してから、 **[適用]** を選択します ( **[すべてのサブスクリプション]** チェック ボックスをオンまたはオフにすることで、一覧の Azure Stack サブスクリプションがすべて選択された状態と、1 つも選択されていない状態を切り替えることができます)。

    ![カスタム クラウド環境のダイアログ ボックスに情報を入力した後、Azure Stack サブスクリプションを選択する](./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png)

    左側のウィンドウに、選択した Azure Stack サブスクリプションに関連付けられているストレージ アカウントが表示されます。

    ![Azure Stack サブスクリプション アカウントを含むストレージ アカウントの一覧](./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png)

## <a name="connect-to-an-azure-stack-subscription-with-ad-fs-account"></a>AD FS アカウントを使用して Azure Stack サブスクリプションに接続する

> [!Note]  
> Azure のフェデレーション サービス (AD FS) のサインイン エクスペリエンスでは、Azure Stack 1804 以降の更新プログラムが適用されたストレージ エクスプローラー 1.2.0 以降のバージョンがサポートされます。
AD FS アカウントに属している Azure Stack サブスクリプションに Storage Explorer を接続するには、次の手順を使用します。

1. **[アカウントの管理]** を選択します。 エクスプローラーには、サインインした Microsoft サブスクリプションが一覧表示されます。
2. **[アカウントの追加]** を選択して、Azure Stack サブスクリプションに接続します。

    ![アカウントの追加 - Storage Explorer](media/azure-stack-storage-connect-se/add-an-account.png)

3. **[次へ]** を選択します。 [Azure Storage へ接続] ダイアログ ボックスの **[Azure 環境]** で **[Use Custom Environment] (カスタム環境の使用)** を選択し、 **[次へ]** をクリックします。

    ![Azure Storage へ接続](media/azure-stack-storage-connect-se/connect-to-azure-storage.png)

4. Azure Stack のカスタム環境に必要な情報を入力します。 

    | フィールド | メモ |
    | ---   | ---   |
    | 環境名 | このフィールドはユーザーがカスタマイズできます。 |
    | Azure Resource Manager endpoint (Azure Resource Manager エンドポイント) | Azure Stack Development Kit の Azure Resource Manager リソース エンドポイントの例は次のとおりです。<br>オペレーター用: https://adminmanagement.local.azurestack.external <br> ユーザー用: https://management.local.azurestack.external |

    Azure Stack 統合システムで作業していて、管理エンドポイントが不明な場合は、オペレーターに問い合わせてください。

    ![アカウントの追加 - カスタム環境](./media/azure-stack-storage-connect-se/custom-environments.png)

5. **[サインイン]** を選択し、少なくとも 1 つのアクティブな Azure Stack サブスクリプションに関連付けられている Azure Stack アカウントに接続します。



6. 操作する Azure Stack サブスクリプションを選択してから、 **[適用]** を選択します。

    ![アカウント管理](./media/azure-stack-storage-connect-se/account-management.png)

    左側のウィンドウに、選択した Azure Stack サブスクリプションに関連付けられているストレージ アカウントが表示されます。

    ![関連付けられているサブスクリプションの一覧](./media/azure-stack-storage-connect-se/list-of-associated-subscriptions.png)

## <a name="connect-to-an-azure-stack-storage-account"></a>Azure Stack ストレージ アカウントに接続する

ストレージ アカウント名とキーのペアを使用して Azure Stack ストレージ アカウントに接続することもできます。

1. Storage Explorer の左側のウィンドウで、[アカウントの管理] を選択します。 サインインしたすべての Microsoft アカウントが表示されます。

    ![アカウントの追加 - Storage Explorer](./media/azure-stack-storage-connect-se/azure-stack-sub-add-an-account.png)

2. Azure Stack サブスクリプションに接続するには、 **[アカウントの追加]** を選択します。

    ![アカウントの追加 - Azure Storage へ接続](./media/azure-stack-storage-connect-se/azure-stack-use-a-storage-and-key.png)

3. [Azure Storage へ接続] ダイアログ ボックスで、 **[Use a storage account name and key]\(ストレージ アカウント名とキーを使用\)** を選択します。

4. ご使用のアカウント名を **[アカウント名]** に入力し、アカウント キーを **[アカウント キー]** テキスト ボックスに貼り付けます。 次に、 **[ストレージ エンドポイントのドメイン]** で **[その他 (以下に入力)] (Other (enter below))** を選択し、Azure Stack エンドポイントを入力します。

    Azure Stack のエンドポイントには、リージョン名と Azure Stack ドメインの 2 つの部分があります。 Azure Stack Development Kit の既定のエンドポイントは **local.azurestack.external** です。 エンドポイントが不明の場合は、クラウド管理者に問い合わせてください。

    ![名前とキーを使用して接続する](./media/azure-stack-storage-connect-se/azure-stack-attach-name-and-key.png)

5. **[接続]** を選択します。
6. ストレージ アカウントが正常に接続されると、そのストレージ アカウントが、名前の後に "**External, Other (外部、その他)** " を伴って表示されます。

    ![VMWINDISK](./media/azure-stack-storage-connect-se/azure-stack-vmwindisk.png)

## <a name="next-steps"></a>次の手順

* [ストレージ エクスプローラーの概要](/azure/vs-azure-tools-storage-manage-with-storage-explorer)
* [Azure Stack Storage: 違いと考慮事項](azure-stack-acs-differences.md)
* Azure Storage の詳細については、「[Microsoft Azure Storage の概要](/azure/storage/common/storage-introduction)」を参照してください。
