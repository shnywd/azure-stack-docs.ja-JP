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
ms.date: 05/16/2019
ms.author: mabrigg
ms.lastreviewed: 01/18/2019
ms.openlocfilehash: 77d4963d2e3e468cb2de1e41a5c483e0339a3449
ms.sourcegitcommit: 889fd09e0ab51ad0e43552a800bbe39dc9429579
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/16/2019
ms.locfileid: "65782458"
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
