---
title: Azure Stack で MySQL データベースを作成する | Microsoft Docs
description: Azure Stack で MySQL アダプター リソース プロバイダーを使用してプロビジョニングされた MySQL データベースの作成と管理を行う方法について説明します。
services: azure-stack
documentationCenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: 9daaea395ee28da79da1d6fd553d2e0b30e28156
ms.sourcegitcommit: a23b80b57668615c341c370b70d0a106a37a02da
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72682230"
---
# <a name="create-mysql-databases-in-azure-stack"></a>Azure Stack で MySQL データベースを作成する
MySQL データベース サービスを含むオファーにサブスクライブした Azure Stack ユーザーは、ユーザー ポータルで、セルフ サービスの MySQL データベースを作成し、管理することができます。

## <a name="create-a-mysql-database"></a>MySQL データベースを作成する

1. Azure Stack ユーザー ポータルにサインインします。
2. **[+ リソースの作成]**  >  **[データ + ストレージ]**  >  **[MySQL データベース]**  >  **[追加]** の順に選択します。
3. **[MySQL Database の作成]** でデータベース名を入力し、環境で必要な他の設定を構成します。

    ![テスト MySQL データベースの作成](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. **[データベースの作成]** で、 **[SKU]** を選択します。 **[MySQL SKU の選択]** で、データベースで使用する SKU を選択します。

    ![MySQL SKU を選択する](./media/azure-stack-mysql-rp-deploy/mysql-select-sku.png)

    >[!Note]
    >ホスティング サーバーは、Azure Stack に追加されるときに SKU が割り当てられます。 データベースは、ホスティング サーバー プールの SKU 内に作成されます。

5. **[ログイン]** で ***[必要な設定の構成]*** を選択します。
6. **[ログインの選択]** で、既存のログインを選択するか、 **[+ 新しいログインの作成]** を選択します。  **[データベース ログイン]** 名と **[パスワード]** を入力して **[OK]** を選択します。

    ![新しいデータベース ログインの作成](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    >[!NOTE]
    >MySQL 5.7 で使用できるデータベース ログイン名は 32 文字以内です。 前のエディションでは、16 文字を超えることはできません。

7. **[作成]** を選択してデータベースの設定を完了します。

データベースの配置後、 **[要点]** に表示される **[接続文字列]** の情報を書き留めます。 この文字列は、MySQL データベースにアクセスする必要があるすべてのアプリケーションで使用できます。

![MySQL データベースの接続文字列の取得](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

## <a name="update-the-administrative-password"></a>管理パスワードの更新

パスワードは、MySQL サーバー インスタンス上で変更することで変更できます。

1. **[管理リソース]**  >  **[MySQL ホスティング サーバー]** を選択します。 ホスティング サーバーを選択します。
2. **[設定]** で **[パスワード]** を選択します。
3. **[パスワード]** に新しいパスワードを入力し、 **[保存]** を選択します。

![管理パスワードの更新](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="next-steps"></a>次の手順

[高可用性 MySQL データベースの提供](azure-stack-tutorial-mysql.md)方法を確認します。
