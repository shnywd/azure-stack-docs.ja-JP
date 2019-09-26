---
title: Azure Stack 上の AKS エンジンのトラブルシューティング | Microsoft Docs
description: このトピックでは Azure Stack 上の AKS エンジンのトラブルシューティングの手順について説明します。
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
ms.openlocfilehash: eb8a46c5b226d1be40d922a78c6ecdcdda5e45ad
ms.sourcegitcommit: 09d14eb77a43fd585e7e6be93c32fa427770adb6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019385"
---
# <a name="troubleshoot-the-aks-engine-on-azure-stack"></a>Azure Stack 上の AKS エンジンのトラブルシューティング

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure Stack で AKS エンジンをデプロイまたは使用するときに問題が発生することがあります。 この記事では、AKS エンジンのデプロイのトラブルシューティング、ご使用の AKS エンジンに関する情報の収集、Kubernetes ログの収集、カスタム スクリプト拡張機能のエラー コードの確認、および AKS エンジンに関する GitHub イシューを開くための手順について説明します。

> [!IMPORTANT]
> AKS エンジンは、現在、パブリック プレビュー段階にあります。
> このプレビュー バージョンはサービス レベル アグリーメントなしで提供されています。運用環境のワークロードに使用することはお勧めできません。 特定の機能はサポート対象ではなく、機能が制限されることがあります。 詳しくは、[Microsoft Azure プレビューの追加使用条件](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)に関するページをご覧ください。

## <a name="troubleshoot-the-aks-engine-install"></a>AKS エンジンのインストールのトラブルシューティング

### <a name="try-gofish"></a>GoFish を試す

インストール手順が失敗した場合は、GoFish パッケージ マネージャーを使用してインストールを試します。 [GoFish](https://gofi.sh) では、自身がクロスプラットフォームの Homebrew として記述されます。

#### <a name="install-the-aks-engine-with-gofish-on-linux"></a>GoFish を使用して Linux に AKS エンジンをインストールする

[[インストール]](https://gofi.sh/#install) ページから GoFish をインストールします。

1. Bash プロンプトから、次のコマンドを実行します。

    ```bash
    curl -fsSL https://raw.githubusercontent.com/fishworks/gofish/master/scripts/install.sh | bash
    ```

2.  次のコマンドを実行して、GoFish で AKS エンジンをインストールします。

    ```bash
    Run "gofish install aks-engine"
    ```

#### <a name="install-the-aks-engine-with-gofish-on-windows"></a>GoFish を使用して Windows に AKS エンジンをインストールする

[[インストール]](https://gofi.sh/#install) ページから GoFish をインストールします。

1. 管理者特権の PowerShell プロンプトから、次のコマンドを実行します。

    ```PowerShell
    Set-ExecutionPolicy Bypass -Scope Process -Force
iex ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/fishworks/gofish/master/scripts/install.ps1'))
    ```

2.  同じセッションで次のコマンドを実行して、GoFish で AKS エンジンをインストールします。

    ```PowerShell
    gofish install aks-engine
    ```

### <a name="checklist-for-common-deployment-issues"></a>デプロイに関する一般的な問題のチェックリスト

AKS エンジンを使用して Kubernetes クラスターをデプロイしているときにエラーが発生した場合は、次のことを確認します。

1.  正しいサービス プリンシパル資格情報 (SPN) を使用していますか?
2.  SPN には Azure Stack サブスクリプションに対する "共同作成者" ロールがありますか?
3. Azure Stack プランに十分な大きさのクォータがありますか?
4.  Azure Stack インスタンスには、適用される修正プログラムまたはアップグレードがありますが?

詳細については、**Azure/aks-engine GitHub** リポジトリにある「[トラブルシューティング](https://github.com/Azure/aks-engine/blob/master/docs/howto/troubleshooting.md)」を参照してください。

## <a name="collect-aks-engine-logs"></a>AKS エンジン ログの収集

AKS エンジンによって作成されたレビュー情報にアクセスできます。 AKS エンジンでは、アプリケーションの実行時の状態とエラーが報告されます。 出力をテキスト ファイルにパイプすることも、コマンドライン コンソールから直接コピーすることもできます。

1.  AKS エンジンのコマンドライン ツールに表示される情報から標準出力とエラーを収集します。

2. ローカル ファイルからログを取得します。 出力ディレクトリは、 **--output-directory** パラメーターを使用して設定できます。

    ログのローカル パスを設定するには:

    ```bash  
    aks-engine --output-directory <path to the directory>
    ```

## <a name="collect-kubernetes-logs"></a>Kubernetes のログの収集

AKS エンジンのログに加えて、Kubernetes コンポーネントではステータス メッセージとエラー メッセージが生成されます。 これらのログは、Bash スクリプト [getkuberneteslogs.sh](https://github.com/msazurestackworkloads/azurestack-gallery/releases/download/diagnosis-v0.1.0/diagnosis.zip) を使用して収集できます。

このスクリプトにより、次のログの収集プロセスが自動化されます。 

 - Microsoft Azure Linux エージェント (waagent) ログ
 - カスタム スクリプト拡張機能ログ
 - kube-system コンテナーの実行メタデータ
 - kube-system コンテナーの実行ログ
 - Kubelet サービスの状態と履歴
 - etcd サービスの状態と履歴
 - ギャラリー アイテムの DVM ログ
 - kube-system スナップショット

このスクリプトがない場合は、クラスター内の各ノードに接続して、手動でログを検索してダウンロードする必要があります。 また、このスクリプトでは、必要に応じて、収集したログをストレージ アカウントにアップロードして、他のユーザーとログを共有することもできます。

要件:

 - Linux VM、Git Bash、または Bash on Windows。
 - [Azure CLI](azure-stack-version-profiles-azurecli2.md) がスクリプトを実行するマシンにインストールされていること。
 - Azure Stack への Azure CLI セッションにサインインしたサービス プリンシパル ID。 このスクリプトは、作業を行うために ARM リソースを検出して作成することができるため、Azure CLI とサービス プリンシパル ID が必要です。
 - 環境で Kubernetes クラスターが既に選択されているユーザー アカウント (サブスクリプション)。 
1. スクリプト tar ファイルの最新リリースをご自分のクライアント VM、Kubernetes クラスターにアクセスできるマシン、または AKS エンジンを使用してクラスターをデプロイするために使用したものと同じマシンにダウンロードします。

    次のコマンドを実行します。

    ```bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    wget https://github.com/msazurestackworkloads/azurestack-gallery/releases/download/diagnosis-v0.1.0/diagnosis.tar.gz
    tar xvzf diagnosis.tar.gz -C ./
    ```

2. `getkuberneteslogs.sh` スクリプトに必要なパラメーターを探します。 このスクリプトでは、次のパラメーターが使用されます。

    | パラメーター | 説明 | 必須 | 例 |
    | --- | --- | --- | --- |
    | -h, --help | Print コマンドの使用方法。 | × | 
    -u,--user | クラスター VM の管理者ユーザー名 | はい | azureuser<br>(既定値) |
    | -i、--identity-file | Kubernetes クラスターの作成に使用される公開キーに関連付けられた RSA 秘密キー (' id_rsa ' と呼ばれることもあります)  | はい | `./rsa.pem` (Putty)<br>`~/.ssh/id_rsa` (SSH) |
    |   -g, --resource-group    | Kubernetes クラスター リソース グループ | はい | k8sresourcegroup |
    |   -n, --user-namespace               | 指定された名前空間のコンテナーからログが収集されます (kube-system ログは常に収集されます) | × |   monitoring |
    |       --api-model                    | Azure Stack ストレージ アカウントで apimodel.json ファイルが保持されます。 --upload-logs パラメーターも指定すると、apimodel.json ファイルがストレージ アカウントにアップロードされます。 | × | `./apimodel.json` |
    | --all-namespaces               | すべての名前空間のコンテナーからログが収集されます。 --user-namespace がオーバーライドされます。 | × | |
    | --upload-logs                  | Azure Stack ストレージ アカウントで取得したログが保持されます。 ログは KubernetesLogs リソース グループにあります。 | × | |
    --disable-host-key-checking    | スクリプトの実行中に、SSH の StrictHostKeyChecking オプションが "no" に設定されます。 安全な環境でのみ使用してください。 | × | |

3. ご自分の情報を使用して、次のサンプル コマンドのいずれかを実行します。

    ```bash
    ./getkuberneteslogs.sh -u azureuser -i private.key.1.pem -g k8s-rg
    ./getkuberneteslogs.sh -u azureuser -i ~/.ssh/id_rsa -g k8s-rg --disable-host-key-checking
    ./getkuberneteslogs.sh -u azureuser -i ~/.ssh/id_rsa -g k8s-rg -n default -n monitoring
    ./getkuberneteslogs.sh -u azureuser -i ~/.ssh/id_rsa -g k8s-rg --upload-logs --api-model clusterDefinition.json
    ./getkuberneteslogs.sh -u azureuser -i ~/.ssh/id_rsa -g k8s-rg --upload-logs
    ```

## <a name="review-custom-script-extension-error-codes"></a>カスタム スクリプト拡張機能のエラー コードを確認する

クラスターを実行しているカスタム スクリプト拡張機能 (CSE) によって作成されたエラー コードの一覧を参照できます。 CSE エラーは、問題の根本原因を診断するのに役立ちます。 Kubernetes クラスターで使用されている Ubuntu サーバーの CSE では、AKS エンジンの多くの操作がサポートされます。 CSE 終了コードの詳細については、「[cse_helpers.sh](https://github.com/Azure/aks-engine/blob/master/parts/k8s/cloud-init/artifacts/cse_helpers.sh)」を参照してください。

## <a name="open-github-issues"></a>GitHub イシューを開く

デプロイ エラーを解決できない場合は、GitHub イシューを開くことができます。 

1. AKS エンジン リポジトリで [GitHub イシュー](https://github.com/Azure/aks-engine/issues/new)を開きます。
2. 次の形式を使用してタイトルを追加します。C`SE error: exit code <INSERT_YOUR_EXIT_CODE>`.
3. イシューには次の情報を含めます。

    - クラスターのデプロイに使用されるクラスター構成ファイル `apimodel json`。 GitHub に投稿する前に、すべてのシークレットとキーを削除します。  
     - 次の **kubectl** コマンド `get nodes` の出力。  
     - `/var/log/azure/cluster-provision.log` と `/var/log/cloud-init-output.log` の内容

## <a name="next-steps"></a>次の手順

- [Azure Stack 上の AKS エンジン](azure-stack-kubernetes-aks-engine-overview.md)を確認してください。
