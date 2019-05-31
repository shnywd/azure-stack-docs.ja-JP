---
title: Azure Stack で PowerShell を使用して Linux VM を作成する | Microsoft Docs
description: Azure Stack で PowerShell を使用して Linux VM を作成します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: mabrigg
ms.custom: mvc
ms.lastreviewed: 12/03/2018
ms.openlocfilehash: 4e60b2f2df2aae9cf27bdec41c6492ad3ff04fb3
ms.sourcegitcommit: 797dbacd1c6b8479d8c9189a939a13709228d816
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2019
ms.locfileid: "66269589"
---
# <a name="quickstart-create-a-linux-server-vm-using-powershell-in-azure-stack"></a>クイック スタート:Azure Stack で PowerShell を使用して Linux サーバー VM を作成する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure Stack の PowerShell を使用して、Ubuntu Server 16.04 LTS 仮想マシン (VM) を作成できます。 この記事の手順に従って、VM を作成し、使用します。  この記事では、以下のことを実行する手順も示します。

* リモート クライアントを使用して VM に接続する。
* NGINX Web サーバーをインストールし、既定のホーム ページを表示します。
* 使用されていないリソースをクリーンアップします。

## <a name="prerequisites"></a>前提条件

* **Azure Stack Marketplace 内の Linux イメージ**

   Azure Stack Marketplace には、既定では Linux イメージが含まれていません。 必要な **Ubuntu Server 16.04 LTS** イメージを提供する Azure Stack オペレーターを取得します。 オペレーターは、「[Azure から Azure Stack に Marketplace の項目をダウンロードする](../operator/azure-stack-download-azure-marketplace-item.md)」という記事に記載されている手順を使用できます。

* リソースを作成して管理するため、Azure Stack には Azure PowerShell の特定のバージョンが必要です。 Azure Stack 用に構成された PowerShell がない場合は、PowerShell を[インストール](../operator/azure-stack-powershell-install.md)する手順に従います。

* Azure Stack PowerShell の設定を使用して、ご自身の Azure Stack 環境に接続する必要があります。 手順については、[PowerShell を使用したユーザーとしての Azure Stack への接続](azure-stack-powershell-configure-user.md)に関するページをご覧ください。

* Windows ユーザー プロファイルの .ssh ディレクトリに保存された id_rsa.pub という名前の SSH 公開キー。 SSH キーの作成の詳細については、「[SSH 公開キーの使用方法](azure-stack-dev-start-howto-ssh-public-key.md)」を参照してください。

## <a name="create-a-resource-group"></a>リソース グループの作成

リソース グループは、Azure Stack リソースのデプロイと管理を行うことができる論理コンテナーです。 開発キットまたは Azure Stack 統合システムから、次のコード ブロックを実行してリソース グループを作成します。 

> [!NOTE]
> 値は、コード例のすべての変数に割り当てられます。 ただし、必要に応じて新しい値を割り当てることができます。

```powershell  
# Create variables to store the location and resource group names.
$location = "local"
$ResourceGroupName = "myResourceGroup"

New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location
```

## <a name="create-storage-resources"></a>ストレージ リソースの作成

ストレージ アカウントを作成してから、Ubuntu Server 16.04 LTS イメージのストレージ コンテナーを作成します。

```powershell  
# Create variables to store the storage account name and the storage account SKU information
$StorageAccountName = "mystorageaccount"
$SkuName = "Standard_LRS"

# Create a new storage account
$StorageAccount = New-AzureRMStorageAccount `
  -Location $location `
  -ResourceGroupName $ResourceGroupName `
  -Type $SkuName `
  -Name $StorageAccountName

Set-AzureRmCurrentStorageAccount `
  -StorageAccountName $storageAccountName `
  -ResourceGroupName $resourceGroupName

```

## <a name="create-networking-resources"></a>ネットワーク リソースの作成

仮想ネットワーク、サブネット、パブリック IP アドレスを作成します。 これらのリソースは、VM にネットワーク接続を提供するために使用されます。

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name MyVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -AllocationMethod Static `
  -IdleTimeoutInMinutes 4 `
  -Name "mypublicdns$(Get-Random)"

```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>ネットワーク セキュリティ グループとネットワーク セキュリティ グループの規則を作成する

ネットワーク セキュリティ グループは、受信規則と送信規則を使用して VM をセキュリティで保護します。 ポート 3389 に、受信リモート デスクトップ接続を許可する受信規則を作成し、ポート 80 に、受信 Web トラフィックを許可する受信規則を作成します。

```powershell
# Create variables to store the network security group and rules names.
$nsgName = "myNetworkSecurityGroup"
$nsgRuleSSHName = "myNetworkSecurityGroupRuleSSH"
$nsgRuleWebName = "myNetworkSecurityGroupRuleWeb"


# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name $nsgRuleSSHName -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name $nsgRuleWebName -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName -Location $location `
-Name $nsgName -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

### <a name="create-a-network-card-for-the-vm"></a>VM 用のネットワーク カードを作成する

ネットワーク カードは、VM をサブネット、ネットワーク セキュリティ グループ、パブリック IP アドレスに接続します。

```powershell
# Create a virtual network card and associate it with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
  -Name myNic `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id `
  -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-a-vm"></a>VM の作成

VM 構成を作成します。 この構成には、VM をデプロイするときに使用される設定が含まれています。 たとえば、ユーザー資格情報、サイズ、VM イメージなどです。

```powershell
# Define a credential object.
$UserName='demouser'
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ($UserName, $securePassword)

# Create the VM configuration object
$VmName = "VirtualMachinelatest"
$VmSize = "Standard_D1"
$VirtualMachine = New-AzureRmVMConfig `
  -VMName $VmName `
  -VMSize $VmSize

$VirtualMachine = Set-AzureRmVMOperatingSystem `
  -VM $VirtualMachine `
  -Linux `
  -ComputerName "MainComputer" `
  -Credential $cred

$VirtualMachine = Set-AzureRmVMSourceImage `
  -VM $VirtualMachine `
  -PublisherName "Canonical" `
  -Offer "UbuntuServer" `
  -Skus "16.04-LTS" `
  -Version "latest"

# Sets the operating system disk properties on a VM.
$VirtualMachine = Set-AzureRmVMOSDisk `
  -VM $VirtualMachine `
  -CreateOption FromImage | `
  Set-AzureRmVMBootDiagnostics -ResourceGroupName $ResourceGroupName `
  -StorageAccountName $StorageAccountName -Enable |`
  Add-AzureRmVMNetworkInterface -Id $nic.Id


# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"

# Adds the SSH Key to the VM
Add-AzureRmVMSshPublicKey -VM $VirtualMachine `
 -KeyData $sshPublicKey `
 -Path "/home/azureuser/.ssh/authorized_keys"

# Create the VM.
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
 -Location $location `
  -VM $VirtualMachine
```

## <a name="quick-create-vm---full-script"></a>VM の簡易作成 - 完全なスクリプト

> [!NOTE]
> これは基本的には上記のコードがマージされたものですが、認証には SSH キーではなくパスワードが使用されます。

```powershell
## Create a resource group

<#
A resource group is a logical container where you can deploy and manage Azure Stack resources. From your development kit or the Azure Stack integrated system, run the following code block to create a resource group. Values are assigned for all the variables in this document, you can use these values or assign new values.
#>

# Edit your variables here if required

# Create variables to store the location and resource group names.
$location = "local"
$ResourceGroupName = "myResourceGroup"

# Create variables to store the storage account name and the storage account SKU information
$StorageAccountName = "mystorageaccount"
$SkuName = "Standard_LRS"

# Create variables to store the network security group and rules names.
$nsgName = "myNetworkSecurityGroup"
$nsgRuleSSHName = "myNetworkSecurityGroupRuleSSH"
$nsgRuleWebName = "myNetworkSecurityGroupRuleWeb"

# Create variable for VM password
$VMPassword = 'Password123!'

# End of Variables - no need to edit anything past that point to deploy a single VM

# Create Resource Group
New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location

## Create storage resources

# Create a storage account and then create a storage container for the Ubuntu Server 16.04 LTS image.

# Create a new storage account
$StorageAccount = New-AzureRMStorageAccount `
  -Location $location `
  -ResourceGroupName $ResourceGroupName `
  -Type $SkuName `
  -Name $StorageAccountName

Set-AzureRmCurrentStorageAccount `
  -StorageAccountName $storageAccountName `
  -ResourceGroupName $resourceGroupName

# Create a storage container to store the VM image
$containerName = 'osdisks'
$container = New-AzureStorageContainer `
  -Name $containerName `
  -Permission Blob


## Create networking resources

# Create a virtual network, subnet, and a public IP address. These resources are used to provide network connectivity to the VM.

# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name MyVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -AllocationMethod Static `
  -IdleTimeoutInMinutes 4 `
  -Name "mypublicdns$(Get-Random)"


### Create a network security group and a network security group rule

<#
The network security group secures the VM by using inbound and outbound rules. Create an inbound rule for port 3389 to allow incoming Remote Desktop connections and an inbound rule for port 80 to allow incoming web traffic.
#>

# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name $nsgRuleSSHName -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name $nsgRuleWebName -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName -Location $location `
-Name $nsgName -SecurityRules $nsgRuleSSH,$nsgRuleWeb

### Create a network card for the VM

# The network card connects the VM to a subnet, network security group, and public IP address.

# Create a virtual network card and associate it with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
  -Name myNic `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id `
  -NetworkSecurityGroupId $nsg.Id

## Create a VM
<#
Create a VM configuration. This configuration includes the settings used when deploying the VM. For example: user credentials, size, and the VM image.
#>

# Define a credential object.
$UserName='demouser'
$securePassword = ConvertTo-SecureString $VMPassword -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ($UserName, $securePassword)

# Create the VM configuration object
$VmName = "VirtualMachinelatest"
$VmSize = "Standard_D1"
$VirtualMachine = New-AzureRmVMConfig `
  -VMName $VmName `
  -VMSize $VmSize

$VirtualMachine = Set-AzureRmVMOperatingSystem `
  -VM $VirtualMachine `
  -Linux `
  -ComputerName "MainComputer" `
  -Credential $cred

$VirtualMachine = Set-AzureRmVMSourceImage `
  -VM $VirtualMachine `
  -PublisherName "Canonical" `
  -Offer "UbuntuServer" `
  -Skus "16.04-LTS" `
  -Version "latest"

$osDiskName = "OsDisk"
$osDiskUri = '{0}vhds/{1}-{2}.vhd' -f `
  $StorageAccount.PrimaryEndpoints.Blob.ToString(),`
  $vmName.ToLower(), `
  $osDiskName

# Sets the operating system disk properties on a VM.
$VirtualMachine = Set-AzureRmVMOSDisk `
  -VM $VirtualMachine `
  -Name $osDiskName `
  -VhdUri $OsDiskUri `
  -CreateOption FromImage | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create the VM.
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
 -Location $location `
  -VM $VirtualMachine
```

## <a name="connect-to-the-vm"></a>VM に接続します

VM がデプロイされたら、VM の SSH 接続を構成します。 [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) コマンドを使用して、VM のパブリック IP アドレスを取得します。

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

SSH がインストールされているクライアント システムから、次のコマンドを使用して VM に接続します。 Windows で作業している場合は、[Putty](https://www.putty.org/) を使用して接続を作成できます。

```
ssh <Public IP Address>
```

入力を求められたら、**azureuser** としてサインインします。 SSH キーを作成したときにパスフレーズを使用した場合は、そのパスフレーズを入力する必要があります。

## <a name="install-the-nginx-web-server"></a>NGINX Web サーバーのインストール

パッケージ リソースを更新し、最新の NGINX パッケージをインストールするため、次のスクリプトを実行します。

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>NGINX のようこそページの表示

NGINX がインストールされ、VM 上のポート 80 が開かれたので、その VM のパブリック IP アドレスを使用して Web サーバーにアクセスできます。 Web ブラウザーを開き、```http://<public IP address>``` に移動します｡

![NGINX Web サーバーのようこそページ](./media/azure-stack-quick-create-vm-linux-cli/nginx.png)

## <a name="clean-up-resources"></a>リソースのクリーンアップ

不要になったリソースをクリーンアップします。 [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) コマンドを使用して、これらのリソース グループを削除できます。 リソース グループとそのすべてのリソースを削除するには、次のコマンドを実行します。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>次の手順

このクイック スタートでは、基本の Linux サーバー VM をデプロイしました。 Azure Stack VM の詳細については、[Azure Stack VM 機能](azure-stack-vm-considerations.md)に関する記事に進んでください。
