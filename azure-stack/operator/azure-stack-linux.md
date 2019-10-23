---
title: Linux イメージを Azure Stack Marketplace に追加する | Microsoft Docs
description: Azure Stack Marketplace へ Linux イメージを追加する方法について説明します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/16/2018
ms.openlocfilehash: d7723dcdd755a926990ee52e96c3b75694651520
ms.sourcegitcommit: a6d47164c13f651c54ea0986d825e637e1f77018
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/11/2019
ms.locfileid: "72277213"
---
# <a name="add-linux-images-to-azure-stack-marketplace"></a>Linux イメージを Azure Stack Marketplace に追加する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure Stack Marketplace に Linux ベースのイメージを追加することによって、Azure Stack 上に Linux 仮想マシン (VM) をデプロイできます。 最も容易に Linux イメージを Azure Stack に追加する方法は、Marketplace Management からです。 これらのイメージは、Azure Stack との互換性を確保できるよう、あらかじめ準備され、テストされています。

## <a name="marketplace-management"></a>Marketplace Management

Azure Marketplace から Linux イメージをダウンロードするには、[Azure から Azure Stack への Marketplace 項目のダウンロード](azure-stack-download-azure-marketplace-item.md)に関するページを参照してください。 Azure Stack で、ユーザーに提供する Linux のイメージを選択します。

これらのイメージは頻繁に更新されるので、Marketplace Management をこまめに確認して最新の状態に保つようにしてください。

## <a name="prepare-your-own-image"></a>独自のイメージを準備する

可能な限り、使用可能なイメージは、Marketplace Management からダウンロードしてください。 これらのイメージは、Azure Stack 用にあらかじめ準備され、テストされています。

### <a name="azure-linux-agent"></a>Azure Linux エージェント

Azure Linux エージェント (一般に **WALinuxAgent** または **walinuxagent** と呼ばれる) が必要であり、エージェントのバージョンによっては Azure Stack 上で動作しないものがあります。 2\.2.20 から 2.2.35 までのバージョンは、Azure Stack 上ではサポートされません。 バージョン 2.2.35 より後の最新のエージェントを使用する場合は、1901 または1902 の修正プログラムを適用するか、Azure Stack を 1903 リリース (またはそれ以降) に更新してください。 Azure Stack では、現時点で [cloud-init](https://cloud-init.io/) がサポートされていないことに注意してください。

| Azure Stack のビルド | Azure Linux エージェントのビルド |
| ------------- | ------------- |
| 1.1901.0.99 以前 | 2.2.20 |
| 1.1902.0.69  | 2.2.20  |
|  1.1901.3.105   | 2.2.35 以降 |
| 1.1902.2.73  | 2.2.35 以降 |
| 1.1903.0.35  | 2.2.35 以降 |
| 1903 移行のビルド | 2.2.35 以降 |
| サポートされていません | 2.2.21-2.2.34 |

次の手順を使って、独自の Linux イメージを準備できます。

* [CentOS ベースのディストリビューション](/azure/virtual-machines/linux/create-upload-centos?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](/azure/virtual-machines/linux/debian-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
* [SLES と openSUSE](/azure/virtual-machines/linux/suse-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu Server](/azure/virtual-machines/linux/create-upload-ubuntu?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="add-your-image-to-marketplace"></a>Marketplace にイメージを追加する

[Marketplace へのイメージの追加](azure-stack-add-vm-image.md)に関するページに従ってください。 `OSType` パラメーターが `Linux` に設定されていることを確認してください。

Marketplace にイメージを追加した後は、Marketplace の項目が作成され、ユーザーが Linux VM をデプロイできます。

## <a name="next-steps"></a>次の手順

* [Azure から Azure Stack に Marketplace の項目をダウンロードする](azure-stack-download-azure-marketplace-item.md)
* [Azure Stack Marketplace の概要](azure-stack-marketplace.md)
