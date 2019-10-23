---
title: カスタム VM イメージを Azure Stack に追加する | Microsoft Docs
description: Azure Stack にカスタム VM イメージを追加または削除する方法について説明します。
services: azure-stack
documentationcenter: ''
author: Justinha
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: Justinha
ms.reviewer: kivenkat
ms.lastreviewed: 06/08/2018
ms.openlocfilehash: 9dc5039a2c8b74b14da59573758a4cf8d1a3657a
ms.sourcegitcommit: d159652f50de7875eb4be34c14866a601a045547
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282646"
---
# <a name="add-a-custom-vm-to-azure-stack"></a>Azure Stack にカスタム VM を追加する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure Stack では、カスタム仮想マシン (VM) のイメージをマーケットプレースに追加してご自分のユーザーに提供することができます。 イメージは、Azure Stack 用の Azure Resource Manager テンプレートを使用して追加します。 管理者ポータルまたは Windows PowerShell を使用して、VM イメージを Marketplace 項目として Azure Marketplace UI に追加することもできます。 グローバル Azure Marketplace のイメージか、独自のカスタム VM イメージを使用します。

## <a name="generalize-the-vm-image"></a>VM イメージの汎用化

### <a name="windows"></a>Windows

汎用化したカスタム VHD を作成します。 VHD が Azure の外部からのものである場合は、「[汎用化した VHD をアップロードして Azure で新しい VM を作成する](/azure/virtual-machines/windows/upload-generalized-managed)」の手順に従い、VHD に対して **Sysprep** を正しく実行して汎用化します。

VHD が Azure からのものである場合は、Azure Stack に移植する前に、「[Sysprep を使用してソース VM を一般化する](/azure/virtual-machines/windows/upload-generalized-managed#generalize-the-source-vm-by-using-sysprep)」の手順に従います。

### <a name="linux"></a>Linux

VHD が Azure の外部からのものである場合は、適切な手順に従って VHD を汎用化します。

- [CentOS ベースのディストリビューション](/azure/virtual-machines/linux/create-upload-centos?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Debian Linux](/azure/virtual-machines/linux/debian-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Red Hat Enterprise Linux](/azure/azure-stack/azure-stack-redhat-create-upload-vhd)
- [SLES または openSUSE](/azure/virtual-machines/linux/suse-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Ubuntu Server](/azure/virtual-machines/linux/create-upload-ubuntu?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

VHD が Azure からのものである場合は、次の手順に従って VHD を汎用化します。

1. **waagent** サービスを停止します。

   ```bash
   # sudo waagent -force -deprovision
   # export HISTSIZE=0
   # logout
   ```

2. VM をシャットダウンし、VHD をダウンロードします。 Azure から VHD を取り込む場合は、「[Azure から Windows VHD をダウンロードする](/azure/virtual-machines/windows/download-vhd)」に示されているように、ディスクのエクスポートを使用してこれを行うことができます。

### <a name="common-steps-for-both-windows-and-linux"></a>Windows と Linux の両方に共通の手順

イメージをアップロードする前に、以下を考慮する必要があります。

- Azure Stack では、VHD フォーマットの固定ディスク内で第 1 世代の VM のみサポートされます。 固定フォーマットの場合、ファイル内で論理ディスクが線形に構成されるため、ディスク オフセット *X* は BLOB オフセット *X* に格納されます。BLOB 末尾の小さなフッターに、VHD のプロパティが記述されます。 ディスクが固定フォーマットであるかどうかを確認するには、**Get-VHD** PowerShell コマンドレットを使用します。

- Azure Stack では、ダイナミック ディスク VHD はサポートされていません。 VM に接続されているダイナミック ディスクのサイズを変更すると、VM はエラー状態になります。 この問題を軽減するには、VM のディスク、つまりストレージ アカウントの VHD BLOB を削除せずに VM を削除します。 次に、VHD をダイナミック ディスクから固定ディスクに変換した後、VM を再作成します。

## <a name="add-a-vm-image-as-an-azure-stack-operator-using-the-portal"></a>ポータルを使用して VM イメージを Azure Stack オペレーターとして追加する

1. イメージは、Blob Storage URI で参照できなければなりません。 Windows または Linux オペレーティング システムのイメージを VHD 形式 (VHDX ではない) で準備して、そのイメージを Azure のストレージ アカウントまたは Azure Stack にアップロードします。

   - VHD が Azure 内にある場合、接続された Azure Stack 上で実行していれば、[Azcopy](/azure/storage/common/storage-use-azcopy) などのツールを使用して、Azure と自分の Azure Stack ストレージ アカウントの間で VHD を直接転送できます。

   - 切断された Azure Stack 上で、VHD が Azure 内にある場合は、Azure と Azure Stack の両方に接続されているマシンに VHD をダウンロードする必要があります。 その後、VHD を Azure からこのマシンにコピーしてから、Azure と Azure Stack 間で使用できる一般的な[ストレージ データ転送ツール](../user/azure-stack-storage-transfer.md)を使用して Azure Stack に VHD を転送します。

2. [この例](/powershell/module/azurerm.compute/add-azurermvhd?view=azurermps-6.13.0)に従って、Azure Stack 管理者ポータル内でストレージ アカウントに VHD をアップロードできます。

   - Azure Blob Storage よりも Azure Stack Blob Storage の方がイメージを効率よくアップロードできます。イメージを Azure Stack イメージ リポジトリにプッシュする方が時間がかからないためです。

   - [Windows VM イメージ](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/)をアップロードするときは、**Azure へのログイン**の手順の代わりに [Azure Stack オペレーターの PowerShell 環境を構成](azure-stack-powershell-configure-admin.md)する手順を実行してください。  

3. イメージのアップロード先の Blob Storage URI をメモしておきます。 Blob Storage URI の形式は *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;* .vhd です。

4. BLOB に匿名でアクセスできるようにするには、VM イメージ VHD がアップロードされたストレージ アカウントの BLOB コンテナーに移動します。 **[BLOB]** を選択し、 **[アクセス ポリシー]** を選択してください。 必要であれば、このコンテナーの Shared Access Signature を生成し、それを BLOB URI に含めることもできます。 この手順により、確実に BLOB が利用できるようになります。 BLOB に匿名でアクセスできない場合、VM イメージがエラー状態で作成されます。

   ![ストレージ アカウント BLOB に移動](./media/azure-stack-add-vm-image/image1.png)

   ![BLOB のアクセスを公開に設定](./media/azure-stack-add-vm-image/image2.png)

5. Azure Stack にオペレーターとしてサインインします。 メニューから、 **[コンピューティング]**  >  **[追加]** の **[すべてのサービス]**  >  **[イメージ]** を選択します。

6. **[イメージの作成]** で、名前、サブスクリプション、リソース グループ、場所、OS ディスク、OS の種類、BLOB ストレージ URI、アカウントの種類、ホストのキャッシュを入力します。 その後 **[作成]** をクリックすると VM イメージの作成が開始されます。

   ![イメージの作成を開始](./media/azure-stack-add-vm-image/image4.png)

   イメージが正常に作成されると、VM イメージの状態が **[成功]** に変わります。

7. イメージを追加すると、そのイメージは Azure Resource Manager ベースのテンプレートと PowerShell のデプロイのみで使用できるようになります。 イメージを Marketplace 項目としてユーザーが使用できるようにするには、「[Marketplace アイテムを作成および発行する](azure-stack-create-and-publish-marketplace-item.md)」の手順を使用して Marketplace 項目を公開してください。 **Publisher**、**Offer**、**SKU**、**Version** の値を書き留めておいてください。 これらは、カスタムの .azpkg でリソース マネージャー テンプレートと Manifest.json を編集するときに必要になります。

## <a name="remove-the-vm-image-as-an-azure-stack-operator-using-the-portal"></a>ポータルを使用して VM イメージを Azure Stack オペレーターとして削除する

1. [Azure Stack 管理者ポータル](https://adminportal.local.azurestack.external)を開きます。

2. VM イメージに Marketplace 項目が関連付けられている場合は、 **[Marketplace management]\(Marketplace 管理\)** を選択し、削除する VM Marketplace 項目を選択します。

3. VM イメージに Marketplace 項目が関連付けられていない場合は、 **[すべてのサービス] > [コンピューティング] > [VM イメージ]** に移動し、VM イメージの横にある省略記号 ( **...** ) を選択します。

4. **[削除]** を選択します。

## <a name="add-a-vm-image-as-an-azure-stack-operator-using-powershell"></a>PowerShell を使用して VM イメージを Azure Stack オペレーターとして追加する

1. [PowerShell for Azure Stack をインストールします](azure-stack-powershell-install.md)。  

2. Azure Stack にオペレーターとしてサインインします。 手順については、[オペレーターとしての Azure Stack へのサインイン](azure-stack-powershell-configure-admin.md)に関するページをご覧ください。

3. 管理者特権のプロンプトで PowerShell を開き、次を実行します。

   ```powershell
    Add-AzsPlatformimage -publisher "<publisher>" `
      -offer "<Offer>" `
      -sku "<SKU>" `
      -version "<#.#.#>" `
      -OSType "<OS type>" `
      -OSUri "<OS URI>"
   ```

   **Add-AzsPlatformimage** コマンドレットでは、VM イメージを参照するために Azure Resource Manager テンプレートによって使用される値が指定されます。 値は次のとおりです。
   - **publisher**  
     次に例を示します。`Canonical`  
     イメージをデプロイするときにユーザーが使用する VM イメージの**発行元**名のセグメント。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **offer**  
     次に例を示します。`UbuntuServer`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージの**オファー**名のセグメント。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **sku**  
     次に例を示します。`14.04.3-LTS`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージの **SKU** 名のセグメント。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **version**  
     次に例を示します。`1.0.0`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージのバージョン。 このバージョンの形式は *\#.\#.\#* です。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **osType**  
     次に例を示します。`Linux`  
     イメージの **osType** には **Windows** または **Linux** を指定する必要があります。  
   - **OSUri**  
     次に例を示します。`https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
     `osDisk` の Blob Storage URI を指定できます。  

     詳しくは、[Add-AzsPlatformimage](/powershell/module/azs.compute.admin/add-azsplatformimage) コマンドレットおよび [New-DataDiskObject](/powershell/module/Azs.Compute.Admin/New-DataDiskObject) コマンドレットの PowerShell リファレンスをご覧ください。

## <a name="remove-a-vm-image-as-an-azure-stack-operator-using-powershell"></a>PowerShell を使用して VM イメージを Azure Stack オペレーターとして削除する

過去にアップロードした VM イメージが必要なくなった場合は、次のコマンドレットを使用して Marketplace から削除できます。

1. [PowerShell for Azure Stack をインストールします](azure-stack-powershell-install.md)。

2. Azure Stack にオペレーターとしてサインインします。

3. 管理者特権のプロンプトで PowerShell を開き、次を実行します。

   ```powershell  
   Remove-AzsPlatformImage `
    -publisher "<Publisher>" `
    -offer "<Offer>" `
    -sku "<SKU>" `
    -version "<Version>" `
   ```

   **Remove-AzsPlatformImage** コマンドレットでは、VM イメージを参照するために Azure Resource Manager テンプレートによって使用される値が指定されます。 値は次のとおりです。
   - **publisher**  
     次に例を示します。`Canonical`  
     イメージをデプロイするときにユーザーが使用する VM イメージの**発行元**名のセグメント。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **offer**  
     次に例を示します。`UbuntuServer`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージの**オファー**名のセグメント。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **sku**  
     次に例を示します。`14.04.3-LTS`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージの **SKU** 名のセグメント。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **version**  
     次に例を示します。`1.0.0`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージのバージョン。 このバージョンの形式は *\#.\#.\#* です。 このフィールドではスペースや他の特殊文字は使用できません。  

     **Remove-AzsPlatformImage** コマンドレットの詳細については、Microsoft PowerShell の [Azure Stack オペレーター モジュールのドキュメント](/powershell/module/)を参照してください。

## <a name="next-steps"></a>次の手順

- [カスタム Azure Stack Marketplace 項目の作成と発行](azure-stack-create-and-publish-marketplace-item.md)
- [仮想マシンのプロビジョニング](../user/azure-stack-create-vm-template.md)
