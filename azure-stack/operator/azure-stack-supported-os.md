---
title: Azure Stack でサポートされているゲスト オペレーティング システム | Microsoft Docs
description: これらのゲスト オペレーティング システムを Azure Stack で使用できます。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2019
ms.author: sethm
ms.reviewer: kivenkat
ms.lastreviewed: 06/06/2019
ms.openlocfilehash: fc3eecb8f4e06ce02737e513c48d946f2a667c21
ms.sourcegitcommit: cefba8d6a93efaedff303d3c605b02bd28996c5d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2019
ms.locfileid: "74298758"
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Azure Stack でサポートされているゲスト オペレーティング システム

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

## <a name="windows"></a>Windows

Azure Stack は、次の表に示す Windows ゲスト オペレーティング システムをサポートしています。

| オペレーティング システム | 説明 | Marketplace で利用可能 |
| --- | --- | --- |
| Windows Server、バージョン 1709 | 64 ビット | コンテナー付き Core |
| Windows Server 2019 | 64 ビット |  データセンター、Datacenter Core、コンテナー付きデータセンター |
| Windows Server 2016 | 64 ビット |  データセンター、Datacenter Core、コンテナー付きデータセンター |
| Windows Server 2012 R2 | 64 ビット |  データセンター |
| Windows Server 2012 | 64 ビット |  データセンター |
| Windows Server 2008 R2 SP1 | 64 ビット |  データセンター |
| Windows Server 2008 SP2 | 64 ビット |  Bring your own image |
| Windows 10 *(注 1 をご覧ください)* | 64 ビット、Pro、および Enterprise | Bring your own image |

> [!NOTE]
> Azure Stack で Windows 10 クライアント オペレーティング システムをデプロイするには、[Windows per User Licensing](https://www.microsoft.com/Licensing/product-licensing/windows10.aspx) を所持しているか、Qualified Multitenant Hoster [(QMTH](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx)) から購入する必要があります。

Marketplace イメージは従量課金制または BYOL (EA/SPLA) ライセンスで利用できます。 1 つの Azure Stack インスタンスで両方を使うことはサポートされていません。 デプロイ時に、Azure Stack により、適切なバージョンのゲスト エージェントがイメージに挿入されます。

Datacenter エディションは、Marketplace でダウンロードできます。お客様は、他のエディションを含む独自のサーバー イメージを使用できます。 Marketplace では、Windows クライアント イメージを利用できません。

## <a name="linux"></a>Linux

Marketplace で入手可能として示されている Linux ディストリビューションには、必要な Windows Azure Linux エージェント (WALA) が含まれます。 Azure Stack に独自のイメージを持ち込む場合は、「[Azure Stack への Linux イメージの追加](azure-stack-linux.md)」のガイドラインに従ってください。

> [!NOTE]
> カスタム イメージは、最新のパブリック WALA バージョン (1903 Azure Stack ビルド以上で、または 1901/1902 修正プログラムを使用して)、またはバージョン 2.2.20 を使ってビルドする必要があります。 2\.2.20 以前のバージョンと、2.2.21 から 2.2.34 までのバージョン (これらのバージョンを含みます) は、Azure Stack 上で正しく機能しない可能性があります。 Azure Stack 1910 リリース以降では、Azure WALA エージェントのすべてのバージョンが Azure Stack で動作します。
>
> 現時点では、Azure Stack 1910 リリース以降で [cloud-init](https://cloud-init.io/) がサポートされます。

| ディストリビューション | 説明 | Publisher | マーケットプレース |
| --- | --- | --- | --- |
| CentOS-based 6.9 | 64 ビット | Rogue Wave | はい |
| CentOS-based 7.5 | 64 ビット | Rogue Wave | はい |
| CentOS-based 7.3 | 64 ビット | Rogue Wave | はい |
| ClearLinux | 64 ビット | ClearLinux.org | はい |
| CoreOS Linux (Stable) |  64 ビット | CoreOS | はい |
| Debian 8 "Jessie" | 64 ビット | credativ |  はい |
| Debian 9 "Stretch" | 64 ビット | credativ | はい |
| Oracle Linux | 64 ビット | Oracle | はい |
| Red Hat Enterprise Linux 7.1 (およびそれ以降) | 64 ビット | Red Hat | Bring your own image |
| SLES 11SP4 | 64 ビット | SUSE | はい |
| SLES 12SP3 | 64 ビット | SUSE | はい |
| Ubuntu 14.04-LTS | 64 ビット | Canonical | はい |
| Ubuntu 16.04-LTS | 64 ビット | Canonical | はい |
| Ubuntu 18.04-LTS | 64 ビット | Canonical | はい |

Red Hat Enterprise Linux のサポート情報については、「[Red Hat and Azure Stack: Frequently Asked Questions](https://access.redhat.com/articles/3413531)」(Red Hat と Azure Stack: よく寄せられる質問) を参照してください。

## <a name="next-steps"></a>次の手順

Azure Stack Marketplace の詳細については、次の記事をご覧ください。

- [Marketplace アイテムをダウンロードする](azure-stack-download-azure-marketplace-item.md)  
- [Marketplace アイテムの作成と発行](azure-stack-create-and-publish-marketplace-item.md)
