---
title: Azure Stack Hub 上の AKS エンジンに関する既知の問題
description: Azure Stack Hub 上で AKS エンジンを使用する場合の既知の問題について説明します。
author: mattbriggs
ms.topic: article
ms.date: 09/02/2020
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 09/02/2020
ms.openlocfilehash: 106aafd33441c2961cf933606cc8f48cbe4fc60d
ms.sourcegitcommit: b80d529ff47b15b8b612d8a787340c7b0f68165b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2020
ms.locfileid: "89473214"
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

## <a name="aks-engine-get-versions-command-limitations"></a>aks-engine get-versions コマンドの制限

**aks-engine** `get-versions` コマンドの出力は、Azure Stack Hub ではなくグローバル Azure にのみ関連します。 さまざまなアップグレード パスの詳細については、「[新しい Kubernetes バージョンにアップグレードする手順](azure-stack-kubernetes-aks-engine-upgrade.md#steps-to-upgrade-to-a-newer-kubernetes-version)」を参照してください。

## <a name="next-steps"></a>次のステップ

[Azure Stack Hub 上の AKS エンジンの概要](azure-stack-kubernetes-aks-engine-overview.md)
