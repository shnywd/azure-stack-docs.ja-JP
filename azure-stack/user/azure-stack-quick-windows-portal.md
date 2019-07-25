---
title: Azure Stack ポータルを使用して Windows VM を作成する | Microsoft Docs
description: Azure Stack ポータルを使用して Windows Server 2016 仮想マシン (VM) を作成する方法について説明します。
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 05/16/2019
ms.author: mabrigg
ms.custom: mvc
ms.reviewer: kivenkat
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: cc9a6baa3c71e58c2671b1f1b221e18a0c4f38c1
ms.sourcegitcommit: b36d078e699c7924624b79641dbe9021af9606ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67816166"
---
# <a name="quickstart-create-a-windows-server-vm-with-the-azure-stack-portal"></a>クイック スタート:Azure Stack ポータルを使用して Windows サーバー VM を作成する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure Stack ポータルを使用して Windows Server 2016 仮想マシン (VM) を作成する方法について説明します。

> [!NOTE]  
> この記事のスクリーンショットは、Azure Stack バージョン 1808 で導入されたユーザー インターフェイスに合わせて更新されます。 1808 では、アンマネージド ディスクに加え、"*マネージド ディスク*" の使用のサポートが追加されています。 以前のバージョンを使用する場合は、ディスクの選択など、一部の画像が、この記事に示されているものとは異なります。  


## <a name="sign-in-to-the-azure-stack-portal"></a>Azure Stack ポータルへのサインイン

Azure Stack ポータルにサインインします。 Azure Stack ポータルのアドレスは、接続している Azure Stack 製品によって異なります。

* Azure Stack Development Kit (ASDK) の場合は、 https://portal.local.azurestack.external にアクセスします。
* Azure Stack 統合システムの場合は、Azure Stack オペレーターによって提供された URL にアクセスします。

## <a name="create-a-vm"></a>VM の作成

1. **[+ リソースの作成]**  >  **[Compute]**  >  **[Windows Server 2016 Datacenter - 従量課金]**  >  **[作成]** の順にクリックします。 <br> **[Windows Server 2016 Datacenter - 従量課金]** エントリが表示されない場合は、Azure Stack オペレーターに問い合わせて、「[Windows Server 2016 VM イメージの Azure Stack Marketplace への追加](../operator/azure-stack-create-and-publish-marketplace-item.md)」の記事で説明されているように、それをマーケットプレースに追加するようオペレーターに依頼してください。

    ![ポータルで Windows VM を作成する手順](media/azure-stack-quick-windows-portal/image01.png)

2. **[基本]** で、 **[名前]** 、 **[ユーザー名]** 、 **[パスワード]** を入力します。 **[サブスクリプション]** を選択します。 **リソース グループ**を作成するか、既存のリソース グループを選択し、 **[場所]** を選択して、 **[OK]** をクリックします。

    ![基本設定を構成する](media/azure-stack-quick-windows-portal/image02.png)

3. **[サイズ]** で **[D1 Standard]\(D1 Standard\)** を選択して、 **[選択]** をクリックします。  

    ![VM のサイズを選択する](media/azure-stack-quick-windows-portal/image03.png)

4. **[設定]** ページで、必要に応じて既定値を変更します。
   - Azure Stack バージョン 1808 以降では、**ストレージ**を構成できます。ここで "*マネージド ディスク*" の使用を選択できます。 1808 より前のバージョンでは、アンマネージド ディスクのみを使用できます。  

   ![VM 設定を構成する](media/azure-stack-quick-windows-portal/image04.png)  

   お使いの構成の準備が整ったら、 **[OK]** を選択して続行します。

5. **[概要]** で、 **[OK]** をクリックして VM を作成します。
    ![概要の表示と VM の作成](media/azure-stack-quick-windows-portal/image05.png)

6. 新しい VM を表示するには、 **[すべてのリソース]** をクリックし、VM 名を検索して、検索結果でそれを選択します。

    ![VM の参照](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>リソースのクリーンアップ

VM の使用が完了したら、VM とそのリソースを削除します。 そのためには、VM ページでリソース グループを選択し、 **[削除]** をクリックします。

## <a name="next-steps"></a>次の手順

このクイック スタートでは、基本の Windows Server VM をデプロイしました。 Azure Stack VM の詳細については、「[Azure Stack の仮想マシンに関する考慮事項](azure-stack-vm-considerations.md)」に進んでください。
