---
title: Azure Stack のリリース ノート | Microsoft Docs
description: 新機能や更新プログラムをダウンロードする場所など、Azure Stack 統合システムの更新プログラムについて学習します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2019
ms.author: sethm
ms.reviewer: prchint
ms.lastreviewed: 11/22/2019
ms.openlocfilehash: 75f1c4cae33987a7a2c662ced7806ed094c6ca82
ms.sourcegitcommit: 3a8e116fd0b16e1201e55e2088dde2e581004045
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/27/2019
ms.locfileid: "74557703"
---
# <a name="azure-stack-updates-release-notes"></a>Azure Stack の更新プログラム: リリース ノート

*適用対象:Azure Stack 統合システム*

この記事では、Azure Stack 更新プログラム パッケージの内容について説明します。 この更新プログラムには、このリリースの Azure Stack に対する新機能、機能強化、および修正が含まれています。

別のバージョンのリリース ノートにアクセスするには、左側の目次の上部にあるバージョン セレクターのドロップダウンを使用します。

::: moniker range=">=azs-1906"
> [!IMPORTANT]  
> 更新プログラム パッケージは、Azure Stack 統合システム専用です。 Azure Stack Development Kit にこの更新プログラム パッケージは適用しないでください。
::: moniker-end
::: moniker range="<azs-1906"
> [!IMPORTANT]  
> ご利用の Azure Stack インスタンスが 2 つ前の更新プログラムより遅れている場合、コンプライアンスに対応していないとみなされます。 [サポートを受けるためには、少なくともサポートされる最小バージョンまで更新する](azure-stack-servicing-policy.md#keep-your-system-under-support)必要があります。
::: moniker-end

## <a name="update-planning"></a>計画の更新

更新プログラムを適用する前に、必ず次の情報を確認してください。

- [既知の問題](known-issues.md)
- [セキュリティ更新プログラム](release-notes-security-updates.md)
- [更新プログラム適用前後のアクティビティのチェックリスト](release-notes-checklist.md)

更新プログラムのトラブルシューティングと更新プロセスの詳細については、[Azure Stack の修正プログラムと更新プログラムに関する問題のトラブルシューティング](azure-stack-updates-troubleshoot.md)に関するページをご確認ください。

<!---------------------------------------------------------->
<!------------------- SUPPORTED VERSIONS ------------------->
<!---------------------------------------------------------->
::: moniker range="azs-1910"
## <a name="1910-build-reference"></a>1910 ビルドのリファレンス

Azure Stack 1910 更新プログラムのビルド番号は **1.1910.0.58** です。

### <a name="update-type"></a>更新の種類

1908 以降、Azure Stack が実行される基盤のオペレーティング システムは Windows Server 2019 に更新されました。 これにより、核となる基本的な機能強化だけでなく、近い将来に Azure Stack に機能を追加する機能も使用可能になります。

Azure Stack 1910 更新プログラムのビルドの種類は**高速**です。

1910 更新プログラム パッケージは、以前の更新プログラムよりも大きなサイズです。 サイズが大きいため、ダウンロード時間が長くなります。 この更新プログラムは**準備中**状態の時間が長くなります。オペレーターは以前の更新プログラムよりもこのプロセスの時間が長くなることを想定してください。 1910 更新プログラムが完了するまでの予測所要時間は、ご使用の Azure Stack 環境内の物理ノード数に関係なく、約 10 時間です。 更新プログラムの正確なランタイムは一般的に、ご使用のシステムでテナント ワークロードによって使用されている容量、システム ネットワーク接続 (インターネットに接続されている場合)、およびシステム ハードウェアの仕様に左右されます。 ランタイムがこの予測値よりも長くなることは一般的ではなく、更新が失敗した場合を除き、Azure Stack オペレーターによるアクションは不要です。 このおおよその実行時間は、1910 更新プログラムに固有であり、他の Azure Stack 更新プログラムと比較することはできません。

更新プログラムのビルドの種類については、「[Azure Stack での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。

<!-- ## What's in this update -->

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>新機能

<!-- What's new, also net new experiences and features. -->

- 管理者ポータルでは、リージョンのプロパティ メニューに特権エンドポイントの IP アドレスが表示され、簡単に見つけられるようになりました。 さらに、現在構成されているタイム サーバーと DNS フォワーダーも表示されます。

- Azure Stack の正常性および監視システムでは、エラーが発生した場合、さまざまなハードウェア コンポーネントに対してアラートを生成できるようになりました。 これには追加の構成が必要です。 詳細については、「[Azure Stack のハードウェア コンポーネントを監視する](azure-stack-hardware-monitoring.md)」を参照してください。

- [Azure Stack の cloud-Init のサポート](/azure/virtual-machines/linux/using-cloud-init):cloud-Init は、Linux VM を初回起動時にカスタマイズするために広く使用されているアプローチです。 cloud-init を使って、パッケージをインストールしてファイルを書き込んだり、ユーザーとセキュリティを構成したりすることができます。 cloud-init は初回起動プロセスの間に呼び出されるので、構成を適用するために追加の手順や必要なエージェントはありません。 マーケットプレースの Ubuntu イメージは、プロビジョニング用の cloud-init をサポートするように更新されました。

- Azure Stack では、すべての Windows Azure Linux エージェント バージョンバージョンが Azure としてサポートされるようになりました。

- Azure Stack Admin PowerShell モジュールの新しいバージョンがリリースされました。 <!-- For more information, see -->

- Azure Stack インフラストラクチャの Windows Defender 定義の手動更新を構成するために、特権エンドポイント (PEP) に **Set-AzSDefenderManualUpdate** コマンドレットを追加しました。 詳細については、「[Azure Stack 上で Windows Defender ウイルス対策を更新する](azure-stack-security-av.md)」を参照してください。

- Azure Stack インフラストラクチャの Windows Defender 定義の手動更新の構成を取得するために、特権エンドポイント (PEP) に **Get-AzSDefenderManualUpdate** コマンドレットを追加しました。 詳細については、「[Azure Stack 上で Windows Defender ウイルス対策を更新する](azure-stack-security-av.md)」を参照してください。

- 特権エンドポイント (PEP) に Azure Stack の DNS サーバーのフォワーダー設定を変更する **Set-AzSDnsForwarder** コマンドレットを追加しました。 DNS 構成の詳細については、「[Azure Stack データセンターの DNS の統合](azure-stack-integrate-dns.md)」を参照してください。

- 特権エンドポイント (PEP) に Azure Stack の DNS サーバーのフォワーダー設定を取得する **Get-AzSDnsForwarder** コマンドレットを追加しました。 DNS 構成の詳細については、「[Azure Stack データセンターの DNS の統合](azure-stack-integrate-dns.md)」を参照してください。

- [AKS エンジン](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview)を使用した **Kubernetes クラスター**の管理のサポートを追加しました。 この更新プログラムから、運用環境の Kubernetes クラスターをデプロイできるようになりました。 AKS エンジンを使うと、次のことができます。
   - Kubernetes クラスターのライフサイクルを管理できます。 クラスターの作成、更新、およびスケールを行うことができます。
   - AKS チームおよび Azure Stack チームによって作成されたマネージド イメージを使用して、クラスターを維持できます。
   - Azure Resource Manager に統合された Kubernetes クラウド プロバイダーを利用し、Azure ネイティブ リソースを使用してクラスターを構築できます。
   - 接続または切断された Azure Stack スタンプでクラスターをデプロイおよび管理できます。
   - Azure ハイブリッド機能を利用できます。
      * Azure Arc との統合 (プライベート プレビューが近日公開予定)
      * Azure Monitor for containers との統合 (パブリック プレビュー段階)
   - Windows コンテナーを AKS エンジン (プライベート プレビュー段階) と共に使用できます。
   - デプロイの CSS と PG サポートを受けることができます。

### <a name="improvements"></a>機能強化

<!-- Changes and product improvements with tangible customer-facing value. -->

- 外部デバイス (USB キーなど) が Azure Stack インフラストラクチャのノードにマウントされたときにレポートする監査ルールが追加されました。 監査ログは syslog を介して出力され、"**Microsoft-Windows-Security-Auditing:6416|Plug and Play Events** (Microsoft-Windows-Security-Auditing: 6416|プラグ アンド プレイ イベント)" と表示されます。 syslog クライアントの構成方法の詳細については、[Syslog の転送](azure-stack-integrate-security.md)に関する記事を参照してください。

- Azure Stack は、内部証明書用に 4096 ビット RSA キーに移行しています。 内部シークレット ローテーションを実行すると、以前の 2048 ビット証明書は 4096 ビット長の証明書に置き換えられます。 Azure Stack のシークレット ローテーションの詳細については、「[Azure Stack でシークレットをローテーションする](azure-stack-rotate-secrets.md)」を参照してください。

- Committee on National Security Systems - Policy 15 (CNSSP-15) (安全な情報共有のための公開標準の使用に関するベスト プラクティスが記載されています) に準拠するために、いくつかの内部コンポーネントについて暗号化アルゴリズムの複雑さとキー強度をアップグレードします。 強化された点として、Kerberos 認証用の AES256 と、VPN 暗号化用の SHA384 があります。 CNSSP-15 の詳細については、[Committee on National Security Systems の「Policies (ポリシー)」ページ](http://www.cnss.gov/CNSS/issuances/Policies.cfm)を参照してください。

- 上記のアップグレードの結果、Azure Stack には IPsec/IKEv2 構成の新しい既定値が追加されました。 Azure Stack 側で使用される新しい既定値は次のとおりです。

   **IKE フェーズ 1 (メイン モード) のパラメーター**

   | プロパティ              | 値|
   |-|-|
   | IKE のバージョン           | IKEv2 |
   |Diffie-hellman グループ   | ECP384 |
   | 認証方法 | 事前共有キー |
   |暗号化とハッシュ アルゴリズム | AES256、SHA384 |
   |SA の有効期間 (時間)     | 28,800 秒|

   **IKE フェーズ 2 (クイック モード) のパラメーター**

   | プロパティ| 値|
   |-|-|
   |IKE のバージョン |IKEv2 |
   |暗号化とハッシュ アルゴリズム (暗号化)     | GCMAES256|
   |暗号化とハッシュ アルゴリズム (認証) | GCMAES256|
   |SA の有効期間 (時間)  | 27,000 秒  |
   |SA の有効期間 (キロバイト単位) | 33,553,408     |
   |Perfect Forward Secrecy (PFS) | ECP384 |
   |Dead Peer Detection | サポートされています|

   これらの変更は、[既定の IPsec/IKE 提案](../user/azure-stack-vpn-gateway-settings.md#ipsecike-parameters)のドキュメントにも反映されます。

- インフラストラクチャ バックアップ サービスでは、固定のしきい値を利用するのではなく、バックアップに必要な空き領域を計算するようにロジックが改善されました。 このサービスでは、バックアップのサイズ、アイテム保持ポリシー、予約、および外部ストレージの場所の現在の使用率が使用され、オペレーターに警告を発する必要があるかどうかが判断されます。 

### <a name="changes"></a>変更点

- Azure から Azure Stack にマーケットプレース アイテムをダウンロードするときに、複数のバージョンが存在する場合、アイテムのバージョンを指定できる新しいユーザー インターフェイスが追加されました。 新しい UI は、接続されたシナリオと切断されたシナリオの両方で使用できます。 詳細については、「[Azure から Azure Stack に Marketplace の項目をダウンロードする](azure-stack-download-azure-marketplace-item.md)」を参照してください。  

- 1910 リリース以降、Azure Stack システムには追加で /20 プライベート内部 IP 空間が必要になりました。 このネットワークは Azure Stack システム専用であり、データセンター内の複数の Azure Stack システムで再利用できます。 このネットワークは Azure Stack 専用ですが、データセンター内のネットワークと重複することはできません。 /20 プライベート IP 空間は複数のネットワークに分割され、コンテナー上で Azure Stack インフラストラクチャを実行できるようになりました ([1905 リリース ノート](release-notes.md?view=azs-1905)にも記載されています)。 コンテナーで Azure Stack インフラストラクチャを実行する目的は、使用率を最適化し、パフォーマンスを向上させることです。 さらに、/20 プライベート IP 空間は、デプロイ前に必要なルーティング可能な IP 空間を減らす継続的な取り組みを可能にするためにも使用されます。

  - /20 の入力は、1910 の次の Azure Stack 更新プログラムの前提条件となることに注意してください。 1910 の次の Azure Stack 更新プログラムがリリースされ、それをインストールしようとすると、以下の修復手順で説明するように、/20 の入力を完了していない場合、更新は失敗します。 上記の修復手順が完了するまで、管理ポータルにアラートが表示されます。 この新しいプライベート空間の使用方法については、「[データセンターのネットワーク統合](azure-stack-network.md#private-network)」の記事を参照してください。 

  - 修復手順:修復するには、次の手順に従って [PEP セッション開きます](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint)。 サイズ /20 の[プライベート内部 IP 範囲](azure-stack-network.md#logical-networks)を準備し、PEP セッションで、次のコマンドレット (1910 以降でのみ使用可能) を次の形式を使用して実行します`Set-AzsPrivateNetwork -UserSubnet 100.87.0.0/20`。 操作が正常に実行されると、"**Azs Internal Network range added to the config** (構成に Azs 内部ネットワーク範囲が追加されました)" というメッセージが表示されます。正常に完了すると、管理ポータルのアラートは閉じます。 Azure Stack システムは、次のバージョンに更新できるようになります。
  
- アップロード手順中に外部ストレージの場所の容量が不足すると、インフラストラクチャ バックアップ サービスによって、部分的にアップロードされたバックアップ データが削除されます。  

- インフラストラクチャ バックアップ サービスに、AAD デプロイのバックアップ ペイロードに ID サービスが追加されました。  

- 1910 リリースでは、AzureStack PowerShell モジュールがバージョン 1.8.0 に更新されました。<br>変更内容:
   - **新しい DRP 管理モジュール**:デプロイ リソース プロバイダー (DRP) を使用すると、Azure Stack に対するリソース プロバイダーの調整されたデプロイが可能になります。 これらのコマンドを使うと、DRP とやり取りする Azure Resource Manager レイヤーとやり取りできます。
   - **BRP**: <br />
           - Azure Stack インフラストラクチャ バックアップの 1 つのロールの復元をサポートします。 <br />
           - パラメーター `RoleName` をコマンドレット `Restore-AzsBackup` に追加します。
   - **FRP**:API バージョン `2019-05-01` の**ドライブ** リソースと**ボリューム** リソースの重大な変更。 これらの機能は Azure Stack 1910 以降でサポートされています。 <br />
            - `ID`、`Name`、`HealthStatus`、および `OperationalStatus` の値が変更されました。 <br />
            - **ドライブ** リソースの新しいプロパティ `FirmwareVersion`、`IsIndicationEnabled`、`Manufacturer`、`StoragePool` がサポートされるようになりました。 <br />
            - **ドライブ** リソースのプロパティ `CanPool` および `CannotPoolReason` は廃止されました。代わりに `OperationalStatus` を使用してください。

### <a name="fixes"></a>修正

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

- Azure Stack 1904 リリースより前にデプロイされた環境で TLS 1.2 ポリシーを実施できなかった問題を修正しました。
- SSH の承認を有効にして作成した Ubuntu 18.04 VM では、SSH キーを使用してサインインすることができない問題を修正しました。 
- 仮想マシン スケール セットの UI から **[パスワードのリセット]** を削除しました。
- ポータルからロード バランサーを削除しても、インフラストラクチャ レイヤーのオブジェクトが削除されない問題を修正しました。
- 管理ポータルでゲートウェイ プール使用率アラートの割合が正しく表示されない問題を修正しました。
- 仮想マシンの同じ NIC に複数のパブリック IP を追加すると、インターネット接続の問題が発生する問題を修正しました。 これで、2 つのパブリック IP を持つ NIC は正常に動作するようになります。

## <a name="security-updates"></a>セキュリティ更新プログラム

Azure Stack のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack security updates](release-notes-security-updates.md)」 (Azure Stack のセキュリティ更新プログラム) をご覧ください。

## <a name="update-planning"></a>計画の更新

更新プログラムを適用する前に、必ず次の情報を確認してください。

- [既知の問題](known-issues.md)
- [セキュリティ更新プログラム](release-notes-security-updates.md)
- [更新プログラム適用前後のアクティビティのチェックリスト](release-notes-checklist.md)

## <a name="download-the-update"></a>更新プログラムをダウンロードする

Azure Stack 1910 更新プログラム パッケージは、[Azure Stack ダウンロード ページ](https://aka.ms/azurestackupdatedownload)からダウンロードできます。

## <a name="hotfixes"></a>修正プログラム

Azure Stack では、修正プログラムが定期的にリリースされます。 Azure Stack を 1910 に更新する前に、必ず 1908 用の最新の Azure Stack 修正プログラムをインストールしてください。

Azure Stack 修正プログラムを適用できるのは Azure Stack 統合システムのみです。ASDK には修正プログラムをインストールしないでください。

### <a name="prerequisites-before-applying-the-1910-update"></a>前提条件:1910 更新プログラムを適用する前

Azure Stack の 1910 リリースは、次の修正プログラムが適用された 1908 リリースに適用する必要があります。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack 修正プログラム 1.1908.9.43](https://support.microsoft.com/help/4531007)

### <a name="after-successfully-applying-the-1910-update"></a>1910 更新プログラムの適用に成功した後

この更新プログラムをインストールした後、適用可能な修正プログラムがあればインストールします。 詳細については、[サービス ポリシー](azure-stack-servicing-policy.md)に関する記事を参照してください。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- 1910 用の修正プログラムはありません。
::: moniker-end

::: moniker range="azs-1908"
## <a name="1908-build-reference"></a>1908 ビルドのリファレンス

Azure Stack 1908 更新プログラムのビルド番号は **1.1908.4.33** です。

### <a name="update-type"></a>更新の種類

1908 では、Azure Stack が実行される基になるオペレーティング システムが Windows Server 2019 に更新されています。 これにより、核となる基本的な機能強化だけでなく、近い将来に Azure Stack に機能を追加する機能も使用可能になります。

Azure Stack 1908 更新プログラムのビルドの種類は**完全**です。 そのため、1908 更新プログラムは、1906 や 1907 のような高速更新プログラムよりもランタイムが長くなります。 完全な更新プログラムの正確なランタイムは、Azure Stack インスタンスに含まれているノード数、テナントのワークロードごとにシステムで使用される容量、システムのネットワーク接続 (インターネットに接続されている場合)、システムのハードウェア構成によって異なります。 内部テストでは、1908 更新プログラムのランタイムが次のように予測されました。4 ノード - 42 時間、8 ノード - 50 時間、12 ノード - 60 時間、16 ノード - 70 時間。 更新プログラムのランタイムがこの予測値よりも長くなることは一般的ではなく、更新が失敗した場合を除き、Azure Stack オペレーターによるアクションは不要です。

更新プログラムのビルドの種類については、「[Azure Stack での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。

- 更新プログラムの正確な実行時間は一般に、ご使用のシステムでテナント ワークロードによって使用されている容量、システム ネットワーク接続 (インターネットに接続されている場合)、およびシステム ハードウェアの構成に左右されます。
- 実行時間が予測よりも長くなることは一般的ではなく、更新が失敗した場合を除き、Azure Stack オペレーターによるアクションは不要です。
- このおおよその実行時間は、1908 更新プログラムに固有であり、他の Azure Stack 更新プログラムと比較することはできません。

<!-- ## What's in this update -->

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>新機能

<!-- What's new, also net new experiences and features. -->

- 1908 では、Azure Stack が実行される基になるオペレーティング システムが Windows Server 2019 に更新されていることに注意してください。 これにより、核となる基本的な機能強化だけでなく、近い将来に Azure Stack に機能を追加する機能も使用可能になります。
- Azure Stack インフラストラクチャのすべてのコンポーネントが FIPS 140-2 モードで動作するようになりました。
- Azure Stack オペレーターは、ポータル ユーザー データを削除できるようになりました。 詳細については、「[Clear portal user data from Azure Stack](azure-stack-portal-clear.md)」 (Azure Stack からポータル ユーザー データをクリアする) を参照してください。

### <a name="improvements"></a>機能強化

<!-- Changes and product improvements with tangible customer-facing value. -->
- 物理ノードのハードウェアのトラステッド プラットフォーム モジュール (TPM) にシークレットを保持するために、Azure Stack の保存データの暗号化が向上しました。

### <a name="changes"></a>変更点

- ハードウェア プロバイダーは Azure Stack バージョン 1908 と同時に OEM 拡張機能パッケージ 2.1 以降をリリースします。 Azure Stack バージョン 1908 には OEM 拡張機能パッケージ 2.1 以降が前提条件です。 OEM 拡張機能パッケージ 2.1 以降をダウンロードする方法の詳細については、システムのハードウェア プロバイダーに問い合わせてください。また、[OEM 更新プログラム](azure-stack-update-oem.md#oem-contact-information)の記事を参照してください。  

### <a name="fixes"></a>修正

- 今後の Azure Stack OEM 更新プログラムとの互換性、およびユーザーのユーザー イメージを使用した VM デプロイに関する問題が修正されました。 この問題は 1907 で見つかり、修正プログラム [KB4517473](https://support.microsoft.com/en-us/help/4517473/azure-stack-hotfix-1-1907-12-44) で修正されました  
- OEM ファームウェア更新プログラムに関する問題が修正され、Fabric リングの正常性についての Test-AzureStack での誤診断に関する問題が修正されました。 この問題は 1907 で見つかり、修正プログラム [KB4515310](https://support.microsoft.com/en-us/help/4515310/azure-stack-hotfix-1-1907-7-35) で修正されました
- OEM ファームウェア更新プロセスに関する問題を修正しました。 この問題は 1907 で見つかり、修正プログラム [KB4515650](https://support.microsoft.com/en-us/help/4515650/azure-stack-hotfix-1-1907-8-37) で修正されました

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

## <a name="security-updates"></a>セキュリティ更新プログラム

Azure Stack のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack security updates](release-notes-security-updates.md)」 (Azure Stack のセキュリティ更新プログラム) をご覧ください。

## <a name="download-the-update-1908"></a>更新プログラムのダウンロード

Azure Stack 1908 更新プログラム パッケージは、[Azure Stack ダウンロード ページ](https://aka.ms/azurestackupdatedownload)からダウンロードできます。

## <a name="hotfixes"></a>修正プログラム

Azure Stack では、修正プログラムが定期的にリリースされます。 Azure Stack を 1908 に更新する前に、必ず 1907 用の最新の Azure Stack 修正プログラムをインストールしてください。

Azure Stack 修正プログラムを適用できるのは Azure Stack 統合システムのみです。ASDK には修正プログラムをインストールしないでください。

### <a name="prerequisites-before-applying-the-1908-update"></a>前提条件:1908 更新プログラムを適用する前

Azure Stack の 1908 リリースは、次の修正プログラムが適用された 1907 リリースに適用する必要があります。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack 修正プログラム 1.1907.18.56](https://support.microsoft.com/help/4528552)

Azure Stack 1908 更新プログラムには、システムのハードウェア プロバイダーからの **Azure Stack OEM バージョン 2.1 以降**が必要です。 OEM 更新プログラムには、Azure Stack システム ハードウェアのドライバーとファームウェアの更新プログラムが含まれています。 OEM 更新プログラムの適用の詳細については、「[Azure Stack に OEM (相手先ブランド供給) 更新プログラムを適用する](azure-stack-update-oem.md)」を参照してください

### <a name="after-successfully-applying-the-1908-update"></a>1908 更新プログラムの適用に成功した後

この更新プログラムをインストールした後、適用可能な修正プログラムがあればインストールします。 詳細については、[サービス ポリシー](azure-stack-servicing-policy.md)に関する記事を参照してください。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack 修正プログラム 1.1908.9.43](https://support.microsoft.com/help/4531007)
::: moniker-end

::: moniker range="azs-1907"
## <a name="1907-build-reference"></a>1907 ビルドのリファレンス

Azure Stack 1907 更新プログラムのビルド番号は **1.1907.0.20** です。

### <a name="update-type"></a>更新の種類

Azure Stack 1907 更新プログラムのビルドの種類は**高速**です。 更新プログラムのビルドの種類については、「[Azure Stack での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。 内部テストに基づいて、1907 更新プログラムが完了するまでの予測所要時間は約 13 時間です。

- 更新プログラムの正確な実行時間は一般に、ご使用のシステムでテナント ワークロードによって使用されている容量、システム ネットワーク接続 (インターネットに接続されている場合)、およびシステム ハードウェアの構成に左右されます。
- 実行時間が予測よりも長くなることは一般的ではなく、更新が失敗した場合を除き、Azure Stack オペレーターによるアクションは不要です。
- このおおよその実行時間は、1907 更新プログラムに固有であり、他の Azure Stack 更新プログラムと比較することはできません。

## <a name="whats-in-this-update"></a>この更新プログラムの新機能

<!-- The current theme (if any) of this release. -->

### <a name="whats-new"></a>新機能

<!-- What's new, also net new experiences and features. -->

- 診断ログの収集を容易にし、改善するための Azure Stack 診断ログ収集サービスの一般公開リリース。 Azure Stack 診断ログ収集サービスは、診断ログを収集して、Microsoft カスタマー サポート サービス (CSS) と共有するための簡単な方法を提供します。 この診断ログ収集サービスは、Azure Stack 管理者ポータルでの新しいユーザー エクスペリエンスを提供します。これにより、オペレーターは、特定の重要なアラートが発生したときに、ストレージ BLOB への診断ログの自動アップロードを設定したり、オンデマンドで同じ操作を実行したりすることができます。 詳細については、[診断ログの収集](azure-stack-diagnostic-log-collection-overview.md)に関する記事を参照してください。

- Azure Stack 検証ツール **Test-AzureStack** の一部としての Azure Stack ネットワーク インフラストラクチャ検証の一般公開リリース。 Azure Stack ネットワーク インフラストラクチャは、**Test-AzureStack** の一部になり、Azure Stack のネットワーク インフラストラクチャで障害が発生したかどうかを特定します。 テストでは、Azure Stack ソフトウェアで定義されたネットワークをバイパスすることによって、ネットワーク インフラストラクチャの接続が確認されます。 パブリック VIP から構成済みの DNS フォワーダー、NTP サーバー、および ID エンドポイントへの接続が示されます。 さらに、ID プロバイダーとして Azure AD を使用する場合に Azure への、または ADFS を使用する場合にフェデレーション サーバーへの接続が確認されます。 詳細については、[Azure Stack 検証ツール](azure-stack-diagnostic-test.md)に関する記事を参照してください。

- システムの更新中に、必要に応じて、内部の SQL TLS 証明書をローテーションする、内部シークレットのローテーション プロシージャが追加されました。

### <a name="improvements"></a>機能強化

<!-- Changes and product improvements with tangible customer-facing value. -->

- Azure Stack の更新ブレードに、アクティブな更新の**最後のステップが完了した**時刻が表示されるようになりました。 これは、更新ブレードに移動し、実行中の更新をクリックすると表示されます。 **[Last Step Completed]\(最後のステップの完了\)** は、 **[Update run details]\(更新実行の詳細\)** セクションにあります。

- **Start-AzureStack** と **Stop-AzureStack** オペレーター アクションの改善。 Azure Stack を起動する時間が、平均で 50% 短縮されました。 Azure Stack をシャットダウンする時間が、平均で 30% 短縮されました。 平均の起動とシャットダウンの時間は、スケールユニットのノード数が増加しても変わりません。

- 切断された Marketplace ツールのエラー処理が改善されました。 **Export-AzSOfflineMarketplaceItem** を使用した場合に、ダウンロードが失敗するか、部分的に成功した場合、エラーと軽減手順 (存在する場合) に関する詳細を示す詳細なエラーメッセージが表示されます。

- 大きなページ BLOB/スナップショットからのマネージド ディスク作成のパフォーマンスが向上しました。 以前は、大容量ディスクの作成時にタイムアウトが発生しました。  

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/127669774/home -->
- 予期しない仮想ディスクのデタッチを避けるために、ノードをシャットダウンする前の仮想ディスクの正常性チェックが改善されました。

- 管理者操作のための内部ログのストレージが改善されました。 その結果、内部ログ プロセスのメモリとストレージの消費量を最小限に抑えることで、管理者の操作中にパフォーマンスと信頼性が向上します。 また、管理者ポータルの更新ブレードのページ読み込み時間が短縮される可能性もあります。 この改善の一環として、6 か月を経過した更新ログはシステムで使用できなくなります。 これらの更新のログが必要な場合は、1907 更新を実行する前に、6 か月より前のすべての更新の実行の[概要をダウンロード](azure-stack-apply-updates.md)してください。

### <a name="changes"></a>変更点

- Azure Stack バージョン 1907 には、バージョン 1908 に更新する前に、システムの OEM パッケージをバージョン 2.1 以降に更新するようにオペレーターに指示する警告アラートが含まれています。 Azure Stack の OEM 更新プログラムの適用方法の詳細については、「[Azure Stack に OEM (相手先ブランド供給) 更新プログラムを適用する](azure-stack-update-oem.md)」を参照してください。

- Azure Stack 診断ログ収集サービスの通信を有効にするための新しいアウトバウンド規則 (HTTPS) が追加されました。 詳細については、[Azure Stack データセンター統合 - エンドポイントの発行](azure-stack-integrate-endpoints.md#ports-and-urls-outbound)に関するページをご覧ください。

- 外部ストレージの場所の容量が不足している場合、インフラストラクチャ バックアップ サービスによって、部分的にアップロードされたバックアップが削除されるようになりました。

- インフラストラクチャのバックアップに、ドメイン サービス データのバックアップが含まれなくなりました。 これは、Azure Active Directory を ID プロバイダーとして使用するシステムにのみ適用されます。

- **[コンピューティング] > [VM イメージ]** ブレードに取り込まれたイメージが、ページ BLOB の種類であることを検証するようになりました。

- 特権エンドポイント コマンド **Set-BmcCredential** を使って、ベースボード管理コントローラーの資格情報を更新できるようになりました。

### <a name="fixes"></a>修正

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->
- Resource Manager テンプレートで、発行元、オファー、SKU が大文字と小文字を区別して扱われていた問題を修正しました (イメージ パラメーターが発行元、オファー、および SKU と同じ大文字または小文字でない場合、イメージがデプロイ用にフェッチされませんでした)。

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/129536438/home -->
- ストレージ サービス メタデータのバックアップ中のタイムアウトのため、**PartialSucceeded** エラー メッセージでバックアップが失敗する問題を修正しました。  

- ユーザー サブスクリプションを削除すると、リソースが孤立する問題を修正しました。

- オファーの作成時に説明フィールドが保存されなかった問題を修正しました。

- **読み取り専用**のアクセス許可を持つユーザーが、リソースの作成、編集、および削除を行うことができる問題を修正しました。 これで、**共同作成者**のアクセス許可が割り当てられている場合にのみ、ユーザーはリソースを作成できるようになりました。 

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/127772311/home -->
- WMI プロバイダー ホストによって DLL ファイルがロックされるために更新が失敗する問題を修正しました。

- 更新サービスで、更新タイルまたはリソース プロバイダーで、使用可能な更新が表示されない問題を修正しました。 この問題は 1906 で見つかり、修正プログラム [KB4511282](https://support.microsoft.com/help/4511282/) で修正されました。

- 不正な構成によって、管理プレーンが異常になるために、更新が失敗する可能性のある問題を修正しました。 この問題は 1906 で見つかり、修正プログラム [KB4512794](https://support.microsoft.com/help/4512794/) で修正されました。

- ユーザーがマーケットプレースからサードパーティのイメージのデプロイを実行できない問題を修正しました。 この問題は 1906 で見つかり、修正プログラム [KB4511259](https://support.microsoft.com/help/4511259/) で修正されました。

- ユーザー イメージ マネージャー サービスのクラッシュのため、管理対象イメージからの VM の作成が失敗する可能性がある問題を修正しました。 この問題は 1906 で見つかり、修正プログラム [KB4512794](https://support.microsoft.com/help/4512794/) で修正されました

- アプリ ゲートウェイ キャッシュが想定どおりに更新されないために VM CRUD 操作が失敗することがある問題を修正しました。 この問題は 1906 で見つかり、修正プログラム [KB4513119](https://support.microsoft.com/en-us/help/4513119/) で修正されました

- 管理者ポータルでリージョンおよびアラート ブレードの可用性に影響を与えていた正常性リソース プロバイダーの問題を修正しました。 この問題は 1906 で見つかり、修正プログラム [KB4512794](https://support.microsoft.com/help/4512794) で修正されました。

## <a name="security-updates"></a>セキュリティ更新プログラム

Azure Stack のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack security updates](release-notes-security-updates.md)」 (Azure Stack のセキュリティ更新プログラム) をご覧ください。

## <a name="update-planning"></a>計画の更新

更新プログラムを適用する前に、必ず次の情報を確認してください。

- [既知の問題](known-issues.md)
- [セキュリティ更新プログラム](release-notes-security-updates.md)
- [更新プログラム適用前後のアクティビティのチェックリスト](release-notes-checklist.md)

## <a name="download-the-update"></a>更新プログラムをダウンロードする

Azure Stack 1907 更新プログラム パッケージは、[Azure Stack ダウンロード ページ](https://aka.ms/azurestackupdatedownload)からダウンロードできます。

## <a name="hotfixes"></a>修正プログラム

Azure Stack では、修正プログラムが定期的にリリースされます。 Azure Stack を 1907 に更新する前に、必ず 1906 用の最新の Azure Stack 修正プログラムをインストールしてください。

Azure Stack 修正プログラムを適用できるのは Azure Stack 統合システムのみです。ASDK には修正プログラムをインストールしないでください。

### <a name="before-applying-the-1907-update"></a>1907 更新プログラムを適用する前

Azure Stack の 1907 リリースは、次の修正プログラムが適用された 1906 リリースに適用する必要があります。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack 修正プログラム 1.1906.15.60](https://support.microsoft.com/help/4524559)

### <a name="after-successfully-applying-the-1907-update"></a>1907 更新プログラムの適用に成功した後

この更新プログラムをインストールした後、適用可能な修正プログラムがあればインストールします。 詳細については、[サービス ポリシー](azure-stack-servicing-policy.md)に関する記事を参照してください。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack 修正プログラム 1.1907.18.56](https://support.microsoft.com/help/4528552)
::: moniker-end

::: moniker range="azs-1906"
## <a name="1906-build-reference"></a>1906 ビルドのリファレンス

Azure Stack 1906 更新プログラムのビルド番号は **1.1906.0.30** です。

### <a name="update-type"></a>更新の種類

Azure Stack 1906 更新プログラムのビルドの種類は**高速**です。 更新プログラムのビルドの種類については、「[Azure Stack での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。 1906 更新プログラムが完了するまでの予測所要時間は、ご使用の Azure Stack 環境内の物理ノード数に関係なく、約 10 時間です。 更新プログラムの正確なランタイムは一般的に、ご使用のシステムでテナント ワークロードによって使用されている容量、システム ネットワーク接続 (インターネットに接続されている場合)、およびシステム ハードウェアの仕様に左右されます。 ランタイムがこの予測値よりも長くなることは一般的ではなく、更新が失敗した場合を除き、Azure Stack オペレーターによるアクションは不要です。 このおおよその実行時間は、1906 更新プログラムに固有であり、他の Azure Stack 更新プログラムと比較することはできません。

## <a name="whats-in-this-update"></a>この更新プログラムの新機能

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

- すべてのエンドポイントで TLS 1.2 を強制するため、特権エンドポイント (PEP) に **Set-TLSPolicy** コマンドレットが追加されました。 詳しくは、[Azure Stack のセキュリティ コントロール](azure-stack-security-configuration.md)に関するページを参照してください。

- 適用されている TLS ポリシーを取得するため、特権エンドポイント (PEP) に **Get-TLSPolicy** コマンドレットが追加されました。 詳しくは、[Azure Stack のセキュリティ コントロール](azure-stack-security-configuration.md)に関するページを参照してください。

- システムの更新中に、必要に応じて、内部の TLS 証明書をローテーションする、内部シークレットのローテーション プロシージャが追加されました。

- 期限切れ間近のシークレットに関する重大なアラートが無視された場合に、内部シークレットのローテーションを強制することで、内部シークレットの有効期限切れを回避するための保護が追加されました。 これを通常の運用手順として依存しないでください。 シークレットのローテーションは、メンテナンス期間中に計画する必要があります。 詳しくは、[Azure Stack シークレットのローテーション](azure-stack-rotate-secrets.md)に関するページを参照してください。

- AD FS を使用した Azure Stack のデプロイで Visual Studio Code がサポートされるようになりました。

### <a name="improvements"></a>機能強化

<!-- Changes and product improvements with tangible customer-facing value. -->

- 特権エンドポイントの **Get-GraphApplication** コマンドレットで、現在使用されている証明書の拇印が表示されるようになりました。 これにより、AD FS で Azure Stack がデプロイされるときのサービス プリンシパルの証明書の管理が改善されます。

- AD Graph と AD FS の可用性を検証するため、アラートを生成する機能を含む、新しい正常性の監視ルールが追加されました。

- インフラストラクチャ バックアップ サービスを別のインスタンスに移動するときに、バックアップ リソース プロバイダーの信頼性が向上しました。

- メンテナンス期間のスケジュール設定を円滑にするため、均一な実行時間を提供する外部シークレットのローテーション プロシージャのパフォーマンスが最適化されました。

- **Test-AzureStack** コマンドレットで、有効期限が近づいている (重大なアラート) 内部シークレットについて報告されるようになりました。

- 特権エンドポイントの **Register-CustomAdfs** コマンドレットで、AD FS に対してフェデレーションの信頼を構成するときに、証明書失効リストのチェックをスキップできるようにする新しいパラメーターが使用できるようになりました。

- 1906 リリースでは、更新プログラムが一時停止していないことが確認できるように、更新プログラムの進行状況の可視性が向上しています。 これにより、 **[更新]** ブレードでオペレーターに表示される更新手順の合計数が増えました。 また、以前の更新プログラムよりも並列で行われる更新手順が増えています。

#### <a name="networking-updates"></a>ネットワークの更新

- DHCP レスポンダーに設定されているリース期間が、Azure と一致するように更新されました。

- リソースのデプロイに失敗したシナリオで、リソース プロバイダーへの再試行回数が改善されました。

- **Standard** SKU オプションは、現在サポートされていないため、ロード バランサーとパブリック IP の両方から削除されました。

### <a name="changes"></a>変更点

- ストレージ アカウントのエクスペリエンスの作成が、Azure と一致するようになりました。

- 内部シークレットの有効期限のアラート トリガーが変更されました。
  - 警告アラートが、シークレットの有効期限の 90 日前に表示されるようになりました。
  - 重大なアラートが、シークレットの有効期限の 30 日前に表示されるようになりました。

- 用語の一貫性のため、インフラストラクチャ バックアップ リソース プロバイダー内の文字列が更新されました。

### <a name="fixes"></a>修正

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

- **内部操作エラー**でマネージド ディスク VM のサイズ変更が失敗する問題を修正しました。

- ユーザー イメージの作成の失敗により、イメージを管理するサービスが不適切な状態になり、これにより失敗したイメージの削除と新しいイメージの作成がブロックされる問題を修正しました。 これは 1905 修正プログラムでも修正されます。

- 期限切れ間近の内部シークレットのアクティブなアラートが、内部シークレットのローテーションが正常に実行された後に自動的に終了されるようになりました。

- 更新プログラムが 99 時間を超えて実行されていた場合、[更新履歴] タブの更新期間の最初の桁がトリミングされる問題を修正しました。

- **[更新]** ブレードに、失敗した更新プログラムの **[再開]** オプションが含まれるようになりました。

- 管理者ポータルとユーザー ポータルで、検索で Docker 拡張機能が不正に返されるが、Azure Stack では使用できないため、それ以上の操作を実行できないマーケットプレースでの問題を修正しました。

- テンプレートの名前がアンダー スコア ('_') で始まる場合、テンプレートのデプロイ UI でパラメーターが設定されない問題を修正しました。

- 仮想マシン スケール セットの作成エクスペリエンスで、デプロイのオプションとして CentOS-based 7.2 が提供される問題を修正しました。 CentOS 7.2 は Azure Stack では利用できません。 Centos 7.5 をデプロイのオプションとして提供するようになりました

- **[仮想マシン スケール セット]** ブレードからスケール セットを削除できるようになりました。

## <a name="security-updates"></a>セキュリティ更新プログラム

Azure Stack のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack security updates](release-notes-security-updates.md)」 (Azure Stack のセキュリティ更新プログラム) をご覧ください。

## <a name="update-planning"></a>計画の更新

更新プログラムを適用する前に、必ず次の情報を確認してください。

- [既知の問題](known-issues.md)
- [セキュリティ更新プログラム](release-notes-security-updates.md)
- [更新プログラム適用前後のアクティビティのチェックリスト](release-notes-checklist.md)

## <a name="download-the-update"></a>更新プログラムをダウンロードする

Azure Stack 1906 更新プログラム パッケージは、[Azure Stack ダウンロード ページ](https://aka.ms/azurestackupdatedownload)からダウンロードできます。

## <a name="hotfixes"></a>修正プログラム

Azure Stack では、修正プログラムが定期的にリリースされます。 Azure Stack を 1906 に更新する前に、必ず 1905 用の最新の Azure Stack 修正プログラムをインストールしてください。 更新後、[1906 に対して利用可能な修正プログラム](#after-successfully-applying-the-1906-update)があればインストールします。

Azure Stack 修正プログラムを適用できるのは Azure Stack 統合システムのみです。ASDK には修正プログラムをインストールしないでください。

### <a name="before-applying-the-1906-update"></a>1906 更新プログラムを適用する前

Azure Stack の 1906 リリースは、次の修正プログラムが適用された 1905 リリースに適用する必要があります。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack 修正プログラム 1.1905.3.48](https://support.microsoft.com/help/4510078)

### <a name="after-successfully-applying-the-1906-update"></a>1906 更新プログラムの適用に成功した後

この更新プログラムをインストールした後、適用可能な修正プログラムがあればインストールします。 詳細については、[サービス ポリシー](azure-stack-servicing-policy.md)に関する記事を参照してください。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack 修正プログラム 1.1906.15.60](https://support.microsoft.com/help/4524559)
::: moniker-end

::: moniker range=">=azs-1906"
## <a name="automatic-update-notifications"></a>自動更新通知

インフラストラクチャ ネットワークからインターネットにアクセスできるシステムでは、オペレーター ポータルに "**利用可能な更新プログラムがあります**" というメッセージが表示されます。 インターネットにアクセスできないシステムでは、対応する .xml を含む .zip ファイルをダウンロードしてインポートできます。

> [!TIP]  
> 以下の *RSS* または *Atom* フィードに登録して、Azure Stack 修正プログラムの最新情報を入手してください。
>
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="archive"></a>アーカイブ

アーカイブされた、以前のバージョンのリリース ノートにアクセスするには、左側の目次の上部にあるバージョン セレクターのドロップダウンを使用して、表示するバージョンを選択します。

## <a name="next-steps"></a>次の手順

- Azure Stack での更新プログラム管理の概要については、「[Azure Stack での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。  
- Azure Stack に更新プログラムを適用する方法については、「[Azure Stack で更新を適用する](azure-stack-apply-updates.md)」を参照してください。
- Azure Stack 統合システムのサービス ポリシーについて、およびサポートを受けられる状態にシステムを維持するために必要な作業について確認するには、「[Azure Stack サービス ポリシー](azure-stack-servicing-policy.md)」を参照してください。  
- 特権エンドポイント (PEP) を使用して更新プログラムを監視および再開するには、「[特権エンドポイントを使用して Azure Stack での更新プログラムをモニターする](azure-stack-monitor-update.md)」をご覧ください。
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
[以前のバージョンの Azure Stack のリリース ノートは TechNet ギャラリー](https://aka.ms/azsarchivedrelnotes)でアクセスできます。 これらのアーカイブされたドキュメントは、参照のみを目的に提供されており、これらのバージョンのサポートを意味しているわけではありません。 Azure Stack のサポートについては、「[Azure Stack サービス ポリシー](azure-stack-servicing-policy.md)」をご覧ください。 さらにサポートが必要な場合は、Microsoft カスタマー サポート サービスにお問い合わせください。
::: moniker-end


