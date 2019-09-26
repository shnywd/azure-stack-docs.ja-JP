---
title: AAzure Stack で Kubernetes クラスターをスケールする | Microsoft Docs
description: Azure Stack で Kubernetes クラスターをスケールする方法を学習します。
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
ms.openlocfilehash: 7847d79d0f2816aa56940fd7b81d25cbb1a3a7f2
ms.sourcegitcommit: 09d14eb77a43fd585e7e6be93c32fa427770adb6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019397"
---
# <a name="scale-a-kubernetes-cluster-on-azure-stack"></a>Azure Stack で Kubernetes クラスターをスケールする

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

AKS エンジンで **scale** コマンドを使用して、クラスターをスケールできます。 **scale** コマンドでは、出力ディレクトリ内のクラスター構成ファイル (`apimodel.json`) を新しい Azure Resource Manager デプロイの入力として再利用します。 エンジンにより、指定されたエージェント プールに対してスケーリング操作が実行されます。 スケーリング操作が完了すると、更新された現在のクラスター構成を反映するために、エンジンにより同じ `apimodel.json` ファイル内のクラスター定義が新しいノード数を反映するように更新されます。

## <a name="scale-a-cluster"></a>クラスターのスケーリング

`aks-engine scale` コマンドにより、`aks-engine` Kubernetes クラスター内の既存のエージェント プール内のノードの数を増減できます。 ノードの追加または削除は、常にエージェント プールの末尾で行われます。 ノードは、削除される前に切断およびドレインされます。

### <a name="values-for-the-scale-command"></a>scale コマンドの値

次のパラメーターは、クラスター定義ファイルを検索してクラスターを更新するために、scale コマンドによって使用されます。

| パラメーター | 例 | 説明 |
| --- | --- | --- | 
| azure-env | AzureStackCloud | Azure Stack を使用する場合は、環境名を `AzureStackCloud` に設定する必要があります。 | 
| location | local | これは、ご使用の Azure Stack インストールのリージョンです。 ASDK の場合、リージョンは `local` に設定されます。  | 
| resource-group | kube-rg | ご使用のクラスターを含むリソース グループの名前。 | 
| subscription-id |  | ご使用のクラスターによって使用されるリソースを含むサブスクリプションの GUID。 スケールするサブスクリプションに十分なクォータがあることを確認します。 | 
| client-id |  | AKS エンジンからクラスターを作成するときに使用されるサービス プリンシパルのクライアント ID。 | 
| client-secret |  | クラスターの作成時に使用されるサービス プリンシパル シークレット。 | 
| api-model | kube-rg/apimodel.json | クラスター定義ファイルへのパス (apimodel.json)。 これは、_output/\<dnsPrefix>/apimodel.json にすることができます。 | 
| -new-node-count | 9 | 目的のノード数。 | 
| -master-FQDN |  | マスター FQDN。 スケールダウンするときに必要です。 | 

Azure Stack でクラスターをスケーリングする場合は、 **–azure-env** パラメーターを指定する必要があります。 AKS エンジンの **scale** コマンドで使用されるパラメーターとその値の詳細については、[Scale - パラメーター](https://github.com/Azure/aks-engine/blob/master/docs/topics/scale.md#parameters)を参照してください。

### <a name="command-to-scale-your-cluster"></a>クラスターをスケールするコマンド

クラスターをスケールするには、次のコマンドを実行します。

```bash
aks-engine scale \
    --azure-env AzureStackCloud   \
    --location <for an ASDK is local> \
    --resource-group <cluster resource group>
    --subscription-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --client-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --client-secret xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --api-model <path to your apomodel.json file>
    --new-node-count <desired node count> \
    --master-FQDN <master FQDN> \
```

## <a name="next-steps"></a>次の手順

- [Azure Stack 上の AKS エンジン](azure-stack-kubernetes-aks-engine-overview.md)を確認してください。
- [Azure Stack で Kubernetes クラスターをアップグレードする](azure-stack-kubernetes-aks-engine-upgrade.md)