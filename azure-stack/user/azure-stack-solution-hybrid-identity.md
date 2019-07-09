---
title: Azure と Azure Stack アプリでハイブリッド クラウド ID を構成する | Microsoft Docs
description: Azure と Azure Stack アプリでハイブリッド クラウド ID を構成する方法について説明します。
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/26/2019
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 06/26/2019
ms.openlocfilehash: 074d971c1f951797b5dc2d53a62eef56d0b7249f
ms.sourcegitcommit: eccbd0098ef652919f357ef6dba62b68abde1090
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/01/2019
ms.locfileid: "67492324"
---
# <a name="tutorial-configure-hybrid-cloud-identity-for-azure-and-azure-stack-applications"></a>チュートリアル:Azure および Azure Stack アプリケーションのハイブリッド クラウド ID を構成する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure と Azure Stack アプリのハイブリッド クラウド ID を構成する方法について説明します。

グローバル Azure と Azure Stack の両方でアプリへのアクセスを許可するには、2 つのオプションがあります。

 * Azure Stack がインターネットに継続的に接続している場合、Azure Active Directory (Azure AD) を使用できます。
 * Azure Stack がインターネットから切断されているときは、Active Directory フェデレーション サービス (AD FS) を使用できます。

Azure Stack 内の Azure Resource Manager を使用したデプロイまたは構成のために、サービス プリンシパルを使用し、Azure Stack アプリへのアクセスを許可します。

このチュートリアルでは、以下を実現するためのサンプル環境を構築します。

> [!div class="checklist"]
> - グローバル Azure および Azure Stack でハイブリッド ID を確立する
> - Azure Stack API にアクセスするためのトークンを取得します。

このチュートリアルの手順を行うには、Azure Stack オペレーターのアクセス許可が必要です。

> [!Tip]  
> ![hybrid-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack は Azure の拡張機能です。 Azure Stack はオンプレミス環境にクラウド コンピューティングの機敏性とイノベーションをもたらし、ハイブリッド アプリをビルドし、どこにでもデプロイできる唯一のハイブリッド クラウドを可能にします。  
> 
> [ハイブリッド アプリケーションのための設計の考慮事項](https://aka.ms/hybrid-cloud-applications-pillars)に関するホワイト ペーパーでは、ハイブリッド アプリケーションを設計、デプロイ、および運用するためのソフトウェア品質の重要な要素 (配置、スケーラビリティ、可用性、回復性、管理容易性、およびセキュリティ) についてレビューしています。 これらの設計の考慮事項は、ハイブリッド アプリケーションの設計を最適化したり、運用環境での課題を最小限に抑えたりするのに役立ちます。


## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>ポータルで Azure AD のサービス プリンシパルを作成する

Azure AD を ID ストアとして使用して Azure Stack をデプロイした場合は、Azure での手順と同様の方法でサービス プリンシパルを作成できます。 [アプリ ID を使用したリソースへのアクセスの提供](../operator/azure-stack-create-service-principals.md#manage-an-azure-ad-service-principal)に関するページには、ポータルから手順を実行する方法が紹介されています。 始める前に、[Azure AD で必要なアクセス許可](/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions)があることを確認してください。

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>PowerShell を使用して AD FS のサービス プリンシパルを作成する

AD FS を使用して Azure Stack をデプロイした場合は、PowerShell を使ってサービス プリンシパルを作成し、アクセスのロールを割り当てて、その ID を使用して PowerShell からサインインできます。 「[アプリ ID を使用してリソースにアクセスする](../operator/azure-stack-create-service-principals.md#manage-an-ad-fs-service-principal)」には、PowerShell で必要な手順を実行する方法が紹介されています。

## <a name="using-the-azure-stack-api"></a>Azure Stack API を使用する

Azure Stack API にアクセスするためのトークンを取得するプロセスについては、[Azure Stack API](azure-stack-rest-api-use.md) に関するチュートリアルで説明されています。

## <a name="connect-to-azure-stack-using-powershell"></a>PowerShell を使用して Azure Stack に接続する

[Azure Stack での PowerShell の設定と実行](../operator/azure-stack-powershell-install.md)に関するクイック スタートでは、Azure PowerShell をインストールして、Azure Stack のインストールに接続するために必要な手順が説明されています。

### <a name="prerequisites"></a>前提条件

アクセスできるサブスクリプションの Azure Active Directory に接続された Azure Stack インストールが必要です。 Azure Stack のインストールがない場合は、この手順に従って [Azure Stack Development Kit](../asdk/asdk-install.md) を設定できます。

#### <a name="connect-to-azure-stack-using-code"></a>コードを使用して Azure Stack に接続する

コードを使用して Azure Stack に接続するには、Azure Resource Manager エンドポイント API を使用して、Azure Stack インストールの認証およびグラフ エンドポイントを取得し、REST 要求を使用して認証します。 サンプル クライアント アプリケーションは [GitHub](https://github.com/shriramnat/HybridARMApplication) で見つかります。

>[!Note]
>選択した言語の Azure SDK で Azure API プロファイルがサポートされていない限り、SDK は Azure Stack で動作しません。 Azure API プロファイルについて詳しくは、「[Azure Stack での API バージョンのプロファイルの管理](azure-stack-version-profiles.md)」をご覧ください。

## <a name="next-steps"></a>次の手順

 - Azure Stack での ID の処理方法の詳細については、「[Azure Stack の ID アーキテクチャ](../operator/azure-stack-identity-architecture.md)」を参照してください。
 - Azure のクラウド パターンの詳細については、「[クラウド設計パターン](https://docs.microsoft.com/azure/architecture/patterns)」を参照してください。
