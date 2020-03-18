---
title: Azure Stack HCI のボリュームを削除する
description: Windows Admin Center を使用して Azure Stack HCI のボリュームを削除する方法について説明します。
author: khdownie
ms.author: v-kedow
ms.topic: article
ms.date: 03/05/2020
ms.openlocfilehash: ab99ed7a49fe2bf003245f139451895a85c4edbf
ms.sourcegitcommit: a77dea675af6500bdad529106f5782d86bec6a34
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "79025979"
---
# <a name="deleting-volumes-in-azure-stack-hci"></a>Azure Stack HCI のボリュームの削除

> 適用対象:Windows Server 2019

このトピックでは、Windows Admin Center を使用して[記憶域スペース ダイレクト](/windows-server/storage/storage-spaces/storage-spaces-direct-overview) クラスターのボリュームを削除する手順について説明します。

ボリュームの削除方法を説明する短いビデオをご覧ください。

> [!VIDEO https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo]

## <a name="use-windows-admin-center-to-delete-a-volume"></a>Windows Admin Center を使用してボリュームを削除する

1. Windows Admin Center で、記憶域スペース ダイレクト クラスターに接続し、 **[ツール]** ペインで **[ボリューム]** を選択します。
2. **[ボリューム]** ページで **[インベントリ]** タブを選択し、削除するボリュームを選択します。
3. ボリュームの詳細ページの上部で、 **[削除]** を選択します。
4. 確認のダイアログで、ボリュームの削除を確認するチェック ボックスをオンにして、 **[削除]** を選択します。

## <a name="next-steps"></a>次のステップ

他の重要な記憶域管理タスクの詳細な手順については、次の項目も参照してください。

- [記憶域スペース ダイレクトのボリュームの計画](/windows-server/storage/storage-spaces/plan-volumes)
- [記憶域スペース ダイレクトのボリュームの作成](/windows-server/storage/storage-spaces/create-volumes)
- [記憶域スペース ダイレクトのボリュームの拡張](/windows-server/storage/storage-spaces/resize-volumes)