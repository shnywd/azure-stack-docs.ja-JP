---
title: 仮想マシン スケール セットを Azure Stack Hub 内で使用できるようにする
description: クラウド オペレーターが Azure Stack Hub Marketplace に仮想マシン スケール セットを追加する方法について学習します。
author: sethmanheim
ms.topic: article
ms.date: 01/22/2020
ms.author: sethm
ms.reviewer: kivenkat
ms.lastreviewed: 10/22/2019
ms.openlocfilehash: f0b23df25ef2e9fffe9c5f43ee649ecfe5f74092
ms.sourcegitcommit: a630894e5a38666c24e7be350f4691ffce81ab81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "77695293"
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack-hub"></a>仮想マシン スケール セットを Azure Stack Hub 内で使用できるようにする

仮想マシン スケール セットは Azure Stack Hub のコンピューティング リソースです。 スケール セットを使用して同一の仮想マシン (VM) のセットをデプロイおよび管理できます。 すべての VM が同一に構成されている場合、スケール セットでは VM を事前にプロビジョニングする必要はありません。 ビッグ コンピューティング、ビッグ データ、コンテナー化されたワークロードを対象にした大規模サービスをより簡単に構築できます。

この記事では、Azure Stack Hub Marketplace 上でスケール セットを入手できるようにするプロセスについて説明します。 この手順を完了すると、ユーザーは各自のサブスクリプションに仮想マシン スケール セットを追加できます。

Azure Stack Hub の仮想マシン スケール セットは、Azure の仮想マシン スケール セットと同様です。 詳細については、次のビデオをご覧ください。

* [Mark Russinovich が語る Azure Scale Sets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [仮想マシン スケール セットを Guy Bowerman が解説](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

Azure Stack Hub 上の仮想マシン スケール セットでは、自動スケールはサポートされません。 Resource Manager テンプレート、CLI、または PowerShell を使用して、スケール セットに複数のインスタンスを追加できます。

## <a name="prerequisites"></a>前提条件

* **Azure Stack Hub Marketplace:** Azure Stack Hub Marketplace 上で項目を入手できるようにするには、Azure Stack Hub をグローバル Azure に登録します。 [Azure Stack Hub の Azure への登録](azure-stack-registration.md)の手順に従います。
* **オペレーティング システム イメージ:** 仮想マシン スケール セットを作成する前に、[Azure Stack Hub Marketplace](azure-stack-download-azure-marketplace-item.md) からスケール セットで使用する VM イメージをダウンロードする必要があります。 ユーザーが新しいスケール セットを作成する前に、イメージが既に存在している必要があります。

## <a name="use-the-azure-stack-hub-portal"></a>Azure Stack Hub ポータルを使用する

>[!IMPORTANT]  
> このセクションの情報は、1808 以降の Azure Stack Hub バージョンを使用する場合に適用されます。

1. Azure Stack Hub ポータルにサインインします。 **[すべてのサービス]** 、 **[仮想マシン スケール セット]** に進み、 **[コンピューター]** の下の **[仮想マシン スケール セット]** を選択します。
   ![[仮想マシン スケール セット] を選択する](media/azure-stack-compute-add-scalesets/all-services.png)

2. ***[仮想マシン スケール セットの作成]*** を選択します。
   ![仮想マシン スケール セットを作成する](media/azure-stack-compute-add-scalesets/create-scale-set.png)

3. 空のフィールドに入力し、 **[オペレーティング システムのディスク イメージ]** 、 **[サブスクリプション]** 、および **[インスタンス サイズ]** のドロップダウン リストから選択します。 **[管理ディスクを使用]** で **[はい]** を選択します。 **[作成]** をクリックします。
    ![仮想マシン スケール セットの構成と作成](media/azure-stack-compute-add-scalesets/create.png)

4. 新しい仮想マシン スケール セットを表示するには、 **[すべてのリソース]** に進んで仮想マシン スケール セット名を検索し、検索でその名前を選択します。
   ![仮想マシン スケール セットの表示](media/azure-stack-compute-add-scalesets/search.png)

## <a name="update-images-in-a-virtual-machine-scale-set"></a>仮想マシン スケール セットのイメージを更新する

仮想マシン スケール セットを作成すると、スケール セットを再作成することなく、スケール セット内のイメージを更新できます。 イメージを更新するプロセスは、次のようにシナリオによって変わります。

1. 仮想マシン スケール セット デプロイ テンプレートの **version** には **latest** を指定します。  

   スケール セットのテンプレートの `imageReference` セクションで `version` を **latest** に設定していると、スケール セットに対するスケールアップ操作で、スケール セット インスタンスの利用可能な最新バージョンのイメージが使用されます。 スケールアップが完了したら、以前の仮想マシン スケール セット インスタンスを削除できます。 `publisher`、`offer`、および `sku` の値は変わりません。

   次の JSON の例では、`latest` を以下のように指定しています。  

    ```json  
    "imageReference": {
        "publisher": "[parameters('osImagePublisher')]",
        "offer": "[parameters('osImageOffer')]",
        "sku": "[parameters('osImageSku')]",
        "version": "latest"
        }
    ```

2. 仮想マシン スケール セット デプロイ テンプレートで **version** に **latest を指定せず**、代わりにバージョン番号を指定します。  

    Azure Stack オペレーターが新しいバージョンのイメージをダウンロード (および古いバージョンを削除) すると、スケール セットはスケールアップできません。 スケール セット テンプレートに指定されたイメージのバージョンを使用できる必要があるため、これは仕様です。  

詳細は、[オペレーティング システムのディスクとイメージ](../user/azure-stack-compute-overview.md#operating-system-disks-and-images)に関するページを参照してください。  

## <a name="scale-a-virtual-machine-scale-set"></a>仮想マシン スケール セットを拡大縮小する

仮想マシン スケール セットのサイズをスケーリングして大きく、または小さくすることができます。

1. ポータルで、スケール セットを選択し、 **[拡大縮小]** を選択します。

2. スライド バーを使用してこの仮想マシン スケール セットのスケーリングの新しいレベルを設定し、 **[保存]** をクリックします。

     ![仮想マシン セットのスケーリング](media/azure-stack-compute-add-scalesets/scale.png)

## <a name="next-steps"></a>次のステップ

* [Azure から Azure Stack Hub に Marketplace の項目をダウンロードする](azure-stack-download-azure-marketplace-item.md)
