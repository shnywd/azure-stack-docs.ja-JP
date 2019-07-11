---
title: Azure Stack のマネージド ディスクとマネージド イメージに関する相違点と考慮事項 | Microsoft Docs
description: Azure Stack でマネージド ディスクとマネージド イメージを操作する際の相違点と考慮事項について説明します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2019
ms.author: sethm
ms.reviewer: jiahan
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: 2e7737033b0f94473f4b794e452c9239f275c8f8
ms.sourcegitcommit: d1fdecdfa843dfc0629bfc226f1baf14f3ea621d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2019
ms.locfileid: "67387743"
---
# <a name="azure-stack-managed-disks-differences-and-considerations"></a>Azure Stack マネージド ディスク: 相違点と考慮事項

この記事は、[Azure Stack のマネージド ディスク](azure-stack-manage-vm-disks.md)と [Azure のマネージド ディスク](/azure/virtual-machines/windows/managed-disks-overview)の既知の相違点をまとめたものです。 Azure Stack と Azure の違いの概要については、「[主な考慮事項](azure-stack-considerations.md)」をご覧ください。

マネージド ディスクを使用すると、VM ディスクに関連付けられている[ストレージ アカウント](../operator/azure-stack-manage-storage-accounts.md)を管理できるため、IaaS VM のディスク管理が簡素化されます。

> [!NOTE]  
> Azure Stack のマネージド ディスクは 1808 更新プログラムから使用できます。 1811 更新プログラム以降で Azure Stack ポータルを使用して仮想マシンを作成すると、既定で有効になります。
  
## <a name="cheat-sheet-managed-disk-differences"></a>チート シート: マネージド ディスクの相違点

| 機能 | Azure (グローバル) | Azure Stack |
| --- | --- | --- |
|保存データの暗号化 |Azure Storage Service Encryption (SSE)、Azure Disk Encryption (ADE)     |BitLocker 128 ビット AES 暗号化      |
|Image          | マネージド カスタム イメージ |サポートされています|
|バックアップ オプション | Azure Backup サービス |まだサポートされていません |
|ディザスター リカバリーのオプション | Azure Site Recovery |まだサポートされていません|
|ディスクの種類     |Premium SSD、Standard SSD、および Standard HDD |Premium SSD、Standard HDD |
|Premium Disk  |完全にサポートされます |プロビジョニング可能ですがパフォーマンス制限や保証がありません  |
|Premium Disk の IOP  |ディスク サイズによって異なります  |ディスクあたりの 2300 IOP |
|Premium ディスクのスループット |ディスク サイズによって異なります |ディスクあたり 145 MB/秒 |
|ディスク サイズ  |Azure Premium ディスク:P4 (32 GiB) ～ P80 (32 TiB)<br>Azure Standard SSD ディスク:E10 (128 GiB) ～ E80 (32 TiB)<br>Azure Standard HDD ディスク:S4 (32 GiB) ～ S80 (32 TiB) |M4:32 GiB<br>M6:64 GiB<br>M10:128 GiB<br>M15:256 GiB<br>M20:512 GiB<br>M30:1,024 GiB |
|ディスクのスナップショット コピー|実行中の VM にアタッチされている Azure のマネージド ディスクのスナップショットはサポートされています|まだサポートされていません |
|ディスクのパフォーマンス分析 |サポートされる合計メトリックおよびディスクあたりのメトリック |まだサポートされていません |
|移行      |VM を再作成する必要なしに既存のアンマネージド Azure Resource Manager VM から移行するためのツールが提供されます  |まだサポートされていません |

> [!NOTE]  
> Azure Stack のマネージド ディスクの IOP とスループットは、プロビジョニングされた数値に代わりの上限数です。これは Azure Stack で実行されているハードウェアとワークロードによって影響を受ける可能性があります。

## <a name="metrics"></a>メトリック

ストレージ メトリックにも相違点があります。

- Azure Stack では、ストレージ メトリックのトランザクション データで、内部と外部のネットワーク帯域幅は区別されません。
- ストレージ メトリックの Azure Stack トランザクション データには、マウントされたディスクへの仮想マシンのアクセスは含まれません。

## <a name="api-versions"></a>API のバージョン

Azure Stack のマネージド ディスクは次の API バージョンをサポートしています。

- 2017-03-30
- 2017-12-01

## <a name="convert-to-managed-disks"></a>マネージド ディスクへの変換

> [!NOTE]  
> Azure Stack でアンマネージド ディスクをマネージド ディスクに変更する場合に、Azure PowerShell コマンドレット **ConvertTo-AzureRmVMManagedDisk** は使用できません。 Azure Stack では、このコマンドレットはまだサポートされません。

次のスクリプトを使用して、現在プロビジョニングされている VM をアンマネージド ディスクからマネージド ディスクに変換できます。 プレースホルダーを実際の値に置き換えてください。

```powershell
$SubscriptionId = "SubId"

# The name of your resource group where your VM to be converted exists.
$ResourceGroupName ="MyResourceGroup"

# The name of the managed disk to be created.
$DiskName = "mngddisk"

# The size of the disks in GB. It should be greater than the VHD file size.
$DiskSize = "50"

# The URI of the VHD file that will be used to create the managed disk.
# The VHD file can be deleted as soon as the managed disk is created.
$VhdUri = "https://rgmgddisks347.blob.local.azurestack.external/vhds/unmngdvm20181109013817.vhd"

# The storage type for the managed disk: PremiumLRS or StandardLRS.
$AccountType = "StandardLRS"

# The Azure Stack location where the managed disk will be located.
# The location should be the same as the location of the storage account in which VHD file is stored.
# Configure the new managed VM point to the old unmanaged VM configuration (network config, VM name, location).
$Location = "local"
$VirtualMachineName = "unmngdvm"
$VirtualMachineSize = "Standard_D1"
$PIpName = "unmngdvm-ip"
$VirtualNetworkName = "unmngdrg-vnet"
$NicName = "unmngdvm"

# Set the context to the subscription ID in which the managed disk will be created.
Select-AzureRmSubscription -SubscriptionId $SubscriptionId

# Delete old VM, but keep the OS disk.
Remove-AzureRmVm -Name $VirtualMachineName -ResourceGroupName $ResourceGroupName

# Create the managed disk configuration.
$DiskConfig = New-AzureRmDiskConfig -AccountType $AccountType -Location $Location -DiskSizeGB $DiskSize -SourceUri $VhdUri -CreateOption Import

# Create managed disk.
New-AzureRmDisk -DiskName $DiskName -Disk $DiskConfig -ResourceGroupName $resourceGroupName
$Disk = Get-AzureRmDisk -DiskName $DiskName -ResourceGroupName $ResourceGroupName
$VirtualMachine = New-AzureRmVMConfig -VMName $VirtualMachineName -VMSize $VirtualMachineSize

# Use the managed disk resource ID to attach it to the virtual machine.
# Change the OS type to "-Windows" if the OS disk has the Windows OS.
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $Disk.Id -CreateOption Attach -Linux

# Create a public IP for the VM.
$PublicIp = Get-AzureRmPublicIpAddress -Name $PIpName -ResourceGroupName $ResourceGroupName

# Get the virtual network where the virtual machine will be hosted.
$VNet = Get-AzureRmVirtualNetwork -Name $VirtualNetworkName -ResourceGroupName $ResourceGroupName

# Create NIC in the first subnet of the virtual network.
$Nic = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $ResourceGroupName

$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Nic.Id

# Create the virtual machine with managed disk.
New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $ResourceGroupName -Location $Location
```

## <a name="managed-images"></a>マネージド イメージ

Azure Stack では*マネージド イメージ*がサポートされています。これにより、汎用化された VM (アンマネージドとマネージドの両方) でマネージド イメージ オブジェクトを作成できます。以降、このオブジェクトではマネージド ディスク VM のみを作成できます。 マネージド イメージにより、次の 2 つのシナリオが可能になります。

- 汎用化されたアンマネージド VM があり、今後はマネージド ディスクを使用する。
- 汎用化されたマネージド VM があり、同様のマネージド VM を複数作成する。

### <a name="step-1-generalize-the-vm"></a>手順 1:VM の汎用化

Windows の場合は、「[Sysprep を使用して Windows VM を一般化する](/azure/virtual-machines/windows/capture-image-resource#generalize-the-windows-vm-using-sysprep)」のセクションに従います。 Linux の場合は、手順 1 ([こちら](/azure/virtual-machines/linux/capture-image#step-1-deprovision-the-vm)) に従います。

> [!NOTE]
> 必ず VM を一般化してください。 適切に一般化されていないイメージから VM を作成すると、**VMProvisioningTimeout** エラーが発生します。

### <a name="step-2-create-the-managed-image"></a>手順 2:マネージド イメージを作成する

ポータル、PowerShell、または CLI を使用してマネージド イメージを作成できます。 [マネージド イメージを作成する](/azure/virtual-machines/windows/capture-image-resource)手順に従います。

### <a name="step-3-choose-the-use-case"></a>手順 3:ユース ケースを選択する

#### <a name="case-1-migrate-unmanaged-vms-to-managed-disks"></a>ケース 1:アンマネージド VM をマネージド ディスクに移行する

この手順を実行する前に、VM を適切に一般化してください。 一般化の後ではその VM を使用できなくなります。 適切に一般化されていないイメージから VM を作成すると、**VMProvisioningTimeout** エラーが発生します。

[ストレージ アカウントで VHD からイメージを作成する](/azure/virtual-machines/windows/capture-image-resource#create-an-image-from-a-vhd-in-a-storage-account)手順に従って、ストレージ アカウントで汎用化された VHD からマネージド イメージを作成します。 以降、このイメージを使用して、マネージド VM を作成します。

#### <a name="case-2-create-managed-vm-from-managed-image-using-powershell"></a>ケース 2:Powershell を使用してマネージド イメージからマネージド VM を作成する

「[PowerShell を使ってマネージド ディスクからイメージを作成する](/azure/virtual-machines/windows/capture-image-resource#create-an-image-from-a-managed-disk-using-powershell)」のスクリプトを使って既存のマネージド ディスク VM からイメージを作成した後、次のサンプル スクリプトで、既存のイメージ オブジェクトから類似の Linux VM を作成します。

Azure Stack PowerShell モジュール 1.7.0 以上: [マネージド イメージから VM を作成する](/azure/virtual-machines/windows/create-vm-generalized-managed)に説明されている手順に従います。

Azure Stack PowerShell モジュール 1.6.0 以前:

```powershell
# Variables for common values
$ResourceGroupName = "MyResourceGroup"
$Location = "local"
$VirtualMachineName = "MyVM"
$ImageRG = "managedlinuxrg"
$ImageName = "simplelinuxvmm-image-2019122"

# Create credential object
$Cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Create a subnet configuration
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name "MySubnet" -AddressPrefix "192.168.1.0/24"

# Create a virtual network
$VNet = New-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroupName -Location $Location `
  -Name "MyVNet" -AddressPrefix "192.168.0.0/16" -Subnet $SubnetConfig

# Create a public IP address and specify a DNS name
$PIp = New-AzureRmPublicIpAddress -ResourceGroupName $ResourceGroupName -Location $Location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create an inbound network security group rule for port 3389
$NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "MyNetworkSecurityGroupRuleRDP"  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow

# Create a network security group
$Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName -Location $Location `
  -Name "MyNetworkSecurityGroup" -SecurityRules $NsgRuleRDP

# Create a virtual network card and associate with public IP address and NSG
$Nic = New-AzureRmNetworkInterface -Name "MyNic" -ResourceGroupName $ResourceGroupName -Location $Location `
  -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PIp.Id -NetworkSecurityGroupId $Nsg.Id

$Image = Get-AzureRmImage -ResourceGroupName $ImageRG -ImageName $ImageName

# Create a virtual machine configuration
$VmConfig = New-AzureRmVMConfig -VMName $VirtualMachineName -VMSize "Standard_D1" | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName $VirtualMachineName -Credential $Cred | `
Set-AzureRmVMSourceImage -Id $Image.Id | `
Add-AzureRmVMNetworkInterface -Id $Nic.Id

# Create a virtual machine
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VmConfig
```

ポータルを使用して、マネージド イメージから VM を作成することもできます。 詳細については、Azure マネージド イメージに関する記事「[Azure で汎用化された VM のマネージド イメージを作成する](/azure/virtual-machines/windows/capture-image-resource)」および「[マネージド イメージから VM を作成する](/azure/virtual-machines/windows/create-vm-generalized-managed)」を参照してください。

## <a name="configuration"></a>構成

1808 以降の更新プログラムを適用した後、マネージド ディスクを使用する前に次の構成を実行する必要があります。

- 1808 更新プログラムの前にサブスクリプションが作成された場合は、次の手順に従ってサブスクリプションを更新します。 そうでないと、このサブスクリプションに VM をデプロイする操作がエラー メッセージ "ディスク マネージャーでの内部エラーです" で失敗することがあります。
   1. Azure Stack ユーザー ポータルで、 **[サブスクリプション]** に移動して、サブスクリプションを検索します。 **[リソース プロバイダー]** をクリックし、 **[Microsoft.Compute]** をクリックした後、 **[再登録]** をクリックします。
   2. 同じサブスクリプションで、 **[アクセス制御 (IAM)]** に移動し、 **[Azure Stack - マネージド ディスク]** がリストに含まれていることを確認します。
- マルチテナント環境を使用している場合は、[この記事](../operator/azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory)の次の手順に従って、各ゲスト ディレクトリを再構成するように (お客様の組織内またはサービス プロバイダーからの) クラウド オペレーターに依頼してください。 そうしないと、そのゲスト ディレクトリに関連付けられているサブスクリプションに VM をデプロイする操作が "**ディスク マネージャーでの内部エラーです**" というエラー メッセージとともに失敗することがあります。

## <a name="next-steps"></a>次の手順

- [Azure Stack 仮想マシンについての詳細](azure-stack-compute-overview.md)
