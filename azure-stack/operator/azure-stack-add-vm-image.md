---
title: VM イメージを Azure Stack に追加する | Microsoft Docs
description: Azure Stack に VM イメージを追加または削除する方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: mabrigg
ms.reviewer: kivenkat
ms.lastreviewed: 06/08/2018
ms.openlocfilehash: 8fec1b3702aa7c8c55f1a90167b1ac13f0ac8847
ms.sourcegitcommit: e2f6205e6469b39c2395ee09424bb7632cb94c40
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271754"
---
# <a name="add-a-vm-image-to-azure-stack"></a>VM イメージを Azure Stack に追加する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure Stack では、仮想マシン (VM) イメージをマーケットプレースに追加してユーザーに提供することができます。 イメージは、Azure Stack 用の Azure Resource Manager テンプレートを使用して追加します。 管理ポータルまたは Windows PowerShell を使用して、VM イメージを Marketplace 項目として Azure Marketplace UI に追加することもできます。 グローバル Azure Marketplace のイメージか、独自のカスタム VM イメージを使用します。

## <a name="add-a-vm-image-through-the-portal"></a>ポータルから VM イメージを追加する

> [!NOTE]  
> この方法では、Marketplace 項目を個別に作成する必要があります。

イメージは、Blob Storage URI で参照できなければなりません。 Windows または Linux オペレーティング システムのイメージを VHD 形式 (VHDX ではない) で準備して、そのイメージを Azure のストレージ アカウントまたは Azure Stack にアップロードします。 イメージが既に Azure の Blob Storage または Azure Stack にアップロードされている場合、手順 1. をスキップできます。

1. [Resource Manager のデプロイのために Windows VM イメージを Azure にアップロードする](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/)か、Linux イメージの場合は、[Azure Stack への Linux 仮想マシンのデプロイ](azure-stack-linux.md)に関するページの説明に従います。 イメージをアップロードする前に、次の事実を考慮する必要があります。

   - Azure Stack は、VHD フォーマットの固定ディスクで第 1 世代の VM のみサポートします。 固定フォーマットの場合、ファイル内で論理ディスクがリニアに構成されるため、ディスク オフセット X は BLOB オフセット X に格納されます。BLOB 末尾の小さなフッターに、VHD のプロパティが記述されます。 ディスクが固定フォーマットであるかどうかを確認するには、[Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd?view=win10-ps) PowerShell コマンドを使用します。  

     > [!IMPORTANT]  
     >  Azure Stack では、ダイナミック ディスク VHD はサポートされていません。 VM に接続されているダイナミック ディスクのサイズを変更すると、VM はエラー状態になります。 この問題を軽減するには、VM のディスク、つまりストレージ アカウントの VHD BLOB を削除せずに VM を削除します。 次に、VHD をダイナミック ディスクから固定ディスクに変換した後、VM を再作成します。

   - Azure Blob Storage よりも Azure Stack Blob Storage の方がイメージを効率よくアップロードできます。イメージを Azure Stack イメージ リポジトリにプッシュする方が時間がかからないためです。

   - [Windows VM イメージ](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/)をアップロードするときは、**Azure へのログイン**の手順の代わりに [Azure Stack オペレーターの PowerShell 環境を構成](azure-stack-powershell-configure-admin.md)する手順を実行してください。  

   - イメージのアップロード先の Blob Storage URI をメモしておきます。 Blob Storage URI の形式は *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;* .vhd です。

   - BLOB に匿名でアクセスできるようにするには、VM イメージ VHD がアップロードされたストレージ アカウントの BLOB コンテナーに移動します。 **[BLOB]** を選択し、 **[アクセス ポリシー]** を選択してください。 必要であれば、このコンテナーの Shared Access Signature を生成し、それを BLOB URI に含めることもできます。 この手順により、確実に BLOB が利用できるようになります。 BLOB に匿名でアクセスできない場合、VM イメージがエラー状態で作成されます。

     ![ストレージ アカウント BLOB に移動](./media/azure-stack-add-vm-image/image1.png)

     ![BLOB のアクセスを公開に設定](./media/azure-stack-add-vm-image/image2.png)

2. Azure Stack にオペレーターとしてサインインします。 メニューから、 **[コンピューティング]**  >  **[追加]** の **[すべてのサービス]**  >  **[イメージ]** を選択します。

3. **[イメージの作成]** で、名前、サブスクリプション、リソース グループ、場所、OS ディスク、OS の種類、BLOB ストレージ URI、アカウントの種類、ホストのキャッシュを入力します。 その後 **[作成]** をクリックすると VM イメージの作成が開始されます。

   ![イメージの作成を開始](./media/azure-stack-add-vm-image/image4.png)

   イメージが正常に作成されると、VM イメージの状態が **[成功]** に変わります。

4. VM イメージをユーザーが UI ですぐに使用できるようにするには、[Marketplace 項目の作成](azure-stack-create-and-publish-marketplace-item.md)が最適です。

## <a name="remove-a-vm-image-through-the-portal"></a>ポータルから VM イメージを削除する

1. [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external) で管理ポータルを開きます。

2. **Marketplace Management** を選択し、削除する VM を選択します。

3. **[削除]** をクリックします。

## <a name="add-a-vm-image-to-the-marketplace-by-using-powershell"></a>PowerShell を使用して VM イメージを Marketplace に追加する

> [!Note]  
> イメージを追加すると、そのイメージは Azure Resource Manager ベースのテンプレートと PowerShell のデプロイのみで使用できるようになります。 イメージを Marketplace 項目としてユーザーが使用できるようにするには、次の記事の手順を使用して Marketplace 項目を公開してください。[Marketplace アイテムの作成と発行](azure-stack-create-and-publish-marketplace-item.md)

1. [PowerShell for Azure Stack をインストールします](azure-stack-powershell-install.md)。  

2. Azure Stack にオペレーターとしてサインインします。 手順については、[オペレーターとしての Azure Stack へのサインイン](azure-stack-powershell-configure-admin.md)に関するページをご覧ください。

3. 管理者特権のプロンプトで PowerShell を開き、次を実行します。

   ```powershell
    Add-AzsPlatformimage -publisher "<publisher>" `
      -offer "<offer>" `
      -sku "<sku>" `
      -version "<#.#.#>" `
      -OSType "<ostype>" `
      -OSUri "<osuri>"
   ```

   **Add-AzsPlatformimage** コマンドレットでは、VM イメージを参照するために Azure Resource Manager テンプレートによって使用される値が指定されます。 値は次のとおりです。
   - **publisher**  
     次に例を示します。`Canonical`  
     イメージをデプロイするときにユーザーが使用する VM イメージの発行元名のセグメント。 たとえば、**Microsoft** です。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **offer**  
     次に例を示します。`UbuntuServer`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージのプラン名のセグメント。 たとえば、**WindowsServer** です。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **sku**  
     次に例を示します。`14.04.3-LTS`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージの SKU 名のセグメント。 たとえば、**Datacenter2016** です。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **version**  
     次に例を示します。`1.0.0`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージのバージョン。 このバージョンの形式は *\#.\#.\#* です。 たとえば、**1.0.0** です。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **osType**  
     次に例を示します。`Linux`  
     イメージの osType には **Windows** または **Linux** を指定する必要があります。  
   - **OSUri**  
     次に例を示します。`https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
     `osDisk` の Blob Storage URI を指定できます。  

     詳しくは、[Add-AzsPlatformimage](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage) コマンドレットおよび [New-DataDiskObject](https://docs.microsoft.com/powershell/module/Azs.Compute.Admin/New-DataDiskObject) コマンドレットの PowerShell リファレンスをご覧ください。

## <a name="add-a-custom-vm-image-to-the-marketplace-by-using-powershell"></a>PowerShell を使用してカスタム VM イメージを Marketplace に追加する
 
1. [PowerShell for Azure Stack をインストールします](azure-stack-powershell-install.md)。

   ```powershell
    # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external" `
      -AzureKeyVaultDnsSuffix adminvault.local.azurestack.external `
      -AzureKeyVaultServiceEndpointResourceId https://adminvault.local.azurestack.external

    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
      -EnvironmentName AzureStackAdmin

    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
   ```

2. **Active Directory フェデレーション サービス (AD FS)** を使用している場合は、次のコマンドレットを使用します。

   ```powershell
   # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
   $ArmEndpoint = "<Resource Manager endpoint for your environment>"

   # For Azure Stack Development Kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
   $GraphAudience = "<GraphAudience endpoint for your environment>"

   # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint "https://adminmanagement.local.azurestack.external" `
      -AzureKeyVaultDnsSuffix adminvault.local.azurestack.external `
      -AzureKeyVaultServiceEndpointResourceId https://adminvault.local.azurestack.external
    ```

3. Azure Stack にオペレーターとしてサインインします。 手順については、[オペレーターとしての Azure Stack へのサインイン](azure-stack-powershell-configure-admin.md)に関するページをご覧ください。

4. グローバル Azure または Azure Stack で、ご自身のカスタム VM イメージを格納するためのストレージ アカウントを作成します。 手順については、「[クイック スタート:Azure portal を使用して BLOB をアップロード、ダウンロード、および一覧表示する](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)」を参照してください。

5. Windows または Linux オペレーティング システムのイメージを VHD 形式 (VHDX ではない) で準備して、そのイメージをご自身のストレージ アカウントにアップロードし、PowerShell で VM イメージを取得するときに使用する URI を取得します。  

   ```powershell
    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
   ```

6. (省略可能) データ ディスクの配列を VM イメージの一部としてアップロードできます。 New-DataDiskObject コマンドレットを使用して、データ ディスクを作成します。 管理者特権のプロンプトで PowerShell を開き、次を実行します。

   ```powershell
    New-DataDiskObject -Lun 2 `
    -Uri "https://storageaccount.blob.core.windows.net/vhds/Datadisk.vhd"
   ```

7. 管理者特権のプロンプトで PowerShell を開き、次を実行します。

   ```powershell
    Add-AzsPlatformimage -publisher "<publisher>" -offer "<offer>" -sku "<sku>" -version "<#.#.#>" -OSType "<ostype>" -OSUri "<osuri>"
   ```

    Add-AzsPlatformimage コマンドレットと New-DataDiskObject コマンドレットの詳細については、Microsoft PowerShell の [Azure Stack オペレーター モジュールのドキュメント](https://docs.microsoft.com/powershell/module/)を参照してください。

## <a name="remove-a-vm-image-by-using-powershell"></a>PowerShell を使用して VM イメージを削除する

過去にアップロードした VM イメージが必要なくなった場合は、次のコマンドレットを使用して Marketplace から削除できます。

1. [PowerShell for Azure Stack をインストールします](azure-stack-powershell-install.md)。

2. Azure Stack にオペレーターとしてサインインします。

3. 管理者特権のプロンプトで PowerShell を開き、次を実行します。

   ```powershell  
   Remove-AzsPlatformImage `
    -publisher "<publisher>" `
    -offer "<offer>" `
    -sku "<sku>" `
    -version "<version>" `
   ```
   **Remove-AzsPlatformImage** コマンドレットでは、VM イメージを参照するために Azure Resource Manager テンプレートによって使用される値が指定されます。 値は次のとおりです。
   - **publisher**  
     次に例を示します。`Canonical`  
     イメージをデプロイするときにユーザーが使用する VM イメージの発行元名のセグメント。 たとえば、**Microsoft** です。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **offer**  
     次に例を示します。`UbuntuServer`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージのプラン名のセグメント。 たとえば、**WindowsServer** です。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **sku**  
     次に例を示します。`14.04.3-LTS`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージの SKU 名のセグメント。 たとえば、**Datacenter2016** です。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **version**  
     次に例を示します。`1.0.0`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージのバージョン。 このバージョンの形式は *\#.\#.\#* です。 たとえば、**1.0.0** です。 このフィールドではスペースや他の特殊文字は使用できません。  
    
     Remove-AzsPlatformImage コマンドレットの詳細については、Microsoft PowerShell の [Azure Stack オペレーター モジュールのドキュメント](https://docs.microsoft.com/powershell/module/)を参照してください。

## <a name="next-steps"></a>次の手順

[仮想マシンのプロビジョニング](../user/azure-stack-create-vm-template.md)
