---
title: Azure Stack Hub にカスタム VM イメージを追加する
description: Azure Stack Hub にカスタム VM イメージを追加または削除する方法について説明します。
author: sethmanheim
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: sethm
ms.reviewer: kivenkat
ms.lastreviewed: 07/10/2020
ms.openlocfilehash: 71b097b46d388371ef997288f43474873d92af85
ms.sourcegitcommit: cdfc7bf5fee5e6cb4c531ba58d23a83c36be7de4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/15/2020
ms.locfileid: "88251410"
---
# <a name="add-a-custom-vm-image-to-azure-stack-hub"></a>Azure Stack Hub にカスタム VM イメージを追加する

Azure Stack Hub では、オペレーターとして、カスタム仮想マシン (VM) のイメージをマーケットプレースに追加して、ユーザーに提供することができます。 VM イメージは、管理者ポータルまたは Windows PowerShell を介して Azure Stack Hub Marketplace に追加できます。 グローバル Azure Marketplace のイメージをカスタム イメージのベースとして使用するか、Hyper-V を使用して独自のイメージを作成します。

Azure Stack Hub のテナント ポータルのユーザーが、手順 1 のガイダンスに従って、カスタム仮想マシン (VM) のイメージを追加することもできます。 ユーザーは、カスタム イメージを仮想ハードディスク (VHD) として作成し、そのイメージを Azure Stack Hub のストレージ アカウントにアップロードできます。 その VHD から VM を作成することができます。

カスタム イメージには、**汎用化**と**特殊化**の 2 つの形式があります。

- **汎用化イメージ**

  汎用化ディスク イメージとは、ユーザー アカウントなどの固有の情報を削除するために **Sysprep** を使用して準備されたもので、これを再利用して複数の VM を作成することができます。 これは、Marketplace 項目に適したオプションです。

- **特殊化イメージ**

  特殊化ディスク イメージとは、既存の VM の仮想ハードディスク (VHD) のコピーで、元の VM のユーザー アカウント、アプリケーション、その他の状態データが含まれます。 これは通常、VM を Azure Stack Hub に移行するときに使用する形式です。

## <a name="step-1-create-the-custom-vm-image"></a>手順 1:カスタム VM イメージを作成する

### <a name="windows---create-a-custom-generalized-vhd"></a>Windows - カスタム汎用化 VHD を作成する

#### <a name="vhd-is-from-outside-azure"></a>VHD が Azure の外部からのものである

VHD をアップロードする前に、「[Azure にアップロードする Windows VHD または VHDX を準備する](/azure/virtual-machines/windows/prepare-for-upload-vhd-image)」の手順に従って正しく汎用化してください。

#### <a name="vhd-is-from-azure"></a>VHD が Azure からのものである

VM を汎用化する前に、次のことを確認してください。

Azure Stack 1910 リリースより前:

- Azure 上で VM をプロビジョニングする場合は、PowerShell を使用し、`-ProvisionVMAgent` フラグなしでそれをプロビジョニングします。
- Azure で VM を汎用化する前に、VM から **Remove-AzureRmVMExtension** コマンドレットを使用してすべての VM 拡張機能を削除します。 `Windows (C:) > WindowsAzure > Logs > Plugins` に移動することにより、どの VM 拡張機能がインストールされているかを見つけることができます。

```powershell
Remove-AzureRmVMExtension -ResourceGroupName winvmrg1 -VMName windowsvm -Name "CustomScriptExtension"
```

Azure Stack 1910 リリース以降:

- 上記の手順は、Azure から 1910 リリース以降の Azure Stack Hub に移行した VHD には適用されません。

VHD を Azure Stack Hub に移植する前に、[この記事](/azure/virtual-machines/windows/download-vhd)の手順に従ってその VHD を正しく汎用化してダウンロードします。

### <a name="windows---specialized"></a>Windows - 特殊化

[こちら](/azure/virtual-machines/windows/create-vm-specialized#prepare-the-vm)の手順を行って、VHD を正しく準備します。
VM 拡張機能をデプロイするには、VM をデプロイする前に、[こちらの記事](/azure/virtual-machines/extensions/agent-windows#manual-installation)で入手できる VM エージェント .msi が VM にインストールされていることを確認します。 VM エージェントが VHD に存在しない場合、拡張機能のデプロイは失敗します。 プロビジョニング中に OS プロファイルを設定したり、`$vm.OSProfile.AllowExtensionOperations = $true` を設定したりする必要はありません。

### <a name="linux---generalized"></a>Linux - 汎用化

#### <a name="vhd-from-outside-azure"></a>VHD が Azure の外部からのものである

VHD が Azure の外部からのものである場合は、適切な手順に従って VHD を汎用化します。

- [CentOS ベースのディストリビューション](/azure/virtual-machines/linux/create-upload-centos?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Debian Linux](/azure/virtual-machines/linux/debian-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
- [SLES または openSUSE](/azure/virtual-machines/linux/suse-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Ubuntu Server](/azure/virtual-machines/linux/create-upload-ubuntu?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

#### <a name="vhd-from-azure"></a>VHD が Azure からのものである

VHD が Azure からのものである場合は、次の手順に従って VHD を汎用化し、ダウンロードします。

1. **waagent** サービスを停止します。

   ```bash
   sudo waagent -force -deprovision
   export HISTSIZE=0
   logout
   ```

   Azure Stack Hub で動作する Azure Linux エージェントのバージョンは[こちらに記載されています](azure-stack-linux.md#azure-linux-agent)。 sysprep されたイメージに、Azure Stack Hub と互換性のある Azure Linux エージェントのバージョンが含まれていることを確認してください。

2. VM を停止 - 割り当て解除します。

3. VHD をダウンロードします。

   1. VHD ファイルをダウンロードするには、共有アクセス署名 (SAS) URL を生成します。 URL が生成されると、その URL に有効期限が割り当てられます。

   1. VM についてのブレードのメニューで、 **[ディスク]** を選択します。

   1. VM 用のオペレーティング システム ディスクを選択して、 **[ディスクのエクスポート]** を選択します。

   1. URL の有効期限を 36000 に設定します。

   1. **[URL の生成]** を選択します。

   1. URL を生成します。

   1. 生成された URL の下にある **[VHD ファイルのダウンロード]** を選択します。

   1. ダウンロードを開始するのに、ブラウザーで **[保存]** を選択する必要がある場合があります。 VHD ファイルの既定の名前は **abcd** です。

   1. この VHD を Azure Stack Hub にポートできるようになりました。

> [!IMPORTANT]  
> 「[VHD を Azure にアップロードし新しい VM を作成するサンプル スクリプト](/azure/virtual-machines/scripts/virtual-machines-windows-powershell-upload-generalized-script)」にあるスクリプトを使用して、Azure Stack Hub ユーザー ストレージ アカウントに VHD をアップロードして VM を作成できます。 Azure Stack Hub ストレージ アカウントとコンテナーの URL として `$urlOfUploadedImageVhd` を指定してください。 汎用化 VHD の場合は、`-CreateOption FromImage` を設定するときに `FromImage` 値を使用するようにしてください。

### <a name="linux---specialized"></a>Linux - 特殊化

特殊化 VHD は、Marketplace 項目のベース VHD として使用しないでください。 これには、汎用化 VHD を使用します。 一方、オンプレミスから Azure Stack Hub に VM を移行する必要がある場合は、特殊化 VHD を使用することをお勧めします。

#### <a name="vhd-from-outside-azure"></a>VHD が Azure の外部からのものである

手順 1:適切な指示に従って、VHD を Azure に適したものにします。 この記事の Linux エージェントをインストールする手順まで実行してから、エージェントをインストールする前に手順 2 に進みます。

- [CentOS ベースのディストリビューション](/azure/virtual-machines/linux/create-upload-centos?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Debian Linux](/azure/virtual-machines/linux/debian-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
- [SLES または openSUSE](/azure/virtual-machines/linux/suse-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Ubuntu Server](/azure/virtual-machines/linux/create-upload-ubuntu?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

> [!IMPORTANT]
> VHD が汎用化されるため、最後の手順 (`sudo waagent -force -deprovision`) は実行しないでください。

手順 2:Linux の特殊化 VHD を Azure の外部から Azure Stack Hub に取り込む場合、VM 拡張機能を実行してプロビジョニングを無効にするには、次の手順を行います。

ソース VM イメージにインストールされている Linux エージェントのバージョンを特定するには、次のコマンドを実行します。 プロビジョニング コードを説明するバージョン番号は、`Goal state agent` ではなく `WALinuxAgent-` です。

```bash
waagent -version
```

次に例を示します。

```bash
waagent -version
WALinuxAgent-2.2.45 running on centos 7.7.1908
Python: 2.7.5
Goal state agent: 2.2.46
```

2\.2.4 よりも前の Linux エージェントで Linux エージェントのプロビジョニングを無効にするには、 **/etc/waagent.conf** に次のパラメーターを設定します: `Provisioning.Enabled=n, and Provisioning.UseCloudInit=n`。

拡張機能を実行するシナリオでは、次のようにします。

1. **/etc/waagent.conf** に次のパラメーターを設定します。

   - `Provisioning.Enabled=n`
   - `Provisioning.UseCloudInit=n`

2. walinuxagent のプロビジョニングを確実に無効にするには、次を実行します: `mkdir -p /var/lib/waagent && touch /var/lib/waagent/provisioned`

3. イメージに cloud-init がある場合は、クラウドの初期化を無効にします。

   ```bash
   touch /etc/cloud/cloud-init.disabled
   sudo sed -i '/azure_resource/d' /etc/fstab
   ```

4. ログアウトを実行します。

2\.2.45 以降の Linux エージェントでプロビジョニングを無効にするには、次の構成オプションを変更します。

- `Provisioning.Enabled` と `Provisioning.UseCloudInit` が無視されるようにする。

このバージョンでは現在、プロビジョニングを完全に無効にする `Provisioning.Agent` オプションはありません。ただし、プロビジョニング マーカー ファイルを追加することはでき、次の設定を行うと、プロビジョニングが無視されます。

1. **/etc/waagent.conf** で、次の構成オプションを追加します: `Provisioning.Agent=Auto`。
2. walinuxagent のプロビジョニングを確実に無効にするには、次を実行します: `mkdir -p /var/lib/waagent && touch /var/lib/waagent/provisioned`。
3. 次を実行して、クラウドの初期化のインストールを無効にします。

   ```bash
   touch /etc/cloud/cloud-init.disabled
   sudo sed -i '/azure_resource/d' /etc/fstab
   ```

4. ログアウトします。

#### <a name="vhd-from-azure"></a>VHD が Azure からのものである

[こちらのガイダンス](/azure/virtual-machines/linux/upload-vhd#requirements)を使用して、VHD を準備できます。

> [!IMPORTANT]  
> 次の PowerShell の例を使用して、VHD を Azure Stack Hub ユーザーのストレージ アカウントにアップロードできます。

```powershell  
# Provide values for the variables
$resourceGroup = 'myResourceGroup'
$location = 'Orlando'
$storageaccount = 'mystorageaccount'
$storageType = 'Standard_LRS'
$storageaccounturl = 'https://resourcegrpabc.blob.orlando.azurestack.corp.microsoft.com/container'
$localPath = 'C:\Users\Public\Documents\Hyper-V\VHDs\generalized.vhd'
$vhdName = 'myUploadedVhd.vhd'

New-AzResourceGroup -Name $resourceGroup -Location $location
New-AzStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccount -Location $location `
  -SkuName $storageType -Kind "Storage"
$urlOfUploadedImageVhd = ($storageaccounturl + '/' + $vhdName)
Add-AzVhd -ResourceGroupName $resourceGroup -Destination $urlOfUploadedImageVhd `
    -LocalFilePath $localPath
```

特殊化 VHD の場合、この VHD から VM を作成するには、[こちらの例](/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks)のように `-CreateOption Attach` を使用して "アタッチ" セマンティックを使用してください。

### <a name="considerations"></a>考慮事項

イメージをアップロードする前に、以下を考慮する必要があります。

- Azure Stack Hub では、VHD フォーマットの固定ディスク内で第 1 世代の VM のみサポートされます。 固定フォーマットの場合、ファイル内で論理ディスクが線形に構成されるため、ディスク オフセット **X** は BLOB オフセット **X** に格納されます。BLOB 末尾の小さなフッターに、VHD のプロパティが記述されます。 ディスクが固定フォーマットであるかどうかを確認するには、**Get-VHD** PowerShell コマンドレットを使用します。

- Azure Stack Hub では、ダイナミック ディスク VHD はサポートされていません。

## <a name="step-2-upload-a-storage-account"></a>手順 2:ストレージ アカウントをアップロードする

Azure Stack Hub オペレーターとして、VM イメージをストレージ アカウントにアップロードします。

1. [PowerShell for Azure Stack Hub をインストールします](azure-stack-powershell-install.md)。  

2. Azure Stack Hub にオペレーターとしてサインインします。 手順については、[オペレーターとしての Azure Stack Hub へのサインイン](azure-stack-powershell-configure-admin.md)に関するページをご覧ください。

3. イメージは、Blob Storage URI で参照される必要があります。 Windows または Linux オペレーティング システムのイメージを (VHDX ではなく) VHD 形式で準備してから、そのイメージを Azure Stack Hub のストレージ アカウントにアップロードします。

   - VHD が Azure 内にある場合、接続された Azure Stack Hub 上で実行していれば、[Azcopy](/azure/storage/common/storage-use-azcopy) などのツールを使用して、Azure と自分の Azure Stack Hub ストレージ アカウントの間で VHD を直接転送できます。

   - 切断された Azure Stack Hub 上で、VHD が Azure 内にある場合は、Azure と Azure Stack Hub の両方に接続されているマシンに VHD をダウンロードする必要があります。 次に、VHD を Azure からこのマシンにコピーした後、Azure と Azure Stack Hub 間で使用できる一般的な[ストレージ データ転送ツール](../user/azure-stack-storage-transfer.md)を使用して Azure Stack Hub に VHD を転送します。

     この例で使用するそうしたツールの 1 つは、Azure Stack Hub 管理者ポータルのストレージ アカウントに VHD をアップロードする **Add-AzureRmVHD** コマンドです。

     ```powershell
     Add-AzureRmVhd -Destination "https://bash.blob.redmond.azurestack.com/sample/vhdtestingmgd.vhd" -LocalFilePath "C:\vhd\vhdtestingmgd.vhd"
     ```

4. イメージのアップロード先の Blob Storage URI をメモしておきます。 Blob ストレージ URI の形式は、 **&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;*.vhd** です。

5. BLOB に匿名でアクセスできるようにするには、VM イメージ VHD がアップロードされたストレージ アカウントの BLOB コンテナーに移動します。 **[BLOB]** を選択し、 **[アクセス ポリシー]** を選択してください。 必要に応じて、このコンテナーの Shared Access Signature (SAS) を生成し、それを BLOB URI に含めることもできます。 この手順により、確実に BLOB が利用できるようになります。 BLOB に匿名でアクセスできない場合、VM イメージがエラー状態で作成されます。

   ![ストレージ アカウント BLOB に移動](./media/azure-stack-add-vm-image/tca1.png)

   ![BLOB のアクセスを公開に設定](./media/azure-stack-add-vm-image/tca2.png)

   ![BLOB のアクセスを公開に設定](./media/azure-stack-add-vm-image/tca3.png)

6. 前の手順を使用して、(ユーザーとしてサインインして) ユーザー ポータルのストレージ アカウントにイメージをアップロードし、そこから直接 VM を作成することもできます。 この場合、マーケットプレースからは入手できないカスタム VHD になります。 また、手順 3 も実行する必要はありません。

## <a name="step-3-option-1-add-using-the-portal"></a>手順 3、オプション 1:ポータルを使用して追加する

Azure Stack Hub オペレーターとしてポータルを使用して VM イメージを追加します。

1. Azure Stack Hub にオペレーターとしてサインインします。 メニューで、 **[VM イメージ]**  >  **[追加]** の下の **[すべてのサービス]**  >  **[コンピューティング]** を選択します。

   ![カスタム イメージ サイドローディング UI](./media/azure-stack-add-vm-image/tca4.png)

2. **[イメージの作成]** で **[発行元]** 、 **[オファー]** 、 **[SKU]** 、 **[バージョン]** 、[OS ディスクの BLOB URI] に入力します。 その後 **[作成]** をクリックすると VM イメージの作成が開始されます。

   ![カスタム イメージ サイドローディング UI](./media/azure-stack-add-vm-image/tca5.png)

   イメージが正常に作成されると、VM イメージの状態が **[成功]** に変わります。

3. イメージを追加すると、そのイメージは Azure Resource Manager ベースのテンプレートと PowerShell のデプロイのみで使用できるようになります。 イメージを Marketplace 項目としてユーザーが使用できるようにするには、「[Marketplace アイテムを作成および発行する](azure-stack-create-and-publish-marketplace-item.md)」の手順を使用して Marketplace 項目を公開してください。 **Publisher**、**Offer**、**SKU**、**Version** の値を書き留めておいてください。 これらは、カスタム .azpkg 内の Resource Manager テンプレートと Manifest.json を編集するときに必要になります。

## <a name="step-3-option-2-add-using-powershell"></a>手順 3、オプション 2:PowerShell を使用して追加する

 Azure Stack Hub オペレーターとして PowerShell を使用して VM イメージを追加します。

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
     VM イメージをデプロイするときにユーザーが使用する VM イメージのバージョン。 このバージョンの形式は **\#.\#.\#** です。 このフィールドではスペースや他の特殊文字は使用できません。  
   - **osType**  
     例: `Linux`  
     イメージの **osType** には **Windows** または **Linux** を指定する必要があります。  
   - **OSUri**  
     例: `https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
     `osDisk` の Blob Storage URI を指定できます。  

     詳細については、[Add-AzsPlatformimage](/powershell/module/azs.compute.admin/add-azsplatformimage) コマンドレットの PowerShell リファレンスを参照してください。

4. イメージを追加すると、そのイメージは Azure Resource Manager ベースのテンプレートと PowerShell のデプロイのみで使用できるようになります。 イメージを Marketplace 項目としてユーザーが使用できるようにするには、「[Marketplace アイテムを作成および発行する](azure-stack-create-and-publish-marketplace-item.md)」の手順を使用して Marketplace 項目を公開してください。 **Publisher**、**Offer**、**SKU**、**Version** の値を書き留めておいてください。 これらは、カスタム .azpkg 内の Resource Manager テンプレートと Manifest.json を編集するときに必要になります。

## <a name="remove-using-the-portal"></a>ポータルを使用して削除する

Azure Stack Hub オペレーターとして Azure Stack Hub ポータルを使用して VM イメージを削除するには、次の手順を行います。

1. [Azure Stack Hub 管理者ポータル](https://portal.azure.com/signin/index)を開きます。

2. VM イメージに Marketplace 項目が関連付けられている場合は、 **[Marketplace management]\(Marketplace 管理\)** を選択し、削除する VM Marketplace 項目を選択します。

3. VM イメージに Marketplace 項目が関連付けられていない場合は、 **[すべてのサービス] > [コンピューティング] > [VM イメージ]** に移動し、VM イメージの横にある省略記号 ( **...** ) を選択します。

4. **[削除]** を選択します。

## <a name="remove-using-powershell"></a>PowerShell を使用して削除する

Azure Stack Hub オペレーターとして PowerShell を使用して VM イメージを削除するには、次の手順を行います。

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
     VM イメージをデプロイするときにユーザーが使用する VM イメージのバージョン。 このバージョンの形式は **\#.\#.\#** です。 このフィールドではスペースや他の特殊文字は使用できません。  

     **Remove-AzsPlatformImage** コマンドレットの詳細については、Microsoft PowerShell の [Azure Stack Hub オペレーター モジュールのドキュメント](/powershell/azure/azure-stack/overview)を参照してください。

## <a name="next-steps"></a>次のステップ

- [Azure Stack Hub Marketplace のカスタム アイテムを作成して発行する](azure-stack-create-and-publish-marketplace-item.md)
- [仮想マシンのプロビジョニング](../user/azure-stack-create-vm-template.md)
