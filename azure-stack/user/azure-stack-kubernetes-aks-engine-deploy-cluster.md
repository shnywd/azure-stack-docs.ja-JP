---
title: AKS エンジンを使用して Azure Stack に Kubernetes クラスターをデプロイする | Microsoft Docs
description: AKS エンジンを実行しているクライアント VM から Azure Stack に Kubernetes クラスターをデプロイする方法。
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
ms.openlocfilehash: 957ea4bc52e6f629ffd7fbd06a14d8dc2fb85021
ms.sourcegitcommit: d967cf8cae320fa09f1e97eeb888e3db5b6e7972
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/26/2019
ms.locfileid: "71279173"
---
# <a name="deploy-a-kubernetes-cluster-with-the-aks-engine-on-azure-stack"></a>AKS エンジンを使用して Azure Stack に Kubernetes クラスターをデプロイする

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

AKS エンジンを実行しているクライアント VM から Azure Stack に Kubernetes クラスターをデプロイできます。 この記事では、クラスター仕様の記述、`apimodel.json` ファイルを使用したクラスターのデプロイ、Helm を使用した MySQL のデプロイによるクラスターの確認について説明します。

## <a name="define-a-cluster-specification"></a>クラスター仕様の定義

[API モデル](https://github.com/Azure/aks-engine/blob/master/docs/topics/architecture.md#architecture-diagram)と呼ばれる JSON 形式を使用して、ドキュメント ファイルでクラスター仕様を指定できます。 AKS エンジンでは、API モデルのクラスター仕様の使用によりクラスターが作成されます。 

### <a name="update-the-api-model"></a>API モデルの更新

このセクションでは、クラスターの API モデルの作成について説明します。

1.  まず、Azure Stack の[サンプル](https://github.com/Azure/aks-engine/tree/master/examples/azure-stack) API モデル ファイルを使用し、デプロイ用のローカル コピーを作成します。 AKS エンジンをインストールしたマシンから、次を実行します。

    ```bash
    curl -o kubernetes-azurestack.json https://raw.githubusercontent.com/Azure/aks-engine/master/examples/azure-stack/kubernetes-azurestack.json
    ```

    > [!Note]  
    > 切断されている場合は、ファイルをダウンロードして、編集する予定の切断されたマシンに手動でコピーすることができます。 [PuTTY や WinSCP](https://www.suse.com/documentation/opensuse103/opensuse103_startup/data/sec_filetrans_winssh.html) などのツールを使用して、ファイルを Linux マシンにコピーできます。

2.  エディターで開くには、nano を使用できます。

    ```bash
    nano ./kubernetes-azurestack.json
    ```

    > [!Note]  
    > nano がインストールされていない場合は、Ubuntu で nano をインストールできます: `sudo apt-get install nano`。

3.  kubernetes-azurestack.json ファイルで、`orchestratorRelease` を見つけます。 サポートされている Kubernetes バージョンのいずれかを選択します。 たとえば、1.11、1.12、1.13、1.14 などです。 多くの場合、バージョンは更新プログラムです。 バージョンは、x. xx. x ではなく、x. xx として指定します。 現在のバージョンの一覧については、[サポートされている Kubernetes バージョン](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#supported-kubernetes-versions)に関する記事を参照してください。 次の AKS エンジン コマンドを実行すると、サポートされているバージョンを確認できます。

    ```bash
    aks-engine get-versions
    ```

4.  `portalURL` を見つけ、テナント ポータルへの URL を指定します。 たとえば、「 `https://portal.local.azurestack.external` 」のように入力します。

5.  配列 `masterProfile` で、次のフィールドを設定します。

    | フィールド | 説明 |
    | --- | --- |
    | dnsPrefix | VM のホスト名を識別するために使用される一意の文字列を入力します。 たとえば、リソース グループ名に基づいて名前を指定します。 |
    | count |  デプロイに必要なマスターの数を入力します。 HA デプロイの最小値は 3 ですが、非 HA デプロイでは 1 が許可されます。 |
    | vmSize |  [Azure Stack でサポートされているサイズ](https://docs.microsoft.com/azure-stack/user/azure-stack-vm-sizes) (例: `Standard_D2_v2`) を入力します。 |
    | ディストリビューション | 「 `aks-ubuntu-16.04` 」を入力します。 |

6.  配列 `agentPoolProfiles` の更新では、次のようにします。

    | フィールド | 説明 |
    | --- | --- |
    | count | デプロイに必要なエージェントの数を入力します。 |
    | vmSize | [Azure Stack でサポートされているサイズ](https://docs.microsoft.com/azure-stack/user/azure-stack-vm-sizes) (例: `Standard_D2_v2`) を入力します。 |
    | ディストリビューション | 「 `aks-ubuntu-16.04` 」を入力します。 |

7.  配列 `linuxProfile` の更新では、次のようにします。

    | フィールド | 説明 |
    | --- | --- |
    | adminUsername | VM の管理者ユーザー名を入力します。 |
    | ssh | VM での SSH 認証に使用される公開キーを入力します。 |

### <a name="more-information-about-the-api-model"></a>API モデルに関する詳細情報

- API モデルで使用可能なすべてのオプションの完全なリファレンスについては、[クラスターの定義](https://github.com/Azure/aks-engine/blob/master/docs/topics/clusterdefinitions.md)に関する記事を参照してください。  
- Azure Stack の特定のオプションに関する詳細については、[Azure Stack クラスター定義の詳細](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#cluster-definition-aka-api-model)に関する記事を参照してください。  

## <a name="deploy-a-kubernetes-cluster"></a>Kubernetes クラスターのデプロイ

API モデルで必要なすべての値を収集したら、クラスターを作成できます。 この時点で、次のことを行う必要があります。

Azure Stack オペレーターに次のことを依頼します。

- システムの正常性を確認し、`Test-AzureStack` および OEM ベンダーのハードウェア監視ツールの実行を提案します。
- メモリ、ストレージ、パブリック IP などのリソースを含むシステム容量を確認します。
- 使用する予定の VM の数に十分な領域があることを確認できるように、サブスクリプションに関連付けられているクォータの詳細を指定します。

クラスターのデプロイに進む:

1.  Azure Stack の [CLI フラグ](https://github.com/Azure/aks-engine/blob/master/docs/topics/azure-stack.md#cli-flags)で AKS エンジンの使用可能なパラメーターを確認します。

    | パラメーター | 例 | 説明 |
    | --- | --- | --- |
    | azure-env | AzureStackCloud | AKS エンジンに対して、ターゲット プラットフォームが Azure Stack であることを示すには、`AzureStackCloud` を使用します。 |
    | identity-system | adfs | 省略可能。 Active Directory フェデレーション サービス (AD FS) を使用している場合に、ID 管理ソリューションを指定します。 |
    | location | local | Azure Stack のリージョン名。 ASDK の場合、リージョンは `local` に設定されます。 |
    | resource-group | kube-rg | 新しいリソース グループの名前を入力するか、既存のリソース グループを選択します。 リソース名は、英数字かつ小文字にする必要があります。 |
    | api-model | ./kubernetes-azurestack.json | クラスター構成ファイルまたは API モデルへのパス。 |
    | output-directory | kube-rg | 出力ファイル `apimodel.json` とその他の生成されたファイルを格納するディレクトリの名前を入力します。 |
    | client-id | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | サービス プリンシパル GUID を入力します。 Azure Stack 管理者がサービス プリンシパルを作成したときにアプリケーション ID として識別されたクライアント ID。 |
    | client-secret | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | サービス プリンシパル シークレットを入力します。 これは、サービスの作成時に設定するクライアント シークレットです。 |
    | subscription-id | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | サブスクリプション ID を入力します。 詳細については、「[プランへのサブスクライブ](https://docs.microsoft.com/azure-stack/user/azure-stack-subscribe-services#subscribe-to-an-offer)」をご覧ください。 |

    たとえば次のようになります。

    ```bash  
    aks-engine deploy \
    --azure-env AzureStackCloud \
    --location <for asdk is local> \
    --resource-group kube-rg \
    --api-model ./kubernetes-azurestack.json \
    --output-directory kube-rg \
    --client-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --client-secret xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --subscription-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx \
    --identity-system adfs # required if using AD FS
    ```

2.  何らかの理由で出力ディレクトリの作成後に実行が失敗する場合は、問題を修正し、コマンドを再実行できます。 デプロイを再実行している場合、以前と同じ出力ディレクトリを使用していた場合は、ディレクトリが既に存在しているというエラーが AKS エンジンから返されます。 フラグ `--force-overwrite` を使用すると、既存のディレクトリを上書きできます。

3.  AKS エンジン クラスター構成を安全で暗号化された場所に保存します。

    `apimodel.json` ファイルを見つけます。 安全な場所に保存します。 このファイルは、他のすべての AKS エンジン操作で入力として使用されます。

    生成された `apimodel.json` には、入力 API モデルで使用するサービス プリンシパル、シークレット、および SSH 公開キーが含まれています。 また、AKS エンジンが他のすべての操作を実行するために必要な、他のすべてのメタデータも含まれています。 紛失した場合、AKS エンジンでクラスターを構成することができません。

    シークレットは**暗号化されていません**。 ファイルは暗号化された安全な場所に保管してください。 

## <a name="verify-your-cluster"></a>クラスターを確認する

クラスターを確認するために Helm を使用して MySQL をデプロイし、クラスターを確認します。

1. Azure Stack ポータルを使用して、いずれかのマスター ノードのパブリック IP アドレスを取得します。

2. Azure Stack インスタンスにアクセスできるマシンから、PuTTY や MobaXterm などのクライアントを使用して、SSH 経由で新しいマスターノードに接続します。 

3. SSH ユーザー名には、"azureuser" と、クラスターのデプロイ用に指定したキー ペアの秘密キー ファイルを使用します。

4.  次のコマンドを実行します。

    ```bash
    sudo snap install helm –classic
    helm repo update
    helm install stable/mysql
    ```

5. その後 `install stable/mysql` を実行しようとすると、`Error: incompatible versions client[v2.XX.X] server[v2.YY.Y]` のようなエラーが表示されます。 次のコマンドを実行します。

    ```bash 
    helm init --force-upgrade
    and retry:
    helm install stable/mysql
    ```

6.  テストをクリーン アップするには、MySQL のデプロイに使用した名前を見つけます。 次の例では、名前は `wintering-rodent` です。 次に、削除します。 

    次のコマンドを実行します。

    ```bash
    helm ls
    NAME REVISION UPDATED STATUS CHART APP VERSION NAMESPACE
    wintering-rodent 1 Thu Oct 18 15:06:58 2018 DEPLOYED mysql-0.10.1 5.7.14 default
    helm delete wintering-rodent
    ```

    CLI で次が表示されます。
    ```bash
    release "wintering-rodent" deleted
    ```

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [Azure Stack 上の AKS エンジンのトラブルシューティング](azure-stack-kubernetes-aks-engine-troubleshoot.md)