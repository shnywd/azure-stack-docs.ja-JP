---
title: Azure と Azure Stack へのアプリのデプロイ
description: ハイブリッド CI/CD パイプラインを使用して、アプリを Azure と Azure Stack にデプロイする方法を説明します。
services: azure-stack
documentationcenter: ''
author: bryanla
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/23/2019
ms.topic: solution
ms.author: bryanla
ms.reviewer: anajod
ms.lastreviewed: 11/07/2018
ms.openlocfilehash: 86b7fca6b9d2b9aaa322849f2490f83851a80940
ms.sourcegitcommit: d8981947a4bf7752d608e21e6fe0bf2ccd4825d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/24/2019
ms.locfileid: "68462885"
---
# <a name="deploy-apps-to-azure-and-azure-stack"></a>Azure と Azure Stack へのアプリのデプロイ

*適用対象: Azure Stack 統合システムと Azure Stack Development Kit*

このソリューションは、Azure Pipelines のハイブリッド継続的インテグレーションと継続的デリバリー (CI/CD) を使用して、Azure と Azure Stack にアプリをデプロイする方法を示します。

[前提条件](#prerequisites)に従って Azure Stack、Azure DevOps、および Web アプリをセットアップした後、次のことができます。

> [!div class="checklist"]
> - Web アプリを登録し、Azure Active Directory (Azure AD) で Azure Pipelines アクセスを設定します。 
> - Azure AD および Active Directory フェデレーション サービス (AD FS) 用の Azure Pipelines エンドポイントを作成します。 
> - Azure Pipelines ビルド エージェントを Azure Stack ビルド サーバーにインストールします。 
> - Azure および Azure Stack への自己完結型アプリ デプロイを構成します。
> - プロジェクトへのコード コミットに基づいて自動ビルドを実行するビルド パイプラインを作成します。
> - Azure AD と Azure Stack の両方にビルドを自動的にデプロイするリリース パイプラインを作成します。 
> - リリースを手動で作成してデプロイし、リリースの概要とログを表示します。 

CI/CD の詳細については、次の記事を参照してください。

* [継続的インテグレーションとは](https://www.visualstudio.com/learn/what-is-continuous-integration/)
* [継続的デリバリーとは](https://www.visualstudio.com/learn/what-is-continuous-delivery/)

## <a name="azure-stack-and-hybrid-cicd"></a>Azure Stack とハイブリッド CI/CD

Microsoft Azure Stack は Azure を拡張したもので、クラウド コンピューティングの俊敏性やイノベーションをオンプレミス環境にもたらします。 オンプレミスおよびパブリック クラウド環境でハイブリッド アプリを構築してデプロイできる唯一のハイブリッド クラウドです。 Azure Stack でアプリを作成し、Azure Stack、Azure、または Azure ハイブリッド クラウドにデプロイすることができます。 

アプリ デプロイの継続性、セキュリティ、および信頼性は、お客様の組織および開発チームにとって非常に重要な要素です。 Azure Pipelines ハイブリッド CI/CD 配信モデルを使用すると、オンプレミス環境とパブリック クラウドにまたがってビルド パイプラインを統合し、アプリを変更することなくデプロイ場所を変更することができます。 他にも、ハイブリッド アプローチの使用には次のような利点があります。

- オンプレミスの Azure Stack 環境と Azure パブリック クラウドとの間で各種開発ツールの一貫性を確保できます。  一連のツールが共通化されることで、CI/CD のパターンや手法が導入しやすくなります。
- Azure または Azure Stack にデプロイされたアプリとサービスは互換性があり、どちらの場所でも同じコードを実行することができます。 オンプレミスとパブリック クラウドの機能を活かすことができます。

> [!TIP]
> ![hybrid-pillars.png](./media/azure-stack-solution-pipeline/hybrid-pillars.png)  
> 「[Azure Stack 向けのハイブリッド クラウド設計パターン](azure-stack-edge-pattern-overview.md)」の記事では、ハイブリッド アプリケーションを設計、デプロイ、および運用するためのソフトウェア品質の柱について説明しています。 品質基準には、配置、スケーラビリティ、可用性、回復性、管理容易性、セキュリティなどがあります。 これらの設計の考慮事項は、ハイブリッド アプリの設計を最適化したり、運用環境での課題を最小限に抑えたりするのに役立ちます。

## <a name="prerequisites"></a>前提条件

- Azure と Azure Stack の基礎知識。 このソリューションをデプロイする前に、次の記事で詳細を確認してください。
  
  - [Azure 入門](https://azure.microsoft.com/overview/what-is-azure/)
  - [Azure Stack の概要](../operator/azure-stack-overview.md)
  
- Azure サブスクリプション。 お持ちでない場合は、開始する前に[無料アカウントを作成](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)してください。
  
- Azure で作成された Web アプリ。 [Azure Resource Manager テンプレート](https://azure.microsoft.com/resources/templates/)を使用して、オンプレミスとパブリック クラウドの両方にデプロイできる Web アプリを作成します。 後で使用するために、アプリの URI をメモしておきます。 
  
- Visual Studio 2019 が[インストール](/visualstudio/install/install-visual-studio)されていること。
  
- パイプラインを作成できる [Azure DevOps](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services) 組織、および DevOps [プロジェクト](/azure/devops/organizations/projects/create-project)または[ワークスペース](/azure/devops/repos/tfvc/create-work-workspaces)への管理者アクセス。 
  
- .NET 3.5 を含む Windows Server 2016 イメージが Azure Stack 上にあること。これは、プライベート ビルド エージェント仮想マシン (VM) の Azure Pipelines ビルド用です。
  
- Azure Stack 統合システム、または Azure Stack Development Kit (ASDK) が、以下の手順に従ってデプロイおよび構成されていること。 
  
  
  Azure Stack Development Kit (ASDK) は、Azure Stack の単一ノード デプロイであり、無料でダウンロードして使用できます。 ASDK では、Azure Stack を評価したり、非運用環境で Azure API やツールを使用したりできます。
  
  Azure AD または Active Directory フェデレーション サービス (AD FS) の管理者の資格情報を持つユーザーなら誰でも、ASDK をデプロイできます。 運用 Azure Stack は、Azure OEM/ハードウェア パートナーがデプロイできます。 次の Azure Stack 構成タスクを実行するには、Azure Stack オペレーターである必要があります。 
  
  - Azure App Service のデプロイ
  - プランとオファーの作成
  - テナント サブスクリプションの作成
  - Windows Server 2016 イメージの適用
  
  > [!Note]
  > ASDK のインストールは 7 時間ほどかかるため、それに応じて計画を立ててください。
    
  ASDK をデプロイして構成するには、次のようにします。
  
  1. [インストーラーを使った ASDK のデプロイ](../asdk/asdk-install.md)の記事にある、詳しいデプロイ手順に従います。
     
  1. [ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1) PowerShell スクリプトを使って ASDK デプロイ後の手順を自動化します。
     
  1. [Azure App Service](../operator/azure-stack-app-service-deploy.md) PaaS サービスを Azure Stack にデプロイします。
     
  1. Azure Stack で[プランとオファー](../operator/azure-stack-plan-offer-quota-overview.md)を作成します。
     
  1. Azure Stack で、オファーの[テナント サブスクリプション](../operator/azure-stack-subscribe-plan-provision-vm.md)を作成します。 
     
  1. テナント サブスクリプションに Web アプリを作成します。 後で使用できるように新しい Web アプリの URL を書き留めておきます。
     
  1. Azure Pipelines を実行するビルド サーバーにするために、.NET 3.5 を含む Windows Server 2016 VM をテナント サブスクリプションにデプロイします。
  
  > [!Note]
  > Windows Server と SQL Server を実行するための適切なイメージが Azure Stack 環境に必要です。 また、App Service がデプロイされている必要があります。

## <a name="register-your-web-app-and-give-it-access-to-resources"></a>Web アプリを登録してリソースへのアクセスを許可する 

Azure Active Directory (Azure AD) では、Azure Pipelines は*サービス プリンシパル*を使用して Azure Resource Manager に対して認証を行います。 Azure Pipelines 用のリソースをプロビジョニングするには、Azure サブスクリプションにおいてサービス プリンシパルが**共同作成者**のロールを持っている必要があります。 

Azure portal を使用してアプリの認証を構成できます。 

1. アプリを登録してサービス プリンシパルを作成します。
1. *ロールベースのアクセス制御 (RBAC)* を使用して、**共同作成者**のロールをサービス プリンシパル名 (SPN) に付与します。
1. アプリケーション ID とテナント ID の値をコピーして保存します。これらは、Azure Pipelines 用のエンドポイントを作成するために必要です。 
1. アプリケーション秘密鍵の値を作成して保存します。

[PowerShell スクリプトを使用して](https://github.com/Microsoft/vsts-rm-extensions/blob/master/TaskModules/powershell/Azure/SPNCreation.ps1#L5)サービス プリンシパルとエンドポイントを作成することもできます。 [既存のサービス プリンシパルとの Azure Resource Manager サービス接続の作成](/vsts/pipelines/library/connect-to-azure?view=vsts#create-an-azure-resource-manager-service-connection-with-an-existing-service-principal)に関する記事で、このプロセスについて説明しています。

 > [!Note]  
 > PowerShell スクリプトを使用して Azure Stack の Azure Resource Manager エンドポイントを作成する場合は、 **-azureStackManagementURL** パラメーターと **-environmentName** パラメーターを渡す必要があります。 例:  
 > `-azureStackManagementURL https://management.local.azurestack.external -environmentName AzureStack`

### <a name="register-your-app-in-azure-ad"></a>Azure AD でアプリを登録する 

1. [Azure portal](https://portal.azure.com) で、 **[Azure Active Directory]** を選択し、左側のナビゲーションで **[アプリの登録]** を選択します。
   
1. **[新規登録]** を選択します。
   
1. **[アプリケーションの登録]** ページで、次のようにします。
   1. Web アプリの名前を入力します。
   1. サポートされているアカウントの種類を選択します。 
   1. **[リダイレクト URI]** で、作成するアプリケーションの種類として **[Web]** を選択して、Web アプリの URI を入力します。 
   1. **[登録]** を選択します。
      
      ![アプリケーションの登録](./media/azure-stack-solution-pipeline/create-app.png) 

### <a name="assign-the-app-to-a-role"></a>アプリをロールに割り当てる

サブスクリプション内のリソースにアクセスするには、それに対するロールにアプリケーションを割り当てる必要があります。 Azure RBAC を使用して、それぞれの職務を遂行するユーザーに必要なアクセスのレベルを制御することができます。 RBAC の詳細については、[Azure サブスクリプション リソースへのアクセスの管理](/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json)に関する記事を参照してください。 利用可能なロールについては、「[RBAC: 組み込みのロール](/azure/role-based-access-control/built-in-roles)に関するページをご覧ください。

Azure Stack サブスクリプションにリソースをプロビジョニングできるようにするには、Azure Pipelines に**共同作成者**ロールが必要です。 

ロール スコープはサブスクリプション、リソース グループ、またはリソースのレベルで設定できます。 アクセス許可は、スコープの下位レベルに継承されます。 たとえば、アプリをリソース グループの**閲覧者**ロールに追加すると、アプリはリソース グループとそのすべてのリソースを読み取ることができます。

アプリケーションを**共同作成者**ロールに割り当てるには、次のようにします。

1. Azure portal で、目的のスコープのレベルに移動します。 たとえば、サブスクリプション スコープでロールを割り当てるには、 **[すべてのサービス]** および **[サブスクリプション]** を選択します。
   
   ![サブスクリプション スコープでロールを割り当てる](./media/azure-stack-solution-pipeline/select-subscription.png)
   
1. アプリケーションを割り当てるサブスクリプションを選択します。
   
1. 左側のナビゲーションで **[アクセス制御 (IAM)]** を選択します。
   
1. **[ロールの割り当てを追加する]** を選択します。
   
1. **[ロールの割り当ての追加]** ダイアログで、 **[共同作成者]** ロールを選択します。 既定では、Azure AD アプリケーションは、使用可能なオプションに表示されません。 アプリケーションを見つけるには、名前を検索し、その名前を選択します。
   
   ![ロールとアプリケーションを選択する](./media/azure-stack-solution-pipeline/select-role.png)
   
1. **[保存]** を選択して、ロールの割り当てを完了します。 該当のスコープのロールに割り当てられたユーザーの一覧にアプリケーションが表示されます。

サービス プリンシパルが設定されました。 次のセクションでは、プログラムによってサインインするために Azure Pipelines が必要とする値を取得する方法について説明します。

### <a name="get-values-for-signing-in"></a>サインインするための値を取得する

Azure Pipelines のエンドポイントを作成するときは、テナント ID とアプリケーション ID を入力する必要があります。 これらの値を取得するには、次のようにします。

1. Azure Portal で、 **[Azure Active Directory]** を選びます。
   
1. 左側のナビゲーションで **[アプリの登録]** を選択し、アプリケーションを選択します。
   
1. エンドポイントの作成に使用する **[ディレクトリ (テナント) ID]** と **[Application (client) ID]\(アプリケーション (クライアント) ID\)** をコピーして保存します。
   
   ![ディレクトリ (テナント ID) とアプリケーション (クライアント) ID をコピーする](./media/azure-stack-solution-pipeline/copy-app-id.png)

### <a name="create-a-new-application-secret"></a>新しいアプリケーション シークレットを作成する

Azure Pipelines 用のエンドポイントを作成するときに、アプリを認証する必要があります。 [証明書](/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets)またはアプリケーション シークレットを使用できます。 AD FS を ID プロバイダーにして Azure Stack をデプロイした場合は、証明書を使用する必要があります。

「[証明書とシークレット](/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets)」の手順に従って、新しい証明書を作成してアップロードします。 

または、新しいアプリケーション シークレットを作成するには、次のようにします。

1. Azure Portal で、 **[Azure Active Directory]** を選びます。
   
1. 左側のナビゲーションで **[アプリの登録]** を選択し、アプリケーションを選択します。
   
1. 左側のナビゲーションで、 **[証明書とシークレット]** を選択します。
   
1. **[クライアント シークレット]** で、 **[新しいクライアント シークレット]** を選択します。
   
1. **[クライアント シークレットの追加]** で、説明を入力し、有効期限を選択して **[追加]** を選択します。
   
1. 新しいシークレットの **[値]** をコピーします。 アプリとしてサインインするための値を指定する必要があります。 この値は、このページを閉じると二度と表示されないため、ここで保存しておくことが重要です。
   
   ![後からの取得はできないのでシークレットの値をコピーする](./media/azure-stack-solution-pipeline/copy-secret.png)

## <a name="create-endpoints"></a>エンドポイントを作成する

エンドポイントを作成することによって、Azure Pipelines ビルドは Azure AD アプリを Azure Stack にデプロイできます。 Azure Pipelines がビルド エージェントに接続し、エージェントが Azure Stack に接続します。

エンドポイント作成のアクセス許可を設定した後、Azure AD または AD FS 用のエンドポイントを作成できます。 

- Azure AD を ID プロバイダーにして Azure Stack をデプロイした場合、証明書またはアプリケーション シークレットを使用して、Azure デプロイ用の Azure Resource Manager サービス接続を作成できます。 
  
- Azure Stack の ID プロバイダーとして Active Directory フェデレーション サービス (AD FS) を使用した場合は、クライアント シークレットではなく証明書を使用して認証用にサービス接続を作成する必要があります。 

### <a name="set-endpoint-creation-permissions"></a>エンドポイント作成のアクセス許可を設定する

1. 対象の Azure DevOps 組織およびプロジェクトで、 **[プロジェクトの設定]** を選択します。 
   
1. **[セキュリティ]** を選択し、 **[Azure DevOps グループ]** で **[エンドポイント管理者]** を選択します。
   
1. **[メンバー]** タブで **[追加]** を選択します。
   
1. **[Add users and groups]\(ユーザーおよびグループの追加\)** で、自分自身を含むユーザー名を一覧から選択して、 **[Save changes]\(変更の保存\)** を選択します。
   
   ![メンバーを追加する](./media/azure-stack-solution-pipeline/endpoint-permissions.png)
   
1. **[Azure DevOps グループ]** の一覧で、 **[エンドポイント作成者]** を選択し、前の手順を繰り返して **[エンドポイント作成者]** グループにユーザーを追加します。 

### <a name="create-an-endpoint-for-azure-ad-or-ad-fs-deployments"></a>Azure AD または AD FS デプロイ用のエンドポイントを作成する

[既存のサービス プリンシパルとの Azure Resource Manager サービス接続の作成](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-with-an-existing-service-principal)に関するページの指示に従って、サービス接続エンドポイントを作成します。  

次の値を使用してフォームに入力します。 

- **[接続名]** : このサービス接続を参照するときに使用するわかりやすい名前を入力します。
  
- **[環境]** : **[AzureCloud]** や **[AzureStack]** などの環境名を選択します。 [AzureStack] がドロップダウン リストにない場合は、[Azure Stack への接続](/azure/devops/pipelines/library/connect-to-azure?view=azure-devops#connect-to-azure-stack)に関する項目を参照してください。
  
- **[環境 URL]** : **[AzureCloud]** を選択しなかった場合、*https:\//management.local.azurestack.external* のような環境 URL を入力します。
  
- **[Scope level]\(スコープ レベル\)** : 必要なスコープ レベル (例: **[サブスクリプション]** ) を選択します。 
  
- **サブスクリプション ID**:サブスクリプション ID を入力します。
  
- **[サブスクリプション名]** : Azure Stack のユーザー名を入力します。
  
- **[サービス プリンシパルのクライアント ID]** : 以前に保存した**アプリケーション (クライアント) ID** を入力します。 
  
- **[サービス プリンシパル キー]** または **[証明書]** : どちらかのオプションを選択します。 
  
  > [!NOTE]
  > AD FS エンドポイントを作成するには、証明書を認証に使用する必要があります。 
  
  - **[サービス プリンシパル キー]** には、以前に保存したクライアント シークレットの値を入力します。
  - **[証明書]** を選択した場合、 *.pem* 証明書ファイルの証明書セクションと秘密キー セクションの両方の内容を入力します。 
    
    > [!NOTE]
    > *.pfx* を *.pem* 証明書ファイルに変換するには、`openssl pkcs12 -in file.pfx -out file.pem -nodes -password pass:<password_here>` を実行します。
  
- **テナント ID**:以前に保存した**ディレクトリ (テナント) ID** を入力します。
  
- **[Connection: Not verified]\(接続: 未検証\)** : **[接続の確認]** を選択して、サービス プリンシパルへの接続設定を検証します。
  
  > [!NOTE]
  > Azure Resource Manager エンドポイントがインターネットに公開されていない場合、接続の検証は失敗します。 これは想定されているため、簡単なタスクのリリース パイプラインを作成することで接続を検証できます。

## <a name="install-and-configure-the-build-agent"></a>ビルド エージェントをインストールして構成する 

Web アプリをビルドしてデプロイする場合、Azure Pipelines でホスト ビルド エージェントを使用すると便利です。 エージェントのメンテナンスとアップグレードは Azure によって自動的に実行されるため、継続的で中断のない開発サイクルが実現します。

Azure DevOps で、Azure Stack に使用する個人用アクセス トークン (PAT) を作成します。 次に、PAT を使用して、Azure Stack ビルド サーバー VM にビルド エージェントをデプロイして構成します。 
   
### <a name="create-a-personal-access-token"></a>個人用アクセス トークンを作成する

1. Azure DevOps にサインインし、右上隅にある **[マイ プロファイル]** を選択します。 
   
1. プロファイル ページで、対象の Azure Stack 組織名の横にあるドロップダウン リストを展開して、 **[セキュリティの管理]** を選択します。 
   
   ![セキュリティの管理](media/azure-stack-solution-pipeline/managesecurity.png)
   
1. **[個人用アクセス トークン]** ページで、 **[新しいトークン]** を選択します。
   
   ![個人用アクセス トークン](media/azure-stack-solution-pipeline/pats.png)
   
1. **[Create a new personal access token]\(新しい個人用アクセス トークンの作成\)** ページで、トークン情報を入力して **[作成]** を選択します。 
   
   ![PAT の作成](media/azure-stack-solution-pipeline/createpat.png)
   
1. トークンをコピーして保存します。 Web ページを閉じた後は二度と表示されません。
   
   ![PAT の警告](media/azure-stack-solution-pipeline/patwarning.png)
   
### <a name="install-and-configure-the-agent-on-the-build-server"></a>ビルド サーバーにエージェントをインストールして構成する

1. Azure Stack ホストにデプロイしたビルド サーバー VM に接続します。
   
1. ビルド エージェントのイメージをダウンロードします。 
   
1. 管理者のコマンド プロンプトで、PAT を使用してエージェントをサービスとしてデプロイし、実行します。
   
1. 抽出したビルド エージェントのフォルダーに移動して、*config.cmd* ファイルを実行します。 *config.cmd* は、追加ファイルでビルド エージェントのフォルダーを更新します。
   
   ![ビルド エージェントをインストールして構成する](media/azure-stack-solution-pipeline/buildagent.png)

エンドポイントを作成し、ビルドサーバーに Azure Pipelines ビルド エージェントをインストールしたので、Azure Stack 接続への Azure Pipelines を使用する準備ができました。 Azure Stack のビルド エージェントは、Azure Pipelines から命令を受け取った後、Azure Stack との通信のためのエンドポイント情報を伝達します。

各エージェントを個別に管理するのではなく、*エージェント プール*にまとめて整理することができます。 エージェント プールでは、そのプール内のすべてのエージェントに対して共有境界が定義されています。 エージェント プールの対象範囲は、Azure DevOps 組織になります。これは、プロジェクト間でエージェント プールを共有できることを意味します。 エージェント プールの詳細については、[エージェント プールとキューの作成](/azure/devops/pipelines/agents/pools-queues)に関するページをご覧ください。

## <a name="create-build-and-release-pipelines"></a>ビルド パイプラインとリリース パイプラインを作成する 

Azure Pipelines が提供するパイプラインは自由に構成でき、管理性にも優れ、開発、ステージング、品質保証 (QA)、運用など、さまざまな環境へのリリースに使用できます。 このリリース プロセスの一環として、アプリケーション ライフ サイクルの特定のステージで承認を要求することもできます。

ソリューションのこの部分では、次のことを行います。

- Visual Studio で、Azure DevOps プロジェクトを複製し、このプロジェクトに接続し、コードを追加します。
- 自己完結型 Web アプリ デプロイを作成します。
- CI/CD ビルド パイプラインおよびリリース パイプラインを構成します。

ハイブリッド CI/CD は、アプリ コードとインフラストラクチャ コードの両方に適用できます。 [Azure Resource Manager ハイブリッド テンプレート](https://azure.microsoft.com/resources/templates/)を使用して、Azure Web アプリのコードをオンプレミスおよびパブリック クラウドにデプロイできます。

### <a name="clone-your-project"></a>プロジェクトを複製する

1. Visual Studio **チーム エクスプローラー**で、 **[接続]** アイコンを選択し、Azure DevOps 組織にサインインします。 
   
1. **[接続の管理]**  >  **[プロジェクトに接続]** を選択します。 
   
   ![チーム エクスプローラーからプロジェクトに接続する](media/azure-stack-solution-pipeline/connecttoprojectteamexp.png)

1. **[プロジェクトに接続]** ダイアログで、Web アプリ プロジェクトを選択し、ローカル パスを設定し、 **[Clone]\(複製\)** を選択して、リポジトリをローカルに複製します。
   
   ![[プロジェクトに接続] でリポジトリを複製](media/azure-stack-solution-pipeline/cloneproject.png)

### <a name="create-a-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>両方のクラウドで App Services の自己完結型 Web アプリ デプロイを作成する

1. Visual Studio **ソリューション エクスプローラー**で、*WebApplication.csproj* ファイルを開いて `<RuntimeIdentifier>win10-x64</RuntimeIdentifier>` を追加します。 この手順の詳細については、「[自己完結型の展開 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)」を参照してください。
   
   ![RuntimeIdentifier を構成する](media/azure-stack-solution-pipeline/runtimeidentifier.png)
   
1. 作業内容を保存し、**チーム エクスプローラー**を使用してコードをプロジェクトにチェックインします。

### <a name="create-a-build-pipeline-and-run-a-build"></a>ビルド パイプラインを作成してビルドを実行する

1. Web ブラウザーで、対象の Azure DevOps 組織およびプロジェクトを開きます。
   
1. 左側のナビゲーションで **[パイプライン]**  >  **[ビルド]** を選択し、 **[新しいパイプライン]** を選択します。 
   
1. **[テンプレートの選択]** で、 **[ASP.NET Core]** テンプレートを選択し、 **[適用]** を選択します。 
   
1. 構成ページの左側のウィンドウで **[Publish]\(発行\)** を選択します。
   
1. 右側のウィンドウの **[引数]** で、`-r win10-x64` を構成に追加します。 
   
   ![ビルド パイプラインの引数を追加する](media/azure-stack-solution-pipeline/buildargument.png)
   
1. ページの最上部で **[保存してキューに登録]** を選択します。
   
1. **[Run pipeline]\(パイプラインの実行\)** ダイアログで、 **[Save and run]\(保存および実行\)** を選択します。 
   
[自己完結型のデプロイ ビルド](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd)により、Azure と Azure Stack の両方で実行できる成果物が発行されます。

### <a name="create-a-release-pipeline"></a>リリース パイプラインを作成する

ハイブリッド CI/CD 構成プロセスの最後の手順は、リリース パイプラインの作成です。 リリース パイプラインを使用してリリースを作成し、ビルドをデプロイします。

1. 対象の Azure DevOps プロジェクトで、左側のナビゲーションから **[パイプライン]**  >  **[リリース]** を選択し、 **[新しいパイプライン]** を選択します。 
   
1. **[テンプレートの選択]** ページで、 **[Azure App Service の配置]** を選んでから **[適用]** を選択します。
   
   ![リリース テンプレートを選択する](media/azure-stack-solution-pipeline/releasetemplate.png)
   
1. **[パイプライン]** タブで、左側のウィンドウから **[成果物の追加]** を選択します。 右側のウィンドウで、作成したばかりの Web アプリ ビルドを **[Source (build pipeline)]\(ソース (ビルド パイプライン)\)** ドロップダウン メニューから選択し、 **[追加]** を選択します。
   
   ![ビルド成果物を追加する](media/azure-stack-solution-pipeline/addartifact.png)
   
1. **[パイプライン]** タブの **[ステージ]** で、 **[Stage 1]\(ステージ 1\)** にあるハイパーリンクを選択して**ステージ タスクを表示**します。
   
   ![ステージ タスクの表示](media/azure-stack-solution-pipeline/viewstagetasks.png)
   
1. **[タスク]** タブで、 **[ステージ名]** に「*Azure*」と入力します。 
   
1. **[パラメーター]** で、ご利用のサブスクリプションを **[Azure サブスクリプション]** ドロップダウン リストから選択し、対象の **[App Service の名前]** を入力します。
   
   ![サブスクリプションの選択と App Service の名前の入力](media/azure-stack-solution-pipeline/stage1.png)
   
1. 左側のウィンドウで、 **[エージェントで実行]** を選択します。 右側のウィンドウで、 **[Hosted VS2017]\(ホストされた VS2017\)** がまだ **[エージェント プール]** ドロップダウン リストから選択されていない場合、選択します。
   
   ![ホストされたエージェントの選択](media/azure-stack-solution-pipeline/agentjob.png)
   
1. 左側のウィンドウで **[Deploy Azure App Service]\(Azure App Service をデプロイする\)** を選択し、右側のウィンドウで、対象の Azure Web アプリ ビルドの**パッケージまたはフォルダー**を参照して指定します。
   
   ![パッケージまたはフォルダーを選択](media/azure-stack-solution-pipeline/packageorfolder.png)
   
1. **[Select a file or folder]\(ファイルまたはフォルダーの選択\)** ダイアログで、 **[OK]** を選択します。
   
1. **[新しいリリース パイプライン]** ページの右上にある **[保存]** を選択します。
   
   ![変更を保存する](media/azure-stack-solution-pipeline/save-devops-icon.png)
   
1. **[パイプライン]** タブで、 **[成果物の追加]** を選択します。 プロジェクトを選択し、対象の Azure Stack ビルドを **[ソース (ビルド パイプライン)]** ドロップダウン メニューから選択します。 **[追加]** を選択します。 
   
1. **[パイプライン]** タブの **[ステージ]** で、 **[追加]** を選択します。
   
1. 新しいステージで、ハイパーリンクを選択して**ステージ タスクを表示**します。 ステージ名を「*Azure Stack*」と入力します。 
   
   ![新しいステージの表示](media/azure-stack-solution-pipeline/newstage.png)
   
1. **[パラメーター]** で、対象の Azure Stack エンドポイントを選択し、 **[App Service の名前]** として、対象の Azure Stack Web アプリの名前を入力します。
   
1. **[エージェントで実行]** で、対象の Azure Stack ビルド サーバー エージェントを **[エージェント プール]** ドロップダウン リストから選択します。
   
1. **[Deploy Azure App Service]\(Azure App Service をデプロイする\)** で、Azure Stack ビルドの**パッケージまたはフォルダー**を選択し、 **[Select a file or folder]\(ファイルまたはフォルダーの選択\)** ダイアログで **[OK]** を選択します。
   
1. **[変数]** タブで、**VSTS_ARM_REST_IGNORE_SSL_ERRORS** という名前の変数を見つけます。 変数の値を **[true]** に設定し、そのスコープを **[Azure Stack]** に設定します。
   
   ![変数の構成](media/azure-stack-solution-pipeline/setvariable.png)
   
1. **[パイプライン]** タブで、各成果物の **[継続的配置トリガー]** アイコンを選択して、 **[有効]** に設定します。  
   
   ![継続的配置トリガーの設定](media/azure-stack-solution-pipeline/contindeploymentenabled.png)
   
1. Azure Stack のステージにある **[Pre-deployment conditions]\(デプロイ前の条件\)** アイコンを選択し、トリガーを **[After release]\(リリース後\)** に設定します。
   
   ![配置前条件のトリガーを設定](media/azure-stack-solution-pipeline/predeployafterrelease.png)
   
1. **[新しいリリース パイプライン]** ページの右上にある **[保存]** を選択して、リリース パイプラインを保存します。

> [!Note]
> リリース タスクの一部の設定は、テンプレートからリリース パイプラインを作成したときに、[カスタム変数](/azure/devops/pipelines/release/variables?view=vsts#custom-variables)として自動的に定義されている可能性があります。 これらの設定は、タスク設定では変更できませんが、親のステージ項目で編集できます。

## <a name="release-the-app"></a>アプリをリリースする

リリース パイプラインが用意できたので、それを使用してリリースを作成し、アプリをデプロイできます。 

継続的デプロイのトリガーがリリース パイプラインで設定されているため、ソース コードに変更を加えると新しいビルドが始まり、新しいリリースが自動的に作成されます。 ただし、この新しいリリースは手動で作成して実行します。

リリースを作成してデプロイするには、次のようにします。

1. 新しいリリース パイプラインのページで、右上にある **[Create release]\(リリースの作成\)** を選択します。 
   
   ![リリースを作成する ](media/azure-stack-solution-pipeline/createreleaseicon.png)
   
1. **[新しいリリースの作成]** ページで、次のようにします。
   1. **[パイプライン]** で、 **[Azure]** ステージを選択して、そのトリガーを自動から手動に変更します。 
   1. **[成果物]** で、正しい成果物が選択されていることを確認します。
   1. **[リリースの説明]** を入力して、 **[作成]** を選択します。 
   
   新しいリリースが作成されることがバナーに表示されます。 リリース名のリンクを選択して、リリースの概要ページを表示できます。ここには、デプロイの状態など、リリースに関する詳細が表示されます。
   
1. 手動リリースをデプロイするには、 **[Azure]** ステージを選択し、 **[Deploy]\(デプロイ\)** を選択し、ステージ ダイアログで **[Deploy]\(デプロイ\)** を選択します。 
   
1. デプロイが正常に完了したら、デプロイされたアプリをブラウザーで開きます。 たとえば、Azure App Services Web サイトの URL `https://<your-app-name>.azurewebsites.net` を開きます。

### <a name="monitor-and-track-releases"></a>リリースを監視および追跡する

リリース ステージにある状態のハイパーリンクを選択して、デプロイの詳細を表示することができます。 

![リリース概要ページ](media/azure-stack-solution-pipeline/releasesummary.png)

管理者は、リリースの全体的な進行状況を追跡し、どのリリースが承認待ちになっているかを簡単に確認できます。

![保留中の承認を示すリリースの概要ページ](media/azure-stack-solution-pipeline/releasepending.png)

すべてのデプロイからのリリース ログを確認できます。 

1. 対象の Azure DevOps プロジェクトで、左側の **[パイプライン]**  >  **[リリース]** を選択して、リリースを選択します。 
   
1. リリースの概要ページで、ステージをポイントまたは選択してから **[ログ]** を選択します。 
   
   リリース ログでは、左側のウィンドウに各ステージの各操作の状態が表示されます。 デプロイ時には、エージェントからリアルタイムのログが右側のウィンドウに表示されます。 デプロイ完了後、ログ ファイル全体が右側のウィンドウに表示されます。 
   
1. 左側のウィンドウで、いずれかのステップ、たとえば **[Pre-deployment approvals]\(デプロイ前の承認\)** を選択して、そのステップのログ ファイルを表示します。 
   
1. 承認を表示するには、右側のウィンドウで **[承認の表示]** を選択して、リリースを承認または却下した人物やその他の詳細を確認します。 
   
個々のステップのログを表示することで、デプロイ全体を構成する各要素の追跡とデバッグがしやすくなっています。 *.zip* ファイルとして**すべてのログをダウンロード**することもできます。
   
![リリース ログ](media/azure-stack-solution-pipeline/releaselog.png)

## <a name="next-steps"></a>次の手順

Azure のクラウド パターンの詳細については、「[Cloud Design Pattern (クラウド設計パターン)](/azure/architecture/patterns)」を参照してください。
