---
title: Azure Stack 更新プログラム パッケージを準備する |Microsoft Docs
description: Azure Stack 更新プログラム パッケージの準備について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2019
ms.author: mabrigg
ms.lastreviewed: 08/15/2019
ms.reviewer: ppacent
ms.openlocfilehash: ab7b764e608ed1fb8008071296d0004f6ef65e7a
ms.sourcegitcommit: 1c45814696e70ba987dd39ce61d93ea4ef5222ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/26/2019
ms.locfileid: "70029473"
---
# <a name="prepare-an-azure-stack-update-package"></a>Azure Stack 更新プログラム パッケージを準備する

*適用対象:Azure Stack 統合システム*

この記事では、Azure Stack 環境を更新するために使用できるように Azure Stack 更新パッケージを準備する概要について説明します。 このプロセスは次のもので構成されます。

- [更新プログラム パッケージのダウンロード](https://docs.microsoft.com/azure-stack/operator/azure-stack-update-prepare-package#download-the-update-package)
- [Azure Stack 管理者ポータルを使用した、Azure Stack 環境への更新プログラム パッケージのインポート](https://docs.microsoft.com/azure-stack/operator/azure-stack-update-prepare-package#import-and-install-updates)

このプロセスは、[Azure Stack 自動更新エンドポイント](https://docs.microsoft.com/azure-stack/operator/azure-stack-update-prepare-package#automatic-download-and-preparation-for-update-packages)にインターネット接続しているシステムで、Azure Stack ソフトウェア更新プログラムと修正プログラムに対して自動的に行われます。

次の表は、更新プログラム パッケージが手動による準備を必要とするタイミングと、それらが自動的に準備されるタイミングを示しています。

| 更新の種類 | [Azure Stack 自動更新エンドポイント](https://docs.microsoft.com/azure-stack/operator/azure-stack-update-prepare-package#automatic-download-and-preparation-for-update-packages)への Azure Stack 環境の接続 | 必要な操作 |
| --- | --- | --- |
| Azure Stack ソフトウェア更新 | 接続中 | 更新が適用されると、更新プログラムが自動的にダウンロードされ、準備されます。 |
| Azure Stack 修正プログラム | 接続中 | 更新が適用されると、更新プログラムが自動的にダウンロードされ、準備されます。 |
| OEM パッケージの更新プログラム | 接続中 | 更新プログラム パッケージを準備する必要があります。 この記事ではその手順を説明します。 |
| Azure Stack ソフトウェア更新 | 切断されているか、または弱い接続 | 更新プログラム パッケージを準備する必要があります。 この記事ではその手順を説明します。 |
| Azure Stack 修正プログラム | 切断されているか、または弱い接続 | 更新プログラム パッケージを準備する必要があります。 この記事ではその手順を説明します。 |
| OEM パッケージの更新プログラム | 切断されているか、または弱い接続 | 更新プログラム パッケージを準備する必要があります。 この記事ではその手順を説明します。 |

## <a name="download-the-update-package"></a>更新プログラム パッケージをダウンロードする
Azure Stack 更新プログラムと修正プログラムの更新プログラム パッケージは、接続されているシステムの [更新] ブレードから入手できます。 OEM パッケージを更新する場合、または切断されたシステムをサポートしている場合、パッケージをダウンロードして、Azure Stack インスタンスにアクセスできる場所にパッケージを移動する必要があります。 また、接続が途切れがちなシステムを実行している場合、パッケージをダウンロードしてから、アクセス可能な場所にアップロードすることが必要になる場合もあります。

パッケージの内容を確認します。 通常、更新プログラム パッケージは、次のファイルで構成されます。

-   **自己解凍形式の \<PackageName>.zip ファイル**。 このファイルには、更新プログラムのペイロードが含まれています。
- **Metadata.xml ファイル**。 このファイルには、更新プログラムに関する重要な情報 (発行元、名前、前提条件、サイズ、サポート パス URL など) が含まれます。

### <a name="automatic-download-and-preparation-for-update-packages"></a>更新プログラム パッケージの自動ダウンロードと準備
Azure Stack ソフトウェア更新プログラムと修正プログラムは、**Azure Stack 自動更新エンドポイント** (https://*.azureedge.net および https://aka.ms/azurestackautomaticupdate ) に接続しているシステムに対して自動的に準備されます。 **Azure Stack 自動更新エンドポイント**への接続の設定の詳細については、[Azure Stack ファイアウォール統合](https://docs.microsoft.com/azure-stack/operator/azure-stack-integrate-endpoints#ports-and-urls-outbound)に関するページに記載されている**修正プログラムと更新**エンドポイントを参照してください

### <a name="where-to-download-azure-stack-update-packages"></a>Azure Stack 更新プログラム パッケージをダウンロードする場所

[フル更新と高速更新](https://docs.microsoft.com/azure-stack/operator/azure-stack-updates#update-package-types)用の Azure Stack 更新プログラムは、セキュリティで保護された Azure エンドポイントでホストされます。 インスタンスが接続されている Azure Stack オペレーターに対しては、[Azure Stack の更新が管理者ポータルに自動的に表示](https://docs.microsoft.com/azure-stack/operator/azure-stack-update-prepare-package#automatic-download-and-preparation-for-update-packages)されます。 インターネットに接続されていないシステム、またはインターネット接続が弱いシステムの場合は、[Azure Stack 更新プログラム ダウンローダー ツール](https://aka.ms/azurestackupdatedownload)を使用して更新プログラム パッケージをダウンロードできます。 Azure Stack ソフトウェアの更新プログラム パッケージには、Azure Stack サービスの更新プログラムのほか、Azure Stack のスケール ユニットのオペレーティング システムの更新プログラムが含まれている場合があります。

### <a name="where-to-download-azure-stack-hotfix-packages"></a>Azure Stack 修正プログラム パッケージをダウンロードする場所

[Azure Stack 修正プログラム](https://docs.microsoft.com/azure-stack/operator/azure-stack-updates#update-package-types)のパッケージは、Azure Stack 更新プログラムと同じセキュリティで保護された Azure エンドポイントでホストされます。 インスタンスが接続されている Azure Stack オペレーターに対しては、[Azure Stack の更新が管理者ポータルに自動的に表示](https://docs.microsoft.com/azure-stack/operator/azure-stack-update-prepare-package#automatic-download-and-preparation-for-update-packages)されます。 各修正プログラムの KB 記事 (たとえば、[Azure Stack 修正プログラム 1.1906.11.52](https://support.microsoft.com/help/4515650)) の埋め込みリンクを使用して、それらをダウンロードできます。 修正プログラムは、お使いの Azure Stack バージョンに対応するリリースノートに記載されています。OEM ハードウェアベンダーによって提供される更新プログラム

### <a name="where-to-download-oem-update-packages"></a>OEM 更新プログラム パッケージをダウンロードする場所
また、OEM ベンダーがドライバーやファームウェアの更新プログラムなど、更新プログラムをリリースします。 これらの更新プログラムは、ハードウェア ベンダーから個別の [OEM パッケージ更新プログラム](https://docs.microsoft.com/azure-stack/operator/azure-stack-updates#update-package-types)として提供されますが、それらは引き続き、Microsoft の更新プログラム パッケージと同じようにインポート、インストール、および管理されます。 ベンダーの連絡先リンクの一覧については、「[Azure Stack に OEM (相手先ブランド供給) 更新プログラムを適用する](https://docs.microsoft.com/azure-stack/operator/azure-stack-update-oem#oem-contact-information)」を参照してください。

## <a name="import-and-install-updates"></a>更新プログラムをインポートしてインストールする

次の手順は、管理者ポータルで更新プログラム パッケージをインポートしてインストールする方法を示しています。

> [!Important]  
> メンテナンス操作についてユーザーに通知し、通常のメンテナンス期間をできるだけ勤務時間外にスケジュールします。 メンテナンス操作は、ユーザーのワークロードとポータル操作の両方に影響を及ぼす可能性があります。

1.  管理者ポータルで、 **[すべてのサービス]** を選択します。 次に、 **[DATA + STORAGE]\(データ + ストレージ\)** カテゴリで、 **[ストレージ アカウント]** を選択します (または、フィルター ボックスで「**ストレージ アカウント**」の入力を開始して選択します)。

    ![Azure Stack 更新プログラム](./media/azure-stack-update-prepare-package/image1.png) 

1.  フィルター ボックスに「**更新**」と入力し、**updateadminaccount** ストレージ アカウントを選択します。

2.  ストレージ アカウントの詳細で、 **[サービス]** の **[BLOB]** を選択します。

    ![Azure Stack 更新プログラム - BLOB](./media/azure-stack-update-prepare-package/image2.png)

1.  **[Blob service]** で、 **[+ コンテナー]** を選択してコンテナーを作成します。 名前 (例: *update-1811*) を入力し、 **[OK]** を選択します。

    ![Azure Stack 更新プログラム - コンテナー](./media/azure-stack-update-prepare-package/image3.png)

1.  コンテナーが作成されたら、コンテナー名をクリックした後、 **[アップロード]** をクリックしてパッケージ ファイルをコンテナーにアップロードします。

    ![Azure Stack 更新プログラム - アップロード](./media/azure-stack-update-prepare-package/image4.png)

1.  **[BLOB のアップロード]** で、フォルダー アイコンをクリックし、更新プログラム パッケージの .zip ファイルを参照し、エクスプローラー ウィンドウで **[開く]** をクリックします。

2.  **[BLOB のアップロード]** で、 **[アップロード]** をクリックします。

    ![Azure Stack 更新プログラム - BLOB のアップロード](./media/azure-stack-update-prepare-package/image5.png)

1.  更新プログラム パッケージの Metadata.xml ファイルおよび追加の .zip ファイルに対して、手順 6. と 7. を繰り返します。 Supplemental Notice.txt ファイルが含まれている場合、そのファイルはインポートしないでください。

2.  終了したら、通知を確認できます (ポータルの右上隅にあるベル アイコン)。 アップロードの完了が通知に示されている必要があります。

3.  ダッシュボードの [更新] ブレードに戻ります。 ブレードに更新プログラムが使用可能であることが示されているはずです。 これは、更新が正常に準備されたことを示します。 新しく追加された更新プログラム パッケージを確認するには、ブレードをクリックします。

4.  更新プログラムをインストールするには、 **[準備完了]** とマークされているパッケージを選択し、パッケージを右クリックして **[今すぐ更新]** を選択するか、上部にある **[今すぐ更新]** アクションをクリックします。

5.  更新プログラム パッケージのインストールをクリックしたら、 **[Update run details]\(更新実行の詳細\)** 領域でその状態を確認できます。 ここから **[Download summary]\(サマリーのダウンロード\)** をクリックして、ログ ファイルをダウンロードすることもできます。 更新実行のログは、試行が終了してから 6 か月間は利用できます。

6.  更新プログラムが完了すると、更新された Azure Stack のバージョンが [更新] ブレードに表示されます。

更新プログラムが Azure Stack にインストールされた後、手動でストレージ アカウントから更新プログラムを削除することができます。 Azure Stack は古い更新プログラム パッケージを定期的にチェックし、ストレージから削除します。 Azure Stack で古いパッケージが削除されるまでに 2 週間ほどかかる場合があります。

## <a name="next-steps"></a>次の手順

[更新プログラムを適用する](azure-stack-apply-updates.md)
