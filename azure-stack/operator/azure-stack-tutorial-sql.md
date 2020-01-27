---
title: Azure Stack Hub での高可用性 SQL データベースの提供
description: Azure Stack Hub で SQL Server リソース プロバイダーのホスト コンピューターと高可用性 SQL Always On データベースを作成する方法について学習します。
services: azure-stack
author: BryanLa
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: article
ms.date: 10/07/2019
ms.author: bryanla
ms.reviewer: xiaofmao
ms.lastreviewed: 10/23/2018
ms.openlocfilehash: 408508119a7535467756b0b45fbfeb59fba7fbf1
ms.sourcegitcommit: d62400454b583249ba5074a5fc375ace0999c412
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "76023009"
---
# <a name="offer-highly-available-sql-databases"></a>高可用性 SQL データベースの提供

Azure Stack Hub オペレーターとして、SQL Server データベースをホストするようにサーバー VM を構成できます。 SQL ホスティング サーバーが正常に作成され、Azure Stack Hub によって管理されていると、SQL サービスにサブスクライブしているユーザーは SQL データベースを簡単に作成できます。

この記事では、Azure Stack Hub クイックスタート テンプレートを使用して [SQL Server Always On 可用性グループ](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-2017)を作成し、それを Azure Stack Hub SQL ホスティング サーバーとして追加し、高可用性 SQL データベースを作成する方法について示します。

学習内容

> [!div class="checklist"]
> * テンプレートから SQL Server AlwaysOn 可用性グループを作成する
> * Azure Stack Hub SQL ホスティング サーバーを作成する
> * 高可用性 SQL データベースを作成する

利用可能な Azure Stack Hub Marketplace 項目を使用して、2 つの VM SQL Server Always On 可用性グループを作成および構成します。 

開始する前に、[SQL Server リソース プロバイダー](azure-stack-sql-resource-provider-deploy.md)が正常にインストールされ、Azure Stack Hub Marketplace で次の項目を確実に利用できるようにしてください。

> [!IMPORTANT]
> Azure Stack Hub クイックスタート テンプレートを使用するには、以下のすべてが必要です。

- [Windows Server 2016 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/MicrosoftWindowsServer.WindowsServer) マーケットプレース イメージ。
- Windows Server 2016 サーバー イメージ上の SQL Server 2016 SP1 または SP2 (Enterprise、Standard、または Developer) この記事では、[Windows Server 2016 マーケットプレース イメージ上で SQL Server 2016 SP2 Enterprise](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoftsqlserver.sql2016sp2-ws2016) を使用します。
- [SQL Server IaaS Extension](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) バージョン 1.2.30 以降 SQL IaaS 拡張機能を使用して、すべての Windows バージョンの Marketplace SQL Server 項目に必要なコンポーネントをインストールします。 SQL 仮想マシン上で SQL 固有の設定を構成することができます。 拡張機能がローカルの Marketplace にインストールされていない場合、SQL のプロビジョニングは失敗します。
- [Windows バージョン 1.9.1 以降のカスタム スクリプト拡張機能](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.CustomScriptExtension)。 カスタム スクリプト拡張機能は、デプロイ後の VM カスタマイズ タスクを自動的に起動するために使用できるツールです。
- [PowerShell Desired State Configuration (DSC)](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.DSC-arm) バージョン 2.76.0.0 以降。 DSC は、Windows PowerShell の管理プラットフォームであり、ソフトウェア サービスの構成データのデプロイと管理、これらのサービスが実行される環境の管理を行うことができます。

Azure Stack Hub Marketplace に項目を追加する方法の詳細については、「[Azure Stack Hub Marketplace の概要](azure-stack-marketplace.md)」を参照してください。

## <a name="create-a-sql-server-alwayson-availability-group"></a>SQL Server AlwaysOn 可用性グループを作成する
このセクションの手順を使用し、[sql-2016-Always On Azure Stack Hub クイックスタート テンプレート](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2016-alwayson)を使用して SQL Server Always On 可用性グループをデプロイします。 このテンプレートでは、Always On 可用性グループに 2 つの SQL Server Enterprise、Standard、または Developer インスタンスをデプロイします。 このテンプレートは次のリソースを作成します。

- ネットワーク セキュリティ グループ
- 仮想ネットワーク
- 4 つのストレージ アカウント (Active Directory (AD) 用、SQL 用、ファイル共有監視用、および VM 診断用にそれぞれ 1 つ)
- 4 つのパブリック IP アドレス (AD 用に 1 つ、各 SQL VM 用に 2 つ、SQL Always On リスナーにバインドされたパブリック ロード バランサー用に 1 つ)
- パブリック IP が SQL Always On リスナーにバインドされた SQL VM 用の 1 つの外部ロードバランサー
- 単一のドメインを使用する新しいフォレスト用のドメイン コントローラーとして構成された 1 台の VM (Windows Server 2016)
- SQL Server 2016 SP1 または SP2 Enterprise、Standard、または Developer エディションで構成され、クラスター化されている 2 つの VM (Windows Server 2016)。 これらは Marketplace イメージである必要があります。
- クラスターのファイル共有監視として構成された 1 つの VM (Windows Server 2016)
- SQL およびファイル共有監視 VM を含む 1 つの可用性セット  

1. 
   [!INCLUDE [azs-admin-portal](../includes/azs-admin-portal.md)]

2. **\+** **[リソースの作成]**  >  **[カスタム]** の順に選択し、 **[テンプレートのデプロイ]** を選択します。

   ![カスタム テンプレートのデプロイ](media/azure-stack-tutorial-sqlrp/1.png)


3. **[カスタム デプロイ]** ブレードで **[テンプレートの編集]**  >  **[クイック スタート テンプレート]** を選択し、使用できるカスタム テンプレートのドロップダウン リストを使用して **sql-2016-AlwaysOn** テンプレートを選択し、 **[OK]** 、 **[保存]** の順にクリックします。

   [![](media/azure-stack-tutorial-sqlrp/2-sm.PNG "Select quickstart template")](media/azure-stack-tutorial-sqlrp/2-lg.PNG#lightbox)

4. **[カスタム デプロイ]** ブレードで、 **[パラメーターの編集]** を選択し、既定値を確認します。 必要に応じて値を変更して、必要なすべてのパラメーター情報を入力し、 **[OK]** をクリックします。<br><br> 少なくとも以下を実行します。

    - ADMINPASSWORD、SQLSERVERSERVICEACCOUNTPASSWORD、および SQLAUTHPASSWORD パラメーターに複雑なパスワードを指定します。
    - DNSSUFFIX パラメーターの場合、すべての小文字で逆引き参照の DNS サフィックスを入力します (ASDK インストールの場合は、**azurestack.external**)。
    
   [![](media/azure-stack-tutorial-sqlrp/3-sm.PNG "Edit custom deployment parameters")](media/azure-stack-tutorial-sqlrp/3-lg.PNG#lightbox)

5. **[カスタム デプロイ]** ブレードで、使用するサブスクリプションを選択し、カスタム デプロイ用に新しいデプロイ グループを作成するか、既存のリソース グループを選択します。<br><br> 次に、リソース グループの場所 (ASDK のインストールの場合は **local**) を選択し、 **[作成]** をクリックします。 カスタムのデプロイ設定が検証され、デプロイが開始されます。

    [![](media/azure-stack-tutorial-sqlrp/4-sm.PNG "Create custom deployment")](media/azure-stack-tutorial-sqlrp/4-lg.PNG#lightbox)


6. 管理ポータルで、 **[リソース グループ]** を選択してから、カスタム デプロイ用に作成したリソース グループの名前を選択します (この例では **resource-group**)。 すべてのデプロイが正常に完了したことを確認するために、デプロイの状態を表示します。<br><br>次に、リソース グループ項目を確認し、**SQLPIPsql\<リソース グループ名\>** パブリック IP アドレス項目を選択します。 ロードバランサー パブリック IP のパブリック IP アドレスと完全な FQDN をメモします。 この SQL Always On 可用性グループを利用する SQL ホスティング サーバーを作成できるようにするには、これを Azure Stack Hub オペレーターに提供する必要があります。

   > [!NOTE]
   > テンプレートのデプロイには数時間かかる場合があります。

   ![カスタム デプロイが完了](./media/azure-stack-tutorial-sqlrp/5.png)

### <a name="enable-automatic-seeding"></a>自動シード処理を有効にする
テンプレートによる SQL AlwaysOn 可用性グループのデプロイと構成が正常に完了した後に、可用性グループの SQL Server の各インスタンスで[自動シード処理](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group)を有効にする必要があります。 

自動シード処理を使用して可用性グループを作成すると、Always On データベースの高可用性を確保するために必要な他の手動作業なしで、SQL Server によってグループ内のすべてのデータベースのセカンダリ レプリカが自動的に作成されます。

これらの SQL コマンドを使用して、AlwaysOn 可用性グループの自動シード処理を構成します。 必要に応じて、\<InstanceName\> をプライマリ インスタンス SQL Server の名前、<availability_group_name> を AlwaysOn 可用性グループの名前に置き換えます。 

プライマリ SQL インスタンスの場合:

  ```sql
  ALTER AVAILABILITY GROUP [<availability_group_name>]
      MODIFY REPLICA ON '<InstanceName>'
      WITH (SEEDING_MODE = AUTOMATIC)
  GO
  ```

>  ![プライマリ SQL インスタンス スクリプト](./media/azure-stack-tutorial-sqlrp/sql1.png)

セカンダリ SQL インスタンスの場合:

  ```sql
  ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
  GO
  ```
>  ![セカンダリ SQL インスタンス スクリプト](./media/azure-stack-tutorial-sqlrp/sql2.png)

### <a name="configure-contained-database-authentication"></a>包含データベース認証を構成する
包含データベースを可用性グループに追加する前に、包含データベース認証サーバー オプションが、可用性グループの可用性レプリカをホストするすべてのサーバー インスタンスで 1 に設定されていることを確認します。 詳細については、[包含データベースの認証](https://docs.microsoft.com/sql/database-engine/configure-windows/contained-database-authentication-server-configuration-option?view=sql-server-2017)に関するページを参照してください。

これらのコマンドを使用して、可用性グループ内の SQL Server インスタンスごとに包含データベース認証サーバー オプションを設定します。

  ```sql
  EXEC sp_configure 'contained database authentication', 1
  GO
  RECONFIGURE
  GO
  ```
>  ![包含データベースの認証を設定する](./media/azure-stack-tutorial-sqlrp/sql3.png)

## <a name="create-an-azure-stack-hub-sql-hosting-server"></a>Azure Stack Hub SQL ホスティング サーバーを作成する
SQL Server Alway On 可用性グループが作成され、適切に構成された後、Azure Stack Hub オペレーターは Azure Stack Hub SQL ホスティング サーバーを作成して、ユーザーがデータベースを作成できるように追加容量を利用可能にする必要があります。 

以前に SQL AlwaysOn 可用性グループのリソース グループが作成されたときにメモした SQL ロード バランサーのパブリック IP またはパブリック IP の完全な FQDN を必ず使用します (**SQLPIPsql\<リソース グループ名\>** )。 また、AlwaysOn 可用性グループの SQL インスタンスにアクセスするために使用される SQL Server 認証資格情報を知る必要があります。

> [!NOTE]
> この手順は、Azure Stack Hub オペレーターが Azure Stack Hub 管理ポータルから実行する必要があります。

Azure Stack Hub オペレーターは、SQL Always On 可用性グループのロード バランサー リスナーのパブリック IP と SQL 認証ログイン情報を使用し、[SQL Always On 可用性グループを使用して SQL ホスティング サーバーを作成](azure-stack-sql-resource-provider-hosting-servers.md#provide-high-availability-using-sql-always-on-availability-groups)できるようになりました。 

また、SQL AlwaysOn データベースをユーザーが作成できるようにするプランとオファーが作成してあることも確認してください。 Operator は **Microsoft.SqlAdapter** サービスをプランに追加し、高可用性データベース専用の新しいクォータを作成する必要があります。 プランの作成の詳細については、「[サービス、プラン、オファー、サブスクリプションの概要](service-plan-offer-subscription-overview.md)」を参照してください。

> [!TIP]
> **Microsoft.SqlAdapter** サービスは、[SQL Server リソース プロバイダーがデプロイされる](azure-stack-sql-resource-provider-deploy.md)までプランに追加することができません。

## <a name="create-a-highly-available-sql-database"></a>高可用性 SQL データベースを作成する
SQL Always On 可用性グループが Azure Stack Hub オペレーターによって Azure Stack Hub SQL ホスティング サーバーとして作成、構成、追加された後、SQL Server データベース機能を含むサブスクリプションを持つテナント ユーザーは、このセクションの次の手順に従って、Always On 機能をサポートする SQL データベースを作成できます。 

> [!NOTE]
> これらの手順は、SQL Server 機能 (Microsoft.SQLAdapter サービス) を提供するサブスクリプションを持つテナント ユーザーとして、Azure Stack Hub ユーザー ポータルから実行します。

1. 
   [!INCLUDE [azs-user-portal](../includes/azs-user-portal.md)]

2. **[\+** **リソースの作成]**  >  **[データ \+ ストレージ]** の順に選択し、 **[SQL Database]** を選択します。<br><br>名前、照合順序、最大サイズ、サブスクリプションなどの必須のデータベースのプロパティと、リソース グループ、およびデプロイに使用する場所を指定します。 

   ![SQL データベースを作成する](./media/azure-stack-tutorial-sqlrp/createdb1.png)

3. **[SKU]** を選択し、使用する適切な SQL ホスティング サーバー SKU を選択します。 この例では、Azure Stack Hub オペレーターは、SQL Always On 可用性グループの高可用性をサポートするために、**Enterprise HA** SKU を作成しました。

   ![SKU を選択する](./media/azure-stack-tutorial-sqlrp/createdb2.png)

4. **[ログイン]**  >  **[Create a new login]\(新しいログインの作成\)** の順に選択し、新しいデータベースに使用する SQL の認証資格情報を入力します。 完了したら **[OK]** 、 **[作成]** の順にクリックし、データベースのデプロイ プロセスを開始します。

   ![ログインを作成する](./media/azure-stack-tutorial-sqlrp/createdb3.png)

5. SQL データベースのデプロイが正常に完了したら、データベースのプロパティを確認し、新しい高可用性データベースへの接続に使用する接続文字列を見つけます。 

   ![接続文字列を表示する](./media/azure-stack-tutorial-sqlrp/createdb4.png)




## <a name="next-steps"></a>次のステップ

[SQL リソース プロバイダーを更新する](azure-stack-sql-resource-provider-update.md)
