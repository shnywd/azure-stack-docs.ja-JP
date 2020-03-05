---
title: AD FS に Azure Stack Hub ユーザーを追加する
description: Active Directory フェデレーション サービス (AD FS) デプロイに Azure Stack Hub ユーザーを追加する方法について説明します。
author: IngridAtMicrosoft
ms.topic: article
ms.date: 06/03/2019
ms.author: inhenkel
ms.reviewer: unknown
ms.lastreviewed: 06/03/2019
ms.openlocfilehash: 79380d40282757e704f9dc0ef3bdaf977253ba14
ms.sourcegitcommit: 4ac711ec37c6653c71b126d09c1f93ec4215a489
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77703351"
---
# <a name="add-a-new-azure-stack-hub-user-account-in-active-directory-federation-services-ad-fs"></a>新しい Azure Stack Hub ユーザー アカウントを Active Directory フェデレーション サービス (AD FS) に追加する

**[Active Directory ユーザーとコンピューター]** スナップインを使用して、AD FS を ID プロバイダーとして利用している Azure Stack Hub 環境にユーザーを追加できます。

## <a name="add-windows-server-active-directory-users"></a>Windows Server Active Directory ユーザーを追加する

1. Windows 管理ツールにアクセスできるアカウントを使用してコンピューターにサインインし、新しい Microsoft 管理コンソール (MMC) を開きます。
2. **[ファイル]、[スナップインの追加と削除]** の順に選択します。

   > [!TIP]
   > *directory-domain* をディレクトリと一致するドメインに置き換えます。 

3. **[Active Directory ユーザーとコンピューター]**  >  *[directory-domain]*  >  **[ユーザー]** の順に選択します。
4. **[操作]**  >  **[新規作成]**  >  **[ユーザー]** の順に選択します。
5. [新しいオブジェクト - ユーザー] で、ユーザーの詳細を入力します。 **[次へ]** を選択します。
6. パスワードの指定と確認入力を行います。
7. **[次へ]** を選択して、値を入力します。 **[完了]** を選択して、ユーザーを作成します。


## <a name="next-steps"></a>次のステップ

[アプリ ID を作成して Azure Stack Hub リソースにアクセスする](azure-stack-create-service-principals.md)