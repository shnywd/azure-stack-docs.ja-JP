---
title: Azure Stack Hub にカスタム VM イメージを追加する
description: Azure Stack Hub にカスタム VM イメージを追加または削除する方法について説明します。
author: sethmanheim
ms.topic: conceptual
ms.date: 02/07/2020
ms.author: sethm
ms.reviewer: kivenkat
ms.lastreviewed: 06/08/2019
ms.openlocfilehash: 4d2f76e1af47800331aac44715b5b8630baceae1
ms.sourcegitcommit: 4ac711ec37c6653c71b126d09c1f93ec4215a489
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77701430"
---
# <a name="add-a-custom-vm-image-to-azure-stack-hub"></a>Azure Stack Hub にカスタム VM イメージを追加する

Azure Stack Hub では、カスタム仮想マシン (VM) のイメージをマーケットプレースに追加してご自分のユーザーに提供することができます。 VM イメージは、管理者ポータルまたは Windows PowerShell を介して Azure Stack Hub Marketplace に追加できます。 グローバル Azure Marketplace のイメージをカスタム イメージのベースとして使用するか、Hyper-V を使用して独自のイメージを作成します。

## <a name="step-1-create-the-custom-vm-image"></a>手順 1:カスタム VM イメージを作成する

### <a name="windows"></a>Windows

汎用化したカスタム VHD を作成します。

**VHD が Azure の外部からのものである場合は**、「[汎用化した VHD をアップロードして Azure で新しい VM を作成する](/azure/virtual-machines/windows/upload-generalized-managed)」の手順に従って VHD に対して **Sysprep** を正しく実行し、その VHD を汎用化します。

**VHD が Azure からのものである場合は**、VM を汎用化する前に、次のことを確認してください。

- Azure 上で VM をプロビジョニングする場合は、PowerShell を使用し、`-ProvisionVMAgent` フラグなしでそれをプロビジョニングします。
- Azure で VM を汎用化する前に、VM から **Remove-AzureRmVMExtension** コマンドレットを使用してすべての VM 拡張機能を削除します。 `Windows (C:) > WindowsAzure > Logs > Plugins` に移動することにより、どの VM 拡張機能がインストールされているかを見つけることができます。

```powershell
Remove-AzureRmVMExtension -ResourceGroupName winvmrg1 -VMName windowsvm -Name "CustomScriptExtension"
```

VHD を Azure Stack Hub に移植する前に、[この記事](/azure/virtual-machines/windows/download-vhd)の手順に従ってその VHD を正しく汎用化してダウンロードします。

### <a name="linux"></a>Linux

**VHD が Azure の外部からのものである場合は**、適切な手順に従って VHD を汎用化します。

- [CentOS ベースのディストリビューション](/azure/virtual-machines/linux/create-upload-centos?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Debian Linux](/azure/virtual-machines/linux/debian-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Red Hat Enterprise Linux](/azure/azure-stack/azure-stack-redhat-create-upload-vhd)
- [SLES または openSUSE](/azure/virtual-machines/linux/suse-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Ubuntu Server](/azure/virtual-machines/linux/create-upload-ubuntu?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

**VHD が Azure からのものである場合は**、次の手順に従って VHD を汎用化し、ダウンロードします。

1. **waagent** サービスを停止します。

   ```bash
   sudo waagent -force -deprovision
   export HISTSIZE=0
   logout
   ```

   Azure Stack Hub で動作する Azure Linux エージェントのバージョンに注意してください ([こちらに記載されています](azure-stack-linux.md#azure-linux-agent))。 sysprep されたイメージに、Azure Stack Hub と互換性のある Azure Linux エージェントのバージョンが含まれていることを確認してください。

2. VM を停止 - 割り当て解除します。

3. VHD をダウンロードします。

   1. VHD ファイルをダウンロードするには、共有アクセス署名 (SAS) URL を生成する必要があります。 URL が生成されると、その URL に有効期限が割り当てられます。

   1. VM についてのブレードのメニューで、 **[ディスク]** を選択します。

   1. VM 用のオペレーティング システム ディスクを選択して、 **[ディスクのエクスポート]** を選択します。

   1. URL の有効期限を 36000 に設定します。

   1. **[URL の生成]** を選択します。

   1. URL を生成します。

   1. 生成された URL の下にある **[VHD ファイルのダウンロード]** を選択します。

   1. ダウンロードを開始するのに、ブラウザーで **[保存]** を選択する必要がある場合があります。 VHD ファイルの既定の名前は _abcd_ です。

### <a name="considerations"></a>考慮事項

イメージをアップロードする前に、以下を考慮する必要があります。

- Azure Stack Hub では、VHD フォーマットの固定ディスク内で第 1 世代の VM のみサポートされます。 固定フォーマットの場合、ファイル内で論理ディスクが線形に構成されるため、ディスク オフセット *X* は BLOB オフセット *X* に格納されます。BLOB 末尾の小さなフッターに、VHD のプロパティが記述されます。 ディスクが固定フォーマットであるかどうかを確認するには、**Get-VHD** PowerShell コマンドレットを使用します。

- Azure Stack Hub では、ダイナミック ディスク VHD はサポートされていません。

## <a name="step-2-upload-the-vm-image-to-a-storage-account"></a>手順 2:ストレージ アカウントに VM イメージをアップロードする

1. [PowerShell for Azure Stack Hub をインストールします](azure-stack-powershell-install.md)。  

2. Azure Stack Hub にオペレーターとしてサインインします。 手順については、[オペレーターとしての Azure Stack Hub へのサインイン](azure-stack-powershell-configure-admin.md)に関するページをご覧ください。

3. イメージは、Blob Storage URI で参照できなければなりません。 Windows または Linux オペレーティング システムのイメージを (VHDX ではなく) VHD 形式で準備してから、そのイメージを Azure Stack Hub のストレージ アカウントにアップロードします。

   - VHD が Azure 内にある場合、接続された Azure Stack Hub 上で実行していれば、[Azcopy](/azure/storage/common/storage-use-azcopy) などのツールを使用して、Azure と自分の Azure Stack Hub ストレージ アカウントの間で VHD を直接転送できます。

   - 切断された Azure Stack Hub 上で、VHD が Azure 内にある場合は、Azure と Azure Stack Hub の両方に接続されているマシンに VHD をダウンロードする必要があります。 その後、VHD を Azure からこのマシンにコピーしてから、Azure と Azure Stack Hub 間で使用できる一般的な[ストレージ データ転送ツール](../user/azure-stack-storage-transfer.md)を使用して Azure Stack Hub に VHD を転送します。

     この例で使用するそうしたツールの 1 つは、Azure Stack Hub 管理者ポータルのストレージ アカウントに VHD をアップロードする Add-AzureRmVHD コマンドです。  

     ```powershell
     Add-AzureRmVhd -Destination "https://bash.blob.redmond.azurestack.com/sample/vhdtestingmgd.vhd" -LocalFilePath "C:\vhd\vhdtestingmgd.vhd"
     ```

4. イメージのアップロード先の Blob Storage URI をメモしておきます。 Blob Storage URI の形式は *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;* .vhd です。

5. BLOB に匿名でアクセスできるようにするには、VM イメージ VHD がアップロードされたストレージ アカウントの BLOB コンテナーに移動します。 **[BLOB]** を選択し、 **[アクセス ポリシー]** を選択してください。 必要であれば、このコンテナーの Shared Access Signature を生成し、それを BLOB URI に含めることもできます。 この手順により、確実に BLOB が利用できるようになります。 BLOB に匿名でアクセスできない場合、VM イメージがエラー状態で作成されます。

   ![ストレージ アカウント BLOB に移動](./media/azure-stack-add-vm-image/tca1.png)

   ![BLOB のアクセスを公開に設定](./media/azure-stack-add-vm-image/tca2.png)

   ![BLOB のアクセスを公開に設定](./media/azure-stack-add-vm-image/tca3.png)

## <a name="step-3-option-1-add-the-vm-image-as-an-azure-stack-hub-operator-using-the-portal"></a>手順 3、オプション 1:Azure Stack Hub オペレーターとしてポータルを使用して VM イメージを追加する

1. Azure Stack Hub にオペレーターとしてサインインします。 メニューで、 **[VM イメージ]**  >  **[追加]** の下の **[すべてのサービス]**  >  **[コンピューティング]** を選択します。

   ![カスタム イメージ サイドローディング UI](./media/azure-stack-add-vm-image/tca4.png)

2. **[イメージの作成]** で、[Publisher] (発行元)、[Offer] (オファー)、[SKU]、[バージョン]、および [OS disk blob URI] (OS ディスクの BLOB URI) に入力します。 その後 **[作成]** をクリックすると VM イメージの作成が開始されます。

   ![カスタム イメージ サイドローディング UI](./media/azure-stack-add-vm-image/tca5.png)

   イメージが正常に作成されると、VM イメージの状態が **[成功]** に変わります。

3. イメージを追加すると、そのイメージは Azure Resource Manager ベースのテンプレートと PowerShell のデプロイのみで使用できるようになります。 イメージを Marketplace 項目としてユーザーが使用できるようにするには、「[Marketplace アイテムを作成および発行する](azure-stack-create-and-publish-marketplace-item.md)」の手順を使用して Marketplace 項目を公開してください。 **Publisher**、**Offer**、**SKU**、**Version** の値を書き留めておいてください。 これらは、カスタム .azpkg 内の Resource Manager テンプレートと Manifest.json を編集するときに必要になります。

## <a name="step-3-option-2-add-a-vm-image-as-an-azure-stack-hub-operator-using-powershell"></a>手順 3、オプション 2:Azure Stack Hub オペレーターとして PowerShell を使用して VM イメージを追加する

1. [PowerShell for Azure Stack Hub をインストールします](azure-stack-powershell-install.md)。  

2. Azure Stack Hub にオペレーターとしてサインインします。 手順については、[オペレーターとしての Azure Stack Hub へのサインイン](azure-stack-powershell-configure-admin.md)に関するページをご覧ください。

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
     例: `Canonical`  
     イメージをデプロイするときにユーザーが使用する VM イメージの**発行元**名のセグメント。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **offer**  
     例: `UbuntuServer`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージの**オファー**名のセグメント。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **sku**  
     例: `14.04.3-LTS`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージの **SKU** 名のセグメント。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **version**  
     例: `1.0.0`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージのバージョン。 このバージョンの形式は *\#.\#.\#* です。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **osType**  
     例: `Linux`  
     イメージの **osType** には **Windows** または **Linux** を指定する必要があります。  
   - **OSUri**  
     例: `https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
     `osDisk` の Blob Storage URI を指定できます。  

     詳細については、[Add-AzsPlatformimage](/powershell/module/azs.compute.admin/add-azsplatformimage) コマンドレットの PowerShell リファレンスを参照してください。

4. イメージを追加すると、そのイメージは Azure Resource Manager ベースのテンプレートと PowerShell のデプロイのみで使用できるようになります。 イメージを Marketplace 項目としてユーザーが使用できるようにするには、「[Marketplace アイテムを作成および発行する](azure-stack-create-and-publish-marketplace-item.md)」の手順を使用して Marketplace 項目を公開してください。 **Publisher**、**Offer**、**SKU**、**Version** の値を書き留めておいてください。 これらは、カスタムの .azpkg でリソース マネージャー テンプレートと Manifest.json を編集するときに必要になります。

## <a name="remove-the-vm-image-as-an-azure-stack-hub-operator-using-the-portal"></a>Azure Stack Hub オペレーターとしてポータルを使用して VM イメージを削除する

1. [Azure Stack Hub 管理者ポータル](https://adminportal.local.azurestack.external)を開きます。

2. VM イメージに Marketplace 項目が関連付けられている場合は、 **[Marketplace management]\(Marketplace 管理\)** を選択し、削除する VM Marketplace 項目を選択します。

3. VM イメージに Marketplace 項目が関連付けられていない場合は、 **[すべてのサービス] > [コンピューティング] > [VM イメージ]** に移動し、VM イメージの横にある省略記号 ( **...** ) を選択します。

4. **[削除]** を選択します。

## <a name="remove-a-vm-image-as-an-azure-stack-hub-operator-using-powershell"></a>Azure Stack Hub オペレーターとして PowerShell を使用して VM イメージを削除する

過去にアップロードした VM イメージが必要なくなった場合は、次のコマンドレットを使用して Marketplace から削除できます。

1. [PowerShell for Azure Stack Hub をインストールします](azure-stack-powershell-install.md)。

2. Azure Stack Hub にオペレーターとしてサインインします。

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
     例: `Canonical`  
     イメージをデプロイするときにユーザーが使用する VM イメージの**発行元**名のセグメント。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **offer**  
     例: `UbuntuServer`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージの**オファー**名のセグメント。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **sku**  
     例: `14.04.3-LTS`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージの **SKU** 名のセグメント。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **version**  
     例: `1.0.0`  
     VM イメージをデプロイするときにユーザーが使用する VM イメージのバージョン。 このバージョンの形式は *\#.\#.\#* です。 このフィールドではスペースや他の特殊文字は使用できません。  

     **Remove-AzsPlatformImage** コマンドレットの詳細については、Microsoft PowerShell の [Azure Stack Hub オペレーター モジュールのドキュメント](/powershell/module/)を参照してください。

## <a name="next-steps"></a>次のステップ

- [Azure Stack Hub Marketplace のカスタム アイテムを作成して発行する](azure-stack-create-and-publish-marketplace-item.md)
- [仮想マシンのプロビジョニング](../user/azure-stack-create-vm-template.md)
