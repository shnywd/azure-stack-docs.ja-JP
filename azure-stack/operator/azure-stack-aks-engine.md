---
title: Azure Kubernetes Services (AKS) エンジンの前提条件を Azure Stack Marketplace に追加する | Microsoft Docs
description: AKS エンジンの前提条件を Azure Stack Marketplace に追加する方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 09/25/2019
ms.openlocfilehash: bcb9dab55d00c48e5b77e7184fae0567ff1f620e
ms.sourcegitcommit: 5e53eb5d43d28ab07b4f84891dd269bbfcf65622
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/26/2019
ms.locfileid: "71311283"
---
# <a name="add-the-azure-kubernetes-services-aks-engine-prerequisites-to-the-azure-stack-marketplace"></a>Azure Kubernetes Services (AKS) エンジンの前提条件を Azure Stack Marketplace に追加する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

この記事で説明する項目を Azure Stack に追加することによって、ユーザーが Azure Kubernetes Services (AKS) エンジンを設定できるようになります。 ユーザーはその後、1 回の連携した操作で Kubernetes クラスターをデプロイできます。 この記事では、接続されている環境と接続されていない環境の両方で、ユーザーが AKS エンジンを使用できるようにするために必要な手順について説明します。 AKS エンジンは、サービス プリンシパル ID と、Marketplace のカスタム スクリプト拡張機能および AKS 基本イメージに依存します。

[AKS エンジン](https://github.com/Azure/aks-engine)は、AKS 基本イメージというビルドされたイメージを使用します。 AKS エンジンのバージョンは、Azure Stack で使用可能にできる特定のイメージ バージョンによって異なります。 「[Supported Kubernetes Versions (サポートされている Kubernetes バージョン)](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#supported-kubernetes-versions)」で、AKS エンジンのバージョンと、これに対応する Kubernetes のバージョンが記載された表をご確認ください。

> [!IMPORTANT]
> AKS エンジンは、現在、パブリック プレビュー段階にあります。
> このプレビュー バージョンはサービス レベル アグリーメントなしで提供されています。運用環境のワークロードに使用することはお勧めできません。 特定の機能はサポート対象ではなく、機能が制限されることがあります。 詳しくは、[Microsoft Azure プレビューの追加使用条件](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)に関するページをご覧ください。

## <a name="check-your-users-service-offering"></a>ユーザーのサービス オファリングを確認する

ユーザーには、十分な領域がある Azure Stack のプラン、オファー、およびサブスクリプションが必要です。 多くの場合、ユーザーは、3 つのマスター ノードと 3 つのワーカー ノードで構成される最大 6 台の仮想マシンによるクラスターをデプロイする必要があります。 ユーザーにとって十分なクォータがあることを確認する必要があります。

サービス オファリングの計画と設定に関する詳細については、「[Azure Stack でのサービスの提供の概要](azure-stack-offer-services-overview.md)」を参照してください

## <a name="create-a-service-principal-and-credentials"></a>サービス プリンシパルと資格情報を作成する

Kubernetes クラスターには、Azure Stack でのサービス プリンシパル (SPN) とロールベースのアクセス許可が必要です。

### <a name="create-an-spn-in-azure-ad"></a>Azure AD で SPN を作成する

ID 管理サービスのために Azure Active Directory (Azure AD) を使用する場合は、Kubernetes クラスターをデプロイするユーザーのサービス プリンシパルを作成する必要があります。 クライアント シークレットを使用してサービス プリンシパルを作成します。 手順については、「[クライアント シークレット資格情報を使用するサービス プリンシパルを作成する](azure-stack-create-service-principals.md#create-a-service-principal-that-uses-a-client-secret-credential)」を参照してください。

### <a name="create-an-spn-in-ad-fs"></a>AD FS で SPN を作成する

ID 管理サービスのために Active Directory Federated Services (AD FS) を使用する場合は、Kubernetes クラスターをデプロイするユーザーのサービス プリンシパルを作成する必要があります。 クライアント シークレットを使用してサービス プリンシパルを作成します。 手順については、[クライアント シークレットを使用したサービス プリンシパルの作成](azure-stack-create-service-principals.md#create-a-service-principal-that-uses-client-secret-credentials)に関するセクションを参照してください。

## <a name="add-the-aks-base-image"></a>AKS 基本イメージを追加する

Azure から項目を取得して、AKS 基本イメージを Marketplace に追加できます。 ただし、Azure Stack が切断されている場合は、次の手順に従って、[Azure から Marketplace の項目をダウンロード](https://docs.microsoft.com/azure-stack/operator/azure-stack-download-azure-marketplace-item?view=azs-1908#disconnected-or-a-partially-connected-scenario)し、項目を追加します。 手順 5. で指定した項目を追加します。

次の項目を Marketplace に追加します。

1. [管理ポータル](https://adminportal.local.azurestack.external)にサインインします。

1. **[すべてのサービス]** を選択し、 **[管理]** カテゴリで **[Marketplace management] (Marketplace 管理)** を選択します。

1. **+ Add from Azure**(+ Azure から追加) を選択します。

1. 「 `AKS Base Image` 」を入力します。

1. AKS エンジンのバージョンと一致するイメージ バージョンを選択します。 AKS エンジンと AKS 基本イメージの対応一覧については、「[Supported Kubernetes Versions (サポートされている Kubernetes バージョン)](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#supported-kubernetes-versions)」を参照してください。 

    一覧で、次のように選択します。
    - **発行元**: microsoft-aks
    - **オファー**: aks
    - **バージョン**:2019.07.30 (または AKS エンジンにマップされているバージョン)
    - **SKU**: aks-ubuntu-1604-201907

1. **[ダウンロード]** を選択します。

## <a name="add-a-custom-script-extension"></a>カスタム スクリプト拡張機能を追加する

Azure から項目を取得して、カスタム スクリプトを Marketplace に追加できます。 ただし、Azure Stack が切断されている場合は、次の手順に従って、[Azure から Marketplace の項目をダウンロード](https://docs.microsoft.com/azure-stack/operator/azure-stack-download-azure-marketplace-item?view=azs-1908#disconnected-or-a-partially-connected-scenario)し、項目を追加します。  手順 5. で指定した項目を追加します。

1. [管理ポータル](https://adminportal.local.azurestack.external)を開きます。

1. **[すべてのサービス]** を選択し、 **[管理]** カテゴリで **[Marketplace management] (Marketplace 管理)** を選択します。

1. **+ Add from Azure**(+ Azure から追加) を選択します。

1. 「 `Custom Script for Linux` 」を入力します。

1. 次のプロファイルを持つスクリプトを選択します。
   - **プラン**: Linux 2.0 用のカスタム スクリプト
   - **バージョン**:2.0.6 (または最新バージョン)
   - **[発行者]** : Microsoft Corp

     > [!Note]  
     > 複数のバージョンの Linux 用カスタム スクリプトが表示されることがあります。 最新バージョンの項目を追加する必要があります。

1. **[ダウンロード]** を選択します。

## <a name="next-steps"></a>次の手順

[Azure Stack の AKS エンジンとは](../user/azure-stack-kubernetes-aks-engine-overview.md)

[Azure Stack でのサービスの提供の概要](azure-stack-offer-services-overview.md)
