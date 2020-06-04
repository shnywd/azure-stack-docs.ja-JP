---
title: Azure Stack HCI クラスターのサーバーを追加または削除する
description: Azure Stack HCI のクラスターに対してサーバー ノードを追加または削除する方法について説明します
ms.topic: article
author: v-dasis
ms.author: v-dasis
ms.reviewer: jgerend
ms.date: 05/20/2020
ms.openlocfilehash: 4d8e122c8a0b68740ecf34f8140322ee627d03e7
ms.sourcegitcommit: 91f4baa7a770b7a82f1ddccec4dbac14be530d06
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/21/2020
ms.locfileid: "83769040"
---
# <a name="add-or-remove-servers-for-an-azure-stack-hci-cluster"></a>Azure Stack HCI クラスターのサーバーを追加または削除する

> 適用対象:Windows Server 2019

Azure Stack HCI のクラスターに対してサーバーを簡単に追加または削除できます。 新しい各物理サーバーは、CPU の種類、メモリ、ドライブの数、ドライブの種類とサイズに関して、クラスター内の他のサーバーと厳密に一致している必要があることに注意してください。

サーバーを追加または削除したときは必ず、後でクラスター検証を実行して、クラスターが正常に機能していることを確認することも必要です。

## <a name="obtain-server-hardware-from-your-oem"></a>OEM からサーバー ハードウェアを入手する

最初の手順は、元の OEM から新しい HCI ハードウェアを入手することです。 クラスターで使用するための新しいサーバー ハードウェアを追加するときは、OEM 提供のドキュメントを必ず参照してください。

1. 新しい物理サーバーをラックに設置し、正しく配線します。
1. 物理スイッチ ポートを有効にし、アクセス制御リスト (ACL) と VLAN ID を調整します (該当する場合)。
1. OEM の指示に従って、ベースボード管理コントローラー (BMC) で正しい IP アドレスを構成し、すべての BIOS 設定を適用します。
1. OEM によって提供されるツールを使用して、最新のファームウェア ベースラインをすべてのコンポーネントに適用します。
1. OEM 検証テストを実行して、既存のクラスター サーバーとの同質性を確保します。

## <a name="add-the-server-to-the-cluster"></a>サーバーをクラスターに追加する

サーバーが正しく起動したら、Windows Admin Center を使用してサーバーをクラスターに参加させます。

:::image type="content" source="media/manage-cluster/add-server.png" alt-text="サーバーの追加画面":::

1. **Windows Admin Center** で、上部のドロップダウン矢印から **[Cluster Manager]\(クラスター マネージャー\)** を選択します。
1. **[Cluster connections]\(クラスター接続\)** の下で、クラスターを選択します。
1. **[ツール]** の下で、 **[サーバー]** を選択します。
1. **[サーバー]** の下で、 **[インベントリ]** タブを選択します。
1. **インベントリ** タブで、**追加］** を選択します。
1. **[サーバー名]** に、追加するサーバーの完全修飾ドメイン名を入力し、 **[追加]** をクリックして、下部にある **[追加]** をもう一度クリックします。
1. サーバーがクラスターに正常に追加されたことを確認します。

## <a name="remove-a-server-from-the-cluster"></a>クラスターからサーバーを削除する

クラスターからサーバーを削除する手順は、クラスターにサーバーを追加する手順と似ています。

サーバーを削除すると、そのサーバーに関連付けられている仮想マシン、ドライブ、およびワークロードもすべて削除されることに注意してください。

:::image type="content" source="media/manage-cluster/remove-server.png" alt-text="サーバーの削除のダイアログ":::

1. **Windows Admin Center** で、上部のドロップダウン矢印から **[Cluster Manager]\(クラスター マネージャー\)** を選択します。
1. **[Cluster connections]\(クラスター接続\)** の下で、クラスターを選択します。
1. **[ツール]** の下で、 **[サーバー]** を選択します。
1. **[サーバー]** の下で、 **[インベントリ]** タブを選択します。
1. **[インベントリ]** タブで、削除するサーバーを選択し、 **[削除]** を選択します。
1. また、記憶域プールからサーバーのドライブをすべて削除するには、そのチェックボックスをオンにします。
1. サーバーがクラスターから正常に削除されたことを確認します。

## <a name="validate-the-cluster"></a>クラスターを検証する

クラスターに対してサーバーを追加または削除したときは常に、クラスターを検証する必要があります。 Microsoft によってサポートされるように、クラスター検証はエラーなしで合格する必要があります。

:::image type="content" source="media//manage-cluster/validate-cluster.png" alt-text="クラスターの検証のダイアログ":::

1. **Windows Admin Center** で、上部のドロップダウン矢印から **[Cluster Manager]\(クラスター マネージャー\)** を選択します。
1. **[ツール]** の下で、 **[サーバー]** を選択します。
1. **[サーバー]** の下で、 **[インベントリ]** タブを選択し、 **[その他]** を選択してから **[クラスターの検証]** を選択します。
1. すべての検証ステップが正常に終了したことを確認します。

## <a name="next-steps"></a>次のステップ

- クラスター検証の詳細については、「[フェールオーバー クラスター用ハードウェアを検証する](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134244(v=ws.11))」を参照してください。