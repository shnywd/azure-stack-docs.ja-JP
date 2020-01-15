---
title: Azure Stack Hub でサポートされているゲスト オペレーティング システム | Microsoft Docs
description: これらのゲスト オペレーティング システムを Azure Stack Hub で使用できます。
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
ms.date: 01/07/2020
ms.author: sethm
ms.reviewer: kivenkat
ms.lastreviewed: 06/06/2019
ms.openlocfilehash: 431af9688eb4f97e3c5400e9fe2b00c0ff18ec11
ms.sourcegitcommit: b9d520f3b7bc441d43d489e3e32f9b89601051e6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2020
ms.locfileid: "75727515"
---
# <a name="guest-operating-systems-supported-on-azure-stack-hub"></a>Azure Stack Hub でサポートされているゲスト オペレーティング システム

*適用対象:Azure Stack Hub 統合システムと Azure Stack Development Kit*

## <a name="windows"></a>Windows

Azure Stack Hub では、次の表に示す Windows ゲスト オペレーティング システムがサポートされています。

| オペレーティング システム | [説明] | Marketplace で利用可能 |
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
> Azure Stack Hub で Windows 10 クライアント オペレーティング システムをデプロイするには、[Windows のユーザーごとのライセンス](https://www.microsoft.com/licensing/product-licensing/windows10.aspx)を持っているか、Qualified Multitenant Hoster [(QMTH](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx)) から購入する必要があります。

Marketplace イメージは従量課金制または BYOL (EA/SPLA) ライセンスで利用できます。 1 つの Azure Stack Hub インスタンスで両方を使うことはサポートされていません。 デプロイ時に Azure Stack Hub によって、適切なバージョンのゲスト エージェントがイメージに挿入されます。

Datacenter エディションは、Marketplace でダウンロードできます。お客様は、他のエディションを含む独自のサーバー イメージを使用できます。 Marketplace では、Windows クライアント イメージを利用できません。

## <a name="linux"></a>Linux

Marketplace で入手可能として示されている Linux ディストリビューションには、必要な Windows Azure Linux エージェント (WALA) が含まれます。 Azure Stack Hub に独自のイメージを持ち込む場合は、[Linux イメージを Azure Stack Hub に追加する](azure-stack-linux.md)ことに関するページのガイドラインに従ってください。

> [!NOTE]
> カスタム イメージは、最新のパブリック WALA バージョン (Azure Stack Hub ビルド 1903 以降、または 1901/1902 修正プログラムを適用した環境のもの) で、またはバージョン 2.2.20 を使ってビルドする必要があります。 2\.2.20 より前のバージョンや、2.2.21 から 2.2.34 までのバージョン (これらのバージョンを含みます) は、Azure Stack Hub で正しく機能しない可能性があります。 Azure Stack Hub 1910 リリース以降では、すべてのバージョンの Azure WALA エージェントが Azure Stack で動作します。
>
> [cloud-init](https://cloud-init.io/) は、Azure Stack 1910 以降でサポートされます。

| Distribution | [説明] | Publisher | マーケットプレース |
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

## <a name="next-steps"></a>次のステップ

Azure Stack Hub Marketplace の詳細については、次の記事をご覧ください。

- [Marketplace アイテムをダウンロードする](azure-stack-download-azure-marketplace-item.md)  
- [Marketplace アイテムの作成と発行](azure-stack-create-and-publish-marketplace-item.md)
