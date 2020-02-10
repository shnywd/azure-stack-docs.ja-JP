---
title: Azure Stack Hub のリリース ノート
description: 更新プログラムやバグ修正プログラムを含む、Azure Stack Hub 統合システムのリリース ノート。
author: sethmanheim
ms.topic: article
ms.date: 02/03/2020
ms.author: sethm
ms.reviewer: prchint
ms.lastreviewed: 11/22/2019
ms.openlocfilehash: 40c727c97035b0f843ce84b0eafad47c77560938
ms.sourcegitcommit: 3974fc544986a5eccfac0ba7c4441b05279ae226
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "77001643"
---
# <a name="azure-stack-hub-release-notes"></a>Azure Stack Hub のリリース ノート

この記事では、Azure Stack Hub 更新プログラム パッケージの内容について説明します。 更新プログラムには、Azure Stack Hub の最新のリリースに対する機能強化と修正が含まれています。

別のバージョンのリリース ノートにアクセスするには、左側の目次の上部にあるバージョン セレクターのドロップダウンを使用します。

::: moniker range=">=azs-1906"
> [!IMPORTANT]  
> この更新プログラム パッケージは、Azure Stack Hub 統合システム専用です。 Azure Stack 開発キット (ASDK) にこの更新プログラム パッケージを適用しないでください。
::: moniker-end
::: moniker range="<azs-1906"
> [!IMPORTANT]  
> お使いの Azure Stack Hub インスタンスが 2 つ前の更新プログラムより古い場合、コンプライアンスに対応していないとみなされます。 [サポートを受けるためには、少なくともサポートされる最小バージョンまで更新する](azure-stack-servicing-policy.md#keep-your-system-under-support)必要があります。
::: moniker-end

## <a name="update-planning"></a>計画の更新

更新プログラムを適用する前に、必ず次の情報を確認してください。

- [既知の問題](known-issues.md)
- [セキュリティ更新プログラム](release-notes-security-updates.md)
- [更新プログラム適用前後のアクティビティのチェックリスト](release-notes-checklist.md)

更新プログラムのトラブルシューティングと更新プロセスの詳細については、[Azure Stack Hub の修正プログラムと更新プログラムについての問題のトラブルシューティング](azure-stack-updates-troubleshoot.md)に関するページを参照してください。

<!---------------------------------------------------------->
<!------------------- SUPPORTED VERSIONS ------------------->
<!---------------------------------------------------------->
::: moniker range="azs-1910"
## <a name="1910-build-reference"></a>1910 ビルドのリファレンス

Azure Stack Hub 1910 更新プログラムのビルド番号は **1.1910.0.58** です。

### <a name="update-type"></a>更新の種類

1908 以降、Azure Stack Hub が実行される基盤のオペレーティング システムは Windows Server 2019 に更新されました。 この更新プログラムによって、核となる基本的な機能強化と、Azure Stack Hub に機能を追加する機能が使用可能になります。

Azure Stack Hub 1910 更新プログラムのビルドの種類は**高速**です。

1910 更新プログラム パッケージは、以前の更新プログラムと比較してサイズが大きくなっており、ダウンロードにより長い時間がかかります。 更新プログラムは長い時間、**準備中**状態のままになり、オペレーターは以前の更新プログラムよりもこのプロセスに長い時間がかかることを予想できます。 1910 更新プログラムが完了するまでの予測所要時間は、ご使用の Azure Stack Hub 環境内の物理ノード数に関係なく、約 10 時間です。 更新プログラムの正確なランタイムは一般的に、ご使用のシステムでテナント ワークロードによって使用されている容量、システム ネットワーク接続 (インターネットに接続されている場合)、およびシステム ハードウェアの仕様に左右されます。 ランタイムが予測値よりも長くなることは珍しいわけではなく、更新が失敗した場合を除いて、Azure Stack Hub オペレーターによるアクションは必要ありません。 このおおよその実行時間は、1910 更新プログラムに固有であり、他の Azure Stack Hub 更新プログラムと比較することはできません。

更新プログラムのビルドの種類については、「[Azure Stack Hub での更新プログラム管理の概要](azure-stack-updates.md)」を参照してください。

<!-- ## What's in this update -->

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>新機能

<!-- What's new, also net new experiences and features. -->

- 管理者ポータルでは、リージョンのプロパティ メニューに特権エンドポイントの IP アドレスが表示され、簡単に見つけられるようになりました。 さらに、現在構成されているタイム サーバーと DNS フォワーダーも表示されます。 詳細については、「[Azure Stack Hub で特権エンドポイントを使用する](azure-stack-privileged-endpoint.md)」を参照してください。

- Azure Stack Hub の正常性および監視システムでは、エラーが発生した場合、さまざまなハードウェア コンポーネントに対してアラートを生成できるようになりました。 これらのアラートには、追加の構成が必要になります。 詳細については、「[Azure Stack Hub のハードウェア コンポーネントを監視する](azure-stack-hardware-monitoring.md)」を参照してください。

- [Azure Stack Hub の cloud-init のサポート](/azure/virtual-machines/linux/using-cloud-init):cloud-Init は、Linux VM を初回起動時にカスタマイズするために広く使用されているアプローチです。 cloud-init を使って、パッケージをインストールしてファイルを書き込んだり、ユーザーとセキュリティを構成したりすることができます。 cloud-init は初回起動プロセスの間に呼び出されるので、構成を適用するために追加の手順や必要なエージェントはありません。 マーケットプレースの Ubuntu イメージは、プロビジョニング用の cloud-init をサポートするように更新されました。

- Azure Stack Hub では、すべての Windows Azure Linux エージェント バージョンバージョンが Azure としてサポートされるようになりました。

- Azure Stack Hub 管理 PowerShell モジュールの新しいバージョンがリリースされました。 <!-- For more information, see -->

- Azure Stack Hub インフラストラクチャの Windows Defender 定義の手動更新を構成するために、特権エンドポイント (PEP) に **Set-AzSDefenderManualUpdate** コマンドレットを追加しました。 詳細については、「[Azure Stack Hub 上で Windows Defender ウイルス対策を更新する](azure-stack-security-av.md)」を参照してください。

- Azure Stack Hub インフラストラクチャの Windows Defender 定義の手動更新の構成を取得するために、特権エンドポイント (PEP) に **Get-AzSDefenderManualUpdate** コマンドレットを追加しました。 詳細については、「[Azure Stack Hub 上で Windows Defender ウイルス対策を更新する](azure-stack-security-av.md)」を参照してください。

- 特権エンドポイント (PEP) に Azure Stack Hub の DNS サーバーのフォワーダー設定を変更する **Set-AzSDnsForwarder** コマンドレットを追加しました。 DNS 構成の詳細については、「[Azure Stack Hub データセンターの DNS の統合](azure-stack-integrate-dns.md)」を参照してください。

- 特権エンドポイント (PEP) に Azure Stack Hub の DNS サーバーのフォワーダー設定を取得する **Get-AzSDnsForwarder** コマンドレットを追加しました。 DNS 構成の詳細については、「[Azure Stack Hub データセンターの DNS の統合](azure-stack-integrate-dns.md)」を参照してください。

- [AKS エンジン](../user/azure-stack-kubernetes-aks-engine-overview.md)を使用する **Kubernetes クラスター**の管理のサポートを追加しました。 この更新プログラムから、運用環境の Kubernetes クラスターをデプロイできるようになりました。 AKS エンジンによって、以下のことが可能になっています。
  - Kubernetes クラスターのライフ サイクルを管理します。 クラスターの作成、更新、およびスケールを行うことができます。
  - AKS チームおよび Azure Stack Hub チームによって作成されたマネージド イメージを使用して、クラスターを維持できます。
  - Azure Resource Manager に統合された Kubernetes クラウド プロバイダーを利用し、ネイティブの Azure リソースを使用してクラスターを構築できます。
  - 接続または切断された Azure Stack Hub スタンプでクラスターをデプロイおよび管理できます。
  - Azure ハイブリッド機能を使用します。
    - Azure Arc との統合。
    - Azure Monitor for Containers との統合。
  - Windows コンテナーを AKS エンジンと共に使用します。
  - デプロイに対して CSS およびエンジニアリグ サポートを受けられます。

### <a name="improvements"></a>機能強化

<!-- Changes and product improvements with tangible customer-facing value. -->

- Azure Stack Hub では、以前に更新の失敗を引き起こしたり、オペレーターによる Azure Stack Hub の更新開始を妨げたりしていた修正プログラムや更新プログラムの問題を自動修復する機能が向上しています。 その結果、**Test-AzureStack -UpdateReadiness** グループに含まれるテストの数が減っています。 詳細については、「[Azure Stack Hub システムの状態を検証する](azure-stack-diagnostic-test.md#groups)」を参照してください。 次の 3 つのテストは **UpdateReadiness** グループに残っています。

  - **AzSInfraFileValidation**
  - **AzSActionPlanStatus**
  - **AzsStampBMCSummary**

- 外部デバイス (USB キーなど) がいつ Azure Stack Hub インフラストラクチャのノードにマウントされたかを報告する監査ルールが追加されました。 監査ログは syslog を介して出力され、"**Microsoft-Windows-Security-Auditing:6416|Plug and Play Events** (Microsoft-Windows-Security-Auditing: 6416|プラグ アンド プレイ イベント)" と表示されます。 syslog クライアントの構成方法の詳細については、[Syslog の転送](azure-stack-integrate-security.md)に関する記事を参照してください。

- 内部証明書については、Azure Stack Hub では 4096 ビット RSA キーに移行中です。 内部シークレットのローテーションを実行すると、以前の 2048 ビット証明書は 4096 ビット長の証明書に置き換えられます。 Azure Stack Hub のシークレット ローテーションの詳細については、「[Azure Stack Hub でシークレットをローテーションする](azure-stack-rotate-secrets.md)」を参照してください。

- Committee on National Security Systems - Policy 15 (CNSSP-15) (安全な情報共有のための公開標準の使用に関するベスト プラクティスが記載されています) に準拠するために、いくつかの内部コンポーネントについて暗号化アルゴリズムの複雑さとキー強度をアップグレードします。 強化された点として、Kerberos 認証用の AES256 と、VPN 暗号化用の SHA384 があります。 CNSSP-15 の詳細については、[Committee on National Security Systems の「Policies (ポリシー)」ページ](http://www.cnss.gov/CNSS/issuances/Policies.cfm)を参照してください。

- 上記のアップグレードに伴い、Azure Stack Hub には IPsec/IKEv2 構成の新しい既定値が追加されました。 Azure Stack Hub 側で使用される新しい既定値は次のとおりです。

   **IKE フェーズ 1 (メイン モード) のパラメーター**

   | プロパティ              | Value|
   |-|-|
   | IKE のバージョン           | IKEv2 |
   |Diffie-hellman グループ   | ECP384 |
   | 認証方法 | 事前共有キー |
   |暗号化とハッシュ アルゴリズム | AES256、SHA384 |
   |SA の有効期間 (時間)     | 28,800 秒|

   **IKE フェーズ 2 (クイック モード) のパラメーター**

   | プロパティ| Value|
   |-|-|
   |IKE のバージョン |IKEv2 |
   |暗号化とハッシュ アルゴリズム (暗号化)     | GCMAES256|
   |暗号化とハッシュ アルゴリズム (認証) | GCMAES256|
   |SA の有効期間 (時間)  | 27,000 秒  |
   |SA の有効期間 (キロバイト単位) | 33,553,408     |
   |Perfect Forward Secrecy (PFS) | ECP384 |
   |Dead Peer Detection | サポートされています|

   これらの変更は、[既定の IPsec/IKE 提案](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)のドキュメントにも反映されます。

- インフラストラクチャ バックアップ サービスのロジックは、固定のしきい値を利用するのではなく、バックアップに必要な空き領域を計算するように改善されました。 このサービスでは、バックアップのサイズ、アイテム保持ポリシー、予約、および外部ストレージの場所の現在の使用率が使用され、オペレーターに警告を発する必要があるかどうかが決定されます。

### <a name="changes"></a>[変更点]

- Azure から Azure Stack Hub にマーケットプレース項目をダウンロードするときに、複数のバージョンが存在する場合、項目のバージョンを指定できる新しいユーザー インターフェイスが追加されました。 新しい UI は、接続されたシナリオと切断されたシナリオの両方で使用できます。 詳細については、「[Azure から Azure Stack Hub に Marketplace の項目をダウンロードする](azure-stack-download-azure-marketplace-item.md)」を参照してください。  

- 1910 リリース以降、Azure Stack Hub システムには追加で /20 プライベート内部 IP 空間が**必要**になりました。 このネットワークは Azure Stack Hub システム専用であり、データセンター内の複数の Azure Stack Hub システム上で再利用できます。 このネットワークは Azure Stack Hub 専用ですが、データセンター内のネットワークと重複することはできません。 /20 プライベート IP 空間は複数のネットワークに分割され、コンテナー上で Azure Stack Hub インフラストラクチャを実行できるようになりました ([1905 リリース ノート](release-notes.md?view=azs-1905)にも記載されています)。 コンテナーで Azure Stack Hub インフラストラクチャを実行する目的は、使用率を最適化し、パフォーマンスを向上させることです。 さらに、/20 プライベート IP 空間は、デプロイ前に必要なルーティング可能な IP 空間を減らす継続的な取り組みを可能にするためにも使用されます。

  - /20 の入力は、1910 の次の Azure Stack Hub 更新プログラムの前提条件となることに注意してください。 1910 の次の Azure Stack Hub 更新プログラムがリリースされ、それをインストールしようとすると、後述する修復手順で説明するように /20 の入力を完了していない場合、更新は失敗します。 前記の修復手順が完了するまで、管理者ポータルにはアラートが表示されます。 この新しいプライベート空間がどのように使用されるかについては、[データセンターのネットワーク統合](azure-stack-network.md#private-network)に関する記事を参照してください。

  - 修復手順:修復するには、次の手順に従って [PEP セッション開きます](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint)。 サイズ /20 の[プライベート内部 IP 範囲](azure-stack-network.md#logical-networks)を準備し、次の例を使用して、PEP セッション内で次のコマンドレット (1910 以降でのみ使用可能) を実行します: `Set-AzsPrivateNetwork -UserSubnet 100.87.0.0/20`。 操作が正常に実行されると、"**Azs Internal Network range added to the config** (構成に Azs 内部ネットワーク範囲が追加されました)" というメッセージが表示されます。正常に完了すると、管理者ポータル上のアラートは閉じられます。 これで、Azure Stack Hub システムは、次のバージョンに更新できるようになります。
  
- アップロード手順中に外部ストレージの場所の容量が不足すると、インフラストラクチャ バックアップ サービスによって、部分的にアップロードされたバックアップ データが削除されます。  

- インフラストラクチャ バックアップ サービスでは、AAD デプロイのためのバックアップ ペイロードに ID サービスが追加されました。  

- 1910 リリースでは、Azure Stack Hub PowerShell モジュールがバージョン 1.8.0 に更新されました。<br>変更内容:
   - **新しい DRP 管理モジュール**:デプロイ リソース プロバイダー (DRP) を使用すると、Azure Stack Hub に対するリソース プロバイダーの調整されたデプロイが可能になります。 これらのコマンドを使うと、DRP とやり取りする Azure Resource Manager レイヤーとやり取りできます。
   - **BRP**: <br />
           - Azure Stack インフラストラクチャ バックアップの 1 つのロールの復元をサポートします。 <br />
           - パラメーター `RoleName` をコマンドレット `Restore-AzsBackup` に追加します。
   - **FRP**:API バージョン `2019-05-01` の**ドライブ** リソースと**ボリューム** リソースの重大な変更。 これらの機能は Azure Stack Hub 1910 以降でサポートされています。 <br />
            - `ID`、`Name`、`HealthStatus`、および `OperationalStatus` の値が変更されました。 <br />
            - **ドライブ** リソースの新しいプロパティ `FirmwareVersion`、`IsIndicationEnabled`、`Manufacturer`、`StoragePool` がサポートされるようになりました。 <br />
            - **ドライブ** リソースのプロパティ `CanPool` および `CannotPoolReason` は廃止されました。代わりに `OperationalStatus` を使用してください。

### <a name="fixes"></a>修正

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there's an SR/ICM associated to it. -->

- Azure Stack Hub 1904 リリースより前にデプロイされた環境では TLS 1.2 ポリシーの適用が妨げられる問題を修正しました。
- SSH の承認を有効にして作成された Ubuntu 18.04 VM では、SSH キーを使用してサインインできない問題を修正しました。
- 仮想マシン スケール セットの UI から **[パスワードのリセット]** を削除しました。
- ポータルからロード バランサーを削除しても、インフラストラクチャ レイヤーにあるオブジェクトが削除されない問題を修正しました。
- 管理者ポータル上でゲートウェイ プール使用率アラートの割合が正しく表示されない問題を修正しました。
<!-- Fixed an issue where adding more than one public IP on the same NIC on a Virtual Machine resulted in internet connectivity issues. Now, a NIC with two public IPs should work as expected.[This fix actually didn't go in 1910 due to build issues, commenting out until next build (2002) ] -->

## <a name="security-updates"></a>セキュリティ更新プログラム

Azure Stack Hub のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack Hub のセキュリティ更新プログラム](release-notes-security-updates.md)」を参照してください。

## <a name="update-planning"></a>計画の更新

更新プログラムを適用する前に、必ず次の情報を確認してください。

- [既知の問題](known-issues.md)
- [セキュリティ更新プログラム](release-notes-security-updates.md)
- [更新プログラム適用前後のアクティビティのチェックリスト](release-notes-checklist.md)

## <a name="download-the-update"></a>更新プログラムをダウンロードする

Azure Stack Hub 1910 更新プログラム パッケージは、[Azure Stack Hub ダウンロード ページ](https://aka.ms/azurestackupdatedownload)からダウンロードできます。

## <a name="hotfixes"></a>修正プログラム

Azure Stack Hub では、修正プログラムが定期的にリリースされます。 Azure Stack Hub を 1910 に更新する前に、必ず 1908 用の最新の Azure Stack Hub 修正プログラムをインストールしてください。

Azure Stack Hub 修正プログラムを適用できるのは Azure Stack Hub 統合システムのみです。ASDK には修正プログラムのインストールを試行しないでください。

### <a name="prerequisites-before-applying-the-1910-update"></a>前提条件:1910 更新プログラムを適用する前

Azure Stack Hub の 1910 リリースは、以下の修正プログラムが適用された 1908 リリースに適用する必要があります。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修正プログラム 1.1908.14.53](https://support.microsoft.com/help/4537661)

### <a name="after-successfully-applying-the-1910-update"></a>1910 更新プログラムの適用に成功した後

この更新プログラムをインストールした後、適用可能な修正プログラムがあればインストールします。 詳細については、[サービス ポリシー](azure-stack-servicing-policy.md)に関する記事を参照してください。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修正プログラム 1.1910.17.95](https://support.microsoft.com/help/4537833)
::: moniker-end

::: moniker range="azs-1908"
## <a name="1908-build-reference"></a>1908 ビルドのリファレンス

Azure Stack Hub 1908 更新プログラムのビルド番号は **1.1908.4.33** です。

### <a name="update-type"></a>更新の種類

1908 では、Azure Stack Hub が実行される基になるオペレーティング システムが Windows Server 2019 に更新されています。 この更新プログラムによって、核となる基本的な機能強化と、Azure Stack Hub に機能を追加する機能が使用可能になります。

Azure Stack Hub 1908 更新プログラムのビルドの種類は**完全**です。 そのため、1908 更新プログラムは、1906 や 1907 のような高速更新プログラムよりもランタイムが長くなります。 完全な更新プログラムの正確なランタイムは、Azure Stack Hub インスタンスに含まれているノード数、テナントのワークロードごとにシステムで使用される容量、システムのネットワーク接続 (インターネットに接続されている場合)、システムのハードウェア構成によって異なります。 内部テストでは、1908 更新プログラムのランタイムが次のように予測されました。4 ノード - 42 時間、8 ノード - 50 時間、12 ノード - 60 時間、16 ノード - 70 時間。 更新プログラムのランタイムがこれらの予測値よりも長くなることは珍しいわけではなく、更新が失敗した場合を除いて、Azure Stack Hub オペレーターによるアクションは必要ありません。

更新プログラムのビルドの種類については、「[Azure Stack Hub での更新プログラム管理の概要](azure-stack-updates.md)」を参照してください。

- 更新プログラムの正確な実行時間は一般に、ご使用のシステムでテナント ワークロードによって使用されている容量、システム ネットワーク接続 (インターネットに接続されている場合)、およびシステム ハードウェアの構成に左右されます。
- 実行時間が予測よりも長くなることは珍しいわけではなく、更新が失敗した場合を除いて、Azure Stack Hub オペレーターによるアクションは必要ありません。
- このおおよその実行時間は、1908 更新プログラムに固有であり、他の Azure Stack Hub 更新プログラムと比較することはできません。

<!-- ## What's in this update -->

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>新機能

<!-- What's new, also net new experiences and features. -->

- 1908 では、Azure Stack Hub が実行される基になるオペレーティング システムが Windows Server 2019 に更新されていることに注意してください。 この更新プログラムによって、核となる基本的な機能強化と、Azure Stack Hub に機能を追加する機能が使用可能になります。
- Azure Stack Hub インフラストラクチャのすべてのコンポーネントが FIPS 140-2 モードで動作するようになりました。
- Azure Stack Hub オペレーターは、ポータル ユーザー データを削除できるようになりました。 詳細については、「[Azure Stack Hub からポータル ユーザー データをクリアする](azure-stack-portal-clear.md)」を参照してください。

### <a name="improvements"></a>機能強化

<!-- Changes and product improvements with tangible customer-facing value. -->
- 物理ノードのハードウェアのトラステッド プラットフォーム モジュール (TPM) にシークレットを保持するために、Azure Stack Hub の保存データの暗号化が向上しました。

### <a name="changes"></a>[変更点]

- ハードウェア プロバイダーは Azure Stack Hub バージョン 1908 と同時に OEM 拡張機能パッケージ 2.1 以降をリリースします。 Azure Stack Hub バージョン 1908 には OEM 拡張機能パッケージ 2.1 以降が前提条件です。 OEM 拡張機能パッケージ 2.1 以降をダウンロードする方法の詳細については、システムのハードウェア プロバイダーに問い合わせてください。また、[OEM 更新プログラム](azure-stack-update-oem.md#oem-contact-information)の記事を参照してください。  

### <a name="fixes"></a>修正

- 今後の Azure Stack Hub OEM 更新プログラムとの互換性、およびユーザーのユーザー イメージを使用した VM デプロイに関する問題が修正されました。 この問題は 1907 で見つかり、修正プログラム [KB4517473](https://support.microsoft.com/en-us/help/4517473/azure-stack-hotfix-1-1907-12-44) で修正されました  
- OEM ファームウェア更新プログラムに関する問題が修正され、Fabric リングの正常性についての Test-AzureStack での誤診断に関する問題が修正されました。 この問題は 1907 で見つかり、修正プログラム [KB4515310](https://support.microsoft.com/en-us/help/4515310/azure-stack-hotfix-1-1907-7-35) で修正されました
- OEM ファームウェア更新プロセスに関する問題を修正しました。 この問題は 1907 で見つかり、修正プログラム [KB4515650](https://support.microsoft.com/en-us/help/4515650/azure-stack-hotfix-1-1907-8-37) で修正されました

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there's an SR/ICM associated to it. -->

## <a name="security-updates"></a>セキュリティ更新プログラム

Azure Stack Hub のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack Hub のセキュリティ更新プログラム](release-notes-security-updates.md)」を参照してください。

## <a name="download-the-update-1908"></a>更新プログラムのダウンロード

Azure Stack Hub 1908 更新プログラム パッケージは、[Azure Stack Hub ダウンロード ページ](https://aka.ms/azurestackupdatedownload)からダウンロードできます。

## <a name="hotfixes"></a>修正プログラム

Azure Stack Hub では、修正プログラムが定期的にリリースされます。 Azure Stack Hub を 1908 に更新する前に、必ず 1907 用の最新の Azure Stack Hub 修正プログラムをインストールしてください。

Azure Stack Hub 修正プログラムを適用できるのは Azure Stack Hub 統合システムのみです。ASDK には修正プログラムのインストールを試行しないでください。

### <a name="prerequisites-before-applying-the-1908-update"></a>前提条件:1908 更新プログラムを適用する前

Azure Stack Hub の 1908 リリースは、以下の修正プログラムが適用された 1907 リリースに適用する必要があります。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修正プログラム 1.1907.18.56](https://support.microsoft.com/help/4528552)

Azure Stack Hub 1908 更新プログラムには、システムのハードウェア プロバイダーからの **Azure Stack Hub OEM バージョン 2.1 以降**が必要です。 OEM 更新プログラムには、Azure Stack Hub システム ハードウェアのドライバーとファームウェアの更新プログラムが含まれています。 OEM 更新プログラムの適用の詳細については、「[Azure Stack Hub に OEM (相手先ブランド供給) 更新プログラムを適用する](azure-stack-update-oem.md)」を参照してください

### <a name="after-successfully-applying-the-1908-update"></a>1908 更新プログラムの適用に成功した後

この更新プログラムをインストールした後、適用可能な修正プログラムがあればインストールします。 詳細については、[サービス ポリシー](azure-stack-servicing-policy.md)に関する記事を参照してください。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修正プログラム 1.1908.14.53](https://support.microsoft.com/help/4537661)
::: moniker-end

::: moniker range="azs-1907"
## <a name="1907-build-reference"></a>1907 ビルドのリファレンス

Azure Stack Hub 1907 更新プログラムのビルド番号は **1.1907.0.20** です。

### <a name="update-type"></a>更新の種類

Azure Stack Hub 1907 更新プログラムのビルドの種類は**高速**です。 更新プログラムのビルドの種類については、「[Azure Stack Hub での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。 内部テストに基づいて、1907 更新プログラムが完了するまでの予測所要時間は約 13 時間です。

- 更新プログラムの正確な実行時間は一般に、ご使用のシステムでテナント ワークロードによって使用されている容量、システム ネットワーク接続 (インターネットに接続されている場合)、およびシステム ハードウェアの構成に左右されます。
- 実行時間が予測よりも長くなることは珍しいわけではなく、更新が失敗した場合を除いて、Azure Stack Hub オペレーターによるアクションは必要ありません。
- このおおよその実行時間は、1907 更新プログラムに固有であり、他の Azure Stack Hub 更新プログラムと比較することはできません。

## <a name="whats-in-this-update"></a>この更新プログラムの新機能

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>新機能

<!-- What's new, also net new experiences and features. -->

- 診断ログの収集を容易にし、改善するための Azure Stack Hub 診断ログ収集サービスの一般公開リリース。 Azure Stack Hub 診断ログ収集サービスは、診断ログを収集して、Microsoft カスタマー サポート サービス (CSS) と共有するための簡単な方法を提供します。 この診断ログ収集サービスは、Azure Stack Hub 管理者ポータルでの新しいユーザー エクスペリエンスを提供します。これにより、オペレーターは、特定の重要なアラートが発生したときに、ストレージ BLOB への診断ログの自動アップロードを設定できます。 オンデマンドで同じ操作を実行するためにも、サービスを使用できます。 詳細については、[診断ログの収集](azure-stack-diagnostic-log-collection-overview.md)に関する記事を参照してください。

- Azure Stack Hub 検証ツール **Test-AzureStack** の一部としての Azure Stack Hub ネットワーク インフラストラクチャ検証の一般公開リリース。 Azure Stack Hub ネットワーク インフラストラクチャは、**Test-AzureStack** の一部になり、Azure Stack Hub のネットワーク インフラストラクチャで障害が発生したかどうかを特定します。 テストでは、Azure Stack Hub ソフトウェアで定義されたネットワークをバイパスすることによって、ネットワーク インフラストラクチャの接続が確認されます。 パブリック VIP から構成済みの DNS フォワーダー、NTP サーバー、および ID エンドポイントへの接続が示されます。 また、ID プロバイダーとして Azure AD を使用する場合には Azure への、または ADFS を使用する場合にはフェデレーション サーバーへの接続が確認されます。 詳細については、[Azure Stack Hub 検証ツール](azure-stack-diagnostic-test.md)に関する記事を参照してください。

- システムの更新中に、必要に応じて、内部の SQL TLS 証明書をローテーションする、内部シークレットのローテーション プロシージャが追加されました。

### <a name="improvements"></a>機能強化

<!-- Changes and product improvements with tangible customer-facing value. -->

- Azure Stack Hub の更新ブレードに、アクティブな更新の**最後のステップが完了した**時刻が表示されるようになりました。 更新ブレードに移動して、実行中の更新をクリックすると、この追加を表示できます。 **[Last Step Completed]\(最後のステップの完了\)** は、 **[Update run details]\(更新実行の詳細\)** セクションにあります。

- **Start-AzureStack** と **Stop-AzureStack** オペレーター アクションの改善。 Azure Stack Hub を起動する時間が、平均で 50% 短縮されました。 Azure Stack Hub をシャットダウンする時間が、平均で 30% 短縮されました。 平均の起動とシャットダウンの時間は、スケールユニットのノード数が増加しても変わりません。

- 切断された Marketplace ツールのエラー処理が改善されました。 **Export-AzSOfflineMarketplaceItem** の使用時に、ダウンロードが失敗または部分的に成功した場合は、エラーと軽減手順 (可能な場合) に関してより詳しく示した詳細なエラーメッセージが表示されます。

- 大きなページ BLOB/スナップショットからのマネージド ディスク作成のパフォーマンスが向上しました。 以前は、大容量ディスクの作成時にタイムアウトが発生しました。  

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/127669774/home -->
- 予期しない仮想ディスクのデタッチを避けるために、ノードをシャットダウンする前の仮想ディスクの正常性チェックが改善されました。

- 管理者操作のための内部ログのストレージが改善されました。 この追加の結果、内部ログ プロセスのメモリとストレージの消費量を最小限に抑えることで、管理者の操作中のパフォーマンスと信頼性が向上します。 また、管理者ポータルの更新ブレードのページ読み込み時間が短縮される可能性もあります。 この改善の一環として、6 か月を経過した更新ログはシステム上で使用できなくなります。 これらの更新のログが必要な場合は、1907 更新プログラムを実行する前に、6 か月を経過したすべての更新プログラムの実行に関する[概要をダウンロード](azure-stack-apply-updates.md)してください。

### <a name="changes"></a>[変更点]

- Azure Stack Hub バージョン 1907 には、バージョン 1908 に更新する前に、システムの OEM パッケージをバージョン 2.1 以降に更新するようにオペレーターに指示する警告アラートが含まれています。 Azure Stack Hub の OEM 更新プログラムの適用方法の詳細については、「[Azure Stack Hub に OEM (相手先ブランド供給) 更新プログラムを適用する](azure-stack-update-oem.md)」を参照してください。

- Azure Stack Hub 診断ログ収集サービスの通信を有効にするための新しいアウトバウンド規則 (HTTPS) が追加されました。 詳細については、[Azure Stack Hub データセンター統合 - エンドポイントの発行](azure-stack-integrate-endpoints.md#ports-and-urls-outbound)に関するページを参照してください。

- 外部ストレージの場所の容量が不足している場合、インフラストラクチャ バックアップ サービスによって、部分的にアップロードされたバックアップが削除されるようになりました。

- インフラストラクチャのバックアップに、ドメイン サービス データのバックアップが含まれなくなりました。 この変更は、Azure Active Directory を ID プロバイダーとして使用するシステムにのみ適用されます。

- **[コンピューティング] > [VM イメージ]** ブレードに取り込まれたイメージが、ページ BLOB の種類であることを検証するようになりました。

- 特権エンドポイント コマンド **Set-BmcCredential** を使って、ベースボード管理コントローラーの資格情報を更新できるようになりました。

### <a name="fixes"></a>修正

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there's an SR/ICM associated to it. -->
- Resource Manager テンプレート上で、発行元、オファー、SKU が大文字と小文字を区別して扱われていた問題を修正しました (イメージ パラメーターが発行元、オファー、および SKU と同じ大文字または小文字でない限り、イメージがデプロイ用にフェッチされませんでした)。

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/129536438/home -->
- ストレージ サービス メタデータのバックアップ中のタイムアウトのため、**PartialSucceeded** エラー メッセージでバックアップが失敗する問題を修正しました。  

- ユーザー サブスクリプションを削除すると、リソースが孤立する問題を修正しました。

- オファーの作成時に説明フィールドが保存されなかった問題を修正しました。

- **読み取り専用**のアクセス許可を持つユーザーが、リソースの作成、編集、および削除を行うことができる問題を修正しました。 これで、**共同作成者**のアクセス許可が割り当てられている場合にのみ、ユーザーはリソースを作成できるようになりました。 

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/127772311/home -->
- WMI プロバイダー ホストによって DLL ファイルがロックされるために更新が失敗する問題を修正しました。

- 更新サービスで、更新タイルまたはリソース プロバイダーで、使用可能な更新が表示されない問題を修正しました。 この問題は 1906 で見つかり、修正プログラム [KB4511282](https://support.microsoft.com/help/4511282/) で修正されました。

- 不正な構成によって、管理プレーンが異常になるために、更新が失敗する可能性のある問題を修正しました。 この問題は 1906 で見つかり、修正プログラム [KB4512794](https://support.microsoft.com/help/4512794/) で修正されました。

- ユーザーがマーケットプレースからサードパーティのイメージのデプロイを完了できない問題を修正しました。 この問題は 1906 で見つかり、修正プログラム [KB4511259](https://support.microsoft.com/help/4511259/) で修正されました。

- ユーザー イメージ マネージャー サービスのクラッシュのため、管理対象イメージからの VM の作成が失敗する可能性がある問題を修正しました。 この問題は 1906 で見つかり、修正プログラム [KB4512794](https://support.microsoft.com/help/4512794/) で修正されました

- アプリ ゲートウェイ キャッシュが想定どおりに更新されないために VM CRUD 操作が失敗することがある問題を修正しました。 この問題は 1906 で見つかり、修正プログラム [KB4513119](https://support.microsoft.com/en-us/help/4513119/) で修正されました

- 管理者ポータルでリージョンおよびアラート ブレードの可用性に影響を与えていた正常性リソース プロバイダーの問題を修正しました。 この問題は 1906 で見つかり、修正プログラム [KB4512794](https://support.microsoft.com/help/4512794) で修正されました。

## <a name="security-updates"></a>セキュリティ更新プログラム

Azure Stack Hub のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack Hub のセキュリティ更新プログラム](release-notes-security-updates.md)」を参照してください。

## <a name="update-planning"></a>計画の更新

更新プログラムを適用する前に、必ず次の情報を確認してください。

- [既知の問題](known-issues.md)
- [セキュリティ更新プログラム](release-notes-security-updates.md)
- [更新プログラム適用前後のアクティビティのチェックリスト](release-notes-checklist.md)

## <a name="download-the-update"></a>更新プログラムをダウンロードする

Azure Stack Hub 1907 更新プログラム パッケージは、[Azure Stack Hub ダウンロード ページ](https://aka.ms/azurestackupdatedownload)からダウンロードできます。

## <a name="hotfixes"></a>修正プログラム

Azure Stack Hub では、修正プログラムが定期的にリリースされます。 Azure Stack Hub を 1907 に更新する前に、必ず 1906 用の最新の Azure Stack Hub 修正プログラムをインストールしてください。

Azure Stack Hub 修正プログラムを適用できるのは Azure Stack Hub 統合システムのみです。ASDK には修正プログラムのインストールを試行しないでください。

### <a name="before-applying-the-1907-update"></a>1907 更新プログラムを適用する前

Azure Stack Hub の 1907 リリースは、以下の修正プログラムが適用された 1906 リリースに適用する必要があります。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修正プログラム 1.1906.15.60](https://support.microsoft.com/help/4524559)

### <a name="after-successfully-applying-the-1907-update"></a>1907 更新プログラムの適用に成功した後

この更新プログラムをインストールした後、適用可能な修正プログラムがあればインストールします。 詳細については、[サービス ポリシー](azure-stack-servicing-policy.md)に関する記事を参照してください。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修正プログラム 1.1907.18.56](https://support.microsoft.com/help/4528552)
::: moniker-end

::: moniker range="azs-1906"
## <a name="1906-build-reference"></a>1906 ビルドのリファレンス

Azure Stack Hub 1906 更新プログラムのビルド番号は **1.1906.0.30** です。

### <a name="update-type"></a>更新の種類

Azure Stack Hub 1906 更新プログラムのビルドの種類は**高速**です。 更新プログラムのビルドの種類については、「[Azure Stack Hub での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。 1906 更新プログラムが完了するまでの予測所要時間は、ご使用の Azure Stack Hub 環境内の物理ノード数に関係なく、約 10 時間です。 更新プログラムの正確なランタイムは一般的に、ご使用のシステムでテナント ワークロードによって使用されている容量、システム ネットワーク接続 (インターネットに接続されている場合)、およびシステム ハードウェアの仕様に左右されます。 ランタイムが予測値よりも長くなることは珍しいわけではなく、更新が失敗した場合を除いて、Azure Stack Hub オペレーターによるアクションは必要ありません。 このおおよその実行時間は、1906 更新プログラムに固有であり、他の Azure Stack Hub 更新プログラムと比較することはできません。

## <a name="whats-in-this-update"></a>この更新プログラムの新機能

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

- すべてのエンドポイントで TLS 1.2 を強制するため、特権エンドポイント (PEP) に **Set-TLSPolicy** コマンドレットが追加されました。 詳しくは、[Azure Stack Hub のセキュリティ コントロール](azure-stack-security-configuration.md)に関するページを参照してください。

- 適用されている TLS ポリシーを取得するため、特権エンドポイント (PEP) に **Get-TLSPolicy** コマンドレットが追加されました。 詳しくは、[Azure Stack Hub のセキュリティ コントロール](azure-stack-security-configuration.md)に関するページを参照してください。

- システムの更新中に、必要に応じて、内部の TLS 証明書をローテーションする、内部シークレットのローテーション プロシージャが追加されました。

- 期限切れ間近のシークレットに関する重大なアラートが無視された場合に、内部シークレットのローテーションを強制することで、内部シークレットの有効期限切れを回避するための保護が追加されました。 通常の運用手順として、この保護機能に依存しないでください。 シークレットのローテーションは、メンテナンス期間中に計画する必要があります。 詳しくは、[Azure Stack Hub シークレットのローテーション](azure-stack-rotate-secrets.md)に関するページを参照してください。

- AD FS を使用した Azure Stack Hub のデプロイで Visual Studio Code がサポートされるようになりました。

### <a name="improvements"></a>機能強化

<!-- Changes and product improvements with tangible customer-facing value. -->

- 特権エンドポイントの **Get-GraphApplication** コマンドレットで、現在使用されている証明書の拇印が表示されるようになりました。 この更新により、AD FS を使用して Azure Stack Hub が展開されるときのサービス プリンシパルの証明書の管理が改善されます。

- AD Graph と AD FS の可用性を検証するため、アラートを生成する機能を含む、新しい正常性の監視ルールが追加されました。

- インフラストラクチャ バックアップ サービスを別のインスタンスに移動するときに、バックアップ リソース プロバイダーの信頼性が向上しました。

- メンテナンス期間のスケジュール設定を円滑にするため、均一な実行時間を提供する外部シークレットのローテーション プロシージャのパフォーマンスが最適化されました。

- **Test-AzureStack** コマンドレットで、有効期限が近づいている (重大なアラート) 内部シークレットについて報告されるようになりました。

- 特権エンドポイントの **Register-CustomAdfs** コマンドレットで、AD FS に対してフェデレーションの信頼を構成するときに、証明書失効リストのチェックをスキップできるようにする新しいパラメーターが使用できるようになりました。

- 1906 リリースでは、更新プログラムが一時停止していないことが確認できるように、更新プログラムの進行状況の可視性が向上しています。 この更新により、 **[更新]** ブレード上でオペレーターに表示される更新手順の合計数が増えました。 また、以前の更新プログラムよりも並列で行われる更新手順が増えています。

#### <a name="networking-updates"></a>ネットワークの更新

- DHCP レスポンダーに設定されているリース期間が、Azure と一致するように更新されました。

- リソースのデプロイに失敗したシナリオで、リソース プロバイダーへの再試行回数が改善されました。

- **Standard** SKU オプションは、現在サポートされていないため、ロード バランサーとパブリック IP の両方から削除されました。

### <a name="changes"></a>[変更点]

- ストレージ アカウントのエクスペリエンスの作成が、Azure と一致するようになりました。

- 内部シークレットの有効期限のアラート トリガーが変更されました。
  - 警告アラートは、シークレットの有効期限の 90 日前に表示されるようになりました。
  - 重大なアラートは、シークレットの有効期限の 30 日前に表示されるようになりました。

- 用語の一貫性のため、インフラストラクチャ バックアップ リソース プロバイダー内の文字列が更新されました。

### <a name="fixes"></a>修正

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there's an SR/ICM associated to it. -->

- **内部操作エラー**でマネージド ディスク VM のサイズ変更が失敗する問題を修正しました。

- ユーザー イメージの作成の失敗により、イメージを管理するサービスが不適切な状態になり、これにより失敗したイメージの削除と新しいイメージの作成がブロックされる問題を修正しました。 この問題は、1905 修正プログラムでも修正されています。

- 期限切れ間近の内部シークレットのアクティブなアラートが、内部シークレットのローテーションが正常に実行された後に自動的に終了されるようになりました。

- 更新プログラムが 99 時間を超えて実行されていた場合、[更新履歴] タブの更新期間の最初の桁がトリミングされる問題を修正しました。

- **[更新]** ブレードに、失敗した更新プログラムの **[再開]** オプションが含まれるようになりました。

- 管理者ポータルとユーザー ポータル上で、検索によって Docker 拡張機能が不正に返されるが、Azure Stack Hub では使用できないために、それ以上の操作が実行できなくなるマーケットプレースでの問題を修正しました。

- テンプレートの名前がアンダースコア ('_') で始まる場合に、テンプレートのデプロイ UI にパラメーターが設定されない問題を修正しました。

- 仮想マシン スケール セットの作成エクスペリエンスで、デプロイのオプションとして CentOS-based 7.2 が提供される問題を修正しました。 CentOS 7.2 は Azure Stack Hub では利用できません。 Centos 7.5 をデプロイのオプションとして提供するようになりました

- **[仮想マシン スケール セット]** ブレードからスケール セットを削除できるようになりました。

## <a name="security-updates"></a>セキュリティ更新プログラム

Azure Stack Hub のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack Hub のセキュリティ更新プログラム](release-notes-security-updates.md)」を参照してください。

## <a name="update-planning"></a>計画の更新

更新プログラムを適用する前に、必ず次の情報を確認してください。

- [既知の問題](known-issues.md)
- [セキュリティ更新プログラム](release-notes-security-updates.md)
- [更新プログラム適用前後のアクティビティのチェックリスト](release-notes-checklist.md)

## <a name="download-the-update"></a>更新プログラムをダウンロードする

Azure Stack Hub 1906 更新プログラム パッケージは、[Azure Stack Hub ダウンロード ページ](https://aka.ms/azurestackupdatedownload)からダウンロードできます。

## <a name="hotfixes"></a>修正プログラム

Azure Stack Hub では、修正プログラムが定期的にリリースされます。 Azure Stack Hub を 1906 に更新する前に、必ず 1905 用の最新の Azure Stack Hub 修正プログラムをインストールしてください。 更新後、[1906 に対して利用可能な修正プログラム](#after-successfully-applying-the-1906-update)があればインストールします。

Azure Stack Hub 修正プログラムを適用できるのは Azure Stack Hub 統合システムのみです。ASDK には修正プログラムのインストールを試行しないでください。

### <a name="before-applying-the-1906-update"></a>1906 更新プログラムを適用する前

Azure Stack Hub の 1906 リリースは、以下の修正プログラムが適用された 1905 リリースに適用する必要があります。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修正プログラム 1.1905.3.48](https://support.microsoft.com/help/4510078)

### <a name="after-successfully-applying-the-1906-update"></a>1906 更新プログラムの適用に成功した後

この更新プログラムをインストールした後、適用可能な修正プログラムがあればインストールします。 詳細については、[サービス ポリシー](azure-stack-servicing-policy.md)に関する記事を参照してください。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack Hub 修正プログラム 1.1906.15.60](https://support.microsoft.com/help/4524559)
::: moniker-end

::: moniker range=">=azs-1906"
## <a name="automatic-update-notifications"></a>自動更新通知

インフラストラクチャ ネットワークからインターネットにアクセスできるシステムでは、オペレーター ポータルに "**利用可能な更新プログラムがあります**" というメッセージが表示されます。 インターネットにアクセスできないシステムでは、対応する .xml を含む .zip ファイルをダウンロードしてインポートできます。

> [!TIP]  
> 以下の *RSS* または *Atom* フィードに登録して、Azure Stack Hub 修正プログラムの最新情報を入手してください。
>
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="archive"></a>アーカイブ

アーカイブされた以前のバージョンのリリース ノートにアクセスするには、左側の目次の上部にあるバージョン セレクターのドロップダウンを使用して、表示するバージョンを選択します。

## <a name="next-steps"></a>次のステップ

- Azure Stack Hub での更新プログラム管理の概要については、「[Azure Stack Hub での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。  
- Azure Stack Hub に更新プログラムを適用する方法については、「[Azure Stack Hub で更新を適用する](azure-stack-apply-updates.md)」を参照してください。
- Azure Stack Hub のサービス ポリシーについて、また、サポートを受けられる状態にシステムを維持するために必要な作業について確認するには、「[Azure Stack Hub サービス ポリシー](azure-stack-servicing-policy.md)」を参照してください。  
- 特権エンドポイント (PEP) を使用して更新プログラムを監視および再開するには、「[特権エンドポイントを使用して Azure Stack Hub での更新プログラムをモニターする](azure-stack-monitor-update.md)」を参照してください。
::: moniker-end

<!------------------------------------------------------------>
<!------------------- UNSUPPORTED VERSIONS ------------------->
<!------------------------------------------------------------>
::: moniker range="azs-1905"
## <a name="1905-archived-release-notes"></a>1905 アーカイブされたリリース ノート
::: moniker-end
::: moniker range="azs-1904"
## <a name="1904-archived-release-notes"></a>1904 アーカイブされたリリース ノート
::: moniker-end
::: moniker range="azs-1903"
## <a name="1903-archived-release-notes"></a>1903 アーカイブされたリリース ノート
::: moniker-end
::: moniker range="azs-1902"
## <a name="1902-archived-release-notes"></a>1902 アーカイブされたリリース ノート
::: moniker-end
::: moniker range="azs-1901"
## <a name="1901-archived-release-notes"></a>1901 アーカイブされたリリース ノート
::: moniker-end
::: moniker range="azs-1811"
## <a name="1811-archived-release-notes"></a>1811 アーカイブされたリリース ノート
::: moniker-end
::: moniker range="azs-1809"
## <a name="1809-archived-release-notes"></a>1809 アーカイブされたリリース ノート
::: moniker-end
::: moniker range="azs-1808"
## <a name="1808-archived-release-notes"></a>1808 アーカイブされたリリース ノート
::: moniker-end
::: moniker range="azs-1807"
## <a name="1807-archived-release-notes"></a>1807 アーカイブされたリリース ノート
::: moniker-end
::: moniker range="azs-1805"
## <a name="1805-archived-release-notes"></a>1805 アーカイブされたリリース ノート
::: moniker-end
::: moniker range="azs-1804"
## <a name="1804-archived-release-notes"></a>1804 アーカイブされたリリース ノート
::: moniker-end
::: moniker range="azs-1803"
## <a name="1803-archived-release-notes"></a>1803 アーカイブされたリリース ノート
::: moniker-end
::: moniker range="azs-1802"
## <a name="1802-archived-release-notes"></a>1802 アーカイブされたリリース ノート
::: moniker-end

::: moniker range="<azs-1906"
[以前のバージョンの Azure Stack Hub のリリース ノートは TechNet ギャラリー](https://aka.ms/azsarchivedrelnotes)でアクセスできます。 これらのアーカイブされたドキュメントは、参照のみを目的に提供されており、これらのバージョンのサポートを意味しているわけではありません。 Azure Stack のサポートについては、「[Azure Stack Hub サービス ポリシー](azure-stack-servicing-policy.md)」を参照してください。 さらにサポートが必要な場合は、Microsoft カスタマー サポート サービスにお問い合わせください。
::: moniker-end


