---
title: コミュニティ テンプレートを使用して VM を作成する | Microsoft Docs
description: Azure Stack Development Kit (ASDK) を使用し、定義済みテンプレートと GitHub カスタム テンプレートを使って VM を作成する方法を学習します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/04/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/13/2018
ms.openlocfilehash: d82cdb1cccd52cc5e4c11571d85914be3f79a74e
ms.sourcegitcommit: 7f39bdc83717c27de54fe67eb23eb55dbab258a9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2019
ms.locfileid: "66691578"
---
# <a name="tutorial-create-a-vm-using-a-community-template"></a>チュートリアル:コミュニティ テンプレートを使用して VM を作成する

Azure Stack のオペレーターやユーザーは、Azure Stack マーケットプレースから手動でテンプレートをデプロイするのではなく、[カスタム GitHub クイック スタート テンプレート](https://github.com/Azure/AzureStack-QuickStart-Templates)を使用して仮想マシン (VM) を作成できます。

このチュートリアルでは、以下の内容を学習します。

> [!div class="checklist"]
> * Azure Stack クイック スタート テンプレートを使用する
> * カスタム GitHub テンプレートを使用して VM を作成する
> * Minikube を開始し、アプリケーションをインストールする

## <a name="azure-stack-quickstart-templates"></a>Azure Stack クイック スタート テンプレート

Azure Stack クイック スタート テンプレートは、GitHub の[グローバル Azure Stack クイック スタート テンプレート リポジトリ](https://github.com/Azure/AzureStack-QuickStart-Templates)に格納されています。 このリポジトリには、Microsoft Azure Stack Development Kit (ASDK) でテスト済みの、Azure Resource Manager デプロイ テンプレートが含まれています。 それらを使用すると、簡単に Azure Stack を評価したり ASDK 環境を使用したりすることができます。

長期間にわたって多くの GitHub ユーザーがリポジトリに貢献してきた結果、400 を超えるデプロイ テンプレートが集まりました。 このリポジトリは、さまざまな種類の環境を Azure Stack にどのようにデプロイできるかをより深く理解するのによい出発点です。

>[!IMPORTANT]
> これらのテンプレートの一部は、Microsoft ではなく、コミュニティのメンバーによって作成されています。 各テンプレートは、Microsoft ではなく、テンプレートの所有者によるライセンス契約に基づいて使用許諾されます。 Microsoft は、これらのテンプレートに対して責任を負わず、セキュリティ、互換性、パフォーマンスなどについての調査を行っていません。 コミュニティ テンプレートは、Microsoft サポート プログラムまたはサービスのサポート対象ではなく、"手を加えず" に提供され、いかなる保証もありません。

Azure Resource Manager テンプレートを GitHub に提供したい場合は、必ず、[AzureStack-QuickStart-Templates](https://github.com/Azure/AzureStack-QuickStart-Templates) リポジトリに提供してください。 このリポジトリの詳細と、リポジトリへの提供方法については、[readme ファイル](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md)を参照してください。

## <a name="create-a-vm-using-a-custom-github-template"></a>カスタム GitHub テンプレートを使用して VM を作成する

このチュートリアルの例では、[101-vm-linux-minikube](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-linux-minikube) Azure Stack クイック スタート テンプレートを使用して、Kubernetes クラスターを管理するために Minikube を実行している Azure Stack に Ubuntu 16.04 VM をデプロイします。

Minikube は、Kubernetes をローカルで簡単に実行できるようにするツールです。 ユーザーが Kubernetes を試用したり日常的に開発していたりできるように、Minikube は VM 内で単一ノードの Kubernetes クラスターを実行します。 Linux VM で実行される単純な単一ノードの Kubernetes クラスターがサポートされています。 Minikube は、完全に機能する Kubernetes クラスターを稼働させるための最も速くて簡単な方法です。 Minikube を使用すると、開発者はローカル マシン上で Kubernetes ベースのアプリケーションのデプロイを開発し、テストすることができます。 アーキテクチャ上、Minikube VM はマスター ノード コンポーネントとエージェント ノード コンポーネントの両方でローカルに実行されます。

* API Server、Scheduler、[etcd Server](https://coreos.com/etcd/) などのマスター ノード コンポーネントは、**LocalKube** という単一の Linux プロセスで実行されます。
* エージェント ノード コンポーネントは、通常のエージェント ノード上で実行される場合とまったく同じように、Docker コンテナー内で実行されます。 アプリケーションのデプロイの観点からは、アプリケーションを Minikube でデプロイしても、通常の Kubernetes クラスターでデプロイしても違いはありません。

このテンプレートは、以下のコンポーネントをインストールします。

* Ubuntu 16.04 LTS VM
* [Docker-CE](https://download.docker.com/linux/ubuntu)
* [Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/linux/amd64/kubectl)
* [Minikube](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)
* xFCE4
* xRDP

> [!IMPORTANT]
> 以下の手順を実行する前に、Ubuntu VM イメージ (この例では Ubuntu Server 16.04 LTS) が既に Azure Stack マーケットプレースに追加されている必要があります。

1. **[+ リソースの作成]** 、 **[カスタム]** 、 **[テンプレートのデプロイ]** の順に選択します。

    ![テンプレートの作成](media/azure-stack-create-vm-template/1.PNG)

2. **[テンプレートの編集]** を選択します。

    ![テンプレートの編集](media/azure-stack-create-vm-template/2.PNG)

3. **[クイック スタート テンプレート]** を選択します。

    ![クイック スタート テンプレート](media/azure-stack-create-vm-template/3.PNG)

4. **[テンプレートの選択]** ドロップダウン リストを使用して、利用可能なテンプレートの中から **[101-vm-linux-minikube]** を選択し、 **[OK]** をクリックします。

    ![テンプレートの選択](media/azure-stack-create-vm-template/4.PNG)

5. テンプレート JSON を変更したい場合は、そのようにできます。 そうでない場合、または変更を完了したら、 **[保存]** を選択して、 **[テンプレートの編集]** ダイアログを閉じます。

    ![テンプレートを保存する](media/azure-stack-create-vm-template/5.PNG)

6. **[パラメーター]** を選択し、必要に応じて使用可能なフィールドを入力するか変更して、 **[OK]** をクリックします。

    ![parameters](media/azure-stack-create-vm-template/6.PNG)

7. 使用するサブスクリプションを選択してからリソース グループを作成するか、既存のリソース グループ名を選びます。 その後、 **[作成]** を選択してテンプレートのデプロイを開始します。

    ![サブスクリプションの選択](media/azure-stack-create-vm-template/7.PNG)

8. リソース グループのデプロイには、カスタム テンプレートに基づく VM を作成するために、数分かかります。 通知やリソース グループのプロパティを通じて、インストールの状態を監視することができます。

    ![デプロイ](media/azure-stack-create-vm-template/8.PNG)

    >[!NOTE]
    > デプロイが完了すると、VM が実行されています。

## <a name="start-minikube-and-install-an-application"></a>Minikube を開始し、アプリケーションをインストールする

これで Linux VM が正常に作成されたので、サインインして Minikube を起動し、アプリケーションをインストールすることができます。

1. デプロイが完了したら、 **[接続]** を選択して、Linux VM への接続に使用されるパブリック IP アドレスを表示します。

    ![接続](media/azure-stack-create-vm-template/9.PNG)

2. 管理者特権でのコマンド プロンプトで **mstsc.exe** を実行してリモート デスクトップ接続を開き、前の手順で調べた Linux VM のパブリック IP アドレスに接続します。 xRDP へのサインインを求めるメッセージが表示されたら、VM の作成時に指定した資格情報を指定します。

    ![リモート](media/azure-stack-create-vm-template/10.PNG)

3. ターミナル エミュレーターを開き、次のコマンドを入力して Minikube を起動します。

    ```shell
    sudo minikube start --vm-driver=none
    sudo minikube addons enable dashboard
    sudo minikube dashboard --url
    ```

    ![コマンドの実行](media/azure-stack-create-vm-template/11.PNG)

4. ブラウザーを開き、Kubernetes ダッシュボードのアドレスに移動します。 これで、Minikube を使用して完全に動作する Kubernetes インストールができました。

    ![ダッシュボード](media/azure-stack-create-vm-template/12.PNG)

5. サンプル アプリケーションをデプロイする場合は、公式 Kubernetes ドキュメント ページにアクセスします。「Create Minikube Cluster」 (Minikube クラスターを作成する) のセクションは、既にクラスターを作成済みなので、スキップします。 [https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/](https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/ ) の「Create your Node.js application (Node.js アプリケーションを作成する)」のセクションに移動します。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、以下の内容を学習しました。

> [!div class="checklist"]
> * Azure Stack クイック スタート テンプレートについて学ぶ
> * カスタム GitHub テンプレートを使用して VM を作成する
> * minikube を開始し、アプリケーションをインストールする
