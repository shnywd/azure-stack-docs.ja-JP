---
title: Azure Stack スケール ユニット ノードのハードウェア コンポーネントを交換する
titleSuffix: Azure Stack
description: Azure Stack 統合システムのハードウェア コンポーネントの交換方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2019
ms.author: thoroet
ms.lastreviewed: 07/18/2019
ms.openlocfilehash: ff78409ecdbdec8b7a6860db18244a4835351ed8
ms.sourcegitcommit: 284f5316677c9a7f4c300177d0e2a905df8cb478
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2019
ms.locfileid: "74465281"
---
# <a name="replace-a-hardware-component-on-an-azure-stack-scale-unit-node"></a>Azure Stack スケール ユニット ノードのハードウェア コンポーネントを交換する

*適用対象:Azure Stack 統合システム*

この記事では、ホットスワップが可能でないハードウェア コンポーネントを交換するための一般的なプロセスについて説明します。 実際の交換手順は、ご利用の OEM (Original Equipment Manufacturer) ハードウェア ベンダーによって異なります。 Azure Stack 統合システムに特化した詳しい手順については、ベンダーの現場交換可能ユニット (FRU) ドキュメントをご覧ください。

> [!CAUTION]  
> この記事で説明している操作を成功させるには、ファームウェアの平準化が重要です。 この手順を実行しないと、システムが不安定になったり、パフォーマンスが低下したり、セキュリティ スレッドが発生したり、Azure Stack の自動化でオペレーティング システムのデプロイが妨げられたりする可能性があります。 ハードウェアを交換する場合は、ハードウェア パートナーのドキュメントを必ず参照して、適用されるファームウェアが、[Azure Stack 管理者ポータル](azure-stack-updates.md)に表示されている OEM バージョンと一致していることを確認してください。

| ハードウェア パートナー | リージョン | URL |
|------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cisco | All | [Cisco Integrated System for Microsoft Azure Stack Operations Guide](https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/azure-stack/b_Azure_Stack_Operations_Guide_4-0/b_Azure_Stack_Operations_Guide_4-0_chapter_00.html#concept_wks_t1q_wbb) (運用ガイド)<br><br>[Release Notes for Cisco Integrated System for Microsoft Azure Stack](https://www.cisco.com/c/en/us/support/servers-unified-computing/ucs-c-series-rack-mount-ucs-managed-server-software/products-release-notes-list.html) (リリース ノート) |
| Dell EMC | All | [Cloud for Microsoft Azure Stack 14G (アカウントとサインインが必要)](https://support.emc.com/downloads/44615_Cloud-for-Microsoft-Azure-Stack-14G)<br><br>[Cloud for Microsoft Azure Stack 13G (アカウントとサインインが必要)](https://support.emc.com/downloads/42238_Cloud-for-Microsoft-Azure-Stack-13G) |
| Fujitsu | 日本 | [富士通マネージド サービス サポート デスク (アカウントとサインインが必要)](https://eservice.fujitsu.com/supportdesk-web/) |
|  | ヨーロッパ、中東およびアフリカ | [Fujitsu サポート: IT 製品およびシステム](https://support.ts.fujitsu.com/IndexContact.asp?lng=COM&ln=no&LC=del) (英語) |
|  | EU | [Fujitsu MySupport (アカウントとサインインが必要)](https://support.ts.fujitsu.com/IndexMySupport.asp) |
| HPE | All | [HPE ProLiant for Microsoft Azure Stack](http://www.hpe.com/info/MASupdates) |
| Lenovo | All | [ThinkAgile SXM Best Recipes](https://datacentersupport.lenovo.com/us/en/solutions/ht505122)
| Wortmann |  | [OEM/ファームウェア パッケージ](https://drive.terracloud.de/dl/fiTdTb66mwDAJWgUXUW8KNsd/OEM)<br>[terra Azure Stack ドキュメント (FRU を含む)](https://drive.terracloud.de/dl/fiWGZwCySZSQyNdykXCFiVCR/TerraAzSDokumentation)

ホットスワップが可能でないコンポーネントには、次の項目が含まれます。

- CPU*
- メモリ*
- マザーボード/ベースボード管理コントローラー (BMC)/ビデオ カード
- ディスク コントローラー/ホスト バス アダプター (HBA)/バックプレーン
- ネットワーク アダプター (NIC)
- オペレーティング システム ディスク*
- データ ドライブ (ホットスワップをサポートしないドライブ。例: PCI-e アドイン カード)*

\* これらのコンポーネントはホットスワップをサポートしている場合がありますが、ベンダーの実装によって異なる可能性があります。 詳しい手順については、OEM ベンダーの FRU ドキュメントを参照してください。

次のフロー図は、ホットスワップが可能でないハードウェア コンポーネントを交換するための一般的な FRU プロセスを示しています。

![コンポーネント交換フローを示すフロー図](media/azure-stack-replace-component/replacecomponentflow.PNG)

* このアクションは、ハードウェアの物理的な状態によっては必須でありません。

** OEM ハードウェア ベンダーがコンポーネントの交換とファームウェア更新を行うかどうかは、サポート契約に応じて異なる可能性があります。

## <a name="review-alert-information"></a>アラート情報を見直す

Azure Stack の正常性および監視システムによって、記憶域スペース ダイレクトで制御されているネットワーク アダプターとデータ ドライブの正常性が追跡されます。 その他のハードウェア コンポーネントは追跡されません。 その他のすべてのハードウェア コンポーネントでは、ハードウェア ライフサイクル ホスト上で実行されているベンダー固有のハードウェア監視ソリューションでアラートが発生します。  

## <a name="component-replacement-process"></a>コンポーネント交換プロセス

次の手順は、コンポーネント交換プロセスの概要を示しています。 OEM 提供の FRU マニュアルを参照せずにこれらの手順を実行しないでください。

1. シャットダウン アクションを使用して、スケール ユニット ノードを正常にシャットダウンします。 このアクションは、ハードウェアの物理的な状態によっては必須でありません。

2. まれなケースでシャットダウン アクションに失敗した場合は、[ドレイン](azure-stack-node-actions.md#drain) アクションを使用して、スケール ユニット ノードをメンテナンス モードにします。 このアクションは、ハードウェアの物理的な状態によっては必須でありません。

   > [!NOTE]  
   > いずれの場合も、S2D (記憶域スペース ダイレクト) を中断せずに一度に無効化して電源をオフにできるノードは 1 つだけです。

3. スケール ユニット ノードがメンテナンス モードに入ったら、[電源オフ](azure-stack-node-actions.md#scale-unit-node-actions) アクションを使用します。 このアクションは、ハードウェアの物理的な状態によっては必須でありません。

   > [!NOTE]  
   > 可能性は低いですが、電源オフ アクションが機能しない場合は、代わりに BMC (Baseboard Management Controller) Web インターフェイスを使用します。

4. 破損したハードウェア コンポーネントを交換します。 OEM ハードウェア ベンダーがコンポーネントの交換を行うかどうかは、サポート契約に応じて異なる場合があります。  
5. ファームウェアを更新します。 ハードウェア ライフサイクル ホストを使用するベンダー固有のファームウェア更新プロセスに従って、交換したハードウェア コンポーネントに承認されたファームウェア レベルが適用されていることを確認します。 OEM ハードウェア ベンダーがこの手順を行うかどうかは、サポート契約に応じて異なる場合があります。  
6. [修復](azure-stack-node-actions.md#scale-unit-node-actions)アクションを使用して、スケール ユニット ノードをスケール ユニットに戻します。
7. 特権エンドポイントを使用して、[仮想ディスクの修復の状態を確認します](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair-using-the-privileged-endpoint)。 新しいデータ ドライブを使用するストレージ修復ジョブ全体は、システムの負荷と消費される領域に応じて、数時間かかる可能性があります。
8. 修復アクションが完了したら、すべてのアクティブなアラートが自動的に閉じていることを確認します。

## <a name="next-steps"></a>次の手順

- ホットスワップ可能物理ディスクの交換については、[ディスクの交換](azure-stack-replace-disk.md)に関する記事を参照してください。
- 物理ノードを交換する方法については、[スケール ユニット ノードの交換](azure-stack-replace-node.md)に関する記事を参照してください。
