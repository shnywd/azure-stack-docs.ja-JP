---
title: Azure Stack Hub のオフライン環境に Azure App Service をデプロイする
description: AD FS によって保護されているオフラインの Azure Stack Hub 環境に Azure App Service をデプロイする方法について説明します。
author: BryanLa
ms.topic: article
ms.date: 01/13/2020
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 01/13/2020
ms.openlocfilehash: 7fa4ac0f63b3f5243a6473c921012614bb01bfb2
ms.sourcegitcommit: a7db4594de43c31fe0c51e60e84fdaf4d41ef1bb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/24/2020
ms.locfileid: "77568624"
---
# <a name="deploy-azure-app-service-in-an-offline-environment-in-azure-stack-hub"></a>Azure Stack Hub のオフライン環境に Azure App Service をデプロイする

> [!IMPORTANT]
> Azure App Service 1.8 をデプロイする前に、Azure Stack Hub 統合システムに 1910 更新プログラムを適用するか、最新の Azure Stack Development Kit (ASDK) をデプロイします。

この記事の手順に従うと、次のような Azure Stack Hub 環境に [Azure App Service リソース プロバイダー](azure-stack-app-service-overview.md)をデプロイできます。
- インターネットに接続されていない。
- Active Directory フェデレーション サービス (AD FS) によって保護されている。

> [!IMPORTANT]
> リソース プロバイダーのインストーラーを実行する前に「[Azure App Service on Azure Stack Hub のデプロイの前提条件](azure-stack-app-service-before-you-get-started.md)」の手順を完了していることを確認してください。 また、1.8 リリースに付属する[リリース ノート](azure-stack-app-service-release-notes-update-eight.md)も参照し、新しい機能、修正、およびデプロイに影響を与える可能性のある既知の問題について確認してください。

Azure App Service リソース プロバイダーをオフラインの Azure Stack Hub のデプロイに追加するには、次の最上位タスクをすべて実行する必要があります。

1. [前提となる手順](azure-stack-app-service-before-you-get-started.md)をすべて実行します (証明書の購入など。受け取るまでに数日かかる場合があります)。
2. インターネットに接続されているマシンに[インストール ファイルとヘルパー ファイルをダウンロードして展開](azure-stack-app-service-before-you-get-started.md)します。
3. オフライン インストール パッケージを作成します。
4. appservice.exe インストーラー ファイルを実行します。

## <a name="create-an-offline-installation-package"></a>オフライン インストール パッケージを作成する

オフライン環境で Azure App Service をデプロイするには、まず、インターネットに接続されているマシンでオフライン インストール パッケージを作成します。

1. インターネットに接続されているマシンで AppService.exe インストーラーを実行します。 

2. **[詳細]**  >  **[オフライン インストール パッケージを作成する]** の順に選択します。 この手順は、完了までに数分かかります。

    ![Azure App Service インストーラーでオフライン パッケージを作成する][1]

3. Azure App Service インストーラーにより、オフライン インストール パッケージが作成され、そのパッケージへのパスが表示されます。 **[フォルダーを開く]** を選択すると、エクスプローラーでフォルダーが開きます。

    ![Azure App Service インストーラーで正常に生成されたオフライン インストール パッケージ](media/azure-stack-app-service-deploy-offline/image02.png)

4. インストーラー (AppService.exe) とオフライン インストール パッケージを Azure Stack Hub ホスト マシンにコピーします。

## <a name="complete-the-offline-installation-of-azure-app-service-on-azure-stack-hub"></a>Azure App Service on Azure Stack Hub のオフライン インストールを完了させる

1. Azure Stack Hub 管理の Azure Resource Management エンドポイントに到達できるコンピューターから、appservice.exe を管理者として実行します。

1. **[詳細設定]**  >  **[Complete offline installation]\(オフライン インストールを完了する\)** の順に選択します。

    ![Azure App Service インストーラーでオフライン インストールを実行する][2]

1. 先ほど作成したオフライン インストール パッケージの場所を参照し、 **[次へ]** を選択します。

    ![Azure App Service インストーラーでオフライン インストール パッケージ パスを指定する](media/azure-stack-app-service-deploy-offline/image04.png)

1. マイクロソフト ソフトウェア ライセンス条項を確認して同意し、 **[次へ]** を選択します。

1. サード パーティのライセンス条項を確認して同意し、 **[次へ]** を選択します。


1. Azure App Service クラウド構成情報が正しいことを確認します。 ASDK のデプロイ中に既定の設定を使用した場合は、ここで既定値をそのまま使用することもできます。 ただし、Azure Stack Hub のデプロイ時にオプションをカスタマイズした場合、または統合システムにデプロイしている場合は、それらの変更を反映するように、このウィンドウで値を編集する必要があります。 たとえば、ドメイン サフィックス mycloud.com を使用する場合は、Azure Stack Hub テナントの Azure Resource Manager エンドポイントを `management.<region>.mycloud.com` に変更する必要があります。 自分の情報を確認したら、 **[次へ]** を選択します。

    ![Azure App Service インストーラーで Azure App Service クラウドを構成する][3]

1. App Service インストーラーの次のページで、Azure Stack Hub に接続します。

    1. 使用する接続方法 ( **[資格情報]** または **[サービス プリンシパル]** ) を選択します。
        - **資格情報**
            - Azure Active Directory (Azure AD) を使用している場合は、Azure Stack Hub のデプロイ時に指定した、Azure AD の管理者アカウントとパスワードを入力します。 **[接続]** を選択します。
            - Active Directory フェデレーション サービス (AD FS) を使用している場合は、ご自分の管理者アカウントを指定します。 たとえば、「 cloudadmin@azurestack.local 」のように入力します。 パスワードを入力し、 **[接続]** を選択します。
        - **サービス プリンシパル**
            - 使用するサービス プリンシパルには、 **[Default Provider Subscription]\(既定のプロバイダー サブスクリプション\)** に対する **[所有者]** 権限が**必要です**
            - **[サービス プリンシパル ID]** 、 **[証明書ファイル]** 、および **[パスワード]** を指定し、 **[接続]** を選択します。

    1. **[Azure Stack Hub Subscriptions]\(Azure Stack Hub サブスクリプション\)** で、 **[Default Provider Subscription]\(既定のプロバイダー サブスクリプション\)** を選択します。  Azure App Service on Azure Stack Hub は、 **[Default Provider Subscription]\(既定のプロバイダー サブスクリプション\)** にデプロイする**必要**があります。

    1. **[Azure Stack Hub Locations]\(Azure Stack Hub の場所\)** で、デプロイしているリージョンに対応する場所を選択します。 たとえば、ASDK にデプロイしている場合は、 **[ローカル]** を選びます。

1. Azure App Service インストーラーでは、仮想ネットワークおよび関連付けられているサブネットを作成できます。 または、[こちらの手順](azure-stack-app-service-before-you-get-started.md#virtual-network)で構成された既存の仮想ネットワークにデプロイすることもできます。
   - Azure App Service インストーラーによる方法を使用する場合は、 **[既定の設定で VNet を作成する]** をオンにし、既定値を受け入れて、 **[次へ]** を選択します。
   - 既存のネットワークにデプロイする場合は、 **[Use existing VNet and Subnets]\(既存の VNet とサブネットを使用する\)** をオンにし、次の手順に従います。
       1. 仮想ネットワークが含まれている**リソース グループ**のオプションを選択します。
       2. デプロイする**仮想ネットワーク**の名前を選択します。
       3. 必要なロール サブネットそれぞれに適切な**サブネット**値を選択します。
       4. **[次へ]** を選択します。

      ![Azure App Service インストーラーでの仮想ネットワークとサブネットの情報][5]

1. ファイル共有の情報を入力してから、 **[次へ]** を選択します。 ファイル共有のアドレスには、ファイル サーバーの完全修飾ドメイン名 (FQDN)、または IP アドレスを使用する必要があります。 たとえば、\\\appservicefileserver.local.cloudapp.azurestack.external\websites、または \\\10.0.0.1\websites を使用します。  ドメインに参加しているファイル サーバーを使用している場合は、ドメインを含む完全なユーザー名を指定する必要があります。 (例: `<myfileserverdomain>\<FileShareOwner>`)。

    > [!NOTE]
    > インストーラーは、続行する前にファイル共有への接続性をテストしようとします。 ただし、既存の仮想ネットワークにデプロイすることを選んだ場合、インストーラーはファイル共有に接続できない可能性があり、続行するかどうかを確認する警告が表示されます。 ファイル共有情報を確認し、正しい場合は続行します。

   ![Azure App Service インストーラーのファイル共有情報][8]

1. 次のページで、次の操作を行います。
    1. **[Identity Application ID]\(ID アプリケーションの ID\)** ボックスで、(Azure AD から) 本人確認のために使っているアプリの GUID を入力します。
    1. **[Identity Application certificate file]\(ID アプリケーションの証明書ファイル\)** ボックスで、証明書ファイルの場所を入力 (または参照) します。
    1. **[Identity Application certificate password]\(ID アプリケーションの証明書パスワード\)** ボックスで、証明書のパスワードを入力します。 このパスワードは、証明書作成のスクリプトを使ったときに書き留めたものです。
    1. **[Azure Resource Manager root certificate file]\(Azure Resource Manager のルート証明書ファイル\)** ボックスで、証明書ファイルの場所を入力 (または参照) します。
    1. **[次へ]** を選択します。

    ![Azure App Service インストーラーでアプリ ID と証明書情報を入力する][10]

1. 3 つの証明書ファイルの各ボックスで、 **[参照]** を選択し、適切な証明書ファイルに移動します。 各証明書のパスワードを入力する必要があります。 これらの証明書は、[必要な証明書の作成の手順](azure-stack-app-service-before-you-get-started.md#get-certificates)で作成したものです。 すべての情報を入力したら、 **[次へ]** を選択します。

    | ボックス | 証明書ファイル名の例 |
    | --- | --- |
    | **App Service の既定の SSL 証明書ファイル** | \_.appservice.local.AzureStack.external.pfx |
    | **App Service API の SSL 証明書ファイル** | api.appservice.local.AzureStack.external.pfx |
    | **App Service 公開元の SSL 証明書ファイル** | ftp.appservice.local.AzureStack.external.pfx |

    証明書の作成時に別のドメイン サフィックスを使った場合は、証明書ファイル名で *local.AzureStack.external* を使わないでください。 代わりに、ご自分のカスタム ドメイン情報を使用します。

    ![Azure App Service インストーラーで SSL 証明書情報を入力する][11]

1. Azure App Service リソース プロバイダー データベースをホストするために使用するサーバー インスタンスについて、SQL Server の詳細を入力し、 **[次へ]** を選択します。 インストーラーにより、SQL 接続のプロパティが検証されます。 内部 IP または SQL Server 名の FQDN を入力する**必要**があります。

    > [!NOTE]
    > インストーラーは、続行する前に SQL Server を実行しているコンピューターへの接続性をテストしようとします。 ただし、既存の仮想ネットワークにデプロイすることを選んだ場合、インストーラーは SQL Server を実行しているコンピューターに接続できない可能性があり、続行するかどうかを確認する警告が表示されます。 SQL Server 情報を確認し、正しい場合は続行します。
    >
    > Azure App Service on Azure Stack Hub 1.3 以降では、インストーラーは、SQL Server を実行しているコンピューターがデータベースのコンテイメントを SQL Server レベルで有効にしているかどうかを確認します。 そうでない場合は、次の例外が表示されます。
    > ```sql
    >    Enable contained database authentication for SQL server by running below command on SQL server (Ctrl+C to copy)
    >    ***********************************************************
    >    sp_configure 'contained database authentication', 1;
    >    GO
    >    RECONFIGURE;
    >    GO
    >    ***********************************************************
    > ```
    > 詳細については、[Azure App Service on Azure Stack Hub 1.3 のリリース ノート](azure-stack-app-service-release-notes-update-three.md)を参照してください。

    ![Azure App Service インストーラーで SQL Server 情報を入力する][12]

1. ロール インスタンスと SKU のオプションを確認します。 ASDK デプロイの各ロールに対するインスタンスの最小数および SKU の最小値には、既定値が入力されています。 デプロイの計画に役立つ vCPU 要件とメモリ要件の概要が表示されています。 必要な項目を選択したら、 **[次へ]** を選択します。

     > [!NOTE]
     > 運用環境デプロイの場合は、「[Azure Stack Hub での Azure App Service サーバー ロールの容量計画](azure-stack-app-service-capacity-planning.md)」のガイダンスに従ってください。
     >
     >

    | Role | インスタンスの最小値 | SKU の最小値 | Notes |
    | --- | --- | --- | --- |
    | コントローラー | 1 | Standard_A2 - (2 vCPU、3584 MB) | Azure App Service クラウドの正常性を管理および維持します。 |
    | 管理 | 1 | Standard_A2 - (2 vCPU、3584 MB) | Azure App Service Azure Resource Manager および API のエンドポイント、ポータル拡張機能 (管理、テナント、Functions ポータル)、データ サービスを管理します。 フェールオーバーをサポートするには、推奨されるインスタンスを 2 つに増やします。 |
    | Publisher | 1 | Standard_A1 - (1 vCPU、1792 MB) | FTP および Web デプロイによってコンテンツを公開します。 |
    | FrontEnd | 1 | Standard_A1 - (1 vCPU、1792 MB) | Azure App Service アプリに要求をルーティングします。 |
    | 共有 Worker | 1 | Standard_A1 - (1 vCPU、1792 MB) | Web または API アプリ、および Azure Functions アプリをホストします。 より多くのインスタンスの追加が必要になる場合があります。 オペレーターは、サービスを定義することや任意の SKU レベルを選ぶことができます。 レベルには、少なくとも 1 つの vCPU が必要です。 |

    ![Azure App Service インストーラーでロール階層と SKU オプションを設定する][14]

1. **[プラットフォーム イメージの選択]** ボックスで、Azure App Service クラウド用のコンピューティング リソース プロバイダーで選択可能なイメージの中から、ご自分のデプロイの Windows Server 2016 仮想マシン (VM) イメージを選びます。 **[次へ]** を選択します。

    > [!NOTE]
    > Windows Server 2016 Core は、Azure App Service on Azure Stack Hub で使用するためにサポートされているプラットフォーム イメージでは*ありません*。  運用環境デプロイには評価版イメージを使用しないでください。 Azure App Service on Azure Stack Hub では、デプロイに使用されるイメージ上で Microsoft .NET 3.5.1 SP1 がアクティブにされている必要があります。 Marketplace にシンジケート化された Windows Server 2016 イメージでは、この機能は有効ではありません。 そのため、この機能をあらかじめ有効にして Windows Server 2016 イメージを作成し、使用する必要があります。
    >
    > カスタム イメージを作成して Marketplace に追加する方法の詳細については、「[Azure Stack Hub にカスタム VM イメージを追加する](azure-stack-add-vm-image.md)」を参照してください。 Marketplace にイメージを追加するときは、必ず以下を指定してください。
    >
    >- 発行元 = MicrosoftWindowsServer
    >- オファー = WindowsServer
    >- SKU = 2016-Datacenter
    >- バージョン = "最新" バージョンを指定

1. 次のページで、次の操作を行います。
     1. Worker ロールの VM 管理者のユーザー名とパスワードを入力します。
     2. その他のロールの VM 管理者のユーザー名とパスワードを入力します。
     3. **[次へ]** を選択します。

    ![Azure App Service インストーラーでロール VM 管理者を入力する][16]

1. 概要ページで、次のことを行います。
    1. 選択した内容を確認します。 変更を加えるには、 **[前へ]** を使って前のページに戻ります。
    2. 構成が正しい場合は、チェック ボックスをオンにします。
    3. デプロイを開始するには、 **[次へ]** を選択します。

    ![Azure App Service インストーラーで行われる選択の概要][17]

1. 次のページで、次の操作を行います。
    1. インストールの進行状況を追跡します。 既定の選択内容と Windows 2016 Datacenter の基本イメージの有効期間に基づいて App Service on Azure Stack Hub をデプロイするには、最長 240 分かかる場合があります。

    2. インストーラーの実行が完了したら、 **[終了]** を選択します。

    ![Azure App Service インストーラーでインストール プロセスを追跡する][18]

## <a name="post-deployment-steps"></a>デプロイ後の手順

> [!IMPORTANT]
> SQL Always On インスタンスで Azure App Service RP を提供した場合は、[appservice_hosting データベースと appservice_metering データベースを可用性グループに追加する](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database)*必要があります*。 また、データベースのフェールオーバーが発生した場合のサービスの損失を防ぐため、データベースを同期する必要もあります。

ファイル サーバーに接続するために既存の仮想ネットワークと内部 IP アドレスへデプロイする場合は、送信セキュリティ規則を追加して、worker サブネットとファイル サーバー間の SMB トラフィックを有効にする必要があります。 管理者ポータルでネットワーク セキュリティ グループである WorkersNsg に移動し、次のプロパティを持つ送信セキュリティ規則を追加します。

- ソース:Any
- 送信元ポート範囲: *
- 変換先:IP アドレス
- 宛先 IP アドレス範囲:ファイル サーバーの IP の範囲
- 送信先ポート範囲:445
- プロトコル:TCP
- アクション:Allow
- 優先順位:700
- 名前:Outbound_Allow_SMB445

## <a name="validate-the-azure-app-service-on-azure-stack-hub-installation"></a>Azure App Service on Azure Stack Hub のインストールを検証する

1. Azure Stack Hub 管理者ポータルで、 **[管理 - App Service]** に移動します。

1. 概要の状態で、 **[状態]** に **[ロールはいずれも準備ができています]** と表示されていることを確認します。

    ![Azure App Service 管理の概要](media/azure-stack-app-service-deploy/image12.png)

## <a name="test-drive-azure-app-service-on-azure-stack-hub"></a>Azure App Service on Azure Stack Hub のテスト

Azure App Service リソース プロバイダーをデプロイして登録したら、テストを行ってユーザーが Web アプリと API アプリをデプロイできることを確認します。

> [!NOTE]
> プラン内に Microsoft.Web 名前空間があるオファーを作る必要があります。 その後、このオファーをサブスクライブするテナント サブスクリプションが必要となります。 詳細については、[オファーの作成](azure-stack-create-offer.md)と[プランの作成](azure-stack-create-plan.md)に関するページをご覧ください。
>
> Azure App Service on Azure Stack Hub を使用するアプリを作成するには、テナント サブスクリプションが*必要です*。 管理者ポータル内でサービス管理者が実行できる機能は、Azure App Service のリソース プロバイダー管理に関連するものだけです。 これらの機能には、容量の追加、展開ソースの構成、Worker 層と SKU の追加などがあります。
>
> 3 番目のテクニカル プレビューの時点では、Web アプリ、API アプリ、Azure Functions アプリを作成するには、ユーザー ポータルを使用する必要があり、テナント サブスクリプションを用意する必要があります。

1. Azure Stack Hub ユーザー ポータルで、 **[+ リソースの作成]**  >  **[Web + モバイル]**  >  **[Web アプリ]** の順に選択します。

1. **[Web アプリ]** ブレードで、 **[Web アプリ]** ボックスに名前を入力します。

1. **[リソース グループ]** で、 **[新規]** を選択します。 **[リソース グループ]** ボックスに名前を入力します。

1. **[App Service プラン/場所]**  >  **[新規作成]** の順に選択します。

1. **[App Service プラン]** ブレードで、 **[App Service プラン]** ボックスに名前を入力します。

1. **[価格レベル]**  >  **[Free-Shared]\(無料 - 共有\)** または **[Shared-Shared]\(共有 - 共有\)**  >  **[選択]**  >  **[OK]**  >  **[作成]** の順に選択します。

1. 1 分未満に、ダッシュボードに新規 Web アプリのタイルが表示されます。 タイルを選択します。

1. **[Web アプリ]** ブレードで、 **[参照]** を選択して、このアプリの既定の Web サイトを表示します。

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>WordPress、DNN、Django の Web サイトを展開する (省略可能)

1. Azure Stack Hub ユーザー ポータルで、 **[+]** を選択して Azure Marketplace にアクセスし、Django Web サイトをデプロイして、正常に完了するまで待ちます。 Django Web プラットフォームでは、ファイル システム ベースのデータベースが使われます。 SQL や MySQL など、追加のリソース プロバイダーは必要ありません。

1. MySQL リソースプロバイダーも展開した場合は、Azure Marketplace から WordPress Web サイトを展開できます。 データベース パラメーターを求められたら、お好きなユーザー名とサーバー名を使用して、ユーザー名を *User1\@Server1* として入力します。

1. SQL Server リソースプロバイダーも展開した場合は、Azure Marketplace から DNN Web サイトを展開できます。 データベース パラメーターを求められたら、お使いのリソース プロバイダーに接続されている SQL Server を実行中のコンピューター内のデータベースを選びます。

## <a name="next-steps"></a>次のステップ

Azure App Service on Azure Stack Hub のための追加の管理者操作を準備します。

- [容量計画](azure-stack-app-service-capacity-planning.md)
- [デプロイ ソースを構成する](azure-stack-app-service-configure-deployment-sources.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-advanced-create-package.png
[2]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-advanced-complete-offline.png
[3]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-arm-endpoints.png
[4]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-subscription-information.png
[5]: ./media/azure-stack-app-service-deploy-offline/app-service-default-VNET-config.png
[6]: ./media/azure-stack-app-service-deploy-offline/app-service-custom-VNET-config.png
[7]: ./media/azure-stack-app-service-deploy-offline/app-service-custom-VNET-config-with-values.png
[8]: ./media/azure-stack-app-service-deploy-offline/app-service-fileshare-configuration.png
[9]: ./media/azure-stack-app-service-deploy-offline/app-service-fileshare-configuration-error.png
[10]: ./media/azure-stack-app-service-deploy-offline/app-service-identity-app.png
[11]: ./media/azure-stack-app-service-deploy-offline/app-service-certificates.png
[12]: ./media/azure-stack-app-service-deploy-offline/app-service-sql-configuration.png
[13]: ./media/azure-stack-app-service-deploy-offline/app-service-sql-configuration-error.png
[14]: ./media/azure-stack-app-service-deploy-offline/app-service-cloud-quantities.png
[15]: ./media/azure-stack-app-service-deploy-offline/app-service-windows-image-selection.png
[16]: ./media/azure-stack-app-service-deploy-offline/app-service-role-credentials.png
[17]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-deployment-summary.png
[18]: ./media/azure-stack-app-service-deploy-offline/app-service-deployment-progress.png
