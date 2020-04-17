---
title: Azure Stack Hub に OEM (相手先ブランド供給) 更新プログラムを適用します。
description: Azure Stack Hub に OEM (相手先ブランド供給) 更新プログラムを適用する方法について説明します。
author: IngridAtMicrosoft
ms.topic: how-to
ms.date: 10/15/2019
ms.author: inhenkel
ms.lastreviewed: 03/04/2020
ms.reviewer: ppacent
ms.openlocfilehash: 982c62ad37d2d19dd3273c7ece3f20ff725ce6b6
ms.sourcegitcommit: a630894e5a38666c24e7be350f4691ffce81ab81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "78367252"
---
# <a name="apply-azure-stack-hub-original-equipment-manufacturer-oem-updates"></a>Azure Stack Hub に OEM (相手先ブランド供給) の更新プログラムを適用する

OEM (相手先ブランド供給) 更新プログラムを Azure Stack Hub ハードウェア コンポーネントに適用すると、ユーザーへの影響を最小限に抑えながら、ドライバーとファームウェアの機能強化、およびセキュリティ更新プログラムを受け取ることができます。 この記事では、OEM 更新プログラムについて、OEM 連絡先情報、および OEM 更新プログラムの適用方法について説明します。

## <a name="overview-of-oem-updates"></a>OEM 更新プログラムの概要

Microsoft Azure Stack Hub 更新プログラムに加えて、多くの OEM が、ドライバーやファームウェアの更新など、Azure Stack Hub ハードウェアの通常の更新プログラムもリリースしています。 これらは **OEM パッケージ更新プログラム**と呼ばれます。 OEM が OEM パッケージ更新プログラムをリリースしているかどうかを知るには、[OEM の Azure Stack Hub ドキュメント](#oem-contact-information)を確認してください。

これらの OEM パッケージ更新プログラムは、**updateadminaccount** ストレージ アカウントにアップロードされ、Azure Stack Hub 管理者ポータル経由で適用されます。 詳しくは、[OEM 更新プログラムの適用](#apply-oem-updates)に関するページをご覧ください。

OEM (相手先ブランド供給) パッケージの更新通知が確実に組織に届くようにする具体的な通知プロセスについては、ご利用の OEM にお問い合わせください。

ハードウェア ベンダーによっては、内部ファームウェア更新プロセスを処理する*ハードウェア ベンダー VM* が必要になる場合があります。 詳細については、「[ハードウェアベンダー VM を構成する](#configure-hardware-vendor-vm)」を参照してください

## <a name="oem-contact-information"></a>OEM の連絡先情報 

このセクションには、OEM の連絡先情報と OEM Azure Stack Hub の参考資料へのリンクが記載されています。

| ハードウェア パートナー | リージョン | URL |
|------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cisco | All | [Cisco Integrated System for Microsoft Azure Stack Hub Operations Guide (Cisco Integrated System for Microsoft Azure Stack Hub 運用ガイド)](https://aka.ms/aa708e2)<br><br>[UCS C シリーズ ラックマウント UCS 管理サーバー ソフトウェア](https://aka.ms/aa700rq) |
| Dell EMC | All | [Cloud for Microsoft Azure Stack Hub 14G (アカウントとログインが必要)](https://support.emc.com/downloads/44615_Cloud-for-Microsoft-Azure-Stack-14G)<br><br>[Cloud for Microsoft Azure Stack Hub 13G (アカウントとログインが必要)](https://support.emc.com/downloads/42238_Cloud-for-Microsoft-Azure-Stack-13G) |
| Fujitsu | 日本 | [富士通マネージド サービス サポート デスク (アカウントとログインが必要)](https://eservice.fujitsu.com/supportdesk-web/) |
|  | ヨーロッパ、中東、アフリカ、および米国 | [Fujitsu サポート: IT 製品およびシステム](https://support.ts.fujitsu.com/IndexContact.asp?lng=COM&ln=no&LC=del) (英語) |
| HPE | All | [HPE ProLiant for Microsoft Azure Stack Hub](http://www.hpe.com/info/MASupdates) |
| Lenovo | All | [ThinkAgile SXM Best Recipes](https://datacentersupport.lenovo.com/us/en/solutions/ht505122)
| Wortmann |  | [OEM/ファームウェア パッケージ](https://aka.ms/AA6z600)<br>[terra Azure Stack Hub ドキュメント (FRU を含む)](https://aka.ms/aa6zktc)

## <a name="apply-oem-updates"></a>OEM の更新プログラムを適用する

次の手順で OEM パッケージを適用します。

> [!IMPORTANT]
> Azure Stack Hub に更新プログラムを適用する前に、**更新前のチェックリスト**の手順を[すべて](release-notes-checklist.md)完了し、適用する更新プログラムの種類に適切なメンテナンス期間をスケジュールしていることを確実にしてください。

1. 次の場合に、OEM に問い合わせる必要があります。
      - OEM パッケージの現在のバージョンを確認する。  
      - OEM パッケージをダウンロードするための最適な方法を見つける。  
2. OEM パッケージ更新プログラムを適用する前に、常にシステムの現在の Azure Stack Hub バージョンで利用可能な最新の Azure Stack Hub 修正プログラムを適用する必要があります。 修正プログラムの詳細については、[Azure Stack Hub 修正プログラム](azure-stack-servicing-policy.md)に関するページを参照してください。
3. 「[統合システムの更新プログラム パッケージをダウンロードする](azure-stack-servicing-policy.md)」に記載されている手順で、OEM パッケージを準備します。
4. [Azure Stack Hub で更新を適用する](azure-stack-apply-updates.md)ことに関するページに記載されている手順で、更新プログラムを適用します。

## <a name="configure-hardware-vendor-vm"></a>ハードウェアベンダー VM を構成する

ハードウェア ベンダーによっては、OEM 更新プロセスを支援するために VM が必要になる場合があります。 ハードウェア ベンダーは、これらの VM を作成し、`ProxyVM`Set-OEMExternalVM`HardwareManager` コマンドレットを実行する際に、 **-VMType** に、**または** が必要な場合、および **-Credential** に使用すべき資格情報について、文書化する責任があります。 VM が作成されたら、特権エンドポイントからの **Set-OEMExternalVM** を使用してそれらを構成します。

Azure Stack Hub 上の特権エンドポイントの詳細については、「[Azure Stack Hub での特権エンドポイントの使用](azure-stack-privileged-endpoint.md)」を参照してください。

1.  特権エンドポイントにアクセスします。

    ```powershell  
    $cred = Get-Credential
    $session = New-PSSession -ComputerName <IP Address of ERCS>
    -ConfigurationName PrivilegedEndpoint -Credential $cred
    ```

2. **Set-OEMExternalVM** コマンドレットを使用して、ハードウェア ベンダー VM を構成します。 このコマンドレットは、 **-VMType** `ProxyVM` の IP アドレスと資格情報を検証します。 **-VMType** `HardwareManager` の場合、コマンドレットによる入力の検証は行われません。 **Set-OEMExternalVM** に指定される **-Credential** パラメーターは、ハードウェアベンダーのドキュメントで明確に文書化されるものです。  これは、特権エンドポイントやその他の既存の Azure Stack Hub 資格情報で使用される CloudAdmin 資格情報ではありません。

    ```powershell  
    $VmCred = Get-Credential
    Invoke-Command -Session $session
        { 
    Set-OEMExternalVM -VMType <Either "ProxyVM" or "HardwareManager">
        -IPAddress <IP Address of hardware vendor VM> -Credential $using:VmCred
        }
    ```

## <a name="next-steps"></a>次のステップ

[Azure Stack Hub の更新プログラム](azure-stack-updates.md)
