---
title: Azure Stack HCI で障害が発生したドライブを交換する
description: Azure Stack HCI で障害が発生したドライブを交換する方法について説明します。
author: khdownie
ms.author: v-kedow
ms.topic: article
ms.date: 02/27/2020
ms.openlocfilehash: e0aaed5d444a0d7b617ecd2ccd350a9812be8a2c
ms.sourcegitcommit: a77dea675af6500bdad529106f5782d86bec6a34
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "79025973"
---
# <a name="replace-failed-drives-on-azure-stack-hci"></a>Azure Stack HCI で障害が発生したドライブを交換する

Azure Stack HCI は直接接続された SATA、SAS、または NVMe ドライブに対応しています。これらは、それぞれ 1 台のサーバーのみに物理的に接続されます。 ドライブに障害が発生した場合は、物理サーバー ハードウェアにアクセスして交換する必要があります。

## <a name="find-the-alert"></a>アラートを見つける
ドライブに障害が発生すると、**Windows Admin Center** ダッシュボードの左上の **[アラート]** 領域にアラートが表示されます。 また、左側にあるナビゲーションから **[ドライブ]** を選択するか、右下隅にあるタイルで **[VIEW DRIVES >]\(ドライブの表示 >\)** リンクをクリックして、ドライブを参照してその状態を自分で確認することもできます。 **[表示]** タブのグリッドでは、並べ替え、グループ化、およびキーワード検索がサポートされています。

1. ダッシュボードからアラートをクリックすると、ドライブの物理的な場所などの詳細が表示されます。
1. 詳細については、 **[ドライブ]** 詳細ページへの **[Go to drive]\(ドライブに移動\)** ショートカットをクリックします。
1. ハードウェアでサポートされている場合は、 **[Turn light on/off]\(ライトをオンにする/オフにする\)** をクリックして、ドライブのインジケーター ライトを制御できます。
   記憶域スペース ダイレクトは、障害が発生したドライブを自動的に廃止して退避させます。 これが発生すると、ドライブの状態は廃止になり、記憶域容量バーは空になります。
1. 障害が発生したドライブを取り外し、代わりのものを挿入します。

## <a name="wait-for-the-alert-to-clear"></a>アラートが消去されるのを待ちます。
**[ドライブ] > [インベントリ]** に新しいドライブが表示されます。 しばらくしてアラートが消去されると、ボリュームは OK 状態に戻り、記憶域は新しいドライブに再調整されます。ユーザー操作は不要です。

## <a name="next-steps"></a>次のステップ
-  ドライブ レベルなど、さまざまなレベルで記憶域の正常性を追跡する方法の詳細については、「[正常性と動作状態](/windows-server/storage/storage-spaces/storage-spaces-states)」を参照してください。
