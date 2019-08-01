---
title: Azure Stack にステージング データ分析ソリューションをデプロイする | Microsoft Docs
description: Azure Stack にステージング データ分析ソリューションをデプロイする方法について説明します
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.topic: conceptual
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 06/20/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/20/2019
ms.openlocfilehash: 859d80c9782926602769664006375cb131de8637
ms.sourcegitcommit: 35b13ea6dc0221a15cd0840be796f4af5370ddaf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602940"
---
# <a name="deploy-a-staged-data-analytics-solution-to-azure-stack"></a>Azure Stack にステージング データ分析ソリューションをデプロイする

この記事では、迅速な意思決定ができるように、収集の時点での分析を必要とするデータを収集するためのソリューションをデプロイする方法を示します。 このデータ収集はインターネットにアクセスせずに行われることがあります。 接続が確立されたら、リソース集約的データ分析を行って、さらに洞察を得ることが必要になる場合があります。

このソリューションでは、以下を実現するためのサンプル環境を作成します。

> [!div class="checklist"]
> - 生データのストレージ Blob を作成します。
> - クリーン データを Azure Stack から Azure に移動する新しい Azure Stack 関数を作成します。
> - Blob Storage でトリガーされる関数を作成します。
> - Blob とキューを含む Azure Stack ストレージ アカウントを作成します。
> - キューによってトリガーされる関数を作成します。
> - キューによってトリガーされる関数をテストします。

> [!Tip]  
> ![hybrid-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack は Azure の拡張機能です。 Azure Stack はオンプレミス環境にクラウド コンピューティングの機敏性とイノベーションをもたらし、ハイブリッド アプリをビルドし、どこにでもデプロイできる唯一のハイブリッド クラウドを可能にします。  
> 
> [ハイブリッド アプリケーションのための設計の考慮事項](azure-stack-edge-pattern-overview.md)に関する記事では、ハイブリッド アプリケーションを設計、デプロイ、および運用するためのソフトウェア品質の重要な要素 (配置、スケーラビリティ、可用性、回復性、管理容易性、およびセキュリティ) についてレビューしています。 これらの設計の考慮事項は、ハイブリッド アプリの設計を最適化したり、運用環境での課題を最小限に抑えたりするのに役立ちます。

## <a name="architecture-for-staged-data-analytics"></a>ステージング データ分析のためのアーキテクチャ

![ステージング データ分析](media/azure-stack-solution-staged-data/image1.png)

## <a name="prerequisites-for-staged-data-analytics"></a>ステージング データ分析の前提条件

  - Azure サブスクリプション。
  - Azure と Azure Stack でのテナント サブスクリプションに対する権限が与えられた Azure Active Directory (AAD) サービス プリンシパル。 Azure Stack が Azure サブスクリプションと異なる AAD テナントを使用している場合は、2 つのサービス プリンシパルを作成する必要があります。 Azure Stack のサービス プリンシパルを作成する方法については、[Azure Stack リソースへのアクセスをアプリケーションに与えるサービス プリンシパルを作成する](https://docs.microsoft.com/azure-stack/user/azure-stack-create-service-principals)方法に関する記事を参照してください。
      - **各サービス プリンシパルのアプリケーション ID、クライアント シークレット、Azure AD テナント ID、テナント名 (xxxxx.onmicrosoft.com) をメモしてください。**
  - データ分析のためのデータのコレクションを提供する必要があります。 サンプル データが提供されます。
  - ローカル コンピューターにインストールされた [Docker for Windows](https://docs.docker.com/docker-for-windows/)。

## <a name="get-the-docker-image"></a>Docker イメージを取得する

各デプロイの Docker イメージにより、異なる Azure PowerShell バージョン間の依存関係イシューがなくなります。
1.  Docker for Windows で Windows コンテナーが使用されていることを確認します。
2.  管理者特権でのプロンプトで次を実行し、Docker コンテナーとデプロイ スクリプトを取得します。

```
 docker pull intelligentedge/stageddatasolution:1.0.0
```

## <a name="deploy-the-solution"></a>ソリューションのデプロイ方法

1.  コンテナー イメージが正常にプルされたら、イメージを起動します。

      ```powershell  
      docker run -it intelligentedge/stageddatasolution:1.0.0 powershell
      ```

2.  コンテナーが起動すると、コンテナーに管理者特権の PowerShell ターミナルが表示されます。 ディレクトリを変更し、デプロイ スクリプトに移動します。

      ```powershell  
      cd .\SDDemo\
      ```

3.  デプロイを実行します。 必要とされる箇所で資格情報とリソース名を入力します。 HA は HA クラスターがデプロイされる Azure Stack を示し、DR は DR クラスターがデプロイされる Azure Stack を示します。

      ```powershell
      .\DeploySolution-Azure-AzureStack.ps1 `
      -AzureApplicationId "applicationIDforAzureServicePrincipal" `
      -AzureApplicationSercet "clientSecretforServicePrincipal" `
      -AzureTenantId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" `
      -AzureStackAADTenantName "azurestacktenant.onmicrosoft.com" `
      -AzureStackTenantARMEndpoint "https://management.haazurestack.com" `
      -AzureStackApplicationId "applicationIDforStackServicePrincipal" `
      -AzureStackApplicationSercet "ClientSecretforStackServicePrincipal" `
      -AzureStackTenantId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" `
      -ResourcePrefix "aPrefixForResources"
      ```

1.  メッセージが表示されたら、Azure のデプロイと Application Insights のためのリージョンを入力します。

2.  「Y」と入力すると、NuGet プロバイダーのインストールが許可され、API プロファイル "2018-03-01-hybrid" モジュールのインストールが開始され、Azure と Azure Stack へのデプロイが可能になります。

3.  リソースがデプロイされたら、データが Azure Stack と Azure の両方に生成されるかどうかをテストします。

    ```powershell  
      .\TDAGenerator.exe
    ```

4.  Azure または Azure Stack にデプロイされた Web アプリケーションに移動して処理されているデータを確認してください。

### <a name="azure-web-app"></a>Azure Web アプリ
 
![ステージング データ分析ソリューション](media/azure-stack-solution-staged-data/image2.png)
 
### <a name="azure-stack-web-app"></a>Azure Stack Web アプリ
 
![Azure Stack のステージング データ分析ソリューション](media/azure-stack-solution-staged-data/image3.png)

## <a name="next-steps"></a>次の手順

  - ハイブリッド クラウド アプリケーションの詳細については、[ハイブリッド クラウド ソリューション](https://aka.ms/azsdevtutorials)に関する記事を参照してください。

  - 独自のデータを使用するか、[GitHub](https://github.com/Azure-Samples/azure-intelligent-edge-patterns) のこのサンプルに合わせてコードを変更します。
