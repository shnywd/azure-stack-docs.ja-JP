---
title: Azure Stack でストレージ アカウントを作成する | Microsoft Docs
titleSuffix: Azure Stack
description: Azure Stack でストレージ アカウントを作成する方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/2/2019
ms.author: mabrigg
ms.lastreviewed: 01/18/2019
ms.openlocfilehash: 449d9e39b650e6f7ccd91f4703709ea033e7a5dc
ms.sourcegitcommit: ca358ea5c91a0441e1d33f540f6dbb5b4d3c92c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2019
ms.locfileid: "73802347"
---
# <a name="create-storage-accounts-in-azure-stack"></a>Azure Stack でストレージ アカウントを作成する

Azure Stack のストレージ アカウントには、BLOB や Table service、およびストレージ データ オブジェクトの一意の名前空間が含まれます。 既定では、アカウントのデータはストレージ アカウント所有者だけが使用できます。

1. Azure Stack POC コンピューターで、[管理者](../asdk/asdk-connect.md)として `https://adminportal.local.azurestack.external` にログインし、 **[+ Create a resource]\(+ リソースの作成\)**  >  **[データ + ストレージ]**  >  **[ストレージ アカウント]** の順にクリックします。

   ![Azure Stack 管理者ポータルでストレージ アカウントを作成する](media/azure-stack-provision-storage-account/image01.png)

2. **[ストレージ アカウントの作成]** ブレードで、ストレージ アカウントの名前を入力します。 新しい**リソース グループ**を作成するか、既存のものを選択し、 **[作成]** をクリックしてストレージ アカウントを作成します。

   ![Azure Stack 管理者ポータルでストレージ アカウントを確認する](media/azure-stack-provision-storage-account/image02.png)

3. 新しいストレージ アカウントを表示するには、 **[すべてのリソース]** をクリックしてストレージ アカウントを検索し、ストレージ アカウント名をクリックします。

    ![Azure Stack 管理者ポータルでのストレージ アカウント名](media/azure-stack-provision-storage-account/image03.png)

### <a name="next-steps"></a>次の手順

- [Azure リソース マネージャー テンプレートの使用](../user/azure-stack-arm-templates.md)
- [Azure ストレージ アカウントについて](/azure/storage/common/storage-create-storage-account)
- [Azure Stack Azure 互換ストレージの検証ガイドのダウンロード](https://aka.ms/azurestacktp1doc)
