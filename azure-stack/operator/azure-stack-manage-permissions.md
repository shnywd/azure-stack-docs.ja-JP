---
title: Azure Stack でユーザーごとにリソースへのアクセス許可を管理する (サービス管理者およびテナント) | Microsoft Azure
description: サービス管理者またはテナントとして、RBAC アクセス許可を管理する方法を説明します。
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
ms.date: 05/16/2019
ms.author: patricka
ms.reviewer: thoroet
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 9fc4e6bb48c8cdd7508a1a85d193a22761c62e99
ms.sourcegitcommit: 797dbacd1c6b8479d8c9189a939a13709228d816
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2019
ms.locfileid: "66268597"
---
# <a name="manage-role-based-access-control"></a>ロールベースのアクセス制御の管理

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure Stack のユーザーは、サブスクリプション、リソース グループ、またはサービスの各インスタンスの閲覧者、所有者、または共同作業者になることができます。 たとえば、ユーザー A にはサブスクリプション 1 に対する閲覧者アクセス許可と、仮想マシン 7 に対する所有者アクセス許可がある可能性があります。

 - 閲覧者: ユーザーはすべてを閲覧できますが、変更を加えることはできません。
 - 共同作成者: ユーザーはリソースへのアクセス以外のすべてを管理できます。
 - 所有者: ユーザーは、リソースへのアクセスを含め、すべてを管理できます。

## <a name="set-access-permissions-for-a-user"></a>ユーザーのアクセス権限の設定

1. 管理するリソースへの所有者アクセス許可があるアカウントでサインインします。
2. リソースのブレードで、 **[アクセス]** アイコン ![](media/azure-stack-manage-permissions/image1.png) をクリックします。
3. **[ユーザー]** ブレードで、 **[役割]** をクリックします。
4. **[役割]** ブレードで、 **[追加]** をクリックし、ユーザーのアクセス許可を追加します。

## <a name="set-access-permissions-for-a-universal-group"></a>ユニバーサル グループのアクセス許可を設定する 

> [!Note]
> Active Directory フェデレーション サービス (AD FS) にのみ適用されます。

1. 管理するリソースへの所有者アクセス許可があるアカウントでサインインします。
2. リソースのブレードで、 **[アクセス]** アイコン ![](media/azure-stack-manage-permissions/image1.png) をクリックします。
3. **[ユーザー]** ブレードで、 **[役割]** をクリックします。
4. **[役割]** ブレードで、 **[追加]** をクリックして、ユニバーサル グループ Active Directory グループのアクセス許可を追加します。

## <a name="next-steps"></a>次の手順

[Azure Stack テナントの追加](azure-stack-add-new-user-aad.md)