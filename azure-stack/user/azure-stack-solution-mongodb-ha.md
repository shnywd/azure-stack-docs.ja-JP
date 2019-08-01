---
title: 高可用性の MongoDB ソリューションを Azure と Azure Stack にデプロイする | Microsoft Docs
description: 高可用性の MongoDB ソリューションを Azure と Azure Stack にデプロイする方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/20/2019
ms.openlocfilehash: 25be3914f58abad44b64870f9da94610a7498d52
ms.sourcegitcommit: 35b13ea6dc0221a15cd0840be796f4af5370ddaf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/29/2019
ms.locfileid: "68603044"
---
# <a name="deploy-a-highly-available-mongodb-solution-to-azure-and-azure-stack"></a>高可用性の MongoDB ソリューションを Azure と Azure Stack にデプロイする

この記事では、2 つの Azure Stack 環境にわたって、DR (ディザスター リカバリー) サイトと共に、基本的な高可用性 (HA) MongoDB クラスターを自動デプロイする方法を段階的に説明します。 MongoDB と高可用性の詳細については、「[Replica Set Members](https://docs.mongodb.com/manual/core/replica-set-members/)」(レプリカ セット メンバー) を参照してください。

このソリューションでは、以下を実現するためのサンプル環境を作成します。

> [!div class="checklist"]
> - 2 つの Azure Stack にわたってデプロイを調整する
> - Docker を使用し、Azure API プロファイルで依存関係の問題を最小限に抑える
> - 基本的な高可用性 MongoDB クラスターをディザスター リカバリー サイトと共にデプロイする


> [!Tip]  
> ![hybrid-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack は Azure の拡張機能です。 Azure Stack はオンプレミス環境にクラウド コンピューティングの機敏性とイノベーションをもたらし、ハイブリッド アプリをビルドし、どこにでもデプロイできる唯一のハイブリッド クラウドを可能にします。  
> 
> [ハイブリッド アプリケーションのための設計の考慮事項](azure-stack-edge-pattern-overview.md)に関する記事では、ハイブリッド アプリケーションを設計、デプロイ、および運用するためのソフトウェア品質の重要な要素 (配置、スケーラビリティ、可用性、回復性、管理容易性、およびセキュリティ) についてレビューしています。 これらの設計の考慮事項は、ハイブリッド アプリの設計を最適化したり、運用環境での課題を最小限に抑えたりするのに役立ちます。



## <a name="architecture-for-mongodb-with-azure-stack"></a>Azure Stack 利用時の MongoDB のアーキテクチャ

![Azure Stack の高可用性 MongoDB](media/azure-stack-solution-mongdb-ha/image1.png)

## <a name="prerequisites-for-mongodb-with-azure-stack"></a>Azure Stack 利用時の MongoDB の前提条件

  - 接続された 2 つの Azure Stack Integrated Systems (Azure Stack)。このデプロイは、Azure Stack 開発キット (ASDK) では動作しません。 Azure Stack の詳細については、「[Azure Stack とは](https://azure.microsoft.com/overview/azure-stack/)」を参照してください。
      - 各 Azure Stack のテナント サブスクリプション。    
      - **各サブスクリプション ID と Azure Resource Manager エンドポイントを Azure Stack ごとにメモしてください。**
  - 各 Azure Stack のテナント サブスクリプションに対する権限が与えられた Azure Active Directory (Azure AD) サービス プリンシパル。 複数の Azure AD テナントに対して Azure Stack がデプロイされている場合、サービス プリンシパルを 2 つ作成しなければならないことがあります。 Azure Stack のサービス プリンシパルを作成する方法については、[Azure Stack リソースへのアクセスをアプリケーションに提供するサービス プリンシパルを作成する](https://docs.microsoft.com/azure-stack/user/azure-stack-create-service-principals)方法に関するページを参照してください。    
      - **各サービス プリンシパルのアプリケーション ID、クライアント シークレット、テナント名 (xxxxx.onmicrosoft.com) をメモしてください。**
  - 各 Azure Stack の Marketplace にシンジケート化された Ubuntu 16.04。 マーケットプレース シンジケーションの詳細については、「[Azure から Azure Stack に Marketplace の項目をダウンロードする](https://docs.microsoft.com/azure-stack/operator/azure-stack-download-azure-marketplace-item)」を参照してください。
  - ローカル コンピューターにインストールされた [Docker for Windows](https://docs.docker.com/docker-for-windows/)。

## <a name="get-the-docker-image"></a>Docker イメージを取得する

各デプロイの Docker イメージにより、異なる Azure PowerShell バージョン間の依存関係イシューがなくなります。
1.  Docker for Windows で Windows コンテナーが使用されていることを確認します。
2.  管理者特権でのコマンド プロンプトで次を実行し、Docker コンテナーとデプロイ スクリプトを取得します。
```powershell  
docker pull intelligentedge/mongodb-hadr:1.0.0
```

## <a name="deploy-the-clusters"></a>クラスターのデプロイ

1.  コンテナー イメージが正常にプルされたら、イメージを起動します。

    ```powershell  
    docker run -it intelligentedge/mongodb-hadr:1.0.0 powershell
    ```

2.  コンテナーが起動すると、コンテナーに管理者特権の PowerShell ターミナルが表示されます。 ディレクトリを変更し、デプロイ スクリプトに移動します。

    ```powershell  
    cd .\MongoHADRDemo\
    ```

3.  デプロイを実行します。 必要とされる箇所で資格情報とリソース名を入力します。 HA は HA クラスターがデプロイされる Azure Stack を参照し、DR は DR クラスターがデプロイされる Azure Stack を参照します。

    ```powershell
    .\Deploy-AzureResourceGroup.ps1 `
    -AzureStackApplicationId_HA "applicationIDforHAServicePrincipal" `
    -AzureStackApplicationSercet_HA "clientSecretforHAServicePrincipal" `
    -AADTenantName_HA "hatenantname.onmicrosoft.com" `
    -AzureStackResourceGroup_HA "haresourcegroupname" `
    -AzureStackArmEndpoint_HA "https://management.haazurestack.com" `
    -AzureStackSubscriptionId_HA "haSubscriptionId" `
    -AzureStackApplicationId_DR "applicationIDforDRServicePrincipal" `
    -AzureStackApplicationSercet_DR "ClientSecretforDRServicePrincipal" `
    -AADTenantName_DR "drtenantname.onmicrosoft.com" `
    -AzureStackResourceGroup_DR "drresourcegroupname" `
    -AzureStackArmEndpoint_DR "https://management.drazurestack.com" `
    -AzureStackSubscriptionId_DR "drSubscriptionId"
    ```

4.  「`Y`」と入力すると、NuGet プロバイダーのインストールが許可され、API Profile "2018-03-01-hybrid" モジュールのインストールが開始されます。

5.  HA リソースが最初にデプロイされます。 デプロイを監視し、完了するまで待ちます。 HA デプロイ完了のメッセージが表示されたら、HA Azure Stack のポータルでデプロイされたリソースを確認できます。 

6.  続いて DR リソースをデプロイし、クラスターとやりとりする目的で、DR Azure Stack でジャンプ ボックスを有効にするかどうかを決定します。

7.  DR リソース デプロイが完了するまで待ちます。

8.  DR リソース デプロイが完了したら、コンテナーを終了します。

  ```powershell
  exit
  ```

## <a name="next-steps"></a>次の手順

  - DR Azure Stack でジャンプ ボックス VM を有効にした場合、mongo CLI をインストールすることで、SSH 経由で接続し、MongoDB クラスターとやりとりできます。 MongoDB とやりとりする方法については、「[mongo Shell](https://docs.mongodb.com/manual/mongo/)」(mongo シェル) を参照してください。

  - ハイブリッド クラウド アプリケーションの詳細については、[ハイブリッド クラウド ソリューション](https://aka.ms/azsdevtutorials)に関するページを参照してください。

  - [GitHub](https://github.com/Azure-Samples/azure-intelligent-edge-patterns) のこのサンプルに合わせてコードを変更します。
