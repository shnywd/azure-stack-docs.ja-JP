---
title: Azure Stack のストレージ アカウントについて | Microsoft Docs
description: Azure Stack のストレージ アカウントの作成方法について説明します。
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
ms.openlocfilehash: 60336477f1dec9618fd6cc439e9d2f5d098b3399
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829380"
---
# <a name="storage-accounts-in-azure-stack"></a>Azure Stack のストレージ アカウント

Azure Stack のストレージ アカウントには、BLOB や Table service、およびストレージ データ オブジェクトの一意の名前空間が含まれます。 既定では、アカウントのデータはストレージ アカウント所有者だけが使用できます。

1. Azure Stack POC コンピューターで、[管理者](../asdk/asdk-connect.md)として `https://adminportal.local.azurestack.external` にログインし、 **[+ Create a resource]\(+ リソースの作成\)**  >  **[データ + ストレージ]**  >  **[ストレージ アカウント]** の順にクリックします。

   ![ストレージ アカウントの作成](media/azure-stack-provision-storage-account/image01.png)
2. **[ストレージ アカウントの作成]** ブレードで、ストレージ アカウントの名前を入力します。 新しい**リソース グループ**を作成するか、既存のものを選択し、 **[作成]** をクリックしてストレージ アカウントを作成します。

   ![ストレージ アカウントの確認](media/azure-stack-provision-storage-account/image02.png)
3. 新しいストレージ アカウントを表示するには、 **[すべてのリソース]** をクリックしてストレージ アカウントを検索し、ストレージ アカウント名をクリックします。

    ![ストレージ アカウント名の取得](media/azure-stack-provision-storage-account/image03.png)

### <a name="next-steps"></a>次の手順
[Azure リソース マネージャー テンプレートの使用](../user/azure-stack-arm-templates.md)

[Azure ストレージ アカウントについて](/azure/storage/common/storage-create-storage-account)

[Azure Stack Azure 互換ストレージの検証ガイドのダウンロード](https://aka.ms/azurestacktp1doc)
