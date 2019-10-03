---
title: Azure Stack でオファーを使用してサブスクリプションを作成する | Microsoft Docs
description: Azure Stack でオファーを使用して新しいサブスクリプションを作成し、テスト VM でオファーをテストする方法について説明します。
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/04/2019
ms.author: bryanla
ms.reviewer: efemmano
ms.lastreviewed: 11/13/2018
ms.openlocfilehash: f47278fed9cd9e93024d37a0a433388594bbd124
ms.sourcegitcommit: 1bae55e754d7be75e03af7a4db3ec43fd7ff3e9c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71319169"
---
# <a name="tutorial-create-and-test-a-subscription-in-azure-stack"></a>チュートリアル:Azure Stack でサブスクリプションを作成してテストする

このチュートリアルでは、オファーが含まれているサブスクリプションを作成する方法とそのテスト方法を示します。 テストの場合は、クラウド管理者として Azure Stack ユーザー ポータルにサインインし、オファーにサブスクライブしてから、仮想マシン (VM) を作成します。

> [!TIP]
> より高度な評価エクスペリエンスを使用するには、[特定のユーザーのサブスクリプションを作成](../operator/azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator)し、ユーザー ポータルでそのユーザーとしてサインインします。

このチュートリアルでは、Azure Stack のオファーにサブスクライブする方法について説明します。

ここでは、次の内容について学習します。

> [!div class="checklist"]
> * プランへのサブスクライブ 
> * オファーのテスト

## <a name="subscribe-to-an-offer"></a>プランへのサブスクライブ

ユーザーとしてオファーにサブスクライブするには、Azure Stack ユーザー ポータルにサインインして、Azure Stack オペレーターから提供される利用可能なサービスを確認します。

1. ユーザー ポータルにサインインし、 **[サブスクリプションの取得]** を選択します。

   ![サブスクリプションの取得](media/azure-stack-subscribe-services/get-subscription.png)

2. **[表示名]** フィールドに、お客様のサブスクリプションの名前を入力します。 次に、 **[Offer]\(オファー\)** を選択し、 **[Choose an offer]\(オファーの選択\)** セクションで利用可能なオファーの 1 つを選択します。 **[作成]** を選択します。

   ![オファーの作成](media/azure-stack-subscribe-services/create-subscription.png)

   > [!TIP]
   > サブスクリプションの使用を開始するには、ユーザー ポータルを更新します。

3. 作成したサブスクリプションを表示するには、 **[すべてのサービス]** を選択します。 その後、 **[一般]** カテゴリで **[サブスクリプション]** を選択し、新しいサブスクリプションを選択します。 オファーにサブスクライブしたら、ポータルを更新して、サービスが新しいサブスクリプションの一部として含まれているかどうかを確認します。 この例では、**仮想マシン**が追加されています。

   ![サブスクリプションの表示](media/azure-stack-subscribe-services/view-subscription.png)

## <a name="test-the-offer"></a>オファーのテスト

ユーザー ポータルにサインインしている間に、新しいサブスクリプション機能を使用して VM をプロビジョニングすることで、オファーをテストします。

> [!NOTE]
> このテストでは、まず Windows Server 2016 Datacenter VM を Azure Stack マーケットプレースに追加する必要があります。

1. ユーザー ポータルにサインインします。

2. ユーザー ポータルで、 **[仮想マシン]** 、 **[追加]** 、 **[Windows Server 2016 Datacenter]** の順に選択し、 **[作成]** を選択します。

3. **[基本]** セクションで、 **[名前]** 、 **[ユーザー名]** 、 **[パスワード]** を入力し、 **[サブスクリプション]** を選択します。次に、 **[リソース グループ]** を作成し (または既存のものを選択し)、 **[OK]** を選択します。

4. **[サイズの選択]** セクションで、 **[Standard A1]** を選択してから **[選択]** を選びます。  

5. **[設定]** ブレードで、既定値のまま **[OK]** を選択します。

6. **[概要]** セクションで、 **[OK]** を選択して VM を作成します。  

7. 新しい VM を表示するには、 **[仮想マシン]** を選択してから新しい VM を検索し、その名前を選択します。

    ![すべてのリソース](media/azure-stack-subscribe-services/view-vm.png)

> [!NOTE]
> VM のデプロイが完了するまでに数分かかる場合があります。

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [コミュニティ テンプレートからの VM の作成](azure-stack-create-vm-template.md)