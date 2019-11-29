---
title: Azure Stack の AKS エンジンとは | Microsoft Docs
description: AKS エンジンのコマンドライン ツールを使用して、Azure および Azure Stack 上で Kubernetes クラスターをデプロイおよび管理する方法を学習します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na (Kubernetes)
ms.devlang: nav
ms.topic: article
ms.date: 11/21/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: cb531570191216ea13a0b1a7a6d36738c0765cd3
ms.sourcegitcommit: 0b783e262ac87ae67929dbd4c366b19bf36740f0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74310255"
---
# <a name="what-is-the-aks-engine-on-azure-stack"></a>Azure Stack の AKS エンジンとは

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

AKS エンジンのコマンドライン ツールを使用して、Azure および Azure Stack 上で Kubernetes クラスターをデプロイおよび管理することができます。 AKS エンジンを使用して、Azure Resource Manager ネイティブ クラスターを作成、アップグレード、スケーリングします。 このエンジンを使用して、接続されている環境と接続されていない環境の両方でクラスターをデプロイできます。 この記事では、AKS エンジンの概要、Azure Stack でエンジンを使用する際にサポートされるシナリオ、デプロイ、アップグレード、スケーリングなどの操作の概要について説明します。

## <a name="overview-of-the-aks-engine"></a>AKS エンジンの概要

[AKS エンジン](https://github.com/Azure/aks-engine)には、Azure および Azure Stack 上で Kubernetes クラスターをブートストラップするためのコマンドライン ツールが用意されています。 Azure Resource Manager を使用することで、AKS エンジンは、VM、仮想ネットワーク、および Azure Stack 内のその他のサービスとしてのインフラストラクチャ (IaaS) リソース上で実行されているクラスターの作成および維持に役立ちます。

## <a name="aks-engine-on-azure-stack-considerations"></a>Azure Stack の AKS エンジンの考慮事項

Azure Stack で AKS エンジンを使用する前に、Azure Stack と Azure の違いを理解しておくことが重要です。 このセクションでは、Azure Stack と AKS エンジンを使用して Kubernetes クラスターを管理する際のさまざまな機能と主な考慮事項について説明します。

Azure Stack の AKS エンジンの詳細と Azure との違いの詳細については、「[AKS engine on Azure Stack](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md)」 (Azure Stack の AKS エンジン) に関する記事を参照してください。

## <a name="supported-scenarios-with-the-aks-engine"></a>AKS エンジンでサポートされるシナリオ

Azure Stack サポート チームでは、次のシナリオがサポートされています。

1.  AKS エンジンでは、このドキュメントのガイドラインに従って、[次のテンプレート](https://github.com/Azure/aks-engine/tree/master/examples/azure-stack)を使用して、すべてのクラスター成果物がデプロイされます。
2.  AKS エンジンにより、既存の VNET にクラスターがデプロイされます。 詳細については、「[Using a custom virtual network with AKS engine](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/custom-vnet.md)」 (AKS エンジンでのカスタム仮想ネットワークの使用) を参照してください。
3.  [アップグレード](azure-stack-kubernetes-aks-engine-upgrade.md)と[スケール](azure-stack-kubernetes-aks-engine-scale.md)操作。

AKS エンジンと Azure Stack の詳細については、「[Azure Stack の AKS エンジンのサポート ポリシー](azure-stack-kubernetes-aks-engine-support.md)」を参照してください。

## <a name="install-the-aks-engine-and-deploy-a-kubernetes-cluster"></a>AKS エンジンをインストールし、Kubernetes クラスターをデプロイする

AKS エンジンを使用して Azure Stack に Kubernetes クラスターをデプロイするには、次の手順を行います。

1. [AKS エンジンの前提条件を設定する](azure-stack-kubernetes-aks-engine-set-up.md)
2. ご使用の Azure Stack 環境にアクセスできるマシンに AKS エンジンをインストールする
     - [Azure Stack の Windows に AKS エンジンをインストールする](azure-stack-kubernetes-aks-engine-deploy-windows.md)
     - [Azure Stack の Linux に AKS エンジンをインストールする](azure-stack-kubernetes-aks-engine-deploy-linux.md)
3. [AKS エンジンを使用して Azure Stack に Kubernetes クラスターをデプロイする](azure-stack-kubernetes-aks-engine-deploy-cluster.md)

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [AKS エンジンの前提条件を設定する](azure-stack-kubernetes-aks-engine-set-up.md)