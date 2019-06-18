---
title: Azure Stack の登録ロールを作成する方法
description: 登録にグローバル管理者の使用を回避するためにカスタム ロールを作成する方法。
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2019
ms.author: patricka
ms.reviewer: rtiberiu
ms.lastreviewed: 06/10/2019
ms.openlocfilehash: 50c15403ec4bf41f7513af30d2dca53310d45298
ms.sourcegitcommit: af63214919e798901399fdffef09650de4176956
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "66828245"
---
# <a name="create-a-custom-role-for-azure-stack-registration"></a>Azure Stack 登録用のカスタム ロールを作成する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

> [!WARNING]
> これは、セキュリティ体制の機能ではありません。 Azure サブスクリプションが誤って変更されないようにする制約が必要なシナリオでは、これを使用します。 ユーザーがこのカスタム ロールに対する権限を委任されている場合、そのユーザーはアクセス許可の編集と権利の昇格の権限を持ちます。 カスタム ロールには信頼できるユーザーのみを割り当ててください。

Azure Stack の登録時には、Azure Active Directory アカウントでサインインする必要があります。 そのアカウントには、次の Azure Active Directory のアクセス許可と Azure サブスクリプションのアクセス許可が必要です。

* **Azure Active Directory テナントでのアプリケーションの登録アクセス許可:** 管理者にはアプリケーション登録アクセス許可があります。 ユーザーのアクセス許可は、テナントのすべてのユーザーに対するグローバル設定です。 この設定を確認または変更する場合は、「[リソースにアクセスできる Azure AD アプリケーションとサービス プリンシパルをポータルで作成する](/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions)」を参照してください。

    *ユーザーがアプリケーションを登録できる*設定は、ユーザー アカウントで Azure Stack を登録できるようにするため、 **[はい]** に設定する必要があります。 アプリ登録設定が **[いいえ]** に設定されている場合は、ユーザー アカウントを使用できず、グローバル管理者アカウントを使用して Azure Stack を登録する必要があります。

* **Azure サブスクリプションの十分なアクセス許可のセット:** 所有者ロールに属しているユーザーには、十分なアクセス許可があります。 他のアカウントの場合は、次のセクションで説明するようにカスタム ロールを割り当てることで、アクセス許可セットを割り当てることができます。

Azure サブスクリプションの所有者アクセス許可を持つアカウントを使う代わりに、カスタム ロールを作成して、特権が低いユーザー アカウントにアクセス許可を割り当てることができます。 その後、このアカウントを使って、Azure Stack を登録できます。

## <a name="create-a-custom-role-using-powershell"></a>PowerShell を使用してカスタム ロールを作成する

カスタム ロールを作成するには、[所有者](/azure/role-based-access-control/built-in-roles#owner)や[ユーザー アクセス管理者](/azure/role-based-access-control/built-in-roles#user-access-administrator)など、すべての `AssignableScopes` に対する `Microsoft.Authorization/roleDefinitions/write` アクセス許可が必要になります。 カスタム ロールの作成を簡素化するには、次の JSON テンプレートを使います。 このテンプレートは、Azure Stack 登録のための読み取りおよび書き込みアクセスを許可するカスタム ロールを作成します。

1. JSON ファイルを作成します。 たとえば、`C:\CustomRoles\registrationrole.json` のように入力します。
2. 次の JSON をファイルに追加します。 `<SubscriptionID>` は、Azure サブスクリプション ID に置き換えてください。

    ```json
    {
      "Name": "Azure Stack registration role",
      "Id": null,
      "IsCustom": true,
      "Description": "Allows access to register Azure Stack",
      "Actions": [
        "Microsoft.Resources/subscriptions/resourceGroups/write",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.AzureStack/registrations/*",
        "Microsoft.AzureStack/register/action",
        "Microsoft.Authorization/roleAssignments/read",
        "Microsoft.Authorization/roleAssignments/write",
        "Microsoft.Authorization/roleAssignments/delete",
        "Microsoft.Authorization/permissions/read"
      ],
      "NotActions": [
      ],
      "AssignableScopes": [
        "/subscriptions/<SubscriptionID>"
      ]
    }
    ```

3. PowerShell で、Azure Resource Manager を使用するために Azure に接続します。 メッセージが表示されたら、[所有者](/azure/role-based-access-control/built-in-roles#owner)または[ユーザー アクセス管理者](/azure/role-based-access-control/built-in-roles#user-access-administrator)などの十分なアクセス許可を持つアカウントを使用して認証します。

    ```azurepowershell
    Connect-AzureRmAccount
    ```

4. カスタム ロールを作成するには、**New-AzureRmRoleDefinition** を使用して JSON テンプレート ファイルを指定します。

    ``` azurepowershell
    New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\registrationrole.json"
    ```

## <a name="assign-a-user-to-registration-role"></a>ユーザーを登録ロールに割り当てる

登録のカスタム ロールを作成した後、Azure Stack の登録に使用するユーザー アカウントにそのロールを割り当てます。

1. 権限 ([所有者](/azure/role-based-access-control/built-in-roles#owner)や[ユーザー アクセス管理者](/azure/role-based-access-control/built-in-roles#user-access-administrator)など) を委任するのに十分な Azure サブスクリプションのアクセス許可を持つアカウントでサインインします。
2. **[サブスクリプション]** で、 **[アクセス制御 (IAM)] > [ロール割り当ての追加]** の順に選択します。
3. **[ロール]** で、*Azure Stack 登録ロール*を作成したカスタム ロールを選択します。
4. ロールに割り当てるユーザーを選択します。
5. **[保存]** を選択して、選択したユーザーをロールに割り当てます。

    ![ロールに割り当てるユーザーを選択する](media/azure-stack-registration-role/assign-role.png)

カスタム ロールの使用の詳細については、「[RBAC と Azure portal を使用してアクセスを管理する](/azure/role-based-access-control/role-assignments-portal)」を参照してください。

## <a name="next-steps"></a>次の手順

[Azure Stack を Azure に登録する](azure-stack-registration.md)
