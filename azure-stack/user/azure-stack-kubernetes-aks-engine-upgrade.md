---
title: Azure Stack で Kubernetes クラスターをアップグレードする | Microsoft Docs
description: Azure Stack で Kubernetes クラスターをアップグレードする方法を学習します。
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
ms.date: 09/25/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 09/25/2019
ms.openlocfilehash: 377857019e6a4d55e6a9372296817e1776c081c9
ms.sourcegitcommit: d967cf8cae320fa09f1e97eeb888e3db5b6e7972
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/26/2019
ms.locfileid: "71279161"
---
# <a name="upgrade-a-kubernetes-cluster-on-azure-stack"></a>Azure Stack で Kubernetes クラスターをアップグレードする

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

## <a name="upgrade-a-cluster"></a>クラスターをアップグレードする

AKS エンジンでは、ツールを使用して最初にデプロイされたクラスターをアップグレードすることができます。 AKS エンジンを使用してクラスターを維持できます。 メンテナンス タスクは、任意の IaaS システムと同様です。 新しい更新プログラムが利用可能かどうかを認識し、AKS エンジンを使用してそれらを適用します。

Microsoft ではお客様のクラスターを管理しません。

デプロイされたクラスターのアップグレードでは、以下がカバーされます。

-   Kubernetes
-   Azure Stack Kubernetes プロバイダー
-   基本 OS

運用クラスターをアップグレードする場合は、次の点を考慮してください。

-   ターゲット クラスターに正しいクラスター仕様 (`apimodel.json`) とリソース グループを使用していますか?
-   AKS エンジンを実行し、アップグレード操作を実行するクライアント マシンに信頼できるマシンを使用していますか?
-   バックアップ クラスターがあり、それが動作していることを確認します。
-   可能であれば、Azure Stack 環境内の VM からコマンドを実行して、ネットワーク ホップと接続障害の可能性を減らします。
-   サブスクリプションにプロセス全体のための十分な領域があることを確認します。 このプロセスでは、プロセス中に新しい VM が割り当てられます。
-   システムの更新やスケジュールされたタスクは計画されていません。
-   運用クラスターとまったく同じように構成されたクラスターで段階的なアップグレードをセットアップし、運用クラスターでアップグレードを実行する前に、そこでアップグレードをテストします。

## <a name="steps-to-upgrade"></a>アップグレードの手順

1. 「[Kubernetes クラスターのアップグレード](https://github.com/Azure/aks-engine/blob/master/docs/topics/upgrade.md)」の記事の手順に従います。 
2. まず、アップグレードの対象となるバージョンを特定する必要があります。 このバージョンは現在お持ちになっているバージョンによって異なります。そのバージョン値を使用してアップグレードを実行します。

    次のコマンドを実行します。

    ```bash  
    $ aks-engine get-versions
    Version Upgrades
    1.15.0
    1.14.3  1.15.0
    1.14.1  1.14.3, 1.15.0
    1.13.7  1.14.1, 1.14.3
    1.13.5  1.13.7, 1.14.1, 1.14.3
    1.12.8  1.13.5, 1.13.7
    1.12.7  1.12.8, 1.13.5, 1.13.7
    1.11.10 1.12.7, 1.12.8
    1.11.9  1.11.10, 1.12.7, 1.12.8
    1.10.13 1.11.9, 1.11.10
    1.10.12 1.10.13, 1.11.9, 1.11.10
    1.9.11  1.10.12, 1.10.13
    1.9.10  1.9.11, 1.10.12, 1.10.13
    1.6.9   1.9.10, 1.9.11
    ```

    たとえば、`get-versions` コマンドの出力によれば、現在の Kubernetes のバージョンが "1.13.5" の場合、"1.13.7, 1.14.1, 1.14.3" にアップグレードできます。

3. `upgrade` コマンドを実行するために必要な情報を収集します。 アップグレードでは、次のパラメーターが使用されます。

    | パラメーター | 例 | 説明 |
    | --- | --- | --- |
    | azure-env | AzureStackCloud | AKS エンジンに対して、ターゲット プラットフォームが Azure Stack であることを示すには、`AzureStackCloud` を使用します。 |
    | location | local | Azure Stack のリージョン名。 ASDK の場合、リージョンは `local` に設定されます。 |
    | resource-group | kube-rg | 新しいリソース グループの名前を入力するか、既存のリソース グループを選択します。 リソース名は、英数字かつ小文字にする必要があります。 |
    | subscription-id | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | サブスクリプション ID を入力します。 詳細については、「[プランへのサブスクライブ](https://docs.microsoft.com/azure-stack/user/azure-stack-subscribe-services#subscribe-to-an-offer)」をご覧ください。 |
    | api-model | ./kubernetes-azurestack.json | クラスター構成ファイルまたは API モデルへのパス。 |
    | client-id | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | サービス プリンシパル GUID を入力します。 Azure Stack 管理者がサービス プリンシパルを作成したときにアプリケーション ID として識別されたクライアント ID。 |
    | client-secret | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | サービス プリンシパル シークレットを入力します。 これは、サービスの作成時に設定するクライアント シークレットです。 |
    | identity-system | adfs | 省略可能。 Active Directory フェデレーション サービス (AD FS) を使用している場合に、ID 管理ソリューションを指定します。 |

4. 値を指定して、次のコマンドを実行します。

    ```bash  
    aks-engine upgrade \
    --azure-env AzureStackCloud   
    --location <for an ASDK is local> \
    --resource-group kube-rg \
    --subscription-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --api-model kube-rg/apimodel.json \
    --upgrade-version 1.13.5 \
    --client-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --client-secret xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --identity-system adfs # required if using AD FS
    ```

5.  何らかの理由でアップグレード操作でエラーが発生した場合は、問題に対処した後でアップグレード コマンドを再実行できます。 AKS エンジンにより、前の時点で失敗した操作が再開されます。

## <a name="forcing-an-upgrade"></a>アップグレードの強制

クラスターを強制的にアップグレードする必要がある場合があります。 たとえば、1 日目に最新の Kubernetes バージョンを使用して、切断された環境にクラスターをデプロイするとします。 次の日に、Ubuntu が、Microsoft の新しい **AKS 基本イメージ**によってもたらされる脆弱性に対する修正プログラムをリリースします。 既にデプロイしたのと同じ Kubernetes バージョンを使用してアップグレードを強制することで、この新しいイメージを適用できます。

    ```bash  
    aks-engine upgrade \
    --azure-env AzureStackCloud   
    --location <for an ASDK is local> \
    --resource-group kube-rg \
    --subscription-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --api-model kube-rg/apimodel.json \
    --upgrade-version 1.13.5 \
    --client-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --client-secret xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    --force
    ```

手順については、[アップグレードの強制](https://github.com/Azure/aks-engine/blob/master/docs/topics/upgrade.md#force-upgrade)に関するセクションを参照してください。

## <a name="next-steps"></a>次の手順

- [Azure Stack 上の AKS エンジン](azure-stack-kubernetes-aks-engine-overview.md)を確認してください。
- [Azure Stack で Kubernetes クラスターをスケールする](azure-stack-kubernetes-aks-engine-scale.md)