---
title: Azure Stack Hub の概要 | Microsoft Docs
description: Azure Stack Hub の概要と、これを使ってデータセンターで Azure サービスをどのように実行できるかについて説明します。
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 09/16/2019
ms.author: justinha
ms.reviewer: unknown
ms.custom: ''
ms.lastreviewed: 05/14/2019
ms.openlocfilehash: 9461b7c306eac0c2a1467378a49d4e91e234a629
ms.sourcegitcommit: 5ef433aa6b75cdfb557fab0ef9308ff2118e66e5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2019
ms.locfileid: "73595417"
---
# <a name="azure-stack-hub-overview"></a>Azure Stack Hub の概要

Azure Stack Hub は、オンプレミス環境でアプリを実行し、データセンターで Azure サービスを実現する方法を提供する Azure の拡張機能です。 一貫性のあるクラウド プラットフォームにより、組織は、テクノロジの制約に基づいてビジネスを決定するのではなく、ビジネスの要件に基づいて自信を持ってテクノロジを決定できます。

## <a name="why-use-azure-stack-hub-"></a>Azure Stack Hub を使用する理由

Azure によって、開発者が最新アプリを構築するための機能豊富なプラットフォームが提供されます。 ただし、一部のクラウドベースのアプリは、待機時間、間欠的な接続、規制などの障害に直面します。 Azure と Azure Stack Hub によって、顧客向けアプリと社内の基幹業務アプリのどちらにも対応する、新しいハイブリッド クラウドのユース ケースが実現します。

- **エッジおよび非接続ソリューション**。 Azure Stack Hub でローカルで処理したデータを Azure で集計したあと、さらに詳しく分析し、Azure Stack Hub と Azure の両方で共通するアプリのロジックを使用して、待機時間や接続性の要件に対応できます。 インターネットから切断された状態で、Azure に接続せずに Azure Stack Hub を展開することもできます。 例としては 工場、クルーズ船、立坑などがあります。

- **さまざまな規制に対応するクラウド アプリ**。 Azure でアプリの開発とデプロイを行えば、Azure Stack Hub 上のオンプレミスにデプロイする完全な柔軟性が得られ、規制やポリシーの要件に対応できます。 コードの変更は不要です。 アプリの例としては、グローバルな監査、財務報告書の作成、外国為替取引、オンライン ゲーム、経費報告書の作成が挙げられます。

- **オンプレミスのクラウド アプリ モデル**。 Azure のサービス、コンテナー、サーバーレス、マイクロサービスの各アーキテクチャを使用して、既存のアプリの更新と拡張を行うか、新しいアプリを構築することができます。 クラウドの Azure と Azure Stack Hub オンプレミスとの間で一貫性のある DevOps プロセスを使用し、コア ミッションクリティカル アプリのアプリ最新化を迅速に行います。

Azure Stack Hub によって以下の使用シナリオが実現します。

- 統合された提供エクスペリエンス。信頼できるハードウェア パートナーから提供される特定用途向けの Azure Stack Hub 統合システムを使用すれば、ただちにサービスを提供できます。 Azure Stack Hub は納入された後、System Center Operations Manager 管理パックまたは Nagios 拡張機能によって監視しながら、容易にデータセンターに統合できます。

- 柔軟性の高い ID 管理は、Azure および Azure Stack Hub ハイブリッド環境向けの Azure Active Directory (Azure AD) を使用し、切断された展開では Active Directory Federation Services (AD FS) を利用します。

- Azure の一貫性があるアプリ開発環境は、開発者の生産性を最大限に高め、一般的な DevOps アプローチをハイブリッド環境全体で実現します。

- ハイブリッド クラウド コンピューティング能力を使用したオンプレミスからの Azure サービス提供。 Azure と Azure Stack Hub で共通の操作プラクティスを採用し、Azure と同じ管理エクスペリエンスとツールを使用して、Azure IaaS (サービスとしてのインフラストラクチャ) および PaaS (サービスとしてのプラットフォーム) サービスを展開して操作します。 Microsoft は、新しい Azure サービス、既存のサービスの更新プログラム、追加の Azure Marketplace アプリやイメージなど、Azure Stack Hub に対し Azure のイノベーションを継続して実現しています。

## <a name="azure-stack-hub-architecture"></a>Azure Stack Hub のアーキテクチャ

Azure Stack Hub 統合システムは、信頼できるパートナーによって構築された 4 - 16 のサーバー ラックで構成され、データセンターに直接納入されます。 納入後は、ソリューション プロバイダーによって統合システムが展開され、Azure Stack Hub ソリューションがビジネス要件に合っていることが確認されます。 必要なすべての電源や冷却装置、境界接続、およびその他の必要なデータセンター統合要件が整っていることを確認してデータセンターを準備します。

> Azure Stack Hub とデータ センターの統合エクスペリエンスの詳細については、「[Azure Stack Hub とデータセンターの統合](azure-stack-customer-journey.md)」を参照してください。

Azure Stack Hub は、業界標準のハードウェアに構築され、Azure サブスクリプションの管理に既に使用しているのと同じツールを使用して管理されます。 その結果、Azure に接続しているかどうかに関係なく、一貫した DevOps プロセスを適用できます。

Azure Stack Hub アーキテクチャを使うと、インターネットから切断された遠隔地や接続が間欠的な場合など、エッジで Azure サービスを提供できます。 データを Azure Stack Hub でローカルに処理してから、追加の処理や分析のために Azure に集約するハイブリッド ソリューションを作成できます。 さらに、Azure Stack Hub はオンプレミスにインストールされるため、コードを変更せずにクラウド アプリをオンプレミスに柔軟に展開でき、特定の規制やポリシー要件を満たすことができます。

## <a name="deployment-options"></a>デプロイ オプション

### <a name="production-or-evaluation-environments"></a>運用環境または評価環境

Azure Stack Hub には、ニーズに合わせて 2 つの展開オプションがあります。運用のための Azure Stack Hub 統合システムと Azure Stack Hub を評価するための Azure Stack Hub Development Kit (ASDK) です。

- **Azure Stack Hub 統合システム**:Azure Stack Hub 統合システムはマイクロソフトとハードウェア パートナーのパートナーシップによって提供され、クラウドと歩調を合わせた革新と、コンピューティングの管理のしやすさを両立させたソリューションを実現します。 Azure Stack Hub は統合されたハードウェアおよび ソフトウェア システムとして提供されているため、必要な柔軟性やコントロールが得られるほか、クラウドから革新を図ることができます。 Azure Stack Hub 統合システムには 4 ノードから 16 ノードまでのサイズがあり、ハードウェア パートナーと Microsoft によって共同でサポートされます。 Azure Stack Hub 統合システムの使用により、運用ワークロードに向けて、新しいシナリオを作成し、新しいソリューションをデプロイできます。

- **Azure Stack Hub 開発キット (ASDK)** :[ASDK](../asdk/asdk-what-is.md) は、Azure Stack Hub の評価と学習に使用できる Azure Stack Hub の無料単一ノード デプロイです。 ASDK を開発環境として使用し、Azure と一貫性のある API やツールを使用してアプリをビルドできます。 ただし、ASDK は運用環境で使用するためのものではありません。完全な統合システム運用展開と比べて次の制限があります。

    - ASDK は、単一の Azure AD または AD FS ID プロバイダーにのみ関連付けることができます。
    - Azure Stack Hub のコンポーネントは単一のホスト コンピューターにデプロイされるため、テナントのリソースに対して使用できる物理リソースは限られています。 この構成は、スケールやパフォーマンスの評価を目的としていません。
    - 単一のホストと NIC のデプロイ要件により、ネットワークのシナリオは限られています。

### <a name="connection-models"></a>接続モデル

Azure Stack Hub をインターネット (および Azure) に**接続した状態**で展開するか、**接続していない状態**で展開するかを選択できます。 この選択により、ID ストア (Azure AD または AD FS) と課金モデル (従量制課金すなわち容量に基づいた課金) の使用可能なオプションが定義されます。

> 詳しくは、[接続されている](azure-stack-connected-deployment.md)展開モデルと[接続されていない](azure-stack-disconnected-deployment.md)展開モデルの考慮事項をご覧ください。

### <a name="identity-provider"></a>ID プロバイダー 

Azure Stack Hub では、Azure AD または Active AD FS を使って ID が提供されます。 Azure AD は、マイクロソフトが提供するクラウドベースのマルチテナント対応 ID プロバイダーです。 展開がインターネットに接続されているほとんどのハイブリッド シナリオでは、ID ストアとして Azure AD を使用します。

Azure Stack Hub の切断されたデプロイの場合は、AD FS を使用する必要があります。 Azure Stack Hub リソース プロバイダーおよびその他のアプリは、AD FS または Azure AD で同様に動作します。 Azure Stack Hub には、独自の Active Directory インスタンスと、Active Directory Graph API が含まれています。

> [!IMPORTANT]
> デプロイ後に ID プロバイダーを変更することはできません。 別の ID プロバイダーを使用するには、Azure Stack Hub を再デプロイする必要があります。 Azure Stack Hub の ID の考慮事項について詳しくは、「[Azure Stack Hub の ID の概要](azure-stack-identity-overview.md)」をご覧ください。

## <a name="how-is-azure-stack-hub-managed"></a>Azure Stack Hub の管理方法

Azure Stack Hub は、管理者ポータル、ユーザー ポータル、または [PowerShell](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.7.1) で管理することができます。 Azure Stack Hub のポータルは、それぞれが Azure Resource Manager の個別のインスタンスでサポートされています。 **Azure Stack Hub オペレーター**は、管理者ポータルを使用して Azure Stack Hub を管理し、テナント オファリングの作成などの操作を行い、統合システムの正常性の維持や状態の監視を行います。 ユーザー ポータルでは、仮想マシン (VM) やストレージ アカウント、Web アプリなどのクラウド リソースを消費するためのセルフサービス エクスペリエンスが提供されます。

> 管理者ポータルを使用した Azure Stack Hub の管理について詳しくは、[Azure Stack Hub 管理ポータルのクイック スタート](azure-stack-manage-portals.md)のページをご覧ください。

Azure Stack Hub オペレーターは、[VM](azure-stack-tutorial-tenant-vm.md)、[Web アプリ](azure-stack-app-service-overview.md)、高可用性 [SQL Server](azure-stack-tutorial-sql.md)、[MySQL Server](azure-stack-tutorial-mysql.md) データベースなど、多岐にわたるサービスとアプリを提供できます。 また、[Azure Stack Hub クイックスタートの Azure Resource Manager テンプレート](https://github.com/Azure/AzureStack-QuickStart-Templates)を使用して、SharePoint や Exchange などを展開できます。

管理者ポータルを使うと、プラン、クォータ、オファー、およびサブスクリプションを使用してテナントに[サービスを提供するように Azure Stack Hub を構成](service-plan-offer-subscription-overview.md)できます。 テナント ユーザーは複数のオファーにサブスクライブできます。 オファーは 1 つまたは複数のプランを含むことができ、プランは 1 つまたは複数のサービスを含むことができます。 さらに、オペレーターは、容量を管理し、アラートに対処します。

Azure Stack Hub が構成されると、**Azure Stack Hub ユーザー**は、オペレーターが提供するサービスを使用します。 ユーザーは、サブスクライブしたサービス (Web アプリ、Storage、VM など) のプロビジョニング、監視、管理を行うことができます。

> どこでどのアカウントを使用するか、通常のオペレーターの役割、ユーザーに何を伝えるか、ヘルプを利用する方法など、Azure Stack Hub の管理について詳しくは、「[Azure Stack Hub の管理の基本](azure-stack-manage-basics.md)」をご覧ください。

## <a name="resource-providers"></a>リソース プロバイダー

リソース プロバイダーは、Azure Stack Hub のあらゆる IaaS サービスと PaaS サービスの基盤となる Web サービスです。 Azure Resource Manager は、さまざまなリソース プロバイダーに依存して、サービスへのアクセスを提供しています。 各リソース プロバイダーを使用して、それぞれのリソースを構成および制御できます。 サービス管理者は、新しいカスタム リソース プロバイダーを追加することもできます。

### <a name="foundational-resource-providers"></a>基本のリソース プロバイダー

基本の IaaS リソース プロバイダーは 3 つあります。

- **コンピューティング**:コンピューティング リソース プロバイダーでは、Azure Stack Hub テナントが独自の VM を作成できます。 コンピューティング リソース プロバイダーには、VM と VM 拡張機能を作成する機能があります。 VM 拡張機能サービスは、Windows および Linux の VM で使用する IaaS 機能を提供します。 たとえば、コンピューティング リソース プロバイダーを使用して Linux VM をプロビジョニングし、デプロイ中に Bash スクリプトを実行して VM を構成できます。
- **ネットワーク リソース プロバイダー**:ネットワーク リソース プロバイダーは、プライベート クラウドでソフトウェアによるネットワーク制御 (SDN) およびネットワーク機能の仮想化 (NFV) に使用する一連の機能を提供します。 ネットワーク リソース プロバイダーを使用して、ソフトウェア ロード バランサー、パブリック IP、ネットワーク セキュリティ グループ、仮想ネットワークなどのリソースを作成できます。
- **ストレージ リソース プロバイダー**:ストレージ リソース プロバイダーによって、一貫性のある 4 つの Azure ストレージ サービス ([blob](https://docs.microsoft.com/azure/storage/common/storage-introduction#blob-storage)、[queue](https://docs.microsoft.com/azure/storage/common/storage-introduction#queue-storage)、[table](https://docs.microsoft.com/azure/storage/common/storage-introduction#table-storage)、[Key Vault](https://docs.microsoft.com/azure/key-vault/)) のアカウント管理が提供され、シークレット (パスワードや証明書) の管理と監査が提供されます。 ストレージ リソース プロバイダーは、Azure との一貫性があるストレージ サービスのサービス プロバイダーによる管理を容易にするためのストレージ クラウド管理サービスも提供します。 Azure Storage には柔軟性があり、ドキュメントやメディア ファイルなどの大量の構造化されていないデータは Azure BLOB により、構造化された NoSQL ベースのデータは Azure テーブルにより格納と取得が行われます。

### <a name="optional-resource-providers"></a>オプションのリソース プロバイダー

Azure Stack Hub に展開して使用できるオプションの PaaS リソース プロバイダーは 3 つあります。

- **App Service**: [Azure App Service on Azure Stack Hub](azure-stack-app-service-overview.md) は、Azure Stack Hub で利用できる Microsoft Azure の、PaaS サービスです。 このサービスにより、内部または外部の顧客はあらゆるプラットフォームやデバイス用の Web、API、Azure Functions アプリを作成できるようになります。
- **SQL Server**:[SQL Server リソース プロバイダー](azure-stack-sql-resource-provider.md)を使用して、SQL データベースを Azure Stack Hub のサービスとして提供します。 リソース プロバイダーをインストールし、それを 1 つ以上の SQL Server インスタンスに接続した後で、クラウドネイティブ アプリ、SQL を使用する Web サイト、SQL を使用する他のワークロードのためにデータベースを作成できます。
- **MySQL サーバー**:[MySQL サーバー リソース プロバイダー](azure-stack-mysql-resource-provider-deploy.md)を使用して、MySQL データベースを Azure Stack Hub サービスとして公開します。 MySQL リソース プロバイダーは、Windows Server 2016 Server Core VM 上でサービスとして実行されます。

## <a name="providing-high-availability"></a>高可用性の提供

Azure でのマルチ VM による実稼働システムの高可用性を実現するため、VM は、複数の障害ドメインと更新ドメインに分散される[可用性セット](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy)に配置されます。 より小さいスケールの Azure Stack Hub では、可用性セット内の障害ドメインは、スケール ユニット内の 1 つのノードとして定義されます。  

Azure Stack Hub のインフラストラクチャは既に障害に対する回復性を備えていますが、基盤となっているテクノロジ (フェールオーバー クラスタリング) では、ハードウェアが故障した場合にその影響を受ける物理サーバー上の VM に多少のダウンタイムが発生します。 Azure Stack Hub では、Azure との一貫性がある最大 3 つの障害ドメインを持つ可用性セットを用意することをサポートしています。

- **[障害ドメイン]** : 可用性セットに配置された VM は、複数の障害ドメイン (Azure Stack Hub ノード) にできる限り均等に分散させることによって、互いに物理的に分離されます。 ハードウェア障害が発生した場合、障害が発生した障害ドメインの VM は、他の障害ドメインで再起動されます。 これらは、他の VM とは別の障害ドメインに、ただし可能な場合、同じ可用性セットに保持されます。 ハードウェアがオンラインに戻ると、高可用性を維持するために VM の再配置が行われます。

- **[更新ドメイン]** : 更新ドメインは、可用性セットに高可用性を提供する Azure の別の概念です。 更新ドメインは、メンテナンスを同時に実行できる、基盤となるハードウェアの論理グループです。 同じ更新ドメイン内の VM は、計画済みメンテナンス中に同時に再起動されます。 テナントが可用性セット内に VM を作成すると、Azure プラットフォームは、これらの更新ドメインに VM を自動的に分散します。 Azure Stack Hub では、VM のホストが更新される前に、クラスター内の他のオンライン ホストに VM がライブ マイグレーションされます。 ホスト更新の際にテナントのダウンタイムは発生しないため、Azure Stack Hub の更新ドメイン機能は、Azure とテンプレートの互換性を保つためにのみ存在します。 可用性セット内の VM は、ポータル上でその更新ドメイン番号として **0** を表示します。

## <a name="role-based-access-control"></a>ロールベースのアクセス制御

ロール ベースのアクセス制御 (RBAC) を使用して、承認されたユーザー、グループ、およびサービスに対してサブスクリプション、リソース グループ、または個々のリソース レベルでロールを割り当てることによって、システムへのアクセスを許可することができます。 各ロールは、Microsoft Azure Stack Hub のリソースに対するユーザー、グループ、またはサービスのアクセス レベルを定義します。

Azure Stack Hub RBAC には、すべてのリソースの種類に適用される 3 つの基本的なロールがあります。所有者、共同作成者、および閲覧者です。 所有者は、他のユーザーへアクセス権を委任する権限を含め、すべてのリソースへのフル アクセス権を持ちます。 共同作成者は、すべての種類の Azure リソースを作成および管理できますが、他のユーザーにアクセス権を付与することはできません。 閲覧者は、既存のリソースの表示しかできません。 残りの RBAC ロールでは、特定の Azure リソースの管理が許可されます。 たとえば、仮想マシンの共同作成者ロールでは、VM の作成と管理は許可されますが、VM に接続される仮想ネットワークまたはサブネットの管理は許可されません。

> 詳しくは、「[ロールベースのアクセス制御の管理](azure-stack-manage-permissions.md)」をご覧ください。

## <a name="reporting-usage-data"></a>使用量データの報告

Azure Stack Hub では、すべてのリソース プロバイダーの使用状況データを収集して集計し、Azure コマースによる処理のために Azure に送信します。 Azure Stack Hub で収集された使用状況データは、REST API を使用して表示できます。 Azure と一貫性のある Tenant API だけでなく、Provider および Delegated Provider API を使用して、すべてのテナント サブスクリプション全体の使用状況データを取得できます。 このデータは、請求や配賦のために外部のツールやサービスと統合できます。 Azure コマースによって処理された使用状況は、Azure の課金ポータルに表示できます。

> 詳しくは、「[Azure Stack Hub 使用状況データの Azure への報告](azure-stack-usage-reporting.md)」をご覧ください。

## <a name="next-steps"></a>次の手順

[Azure Stack Hub とグローバル Azure を比較](compare-azure-azure-stack.md)する。

[管理の基本](azure-stack-manage-basics.md)。

[クイック スタート: Azure Stack Hub 管理ポータルを使用する](azure-stack-manage-portals.md)。
