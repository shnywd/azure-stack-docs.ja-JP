---
title: Azure と Azure Stack に SQL Server 2016 可用性グループをデプロイする | Microsoft Docs
description: Azure と Azure Stack に SQL Server 2016 可用性グループをデプロイする方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 06/20/2019
ms.openlocfilehash: 39b0078345c16b7931f41cd2394476f8258d92dd
ms.sourcegitcommit: 104ccafcb72a16ae7e91b154116f3f312321cff7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/21/2019
ms.locfileid: "67308838"
---
# <a name="tutorial-deploy-a-sql-server-2016-availability-group-to-azure-and-azure-stack"></a>チュートリアル:Azure と Azure Stack に SQL Server 2016 可用性グループをデプロイする

この記事では、2 つの Azure Stack 環境にわたって、非同期 DR (ディザスター リカバリー) サイトと共に、基本的な高可用性 (HA) SQL Server 2016 Enterprise クラスターを自動デプロイする方法を段階的に説明します。 SQL Server 2016 と高可用性の詳細については、「[AlwaysOn 可用性グループ: 高可用性とディザスター リカバリーのソリューション](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2016)」を参照してください。

このチュートリアルでは、以下を実現するためのサンプル環境を構築します。

> [!div class="checklist"]
> - 2 つの Azure Stack にわたってデプロイを調整する
> - Docker を使用し、Azure API プロファイルで依存関係の問題を最小限に抑える
> - 基本的な高可用性 SQL Server 2016 Enterprise クラスターをディザスター リカバリー サイトと共にデプロイする

> [!Tip]  
> ![hybrid-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack は Azure の拡張機能です。 Azure Stack は、オンプレミスの環境にクラウド コンピューティングの俊敏性とイノベーションを提供し、どこでもハイブリッド アプリをビルドしてデプロイできる唯一のハイブリッド クラウドを実現します。  
> 
> [ハイブリッド アプリケーションのための設計の考慮事項](https://aka.ms/hybrid-cloud-applications-pillars)に関するホワイト ペーパーでは、ハイブリッド アプリケーションを設計、デプロイ、および運用するためのソフトウェア品質の重要な要素 (配置、スケーラビリティ、可用性、回復性、管理容易性、およびセキュリティ) についてレビューしています。 これらの設計の考慮事項は、ハイブリッド アプリケーションの設計を最適化したり、運用環境での課題を最小限に抑えたりするのに役立ちます。

## <a name="architecture-for-sql-server-2016"></a>SQL Server 2016 のアーキテクチャ

![SQL Server 2016 SQL HA Azure Stack](media/azure-stack-solution-sql-ha/image1.png)

## <a name="prerequisites-for-sql-server-2016"></a>SQL Server 2016 の前提条件

  - 接続された 2 つの Azure Stack Integrated Systems (Azure Stack)。このデプロイは、Azure Stack 開発キット (ASDK) では動作しません。 Azure Stack の詳細については、「[Azure Stack とは](https://azure.microsoft.com/overview/azure-stack/)」を参照してください。
  - 各 Azure Stack のテナント サブスクリプション。    
      - **各サブスクリプション ID と Azure Resource Manager エンドポイントを Azure Stack ごとにメモしてください。**
  - 各 Azure Stack のテナント サブスクリプションに対する権限が与えられた Azure Active Directory (Azure AD) サービス プリンシパル。 複数の Azure AD テナントに対して Azure Stack がデプロイされている場合、サービス プリンシパルを 2 つ作成しなければならないことがあります。 Azure Stack のサービス プリンシパルを作成する方法については、[Azure Stack リソースへのアクセスをアプリケーションに提供するサービス プリンシパルを作成する](https://docs.microsoft.com/azure-stack/user/azure-stack-create-service-principals)方法に関するページを参照してください。
      - **各サービス プリンシパルのアプリケーション ID、クライアント シークレット、テナント名 (xxxxx.onmicrosoft.com) をメモしてください。**
  - 各 Azure Stack の Marketplace にシンジケート化された SQL Server 2016 Enterprise。 マーケットプレース シンジケーションの詳細については、「[Azure から Azure Stack に Marketplace の項目をダウンロードする](https://docs.microsoft.com/azure-stack/operator/azure-stack-download-azure-marketplace-item)」を参照してください。
    **組織に適切な SQL ライセンスが与えられていることを確認してください。**
  - ローカル コンピューターにインストールされた [Docker for Windows](https://docs.docker.com/docker-for-windows/)。

## <a name="get-the-docker-image"></a>Docker イメージを取得する

各デプロイの Docker イメージにより、異なる Azure PowerShell バージョン間の依存関係問題がなくなります。

1.  Docker for Windows で Windows コンテナーが使用されていることを確認します。
2.  管理者特権でのコマンド プロンプトで次を実行し、Docker コンテナーとデプロイ スクリプトを取得します。

```powershell  
 docker pull intelligentedge/sqlserver2016-hadr:1.0.0
```

## <a name="deploy-the-availability-group"></a>可用性グループをデプロイする

1.  コンテナー イメージが正常にプルされたら、イメージを起動します。

      ```powershell  
      docker run -it intelligentedge/sqlserver2016-hadr:1.0.0 powershell
      ```

2.  コンテナーが起動すると、コンテナーに管理者特権の PowerShell ターミナルが表示されます。 ディレクトリを変更し、デプロイ スクリプトに移動します。

      ```powershell  
      cd .\SQLHADRDemo\
      ```

3.  デプロイを実行します。 必要とされる箇所で資格情報とリソース名を入力します。 HA は HA クラスターがデプロイされる Azure Stack を参照し、DR は DR クラスターがデプロイされる Azure Stack を参照します。

      ```powershell
      > .\Deploy-AzureResourceGroup.ps1 `
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

5.  リソース デプロイが完了するまで待ちます。

6.  DR リソース デプロイが完了したら、コンテナーを終了します。

      ```powershell
      exit
      ```

7.  各 Azure Stack のポータルでリソースを表示し、デプロイを調べます。 HA 環境で SQL インスタンスの 1 つに接続し、SQL Server Management Studio (SSMS) で可用性グループを調べます。

![SQL Server 2016 SQL HA](media/azure-stack-solution-sql-ha/image2.png)

## <a name="next-steps"></a>次の手順

  - SQL Server Management Studio を使用し、手動でクラスターをフェールオーバーします。「[AlwaysOn 可用性グループの強制手動フェールオーバーの実行 (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server?view=sql-server-2017)」を参照してください。
  - ハイブリッド クラウド アプリケーションの詳細については、[ハイブリッド クラウド ソリューション](https://aka.ms/azsdevtutorials)に関するページを参照してください。
  - 独自のデータを使用するか、[GitHub](https://github.com/Azure-Samples/azure-intelligent-edge-patterns) のこのサンプルに合わせてコードを変更します。
