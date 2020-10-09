---
title: Azure Stack Hub ポータルを使用して Windows VM を作成する
description: Azure Stack Hub ポータルを使用して Windows Server 2016 仮想マシン (VM) を作成する方法について説明します。
author: mattbriggs
ms.topic: quickstart
ms.date: 10/06/2020
ms.author: mabrigg
ms.reviewer: kivenkat
ms.lastreviewed: 10/06/2020
ms.openlocfilehash: 77c7e3484186a0bea1f6220330338128aad72fad
ms.sourcegitcommit: 2d2ae0b6db2e4f43f8496b184f30cddbb08b2cbd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2020
ms.locfileid: "91815420"
---
# <a name="quickstart-create-a-windows-server-vm-with-the-azure-stack-hub-portal"></a>クイック スタート:Azure Stack Hub ポータルを使用して Windows サーバー VM を作成する

Azure Stack Hub ポータルを使用して Windows Server 2016 仮想マシン (VM) を作成する方法について説明します。

> [!NOTE]  
> この記事のスクリーンショットは、Azure Stack Hub バージョン 1808 で導入されたユーザー インターフェイスに合わせて更新されます。 1808 では、アンマネージド ディスクに加え、"*マネージド ディスク*" の使用のサポートが追加されています。 以前のバージョンを使用する場合は、ディスクの選択など、一部の画像が、この記事に示されているものとは異なります。  


## <a name="sign-in-to-the-azure-stack-hub-portal"></a>Azure Stack Hub ポータルにサインインする

Azure Stack Hub ポータルにサインインします。 Azure Stack Hub ポータルのアドレスは、接続している Azure Stack Hub 製品によって異なります。

* Azure Stack Development Kit (ASDK) の場合は、 `https://portal.local.azurestack.external` にアクセスします。
* Azure Stack Hub 統合システムの場合は、Azure Stack Hub オペレーターによって提供された URL に移動します。

## <a name="create-a-vm"></a>VM の作成

1. **[リソースの作成]**  >  **[Compute]** の順に選択します。 ` Windows Server 2016 Datacenter - Pay as you use` を検索します。
    **[Windows Server 2016 Datacenter - Pay-as-you-use]** エントリが表示されない場合は、Azure Stack Hub クラウド オペレーターに問い合わせて、Azure Stack Hub Marketplace に追加されるイメージを要求してください。 クラウド オペレータは、手順について、「[Azure Stack Hub Marketplace のカスタム アイテムを作成して発行する](../operator/azure-stack-create-and-publish-marketplace-item.md)」を参照できます。

    ![Windows Server 2016 Datacenter - 従量課金制](./media/azure-stack-quick-windows-portal/image1.png)

1. **［作成］** を選択します

    ![リソースの作成](./media/azure-stack-quick-windows-portal/image2.png)

1. **[基本]** で、 **[名前]** 、 **[ディスクの種類]** 、 **[ユーザー名]** 、および **[パスワード]** を入力します。 **[サブスクリプション]** を選択します。 **リソース グループ**を作成するか、既存のリソース グループを選択し、 **[場所]** を選択して、 **[OK]** を選択します。

    ![VM の作成 - 基本](./media/azure-stack-quick-windows-portal/image3.png)

1. **[サイズ]** で **[D1_v2]** を選択し、 **[選択]** で選択します。

    ![VM の作成 - サイズ](./media/azure-stack-quick-windows-portal/image4.png)

1. **[設定]** ページで、必要に応じて既定値を変更します。 関連するドロップダウンから、必要なパブリック受信ポートを構成する必要があります。 操作が完了したら、 **[OK]** をクリックします。

    ![VM の作成 - 設定](./media/azure-stack-quick-windows-portal/image5.png)

1. **[概要]** で **[OK]** を選択して VM を作成します。

    ![VM の作成 - 概要](./media/azure-stack-quick-windows-portal/image6.png)

1. **[仮想マシン]** を選択して、新しい VM を確認します。 VM 名を検索し、検索結果で VM を選択します。

![VM の作成 - VM の検索](./media/azure-stack-quick-windows-portal/image7.png)

## <a name="clean-up-resources"></a>リソースをクリーンアップする

VM の使用が完了したら、VM とそのリソースを削除します。 そのためには、VM ページでリソース グループを選択し、 **[削除]** を選択します。

## <a name="next-steps"></a>次のステップ

このクイック スタートでは、基本の Windows Server VM をデプロイしました。 Azure Stack Hub VM の詳細については、[Azure Stack Hub の VM の考慮事項](azure-stack-vm-considerations.md)に関する記事に進んでください。
