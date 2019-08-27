---
title: Azure Stack に OEM (相手先ブランド供給) 更新プログラムを適用する | Microsoft Docs
description: Azure Stack に OEM (相手先ブランド供給) 更新プログラムを適用する方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2019
ms.author: mabrigg
ms.lastreviewed: 08/15/2019
ms.reviewer: ppacent
ms.openlocfilehash: 1342eb503abb81308740c0103b1d54887a46cf85
ms.sourcegitcommit: f62d58ae724020a24fa5905b6663abb5f1d62178
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69520926"
---
# <a name="apply-azure-stack-original-equipment-manufacturer-oem-updates"></a>Azure Stack に OEM (相手先ブランド供給) 更新プログラムを適用する

*適用対象:Azure Stack 統合システム*

OEM (相手先ブランド供給) 更新プログラムを Azure Stack ハードウェア コンポーネントに適用すると、ユーザーへの影響を最小限に抑えながら、ドライバーとファームウェアの機能強化、およびセキュリティ更新プログラムを受け取ることができます。 この記事では、OEM 更新プログラムについて、OEM 連絡先情報、および OEM 更新プログラムの適用方法について説明します。

## <a name="overview-of-oem-updates"></a>OEM 更新プログラムの概要

Microsoft Azure Stack 更新プログラムに加えて、多くの OEM が、ドライバーやファームウェアの更新など、Azure Stack ハードウェアの通常の更新プログラムもリリースしています。 これらは **OEM パッケージ更新プログラム**と呼ばれます。 OEM が OEM パッケージ更新プログラムをリリースしているかどうかを知るには、[OEM の Azure Stack ドキュメント](#oem-contact-information)を確認してください。

これらの OEM パッケージ更新プログラムは、**updateadminaccount** ストレージ アカウントにアップロードされ、Azure Stack 管理者ポータル経由で適用されます。 詳しくは、[OEM 更新プログラムの適用](#apply-oem-updates)に関するページをご覧ください。

OEM (相手先ブランド供給) パッケージの更新通知が確実に組織に届くようにする具体的な通知プロセスについては、ご利用の OEM にお問い合わせください。

ハードウェア ベンダーによっては、内部ファームウェア更新プロセスを処理する*ハードウェア ベンダー VM* が必要になる場合があります。 詳細については、「[ハードウェアベンダー VM を構成する](#configure-hardware-vendor-vm)」を参照してください

## <a name="oem-contact-information"></a>OEM の連絡先情報 

このセクションには、OEM の連絡先情報と OEM Azure Stack の参考資料へのリンクが記載されています。

| ハードウェア パートナー | リージョン | URL |
|------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cisco | All | [Cisco Integrated System for Microsoft Azure Stack Operations Guide](https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/azure-stack/b_Azure_Stack_Operations_Guide_4-0/b_Azure_Stack_Operations_Guide_4-0_chapter_00.html#concept_wks_t1q_wbb) (運用ガイド)<br><br>[Release Notes for Cisco Integrated System for Microsoft Azure Stack](https://www.cisco.com/c/en/us/support/servers-unified-computing/ucs-c-series-rack-mount-ucs-managed-server-software/products-release-notes-list.html) (リリース ノート) |
| Dell EMC | All | [Cloud for Microsoft Azure Stack 14G (アカウントとログインが必要)](https://support.emc.com/downloads/44615_Cloud-for-Microsoft-Azure-Stack-14G)<br><br>[Cloud for Microsoft Azure Stack 13G (アカウントとログインが必要)](https://support.emc.com/downloads/42238_Cloud-for-Microsoft-Azure-Stack-13G) |
| Fujitsu | 日本 | [富士通マネージド サービス サポート デスク (アカウントとログインが必要)](https://eservice.fujitsu.com/supportdesk-web/) |
|  | ヨーロッパ、中東およびアフリカ | [Fujitsu サポート: IT 製品およびシステム](https://support.ts.fujitsu.com/IndexContact.asp?lng=COM&ln=no&LC=del) (英語) |
|  | EU | [Fujitsu MySupport (アカウントとログインが必要)](https://support.ts.fujitsu.com/IndexMySupport.asp) (英語) |
| HPE | All | [HPE ProLiant for Microsoft Azure Stack](http://www.hpe.com/info/MASupdates) |
| Lenovo | All | [ThinkAgile SXM Best Recipes](https://datacentersupport.lenovo.com/us/en/solutions/ht505122)
| Wortmann |  | [OEM/ファームウェア パッケージ](https://drive.terracloud.de/dl/fiTdTb66mwDAJWgUXUW8KNsd/OEM)<br>[terra Azure Stack ドキュメント (FRU を含む)](https://drive.terracloud.de/dl/fiWGZwCySZSQyNdykXCFiVCR/TerraAzSDokumentation)

## <a name="apply-oem-updates"></a>OEM の更新プログラムを適用する

次の手順で OEM パッケージを適用します。

1. 次の場合に、OEM に問い合わせる必要があります。
      - OEM パッケージの現在のバージョンを確認する。  
      - OEM パッケージをダウンロードするための最適な方法を見つける。  
2. 「[統合システムの更新プログラム パッケージをダウンロードする](azure-stack-servicing-policy.md#download-update-packages-for-integrated-systems)」に記載されている手順で、OEM パッケージを準備します。
3. 「[Azure Stack で更新を適用する](azure-stack-apply-updates.md)」に記載されている手順で更新プログラムを適用します。

## <a name="configure-hardware-vendor-vm"></a>ハードウェアベンダー VM を構成する

ハードウェア ベンダーによっては、OEM 更新プロセスを支援するために VM が必要になる場合があります。 **Set-OEMExternalVM** コマンドレットを実行する際に、 **-VMType** に、`ProxyVM` または `HardwareManager` が必要な場合、ハードウェア ベンダーは、これらの VM を作成し、文書化する責任があります。 VM が作成されたら、特権エンドポイントからの **Set-OEMExternalVM** を使用してそれらを構成します。

Azure Stack 上の特権エンドポイントの詳細については、「[Azure Stack での特権エンドポイントの使用](azure-stack-privileged-endpoint.md)」を参照してください。

1.  特権エンドポイントにアクセスします。

    ```powershell  
    $cred = Get-Credential
    $session = New-PSSession -ComputerName <IP Address of ERCS>
    -ConfigurationName PrivilegedEndpoint -Credential $cred
    ```

2. **Set-OEMExternalVM** コマンドレットを使用して、ハードウェア ベンダー VM を構成します。 このコマンドレットは、 **-VMType** `ProxyVM` の IP アドレスと資格情報を検証します。 **-VMType** `HardwareManager` の場合、コマンドレットによって入力が検証されません。

    ```powershell  
    
    Invoke-Command -Session $session
        { 
    Set-OEMExternalVM -VMType <Either "ProxyVM" or "HardwareManager">
        -IPAddress <IP Address of hardware vendor VM>
        }
    ```

## <a name="next-steps"></a>次の手順

[Azure Stack の更新プログラム](azure-stack-updates.md)