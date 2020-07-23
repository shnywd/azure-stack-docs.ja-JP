---
title: Azure Stack Hub 上の AKS エンジンに関する既知の問題
description: Azure Stack Hub 上で AKS エンジンを使用する場合の既知の問題について説明します。
author: mattbriggs
ms.topic: article
ms.date: 07/02/2020
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 07/02/2020
ms.openlocfilehash: 02f4d8ec694ffc56966029f35dd12fd263a5cb8b
ms.sourcegitcommit: 0aa5f7f20690839661c8bb3bfdbe32f82bec0c64
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/21/2020
ms.locfileid: "86566143"
---
# <a name="known-issues-with-the-aks-engine-on-azure-stack-hub"></a>Azure Stack Hub 上の AKS エンジンに関する既知の問題

このトピックでは Azure Stack Hub 上の AKS エンジンに関する既知の問題について説明します。

## <a name="upgrade-issues-in-aks-engine-0510"></a>AKS エンジン 0.51.0 のアップグレードに関する問題

* Kubernetes クラスターのバージョン 1.15.x から 1.16.x へのアップグレード (AKS アップグレード) 中に、次の Kubernetes コンポーネントをアップグレードするには、追加の手順が必要になります: **kube-proxy**、**azure-cni-networkmonitor**、**csi-secrets-store**、**kubernetes-dashboard**。 次に、発生する可能性のある問題とその対処方法について説明します。

  * 接続された環境では、影響を受けるコンポーネントがアップグレードされなかったという表示がクラスターで出されないため、この問題に気付かない場合があります。 すべてが予期したとおりに動作しているように見えます。
  <!-- * In disconnected environments, you can see this problem when you run a query for the system pods status and see that the pods for the components mentioned below are not in "Ready" state: -->

    ```bash  
    kubectl get pods -n kube-system
    ```

  * これらのコンポーネントのそれぞれでこの問題を解決するための回避策として、次の表の [回避策] 列のコマンドを実行します。

    |[コンポーネント名] |回避策 |影響を受けるシナリオ|
    |---------------|-----------|------------------|
    |**kube-proxy**     | `kubectl delete ds kube-proxy -n kube-system` |接続、切断 |
    |**azure-cni-networkmonitor**   | `kubectl delete ds azure-cni-networkmonitor -n kube-system`   | 接続、切断 |
    |**csi-secrets-store**  |`sudo sed -i s/Always/IfNotPresent/g /etc/kubernetes/addons/secrets-store-csi-driver.yaml`<br>`kubectl delete ds csi-secrets-store -n kube-system` | [Disconnected]\(切断済み\) |
    |**kubernetes-dashboard** |各マスター ノードで次のコマンドを実行します。<br>`sudo sed -i s/Always/IfNotPresent/g /etc/kubernetes/addons/kubernetes-dashboard.yaml` |[Disconnected]\(切断済み\) |

* Kubernetes 1.17 はこのリリースではサポートされていません。 1\.17 を参照している GitHub pull request (PR) がありますが、サポートされていません。

## <a name="basic-load-balancer-sku-limitations"></a>基本のロード バランサーの SKU の制限事項

* 単一エージェント プールの制限。 現時点の Azure Stack Hub では、基本のロード バランサーの SKU のみがサポートされます。 この SKU では、バックエンド プール エンドポイントは、単一の可用性セット (または仮想マシン スケール セット) 内の仮想マシンに[制限されます](/azure/load-balancer/concepts#limitations)。 これは、LoadBalancer サービスのすべてのレプリカを同じエージェント プールにデプロイする必要があることを意味します。また、個々のクラスターで Linux LoadBalancer サービスまたは Windows LoadBalancer サービスを持つことができることを意味します。

  次の[ノード セレクター](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/)をポッド テンプレートに追加することにより、特定のエージェント プールにポッドを作成するように Kubernetes に強制できます: "agentpool: MY_POOL_NAME"。

  ```json
  nodeSelector:

        agentpool: linuxpool
  ```
  
  クラスターで LoadBalancer サービスが既に作成されている場合は、Azure Stack Hub ポータルでロード バランサーのバックエンド プールのブレードを調べて、ロード バランサーのバックエンド プールとして選択されたエージェント プールを確認できます。 この情報を取得したら、デプロイまたはポッドの YAML (前の段落で説明) を更新して、ターゲットのエージェント プールを指定できます。

* `get-versions` のコマンド スコープ。 `get-versions` コマンドの出力は、Azure Stack Hub クラウドではなく、Azure のみに関連しています。 さまざまなアップグレード パスの詳細については、「[新しい Kubernetes バージョンにアップグレードする手順](azure-stack-kubernetes-aks-engine-upgrade.md#steps-to-upgrade-to-a-newer-kubernetes-version)」を参照してください。

## <a name="next-steps"></a>次のステップ

[Azure Stack Hub 上の AKS エンジンの概要](azure-stack-kubernetes-aks-engine-overview.md)
