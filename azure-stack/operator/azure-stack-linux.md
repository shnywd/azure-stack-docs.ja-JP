---
title: Azure Stack への Linux イメージの追加
description: Azure Stack へ Linux イメージを追加する方法について説明します。
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
ms.date: 02/15/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/16/2018
ms.openlocfilehash: 2f7f0c55f02fd99a419619d878be8300d7326303
ms.sourcegitcommit: 0973dddb81db03cf07c8966ad66526d775ced8b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "64309649"
---
# <a name="add-linux-images-to-azure-stack"></a>Azure Stack への Linux イメージの追加

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure Stack Marketplace に Linux ベースのイメージを追加することによって、Azure Stack に Linux 仮想マシン (VM) をデプロイできます。 最も容易に Linux イメージを Azure Stack に追加する方法は、Marketplace Management からです。 これらのイメージは、Azure Stack との互換性を確保できるよう、あらかじめ準備され、テストされています。

## <a name="marketplace-management"></a>Marketplace Management

Azure Marketplace から Linux イメージをダウンロードするには、「[Azure から Azure Stack に Marketplace の項目をダウンロードする](azure-stack-download-azure-marketplace-item.md)」に記載されている手順を使用します。 Azure Stack で、ユーザーに提供する Linux のイメージを選択します。 

これらのイメージは頻繁に更新されるので、Marketplace Management をこまめに確認して最新の状態に保つようにしてください。

## <a name="prepare-your-own-image"></a>独自のイメージを準備する

可能な限り、Azure Stack 用に準備されてテストされている Marketplace Management から使用可能なイメージをダウンロードします。

### <a name="azure-linux-agent"></a>Azure Linux エージェント
Azure Linux エージェント (一般に `WALinuxAgent` または `walinuxagent` と呼ばれます) が必要であり、エージェントのバージョンによっては Azure Stack で動作しないものがあります。 2.2.20 から 2.2.35 までのバージョンは、Azure Stack ではサポートされません。 バージョン 2.2.35 より後の最新のエージェントを使用する場合は、1901 または1902 の修正プログラムを適用するか、Azure Stack を 1903 リリース (またはそれ以降) に更新してください。 現在、[cloud-init](https://cloud-init.io/) は Azure Stack でサポートされていないことに注意してください。

| Azure Stack のビルド | Azure Linux エージェントのビルド |
| ------------- | ------------- |
| 1.1901.0.99 以前 | 2.2.20 |
| 1.1902.0.69  | 2.2.20  |
|  1.1901.3.105   | 2.2.35 以降 |
| 1.1902.2.73  | 2.2.35 以降 |
| 1.1903.0.35  | 2.2.35 以降 |
| サポートされていません | 2.2.21-2.2.34 |

次の手順を使って、独自の Linux イメージを準備できます。

* [CentOS ベースのディストリビューション](/azure/virtual-machines/linux/create-upload-centos?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](/azure/virtual-machines/linux/debian-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
* [SLES と openSUSE](/azure/virtual-machines/linux/suse-create-upload-vhd?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu Server](/azure/virtual-machines/linux/create-upload-ubuntu?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="add-your-image-to-the-marketplace"></a>Marketplace にイメージを追加する

[Marketplace へのイメージの追加](azure-stack-add-vm-image.md)に関するページに従ってください。 `OSType` パラメーターが `Linux` に設定されていることを確認してください。

Marketplace にイメージを追加すると、Marketplace アイテムが作成され、ユーザーが Linux 仮想マシンをデプロイできます。

## <a name="next-steps"></a>次の手順

詳細については、次の記事を参照してください。

- [Azure から Azure Stack に Marketplace の項目をダウンロードする](azure-stack-download-azure-marketplace-item.md)
- [Azure Stack Marketplace の概要](azure-stack-marketplace.md)
