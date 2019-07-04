---
title: クォータ、プラン、オファーおよびサブスクリプションの削除 |Microsoft Docs
description: Azure Stack のクォータ、プラン、オファーおよびサブスクリプションを削除する方法について説明します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 04/25/2019
ms.openlocfilehash: 2e0e4ef7abd1885d843832ed7cc9e845003d0ed7
ms.sourcegitcommit: 593d40bccf1b2957a763017a8a2d7043f8d8315c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152530"
---
# <a name="delete-quotas-plans-offers-and-subscriptions"></a>クォータ、プラン、オファーおよびサブスクリプションの削除

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

この記事では、もう必要ではなくなったクォータ、プラン、オファーおよびサブスクリプションを削除する方法について説明します。 一般原則として、削除できるのは使用されていないものだけです。 たとえば、オファーを削除できるのは、そのオファーに属するサブスクリプションがない場合のみです。

サブスクリプションは、この一般原則の例外であり、リソースが含まれているサブスクリプションを削除でき、リソースはサブスクリプションと共に削除されます。

このため、クォータを削除したい場合は、そのクォータ使用しているプランやオファーをすべて見直す必要があります。オファーから開始し、サブスクリプションがないことを確認し、各オファーを削除してから、クォータを使用しているプランを削除するといった具合です。

## <a name="delete-a-subscription"></a>サブスクリプションを削除する

サブスクリプションを削除するには、 **[すべてのサービス]** 、 **[ユーザー サブスクリプション]** の順に選択して、システム上のすべてのサブスクリプションの一覧を表示します。 オファーを操作している場合は、そこから **[サブスクリプション]** を選択することもできます。

この一覧からサブスクリプションを削除することも、PowerShell で、「[サブスクリプション - 削除](/rest/api/azurestack/subscriptions/delete)」リファレンスに記載されているコマンドを使用して、すべてのサブスクリプションを削除するスクリプトを記述することもできます。

> [!CAUTION]
> サブスクリプションを削除すると、それに含まれているデータおよびリソースも削除されます。

## <a name="delete-an-offer"></a>オファーを削除する

オファーを削除するには、管理者ポータルで、 **[すべてのサービス]** 、 **[オファー]** の順に移動します。 削除したいオファーを選択して、 **[削除]** を選択します。

![delsub1](media/azure-stack-delete-offer/delsub1.png)

オファーを削除できるのは、それを使用しているサブスクリプションがない場合のみです。 オファーに基づくサブスクリプションが存在する場合、 **[削除]** オプションはグレー表示になります。この場合、「[サブスクリプションを削除する](#delete-a-subscription)」セクションを参照してください。

## <a name="delete-a-plan"></a>プランを削除する

プランを削除するには、管理者ポータルで、 **[すべてのサービス]** 、 **[プラン]** の順に移動します。 削除したいプランを選択して、 **[削除]** を選択します。

![delsub2](media/azure-stack-delete-offer/delsub2.png)

プランを削除できるのは、それを使用しているオファーまたはサブスクリプションがない場合のみです。 プランを使用するオファーがある場合は、プランの削除を行って、それに失敗すると、エラー メッセージが表示されます。 **[Parent offers]\(親のオファー\)** を選択すると、プランを使用しているオファーの一覧を表示できます。 オファーの削除に関する詳細については、「[オファーを削除する](#delete-an-offer)」を参照してください。

プランは、オファーの一部でない場合でも、アドオンのプランとしてサブスクリプションに直接追加されている可能性があります。 この場合、プランを削除する前に、それらを使用しているサブスクリプションから削除する必要があります。

また、プランがサブスクリプションの特定のリソースの唯一のソースである場合、プランはそのサブスクリプションから削除できません。 たとえば、プラン A が、サブスクリプション 1 に追加されていて、そのサブスクリプションにネットワーク クォータを提供する唯一のプランである場合、プラン A をサブスクリプションから削除することはできません。 そのため、それを削除することはできません。

## <a name="edit-and-delete-a-quota"></a>クォータを編集および削除する

管理者ポータルを使用して既存のクォータを表示および編集できます。 **[Region Management]\(リージョン管理\)** を選択してから、関連するリソース プロバイダーを選択して、 **[クォータ]** をクリックします。 特定のリソース プロバイダーのクォータを削除することもできます。

![delsub3](media/azure-stack-delete-offer/delsub3.png)

あるいは、次の REST API を使用して、特定のクォータを削除できます。

- [Compute](/rest/api/azurestack/quotas%20(compute)/delete)
- [ネットワーク](/rest/api/azurestack/quotas%20(network)/delete)
- [Storage](/rest/api/azurestack/storagequotas/delete)

> [!NOTE]
> クォータを使用する現在のプランがある場合、そのクォータは削除できません。 まず、クォータを参照しているプランを削除する必要があります。

## <a name="next-steps"></a>次の手順

- [サブスクリプションを作成する](azure-stack-subscribe-plan-provision-vm.md)
- [仮想マシンのプロビジョニング](../user/azure-stack-create-vm-template.md)