---
title: Azure Stack でのスケール ユニットのノード操作 | Microsoft Docs
description: Azure Stack 統合システム上でノードの状態を表示し、電源オン、電源オフ、無効化、再開のノード アクションを使用する方法を説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 07/18/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 07/18/2019
ms.openlocfilehash: 7ac25e86be91cf6a2e8384c88c79fe3022b3f00d
ms.sourcegitcommit: 159da88a52701679571bbedde1c36b72bbfe32dd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2019
ms.locfileid: "68380473"
---
# <a name="scale-unit-node-actions-in-azure-stack"></a>Azure Stack でのスケール ユニット ノードの操作

*適用対象: Azure Stack 統合システム*

この記事では、スケール ユニットの状態を確認する方法について説明します。 ユニットのノードは、表示することができます。 電源オン、電源オフ、シャットダウン、ドレイン、再開、修復などのノード アクションを実行できます。 通常、これらのノード アクションは、パーツのフィールド交換時に使用されるほか、ノードの復旧の手段として使用されます。

> [!Important]  
> この記事で説明されているすべてのノード アクションは、一度に 1 つのノードを対象にするようにしてください。

## <a name="view-the-node-status"></a>ノードの状態を表示する

管理者ポータルで、スケール ユニットとその関連するノードの状態を表示できます。

スケール ユニットの状態を表示するには、以下のようにします。

1. **[Region management]\(リージョンの管理\)** タイルで、リージョン名をクリックします。
2. 左側の **[インフラストラクチャ リソース]** で、 **[スケール ユニット]** を選択します。
3. 結果画面で、スケール ユニットを選択します。
4. 左側の **[全般]** で、 **[ノード]** を選択します。

   次の情報を確認します。

   - 個々のノードの一覧
   - 動作状態 (以下の一覧を参照)
   - 電源の状態 (実行中または停止)
   - サーバー モデル
   - ベースボード管理コントローラー (BMC) の IP アドレス
   - コアの合計数
   - メモリの総量

![スケール ユニットの状態](media/azure-stack-node-actions/multinodeactions.png)

### <a name="node-operational-states"></a>ノードの動作状態

| Status | 説明 |
|----------------------|-------------------------------------------------------------------|
| 実行中 | ノードは、アクティブにスケール ユニットに参加しています。 |
| 停止済み | ノードは利用不可です。 |
| 追加中 | ノードは、アクティブにスケール ユニットに追加されています。 |
| 修復中 | ノードは現在、アクティブに修復されています。 |
| メンテナンス | ノードは一時停止され、アクティブなユーザー ワークロードは実行されていません。 |
| 修復が必要 | ノードの修復を必要とするエラーが検出されました。 |

## <a name="scale-unit-node-actions"></a>スケール ユニットのノード操作

スケール ユニットのノードに関する情報を表示すると、次のようなノード操作を行うこともできます。
 - 開始と停止 (現在の電源状態による)
 - 無効化と再開 (動作状態による)
 - 修復
 - Shutdown

ノードの操作状態によって、使用可能なオプションが決まります。

Azure Stack PowerShell モジュールをインストールする必要があります。 これらのコマンドレットは **Azs.Fabric.Admin** モジュールに存在します。 PowerShell for Azure Stack のインストールまたはインストールの確認については、「[PowerShell for Azure Stack をインストールする](azure-stack-powershell-install.md)」を参照してください。

## <a name="stop"></a>Stop

**停止**アクションは、ノードをオフにします。 これは、電源ボタンを押した場合と同じです。 オペレーティング システムにシャット ダウンの信号を送ることはしません。 計画されている停止操作の場合は、最初に必ずシャットダウン操作を行ってください。 

この操作は通常、ノードが停止状態であり要求に応答しないときに使用されます。

停止アクションを実行するには、管理者特権の PowerShell プロンプトを開き、次のコマンドレットを実行します。

```powershell  
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

まれなケースで、停止アクションが機能しない場合には、操作を再試行し、2 度目も失敗するようであれば、代わりに BMC Web インターフェイスを使用してください。

詳細については、「[Stop-AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/stop-azsscaleunitnode)」を参照してください。

## <a name="start"></a>start

**開始**操作は、ノードをオンにします。 これは、電源ボタンを押した場合と同じです。 
 
開始アクションを実行するには、管理者特権の PowerShell プロンプトを開き、次のコマンドレットを実行します。

```powershell  
  Start-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

まれなケースで、開始アクションが機能しない場合には、操作を再試行し、2 度目も失敗するようであれば、代わりに BMC Web インターフェイスを使用してください。

詳細については、「[Start-AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/start-azsscaleunitnode)」を参照してください。

## <a name="drain"></a>ドレイン

**ドレイン**操作は、すべてのアクティブなワークロードを、その特定のスケール ユニット内の残りのノードに移動します。

この操作は通常、ノード全体の交換などの、パーツのフィールド交換中に使用されます。

> [!Important]
> ノードのドレイン操作は必ず、ユーザーに通知済みの計画されたメンテナンス期間中に行うようにしてください。 状況によっては、アクティブなワークロードが中断されることがあります。

ドレイン アクションを実行するには、管理者特権の PowerShell プロンプトを開き、次のコマンドレットを実行します。

```powershell  
  Disable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

詳細については、「[Disable-AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/disable-azsscaleunitnode)」を参照してください。

## <a name="resume"></a>Resume

**再開**アクションは、無効化されたノードを再開し、ワークロードのアクティブな配置対象としてマークします。 ノードで実行されていた以前のワークロードはフェールバックしません。 (ノードのドレイン操作を使用する場合は、必ず電源をオフにしてください。 ノードは、電源をオンに戻しても、ワークロードのアクティブな配置対象としてマークされません。 準備ができたら、再開操作を使用しノードをアクティブとしてマークする必要があります。)

再開アクションを実行するには、管理者特権の PowerShell プロンプトを開き、次のコマンドレットを実行します。

```powershell  
  Enable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```

詳細については、「[Enable-AzsScaleUnitNode](https://docs.microsoft.com/powershell/module/azs.fabric.admin/enable-azsscaleunitnode)」を参照してください。

## <a name="repair"></a>修復

> [!CAUTION]  
> この記事で説明している操作を成功させるには、ファームウェアの平準化が重要です。 この手順を実行しないと、システムが不安定になったり、パフォーマンスが低下したり、セキュリティ スレッドが発生したり、オペレーティング システムをデプロイするための Azure Stack の自動化が妨げられたりする可能性があります。 ハードウェアを交換する場合は、ハードウェア パートナーのドキュメントを必ず参照して、適用されているファームウェアが、[Azure Stack 管理者ポータル](azure-stack-updates.md)に表示されている OEM バージョンと一致していることを確認してください。<br>
詳細およびパートナー ドキュメントへのリンクについては、[ハードウェア コンポーネントの交換](azure-stack-replace-component.md)に関する記事を参照してください。

| ハードウェア パートナー | リージョン | URL |
|------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cisco | All | [Cisco Integrated System for Microsoft Azure Stack Operations Guide](https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/azure-stack/b_Azure_Stack_Operations_Guide_4-0/b_Azure_Stack_Operations_Guide_4-0_chapter_00.html#concept_wks_t1q_wbb) (運用ガイド)<br><br>[Release Notes for Cisco Integrated System for Microsoft Azure Stack](https://www.cisco.com/c/en/us/support/servers-unified-computing/ucs-c-series-rack-mount-ucs-managed-server-software/products-release-notes-list.html) (リリース ノート) |
| Dell EMC | All | [Cloud for Microsoft Azure Stack 14G (アカウントとログインが必要)](https://support.emc.com/downloads/44615_Cloud-for-Microsoft-Azure-Stack-14G)<br><br>[Cloud for Microsoft Azure Stack 13G (アカウントとログインが必要)](https://support.emc.com/downloads/42238_Cloud-for-Microsoft-Azure-Stack-13G) |
| Fujitsu | 日本 | [富士通マネージド サービス サポート デスク (アカウントとログインが必要)](https://eservice.fujitsu.com/supportdesk-web/) |
|  | ヨーロッパ、中東およびアフリカ | [Fujitsu サポート: IT 製品およびシステム](https://support.ts.fujitsu.com/IndexContact.asp?lng=COM&ln=no&LC=del) (英語) |
|  |  | [Fujitsu MySupport (アカウントとログインが必要)](https://support.ts.fujitsu.com/IndexMySupport.asp) (英語) |
| HPE | All | [HPE ProLiant for Microsoft Azure Stack](http://www.hpe.com/info/MASupdates) |
| Lenovo | All | [ThinkAgile SXM Best Recipes](https://datacentersupport.lenovo.com/us/en/solutions/ht505122) |

**修復**アクションは、ノードを修復します。 次のシナリオのいずれかに対してのみ使用します。
 - ノードの完全交換 (新しいデータ ディスクあり、またはなし)
 - ハードウェア コンポーネントの障害と交換の後 (フィールド交換可能装置 (FRU) ドキュメントで推奨されている場合)。

> [!Important]  
> ノードまたは個々のハードウェア コンポーネントを置き換える必要がある場合の正確な手順については、OEM ハードウェア ベンダーの FRU ドキュメントを参照してください。 FRU ドキュメントでは、ハードウェア コンポーネントを交換した後、修復操作を実行する必要があるかどうかを指定します。 

修復操作を実行する場合、BMC の IP アドレスを指定する必要があります。 

修復アクションを実行するには、管理者特権の PowerShell プロンプトを開き、次のコマンドレットを実行します。

  ```powershell
  Repair-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -BMCIPv4Address <BMCIPv4Address>
  ```

## <a name="shutdown"></a>Shutdown

**シャットダウン**操作では最初に、すべてのアクティブなワークロードを、同じスケール ユニット内の残りのノードに移動します。 操作では次に、スケール ユニット ノードを適切にシャットダウンします。

シャットダウンされたノードの開始後は、[再開](#resume)操作を実行する必要があります。 ノードで実行されていた以前のワークロードはフェールバックしません。

シャットダウン操作に失敗する場合は、シャットダウン操作の前に[ドレイン](#drain)操作を試行します。

シャットダウン操作を実行するには、管理者特権の PowerShell プロンプトを開き、次のコマンドレットを実行します。

  ```powershell
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -Shutdown
  ```



## <a name="next-steps"></a>次の手順

Azure Stack Fabric 管理者モジュールの詳細については、「[Azs.Fabric.Admin](https://docs.microsoft.com/powershell/module/azs.fabric.admin/?view=azurestackps-1.6.0)」を参照してください。
