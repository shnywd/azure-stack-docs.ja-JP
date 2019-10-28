---
title: Azure Stack Marketplace の概要 | Microsoft Docs
description: Azure Stack Marketplace と Marketplace アイテムの概要。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2019
ms.author: sethm
ms.reviewer: ihcherie
ms.lastreviewed: 05/07/2019
ms.openlocfilehash: da8fdf20446e5ed60271e236f44939d439118b4b
ms.sourcegitcommit: b5eb024d170f12e51cc852aa2c72eabf26792d8d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "72534097"
---
# <a name="azure-stack-marketplace-overview"></a>Azure Stack Marketplace の概要

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure Stack Marketplace は、Azure Stack 向けにカスタマイズされたサービス、アプリ、およびリソースのコレクションです。 リソースには、ネットワーク、仮想マシン (VM)、ストレージなどが含まれます。 Azure Stack Marketplace を使用して、新しいリソースを作成したり、新しいアプリをデプロイしたり、使用するアイテムを参照して選択したりします。 ユーザーが Marketplace アイテムを使用するには、アイテムへのアクセスが許可されているサービスをサブスクライブする必要があります。

Azure Stack オペレーターとして、Azure Stack Marketplace に追加 (公開) するアイテムを決定します。 データベース、アプリ サービスなどのアイテムを公開できます。 公開すると、アイテムはすべてのユーザーに表示されます。 作成したカスタム アイテムを公開することも、増え続けている [Azure Marketplace アイテムの一覧](azure-stack-marketplace-azure-items.md)からアイテムを公開することもできます。 Azure Stack Marketplace にアイテムを公開すると、5 分以内にユーザーがそれを表示できます。

> [!CAUTION]  
> ギャラリー アイテム アーティファクト (イメージや JSON ファイルを含む) はすべて、Azure Stack Marketplace で使用可能にした後、認証なしでアクセスできます。 Marketplace アイテムを発行するときの考慮事項については、「[Marketplace アイテムを作成および発行する](azure-stack-create-and-publish-marketplace-item.md)」を参照してください。

Marketplace を開くには、管理者ポータルで **[+ リソースの作成]** を選択します。

![Azure Stack 管理者ポータルでリソースを作成する](media/azure-stack-marketplace/marketplace1.png)

## <a name="marketplace-items"></a>Marketplace アイテム

Azure Stack Marketplace のアイテムは、ユーザーがダウンロードおよび使用できるサービス、アプリ、またはリソースです。 プランやオファーなどの管理アイテムを含むすべての Azure Stack Marketplace アイテムがすべてのユーザーに表示されます。 これらの管理アイテムは、表示するためのサブスクリプションを必要としませんが、ユーザーは使用することができません。

すべての Marketplace アイテムには以下のものが含まれます。

* リソース プロビジョニングのための Azure Resource Manager テンプレート。
* 文字列、アイコン、その他のマーケティング資料などのメタデータ。
* ポータルでアイテムを表示するための書式設定情報。

Azure Stack Marketplace に公開されるすべてのアイテムでは、Azure ギャラリー パッケージ (.azpkg) 形式が使用されます。 デプロイまたはランタイム リソース (コード、ソフトウェアの .zip ファイル、または VM イメージ) をマーケットプレース アイテムの一部としてではなく、Azure Stack に個別に追加します。

Azure Stack バージョン 1803 以降では、Azure からダウンロードするときや、カスタム画像をアップロードするときに画像はスパース ファイルに変換されます。 このプロセスは画像を追加するときに時間がかかりますが、領域を節約し、画像の展開時間を短縮することができます。 変換は新しい画像にのみ適用されます。 既存の画像は変更されません。

## <a name="next-steps"></a>次の手順

* [Azure から既存の Marketplace の項目をダウンロードして Azure Stack に発行する](azure-stack-download-azure-marketplace-item.md)  
* [カスタム Azure Stack Marketplace 項目の作成と発行](azure-stack-create-and-publish-marketplace-item.md)
