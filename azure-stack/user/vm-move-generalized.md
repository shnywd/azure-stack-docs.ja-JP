---
title: 汎用化 VM をオンプレミスから Azure Stack Hub に移動する
description: 汎用化 VM をオンプレミスから Azure Stack Hub に移動する方法について説明します。
author: mattbriggs
ms.topic: how-to
ms.date: 12/16/2020
ms.author: mabrigg
ms.reviewer: kivenkat
ms.lastreviewed: 9/8/2020
ms.openlocfilehash: 105beaa0805fe0aea1aacfce8bd22f3bd01714b1
ms.sourcegitcommit: 733a22985570df1ad466a73cd26397e7aa726719
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/05/2021
ms.locfileid: "97872876"
---
# <a name="move-a-generalized-vm-from-on-premises-to-azure-stack-hub"></a>汎用化 VM をオンプレミスから Azure Stack Hub に移動する

オンプレミス環境から仮想マシン (VM) イメージを追加できます。 イメージを仮想ハード ディスク (VHD) として作成し、そのイメージを Azure Stack Hub インスタンスのストレージ アカウントにアップロードできます。 その VHD から VM を作成できます。

汎用化ディスク イメージとは、ユーザー アカウントなどの固有の情報を削除するために **Sysprep** を使用して準備されたもので、これを再利用して複数の VM を作成することができます。 Azure Stack Hub クラウド オペレーターが Marketplace 項目として使用するイメージを作成する場合は、汎用化 VHD が適しています。

## <a name="how-to-move-an-image"></a>イメージを移動する方法

VHD を準備するときに、ニーズに合致するセクションを見つけます。

#### <a name="windows-vm"></a>[Windows VM](#tab/port-win)

VHD をアップロードする前に、「[Azure にアップロードする Windows VHD または VHDX を準備する](/azure/virtual-machines/windows/prepare-for-upload-vhd-image)」の手順に従って正しく汎用化してください。 Azure Stack Hub には VHD を使用する必要があります。

#### <a name="linux-vm"></a>[Linux VM](#tab/port-linux)

適切な指示に従って、Linux OS の VHD を汎用化します。

- [CentOS ベースのディストリビューション](/azure/virtual-machines/linux/create-upload-centos?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Debian Linux](/azure/virtual-machines/linux/debian-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Red Hat Enterprise Linux](../operator/azure-stack-redhat-create-upload-vhd.md)
- [SLES または openSUSE](/azure/virtual-machines/linux/suse-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Ubuntu Server](/azure/virtual-machines/linux/create-upload-ubuntu?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

---

## <a name="verify-your-vhd"></a>VHD を検証する

[!INCLUDE [Verify VHD](../includes/user-compute-verify-vhd.md)]
## <a name="upload-to-a-storage-account"></a>ストレージ アカウントにアップロードする

[!INCLUDE [Upload to a storage account](../includes/user-compute-upload-vhd.md)]

## <a name="create-the-image-in-azure-stack-hub"></a>Azure Stack Hub でイメージを作成する

[!INCLUDE [Create the image in Azure Stack Hub](../includes/user-compute-create-image.md)]

## <a name="next-steps"></a>次のステップ

[Azure Stack Hub への VM の移動の概要](vm-move-overview.md)
