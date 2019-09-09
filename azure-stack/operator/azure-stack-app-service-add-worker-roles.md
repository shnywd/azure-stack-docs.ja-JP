---
title: Azure Stack 上の App Service で worker とインフラストラクチャを追加する | Microsoft Docs
description: Azure Stack App Services の詳細なガイダンス
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/06/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 06/08/2018
ms.openlocfilehash: 43ee38c18e2831d1cb96958501cee6f77292edd0
ms.sourcegitcommit: e2f6205e6469b39c2395ee09424bb7632cb94c40
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271676"
---
# <a name="add-workers-and-infrastructure-in-app-service-on-azure-stack"></a>Azure Stack 上の App Service で worker とインフラストラクチャを追加する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*  

ここでは、Azure Stack 上の App Service でインフラストラクチャおよび worker ロールを拡大縮小する手順について説明します。 任意のサイズのアプリをサポートする追加の worker ロールを作成するのに必要なすべての手順について説明します。

> [!NOTE]
> Azure Stack 環境の RAM が 96 GB 以下の場合、容量の追加が困難な可能性があります。

既定では、Azure Stack 上の App Service は無料の共有 worker 層をサポートしています。 他の worker 階層を追加するには、worker ロールを追加する必要があります。

既定の Azure Stack 上の App Service インストールでデプロイされる内容がわからない場合は、「[Azure Stack 上の App Service の概要](azure-stack-app-service-overview.md)」を参照してください。

Azure Stack 上の Azure App Service では、仮想マシン スケール セットを使用するすべてのロールをデプロイします。そのため、このワークロードの拡張機能を活用します。 したがって、worker 階層のすべてのスケーリングは、App Service 管理者を通して行われます。

## <a name="add-additional-workers-with-powershell"></a>PowerShell を使用した他の worker の追加

1. [PowerShell で Azure Stack 管理環境をセットアップします](azure-stack-powershell-configure-admin.md)

2. この例を使用して、スケール セットをスケールアウトします。
   ```powershell
   
    ##### Scale out the AppService Role instances ######
   
    # Set context to AzureStack admin.
    Login-AzureRmAccount -EnvironmentName AzureStackAdmin
                                                 
    ## Name of the Resource group where AppService is deployed.
    $AppServiceResourceGroupName = "AppService.local"

    ## Name of the ScaleSet : e.g. FrontEndsScaleSet, ManagementServersScaleSet, PublishersScaleSet , LargeWorkerTierScaleSet,      MediumWorkerTierScaleSet, SmallWorkerTierScaleSet, SharedWorkerTierScaleSet
    $ScaleSetName = "SharedWorkerTierScaleSet"

    ## TotalCapacity is sum of the instances needed at the end of operation. 
    ## e.g. if your VMSS has 1 instance(s) currently and you need 1 more the TotalCapacity should be set to 2
    $TotalCapacity = 2  

    # Get current scale set
    $vmss = Get-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -VMScaleSetName $ScaleSetName

    # Set and update the capacity
    $vmss.sku.capacity = $TotalCapacity
    Update-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -Name $ScaleSetName -VirtualMachineScaleSet $vmss 
   ```    

   > [!NOTE]
   > ロールの種類とインスタンスの数によっては、この手順が完了するまでに何時間もかかる場合があります。
   >
   >

3. App Service 管理で新しいロール インスタンスの状態を監視します。 個々のロール インスタンスの状態を確認するには、リスト内のロールの種類をクリックします。

## <a name="add-additional-workers-using-the-admin-portal"></a>管理ポータルを使用した他の worker の追加

1. Azure Stack 管理ポータルにサービス管理者としてサインインします。

2. **[App Services]** を参照します。

    ![Azure Stack 管理ポータルの App Service](media/azure-stack-app-service-add-worker-roles/image01.png)

3. **[ロール]** をクリックします。 デプロイされているすべての App Service ロールの内訳が表示されます。

4. 拡大縮小する種類の行を右クリックし、 **[ScaleSet]** をクリックします。

    ![Azure Stack 管理ポータルの ScaleSet App Service ロール](media/azure-stack-app-service-add-worker-roles/image02.png)

5. **[拡大縮小]** をクリックし、拡大縮小するインスタンス数を選択し、 **[保存]** をクリックします。

    ![Azure Stack 管理ポータルの App Service ロールで拡大縮小するためにインスタンスを設定する](media/azure-stack-app-service-add-worker-roles/image03.png)

6. Azure Stack 上の App Service によって、VM が追加および構成され、必要なすべてのソフトウェアがインストールされ、プロセスの完了時には準備完了とマークされます。 このプロセスには約 80 分かかる可能性があります。

7. **[ロール]** ブレードで worker を表示することで、新しいロールの準備状況を監視できます。

## <a name="result"></a>結果

完全にデプロイされ、準備ができると、worker をユーザーが利用できるようになり、それらにワークロードを展開できます。 既定で使用できる複数の価格レベルの例を次のスクリーンショットに示します。 特定の worker 階層に使用できる worker がない場合、対応する価格レベルを選択するオプションは使用できません。

![Azure Stack 管理ポータルの新しい App Service プランの価格レベル](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> 管理、フロントエンド、またはパブリッシャー ロールをスケール アウトするには、適切なロールの種類を選択して同じ手順を実行してください。 コントローラーはスケール セットとしてデプロイされていないため、すべての運用デプロイでインストール時に 2 つをデプロイする必要があります。

### <a name="next-steps"></a>次の手順

[デプロイ ソースを構成する](azure-stack-app-service-configure-deployment-sources.md)
