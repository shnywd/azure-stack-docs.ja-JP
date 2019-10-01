---
title: Azure Stack の AKS エンジンの前提条件を設定する | Microsoft Docs
description: Azure Stack で AKS エンジンを実行するための前提条件を確立します。
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
ms.date: 09/14/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 09/14/2019
ms.openlocfilehash: cc278020bb4d2ba530e20b6f2f56bcb841207bbc
ms.sourcegitcommit: 3af71025e85fc53ce529de2f6a5c396b806121ed
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/20/2019
ms.locfileid: "71159736"
---
# <a name="set-up-the-prerequisites-for-the-aks-engine-on-azure-stack"></a>Azure Stack の AKS エンジンの前提条件を設定する

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

AKS エンジンは、ご自分の環境内の VM、または Azure Stack Resource Manager エンドポイントにアクセスできる任意のクライアント マシンにインストールできます。 エンジンを実行する前に、次のものを用意する必要があります: サブスクリプションで使用できる AKS ベースの Ubuntu サーバーと Linux カスタム スクリプト拡張機能、共同作成者ロールに割り当てられているサービス プリンシパル ID、ご自分の Ubuntu サーバーにアクセスするための SSH 用秘密/公開キー。 また、Azure Stack Development Kit を使用している場合は、ご自分のマシンで適切な証明書を信頼する必要があります。

前提条件がある場合は、[クラスターの定義](azure-stack-kubernetes-aks-engine-deploy-cluster.md)を開始することができます。

Azure Stack のクラウド オペレーターとして AKS エンジンを提供する場合は、[Azure Stack Marketplace への AKS エンジンの追加](../operator/azure-stack-aks-engine.md)に関するページに記載されている手順に従ってください。

## <a name="prerequisites-for-the-aks-engine"></a>AKS エンジンの前提条件

AKS エンジンを使用するには、次のリソースが使用可能である必要があります。 AKS エンジンは、テナント サブスクリプションに Kubernetes クラスターをデプロイするために Azure Stack のテナントによって使用されることを念頭に置いてください。 Azure Stack オペレーターの関与が必要になるのは、マーケットプレースの項目をダウンロードし、サービス プリンシパル ID を作成するところのみです。 詳細については、次の表を参照してください。

| 前提条件 | 説明 | 必須 | Instructions |
| --- | --- | --- | --- |
| Linux カスタム スクリプト拡張機能 | Linux カスタム スクリプト拡張機能 2.0<br>オファー: Linux 2.0 用のカスタム スクリプト<br>バージョン:2.0.6 (または最新バージョン)<br>発行元: Microsoft Corp | 必須 | サブスクリプションにこの項目がない場合は、クラウド オペレーターに問い合わせてください。 |
| AKS 基本 Ubuntu イメージ | AKS 基本イメージ<br>オファー: aks<br>バージョン:2019.07.30 (またはそれ以降のバージョン)<br>発行元: microsoft-aks<br>SKU: aks-ubuntu-1604-201907 | 必須 | サブスクリプションにこの項目がない場合は、クラウド オペレーターに問い合わせてください。 バージョンの依存関係の詳細については、「[Matching engine to base image version](#matching-engine-to-base-image-version)」(エンジンと基本イメージバージョンの照合) を参照してください。 |
| Azure Stack サブスクリプション | サブスクリプションを使用して、Azure Stack 内のオファーにアクセスします。 オファーには、利用可能なサービスが含まれています。 | 必須 | Azure Stack でテナントのワークロードをデプロイするには、最初に [Azure Stack サブスクリプション](https://docs.microsoft.com/azure-stack/user/azure-stack-subscribe-services)を取得する必要があります。 |
| サービス プリンシパル ID (SPN) |  アプリケーションのリソースのデプロイや構成を Azure Resource Manager を通じて行う必要がある場合は、そのアプリケーションをサービス プリンシパルで表す必要があります。 | 必須 | この項目については、Azure Stack オペレーターへの問い合わせが必要な場合があります。  手順については、「[アプリ ID を使用してリソースにアクセスする](https://docs.microsoft.com/azure-stack/operator/azure-stack-create-service-principals)」を参照してください |
| (SPN) 割り当て済み**共同作成者**ロール | 自分のサブスクリプションに含まれるリソースに、アプリケーションからサービス プリンシパルを使用してアクセスできるようにするには、そのサービス プリンシパルを特定のリソースに対するロールに割り当てる必要があります。 | 必須 | 手順については、「[ロールの割り当て](https://docs.microsoft.com/azure-stack/operator/azure-stack-create-service-principals#assign-a-role)」を参照してください |
| Resource group | リソース グループは、Azure ソリューションの関連するリソースを保持するコンテナーです。 既存のリソース グループを指定しない場合は、ツールによって作成されます。 | 省略可能 | [Azure portal を使用して Azure Resource Manager リソース グループを管理する](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-portal) |
| 秘密/公開キー | 開発用マシンから、ご自分の Web アプリをホストする Azure Stack インスタンス内のサーバー VM へのオープン SSH 接続を使用するには、SSH (Secure Shell) 公開キーと秘密キーのペアを作成する必要があります。 | 必須 | キーを生成する手順については、[SSH キーの生成](https://docs.microsoft.com/azure-stack/user/azure-stack-dev-start-howto-ssh-public-key)に関するページを参照してください。|

> [!Note]  
> [Azure Stack 用の Azure CLI](https://docs.microsoft.com/azure-stack/user/azure-stack-version-profiles-azurecli2) または [Azure Stack PowerShell](https://docs.microsoft.com/azure-stack/operator/azure-stack-powershell-install) で AKS エンジンを使用するための前提条件を作成することもできます。

## <a name="matching-engine-to-base-image-version"></a>エンジンと基本イメージバージョンの照合

AKS エンジンは、**AKS 基本イメージ**というビルドされたイメージを使用します。 すべての AKS エンジンのバージョンは、Azure Stack オペレーターが Azure Stack で使用できる特定のイメージ バージョンに依存しています。 AKS エンジンのバージョンと、それに対してサポートされている Kubernetes のバージョンが一覧表示された表は、「[Supported Kubernetes Versions](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#supported-kubernetes-versions)」(サポートされている Kubernetes バージョン) にあります。 たとえば、AKS Engine のバージョン `v0.40.0` は、AKS 基本イメージのバージョン `2019.08.21` に依存します。 Azure Stack のオペレーターに、Azure Marketplace から Azure Stack Marketplace に特定のイメージ バージョンをダウンロードするよう依頼してください。

Azure Stack Marketplace にイメージがない場合は、エラーがトリガーされます。 たとえば、AKS エンジンのバージョン v0.39.1 を使用していて、AKS 基本イメージのバージョン `2019.08.09` がない場合は、AKS エンジンの実行時に次のエラーが表示されます。 

```Text  
The platform image 'microsoft-aks:aks:aks-ubuntu-1604-201908:2019.08.09' is not available. 
Verify that all fields in the storage profile are correct.
```

次のコマンドを実行すると、ご自分の AKS エンジンのバージョンを確認できます。

```bash  
$ aks-engine version
Version: v0.39.1
GitCommit: 6fff62731
GitTreeState: clean
```

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [Azure Stack で Windows 用の AKS エンジンをデプロイする](azure-stack-kubernetes-aks-engine-deploy-windows.md)  
> [Azure Stack で Linux 用の AKS エンジンをデプロイする](azure-stack-kubernetes-aks-engine-deploy-linux.md)