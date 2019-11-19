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
ms.date: 11/08/2019
ms.author: sethm
ms.reviewer: prchint
ms.lastreviewed: 11/08/2019
ms.openlocfilehash: 6cbec7498c482b680beff1478b8eee7775d76703
ms.sourcegitcommit: ed44d477b9fd11573d1e0d1ed3a3c0ef4512df53
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2019
ms.locfileid: "73845874"
---
# <a name="azure-stack-updates-release-notes"></a>Azure Stack の更新プログラム: リリース ノート

*適用対象:Azure Stack 統合システム*

この記事では、Azure Stack 更新プログラム パッケージの内容について説明します。 この更新プログラムには、このリリースの Azure Stack に対する新機能、機能強化、および修正が含まれています。

別のバージョンのリリース ノートにアクセスするには、左側の目次の上部にあるバージョン セレクターのドロップダウンを使用します。

::: moniker range=">=azs-1905"
> [!IMPORTANT]  
> 更新プログラム パッケージは、Azure Stack 統合システム専用です。 Azure Stack Development Kit にこの更新プログラム パッケージは適用しないでください。
::: moniker-end
::: moniker range="<azs-1905"
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
::: moniker range="azs-1908"
## <a name="1908-build-reference"></a>1908 ビルドのリファレンス

Azure Stack 1908 更新プログラムのビルド番号は **1.1908.4.33** です。

### <a name="update-type-1908"></a>更新の種類

1908 では、Azure Stack が実行される基になるオペレーティング システムが Windows Server 2019 に更新されています。 これにより、核となる基本的な機能強化だけでなく、近い将来に Azure Stack に機能を追加する機能も使用可能になります。

Azure Stack 1908 更新プログラムのビルドの種類は**完全**です。 そのため、1908 更新プログラムは、1906 や 1907 のような高速更新プログラムよりもランタイムが長くなります。 完全な更新プログラムの正確なランタイムは、Azure Stack インスタンスに含まれているノード数、テナントのワークロードごとにシステムで使用される容量、システムのネットワーク接続 (インターネットに接続されている場合)、システムのハードウェア構成によって異なります。 内部テストでは、1908 更新プログラムのランタイムが次のように予測されました。4 ノード - 42 時間、8 ノード - 50 時間、12 ノード - 60 時間、16 ノード - 70 時間。 更新プログラムのランタイムがこの予測値よりも長くなることは一般的ではなく、更新が失敗した場合を除き、Azure Stack オペレーターによるアクションは不要です。

更新プログラムのビルドの種類については、「[Azure Stack での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。

- 更新プログラムの正確な実行時間は一般に、ご使用のシステムでテナント ワークロードによって使用されている容量、システム ネットワーク接続 (インターネットに接続されている場合)、およびシステム ハードウェアの構成に左右されます。
- 実行時間が予測よりも長くなることは一般的ではなく、更新が失敗した場合を除き、Azure Stack オペレーターによるアクションは不要です。
- このおおよその実行時間は、1908 更新プログラムに固有であり、他の Azure Stack 更新プログラムと比較することはできません。

<!-- ## What's in this update -->

<!-- The current theme (if any) of this release. -->

### <a name="whats-new-1908"></a>新機能

<!-- What's new, also net new experiences and features. -->

- 1908 では、Azure Stack が実行される基になるオペレーティング システムが Windows Server 2019 に更新されていることに注意してください。 これにより、核となる基本的な機能強化だけでなく、近い将来に Azure Stack に機能を追加する機能も使用可能になります。
- Azure Stack インフラストラクチャのすべてのコンポーネントが FIPS 140-2 モードで動作するようになりました。
- Azure Stack オペレーターは、ポータル ユーザー データを削除できるようになりました。 詳細については、「[Clear portal user data from Azure Stack](azure-stack-portal-clear.md)」 (Azure Stack からポータル ユーザー データをクリアする) を参照してください。

### <a name="improvements-1908"></a>機能強化

<!-- Changes and product improvements with tangible customer-facing value. -->
- 物理ノードのハードウェアのトラステッド プラットフォーム モジュール (TPM) にシークレットを保持するために、Azure Stack の保存データの暗号化が向上しました。

### <a name="changes-1908"></a>変更

- ハードウェア プロバイダーは Azure Stack バージョン 1908 と同時に OEM 拡張機能パッケージ 2.1 以降をリリースします。 Azure Stack バージョン 1908 には OEM 拡張機能パッケージ 2.1 以降が前提条件です。 OEM 拡張機能パッケージ 2.1 以降をダウンロードする方法の詳細については、システムのハードウェア プロバイダーに問い合わせてください。また、[OEM 更新プログラム](azure-stack-update-oem.md#oem-contact-information)の記事を参照してください。  

### <a name="fixes-1908"></a>修正

- 今後の Azure Stack OEM 更新プログラムとの互換性、およびユーザーのユーザー イメージを使用した VM デプロイに関する問題が修正されました。 この問題は 1907 で見つかり、修正プログラム [KB4517473](https://support.microsoft.com/en-us/help/4517473/azure-stack-hotfix-1-1907-12-44) で修正されました  
- OEM ファームウェア更新プログラムに関する問題が修正され、Fabric リングの正常性についての Test-AzureStack での誤診断に関する問題が修正されました。 この問題は 1907 で見つかり、修正プログラム [KB4515310](https://support.microsoft.com/en-us/help/4515310/azure-stack-hotfix-1-1907-7-35) で修正されました
- OEM ファームウェア更新プロセスに関する問題を修正しました。 この問題は 1907 で見つかり、修正プログラム [KB4515650](https://support.microsoft.com/en-us/help/4515650/azure-stack-hotfix-1-1907-8-37) で修正されました


<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

## <a name="security-updates-1908"></a>セキュリティ更新プログラム

Azure Stack のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack security updates](release-notes-security-updates.md)」 (Azure Stack のセキュリティ更新プログラム) をご覧ください。

## <a name="download-the-update-1908"></a>更新プログラムのダウンロード

Azure Stack 1908 更新プログラム パッケージは、[Azure Stack ダウンロード ページ](https://aka.ms/azurestackupdatedownload)からダウンロードできます。

## <a name="hotfixes-1908"></a>修正プログラム

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

### <a name="update-type-1907"></a>更新の種類

Azure Stack 1907 更新プログラムのビルドの種類は**高速**です。 更新プログラムのビルドの種類については、「[Azure Stack での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。 内部テストに基づいて、1907 更新プログラムが完了するまでの予測所要時間は約 13 時間です。

- 更新プログラムの正確な実行時間は一般に、ご使用のシステムでテナント ワークロードによって使用されている容量、システム ネットワーク接続 (インターネットに接続されている場合)、およびシステム ハードウェアの構成に左右されます。
- 実行時間が予測よりも長くなることは一般的ではなく、更新が失敗した場合を除き、Azure Stack オペレーターによるアクションは不要です。
- このおおよその実行時間は、1907 更新プログラムに固有であり、他の Azure Stack 更新プログラムと比較することはできません。

## <a name="whats-in-this-update-1907"></a>この更新プログラムの新機能

<!-- The current theme (if any) of this release. -->

### <a name="whats-new-1907"></a>新機能

<!-- What's new, also net new experiences and features. -->

- 診断ログの収集を容易にし、改善するための Azure Stack 診断ログ収集サービスの一般公開リリース。 Azure Stack 診断ログ収集サービスは、診断ログを収集して、Microsoft カスタマー サポート サービス (CSS) と共有するための簡単な方法を提供します。 この診断ログ収集サービスは、Azure Stack 管理者ポータルでの新しいユーザー エクスペリエンスを提供します。これにより、オペレーターは、特定の重要なアラートが発生したときに、ストレージ BLOB への診断ログの自動アップロードを設定したり、オンデマンドで同じ操作を実行したりすることができます。 詳細については、[診断ログの収集](azure-stack-diagnostic-log-collection-overview.md)に関する記事を参照してください。

- Azure Stack 検証ツール **Test-AzureStack** の一部としての Azure Stack ネットワーク インフラストラクチャ検証の一般公開リリース。 Azure Stack ネットワーク インフラストラクチャは、**Test-AzureStack** の一部になり、Azure Stack のネットワーク インフラストラクチャで障害が発生したかどうかを特定します。 テストでは、Azure Stack ソフトウェアで定義されたネットワークをバイパスすることによって、ネットワーク インフラストラクチャの接続が確認されます。 パブリック VIP から構成済みの DNS フォワーダー、NTP サーバー、および ID エンドポイントへの接続が示されます。 さらに、ID プロバイダーとして Azure AD を使用する場合に Azure への、または ADFS を使用する場合にフェデレーション サーバーへの接続が確認されます。 詳細については、[Azure Stack 検証ツール](azure-stack-diagnostic-test.md)に関する記事を参照してください。

- システムの更新中に、必要に応じて、内部の SQL TLS 証明書をローテーションする、内部シークレットのローテーション プロシージャが追加されました。

### <a name="improvements-1907"></a>機能強化

<!-- Changes and product improvements with tangible customer-facing value. -->

- Azure Stack の更新ブレードに、アクティブな更新の**最後のステップが完了した**時刻が表示されるようになりました。 これは、更新ブレードに移動し、実行中の更新をクリックすると表示されます。 **[Last Step Completed]\(最後のステップの完了\)** は、 **[Update run details]\(更新実行の詳細\)** セクションにあります。

- **Start-AzureStack** と **Stop-AzureStack** オペレーター アクションの改善。 Azure Stack を起動する時間が、平均で 50% 短縮されました。 Azure Stack をシャットダウンする時間が、平均で 30% 短縮されました。 平均の起動とシャットダウンの時間は、スケールユニットのノード数が増加しても変わりません。

- 切断された Marketplace ツールのエラー処理が改善されました。 **Export-AzSOfflineMarketplaceItem** を使用した場合に、ダウンロードが失敗するか、部分的に成功した場合、エラーと軽減手順 (存在する場合) に関する詳細を示す詳細なエラーメッセージが表示されます。

- 大きなページ BLOB/スナップショットからのマネージド ディスク作成のパフォーマンスが向上しました。 以前は、大容量ディスクの作成時にタイムアウトが発生しました。  

<!-- https://icm.ad.msft.net/imp/v3/incidents/details/127669774/home -->
- 予期しない仮想ディスクのデタッチを避けるために、ノードをシャットダウンする前の仮想ディスクの正常性チェックが改善されました。

- 管理者操作のための内部ログのストレージが改善されました。 その結果、内部ログ プロセスのメモリとストレージの消費量を最小限に抑えることで、管理者の操作中にパフォーマンスと信頼性が向上します。 また、管理者ポータルの更新ブレードのページ読み込み時間が短縮される可能性もあります。 この改善の一環として、6 か月を経過した更新ログはシステムで使用できなくなります。 これらの更新のログが必要な場合は、1907 更新を実行する前に、6 か月より前のすべての更新の実行の[概要をダウンロード](azure-stack-apply-updates.md)してください。

### <a name="changes-1907"></a>変更

- Azure Stack バージョン 1907 には、バージョン 1908 に更新する前に、システムの OEM パッケージをバージョン 2.1 以降に更新するようにオペレーターに指示する警告アラートが含まれています。 Azure Stack の OEM 更新プログラムの適用方法の詳細については、「[Azure Stack に OEM (相手先ブランド供給) 更新プログラムを適用する](azure-stack-update-oem.md)」を参照してください。

- Azure Stack 診断ログ収集サービスの通信を有効にするための新しいアウトバウンド規則 (HTTPS) が追加されました。 詳細については、[Azure Stack データセンター統合 - エンドポイントの発行](azure-stack-integrate-endpoints.md#ports-and-urls-outbound)に関するページをご覧ください。

- 外部ストレージの場所の容量が不足している場合、インフラストラクチャ バックアップ サービスによって、部分的にアップロードされたバックアップが削除されるようになりました。

- インフラストラクチャのバックアップに、ドメイン サービス データのバックアップが含まれなくなりました。 これは、Azure Active Directory を ID プロバイダーとして使用するシステムにのみ適用されます。

- **[コンピューティング] > [VM イメージ]** ブレードに取り込まれたイメージが、ページ BLOB の種類であることを検証するようになりました。

### <a name="fixes-1907"></a>修正

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

## <a name="security-updates-1907"></a>セキュリティ更新プログラム

Azure Stack のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack security updates](release-notes-security-updates.md)」 (Azure Stack のセキュリティ更新プログラム) をご覧ください。

## <a name="update-planning-1907"></a>更新プログラムの計画

更新プログラムを適用する前に、必ず次の情報を確認してください。

- [既知の問題](known-issues.md)
- [セキュリティ更新プログラム](release-notes-security-updates.md)
- [更新プログラム適用前後のアクティビティのチェックリスト](release-notes-checklist.md)

## <a name="download-the-update-1907"></a>更新プログラムのダウンロード

Azure Stack 1907 更新プログラム パッケージは、[Azure Stack ダウンロード ページ](https://aka.ms/azurestackupdatedownload)からダウンロードできます。

## <a name="hotfixes-1907"></a>修正プログラム

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

### <a name="update-type-1906"></a>更新の種類

Azure Stack 1906 更新プログラムのビルドの種類は**高速**です。 更新プログラムのビルドの種類については、「[Azure Stack での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。 1906 更新プログラムが完了するまでの予測所要時間は、ご使用の Azure Stack 環境内の物理ノード数に関係なく、約 10 時間です。 更新プログラムの正確なランタイムは一般的に、ご使用のシステムでテナント ワークロードによって使用されている容量、システム ネットワーク接続 (インターネットに接続されている場合)、およびシステム ハードウェアの仕様に左右されます。 ランタイムがこの予測値よりも長くなることは一般的ではなく、更新が失敗した場合を除き、Azure Stack オペレーターによるアクションは不要です。 このおおよその実行時間は、1906 更新プログラムに固有であり、他の Azure Stack 更新プログラムと比較することはできません。

## <a name="whats-in-this-update-1906"></a>この更新プログラムの新機能

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

- すべてのエンドポイントで TLS 1.2 を強制するため、特権エンドポイント (PEP) に **Set-TLSPolicy** コマンドレットが追加されました。 詳しくは、[Azure Stack のセキュリティ コントロール](azure-stack-security-configuration.md)に関するページを参照してください。

- 適用されている TLS ポリシーを取得するため、特権エンドポイント (PEP) に **Get-TLSPolicy** コマンドレットが追加されました。 詳しくは、[Azure Stack のセキュリティ コントロール](azure-stack-security-configuration.md)に関するページを参照してください。

- システムの更新中に、必要に応じて、内部の TLS 証明書をローテーションする、内部シークレットのローテーション プロシージャが追加されました。

- 期限切れ間近のシークレットに関する重大なアラートが無視された場合に、内部シークレットのローテーションを強制することで、内部シークレットの有効期限切れを回避するための保護が追加されました。 これを通常の運用手順として依存しないでください。 シークレットのローテーションは、メンテナンス期間中に計画する必要があります。 詳しくは、[Azure Stack シークレットのローテーション](azure-stack-rotate-secrets.md)に関するページを参照してください。

- AD FS を使用した Azure Stack のデプロイで Visual Studio Code がサポートされるようになりました。

### <a name="improvements-1906"></a>機能強化

<!-- Changes and product improvements with tangible customer-facing value. -->

- 特権エンドポイントの **Get-GraphApplication** コマンドレットで、現在使用されている証明書の拇印が表示されるようになりました。 これにより、AD FS で Azure Stack がデプロイされるときのサービス プリンシパルの証明書の管理が改善されます。

- AD Graph と AD FS の可用性を検証するため、アラートを生成する機能を含む、新しい正常性の監視ルールが追加されました。

- インフラストラクチャ バックアップ サービスを別のインスタンスに移動するときに、バックアップ リソース プロバイダーの信頼性が向上しました。

- メンテナンス期間のスケジュール設定を円滑にするため、均一な実行時間を提供する外部シークレットのローテーション プロシージャのパフォーマンスが最適化されました。

- **Test-AzureStack** コマンドレットで、有効期限が近づいている (重大なアラート) 内部シークレットについて報告されるようになりました。

- 特権エンドポイントの **Register-CustomAdfs** コマンドレットで、AD FS に対してフェデレーションの信頼を構成するときに、証明書失効リストのチェックをスキップできるようにする新しいパラメーターが使用できるようになりました。

- 1906 リリースでは、更新プログラムが一時停止していないことが確認できるように、更新プログラムの進行状況の可視性が向上しています。 これにより、 **[更新]** ブレードでオペレーターに表示される更新手順の合計数が増えました。 また、以前の更新プログラムよりも並列で行われる更新手順が増えています。

#### <a name="networking-updates-1906"></a>ネットワークの更新

- DHCP レスポンダーに設定されているリース期間が、Azure と一致するように更新されました。

- リソースのデプロイに失敗したシナリオで、リソース プロバイダーへの再試行回数が改善されました。

- **Standard** SKU オプションは、現在サポートされていないため、ロード バランサーとパブリック IP の両方から削除されました。

### <a name="changes-1906"></a>変更

- ストレージ アカウントのエクスペリエンスの作成が、Azure と一致するようになりました。

- 内部シークレットの有効期限のアラート トリガーが変更されました。
  - 警告アラートが、シークレットの有効期限の 90 日前に表示されるようになりました。
  - 重大なアラートが、シークレットの有効期限の 30 日前に表示されるようになりました。

- 用語の一貫性のため、インフラストラクチャ バックアップ リソース プロバイダー内の文字列が更新されました。

### <a name="fixes-1906"></a>修正

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

## <a name="security-updates-1906"></a>セキュリティ更新プログラム

Azure Stack のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack security updates](release-notes-security-updates.md)」 (Azure Stack のセキュリティ更新プログラム) をご覧ください。

## <a name="update-planning-1906"></a>更新プログラムの計画

更新プログラムを適用する前に、必ず次の情報を確認してください。

- [既知の問題](known-issues.md)
- [セキュリティ更新プログラム](release-notes-security-updates.md)
- [更新プログラム適用前後のアクティビティのチェックリスト](release-notes-checklist.md)

## <a name="download-the-update-1906"></a>更新プログラムのダウンロード

Azure Stack 1906 更新プログラム パッケージは、[Azure Stack ダウンロード ページ](https://aka.ms/azurestackupdatedownload)からダウンロードできます。

## <a name="hotfixes-1906"></a>修正プログラム

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

::: moniker range="azs-1905"
## <a name="1905-build-reference"></a>1905 ビルドのリファレンス

Azure Stack 1905 更新プログラムのビルド番号は **1.1905.0.40** です。

### <a name="update-type-1905"></a>更新の種類

Azure Stack 1905 更新プログラムのビルドの種類は**完全**です。 そのため、1905 更新プログラムは、1903 や 1904 のような高速更新プログラムよりもランタイムが長くなります。 完全な更新プログラムの正確なランタイムは、Azure Stack インスタンスに含まれているノード数、テナントのワークロードごとにシステムで使用される容量、システムのネットワーク接続 (インターネットに接続されている場合)、システムのハードウェア構成によって異なります。 内部テストでは、1905 更新プログラムのランタイムが次のように予測されました。4 ノード - 35 時間、8 ノード - 45 時間、12 ノード - 55 時間、16 ノード - 70 時間。 1905 のランタイムがこの予測値よりも長くなることは一般的ではなく、更新が失敗した場合を除き、Azure Stack オペレーターによるアクションは不要です。 更新プログラムのビルドの種類については、「[Azure Stack での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。

## <a name="whats-in-this-update-1905"></a>この更新プログラムの新機能

<!-- The current theme (if any) of this release. -->

<!-- What's new, also net new experiences and features. -->

- この更新プログラムでは、Azure Stack の更新エンジンはスケール ユニット ノードのファームウェアを更新できます。 これには、ハードウェア パートナーからの準拠している更新プログラム パッケージが必要です。 使用できるかどうか詳しくは、ハードウェア パートナーに問い合わせてください。

- Windows Server 2019 がサポートされるようになっており、Azure Stack Marketplace を通した配信に利用できます。
この更新プログラムでは、2016 ホスト上で Windows Server 2019 を正常にアクティブにできるようになりました。

- 新しい [Azure アカウント Visual Studio Code 拡張機能](../user/azure-stack-dev-start-vscode-azure.md)を使うと、開発者はサブスクリプションにログインして表示することで、Azure Stack や他の多数のサービスをターゲットにできます。 Azure アカウント拡張機能は、Azure Active Directory (Azure AD) 環境と AD FS 環境の両方で動作し、Visual Studio Code ユーザー設定で必要な変更はわずかです。 Visual Studio Code には、この環境で実行するために、サービス プリンシパルにアクセス許可を付与する必要があります。 これを行うには、ID スクリプトをインポートし、[Azure Stack のマルチテナント](../operator/azure-stack-enable-multitenancy.md)に指定されているコマンドレットを実行します。 これには、ホーム ディレクトリの更新と、各ディレクトリのゲスト テナント ディレクトリの登録が必要です。 1905 以降に更新後、Visual Studio Code サービス プリンシパルが含まれているホーム ディレクトリ テナントを更新するように、アラートが表示されます。 

### <a name="improvements-1905"></a>機能強化

<!-- Changes and product improvements with tangible customer-facing value. -->
- Azure Stack で TLS 1.2 を適用する一部として、次の拡張機能がこれらのバージョンに更新されています。

  - microsoft.customscriptextension-arm-1.9.3
  - microsoft.iaasdiagnostics-1.12.2.2
  - microsoft.antimalware-windows-arm-1.5.5.9
  - microsoft.dsc-arm-2.77.0.0
  - microsoft.vmaccessforlinux-1.5.2

  これらのバージョンの拡張機能をすぐにダウンロードして、将来のリリースで TLS 1.2 が適用されるときに、拡張機能の新しいデプロイが失敗しないようにしてください。 常に **autoUpgradeMinorVersion=true** を設定し、拡張機能に対するマイナー バージョンの更新プログラム (たとえば、1.8 から 1.9) が自動的に実行されるようにします。

- Azure Stack ポータルの新しい**ヘルプとサポートの概要**により、オペレーターは簡単にサポート オプションを確認したり、専門家の支援を得たり、Azure Stack の詳細について学習したりできます。 統合システムでは、サポート リクエストを作成するときは Azure Stack サービスが事前に選択されます。 グローバルな Azure portal を使うのではなく、このエクスペリエンスを使ってチケットを送信することをお客様に強くお勧めします。 詳しくは、「[Azure Stack Help and Support](azure-stack-help-and-support-overview.md)」(Azure Stack のヘルプとサポート) をご覧ください。

- 複数の Azure Active Directory を ([このプロセス](azure-stack-enable-multitenancy.md)で) オンボードすると、特定の更新プログラムが発生したとき、または Azure AD サービス プリンシパルの認可に対する変更によって権限がなくなったときに、スクリプトの再実行が無視されることがあります。 これにより、特定の機能に対するアクセスのブロックから、元の問題まで追跡することが困難なさらに個別の障害まで、さまざまな問題が発生する可能性があります。 これを防ぐため、1905 では、これらのアクセス許可をチェックし、特定の構成の問題が見つかったときはアラートを作成する、新しい機能が導入されています。 この検証は 1 時間ごとに実行され、問題を解決するために必要な修復アクションが表示されます。 すべてのテナントが正常な状態になると、アラートは閉じます。

- サービスのフェールオーバーの間の、インフラストラクチャのバックアップ操作の信頼性の向上。

- 認証に [Azure Active Directory 認証ライブラリ](/azure/active-directory/develop/active-directory-authentication-libraries) (ADAL) を使用する、新しいバージョンの [Azure Stack Nagios プラグイン](azure-stack-integrate-monitor.md#integrate-with-nagios)を利用できます。 プラグインでは、Azure AD と Active Directory フェデレーション サービス (ADFS) での Azure Stack のデプロイもサポートされるようになっています。 詳しくは、[Nagios プラグインの交換](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)に関するサイトをご覧ください。

- Azure Stack のすべての最新機能がサポートされている新しいハイブリッド プロファイル **2019-03-01-Hybrid** がリリースされました。 **2019-03-01-Hybrid** プロファイルは、Azure PowerShell と Azure CLI の両方でサポートされています。 .NET、Ruby、Node.js、Go、Python の SDK で、**2019-03-01-Hybrid** プロファイルをサポートするパッケージが公開されています。 その変更を反映するように、それぞれのドキュメントといくつかのサンプルが更新されています。

- [Node.js SDK](https://www.npmjs.com/search?q=2019-03-01-hybrid) では、API プロファイルがサポートされるようになっています。 **2019-03-01-Hybrid** プロファイルをサポートするパッケージが、公開されます。

- 1905 Azure Stack 更新プログラムでは、プラットフォームの信頼性とサポート性を向上させるために次の 2 つの新しいインフラストラクチャ ロールを追加しています。

  - **インフラストラクチャ リング**: 現在、xrp などは独自に指定したインフラストラクチャ VM が必要ですが、今後はインフラストラクチャ リングがこのような既存のインフラストラクチャ ロールのコンテナー化されたバージョンをホストします。 これにより、プラットフォームの信頼性が向上し、Azure Stack で必要とするインフラストラクチャ VM の数が少なくなります。 これにより、Azure Stack のインフラストラクチャ ロールの全体的なリソース消費が今後大幅に削減されます。
  - **サポート リング**: 今後は、お客様向けに強化されたサポートのシナリオを処理するためにサポート リングが使用されます。  

  さらに、このロールの可用性向上のため、ドメイン コントローラー VM にインスタンスを追加しました。

  この変更により、Azure Stack インフラストラクチャのリソースの消費量が次のように増加します。
  
    | Azure Stack SKU | コンピューティング消費量の増加 | メモリ消費量の増加 |
    | -- | -- | -- |
    |4 ノード|22 vCPU|28 GB|
    |8 ノード|38 vCPU|44 GB|
    |12 ノード|54 vCPU|60 GB|
    |16 ノード|70 vCPU|76 GB|
  
### <a name="changes-1905"></a>変更

- 計画的および非計画的なメンテナンス シナリオでの信頼性と可用性を向上させるため、Azure Stack では、ドメイン サービス用に新しいインフラストラクチャ ロール インスタンスが追加されています。

- この更新プログラムでは、修復および追加のノード操作の間に、ハードウェアが検証されて、スケール ユニット内のスケール ユニット ノードが同種であることが確認されます。

- スケジュールされたバックアップが完了できず、定義されている保持期間を超えた場合、インフラストラクチャ バックアップ コントローラーにより、少なくとも 1 つの成功したバックアップが保持されていることが確認されます。 

### <a name="fixes-1905"></a>修正

<!-- Product fixes that came up from customer deployments worth highlighting, especially if there is an SR/ICM associated to it. -->

- スケール ユニット内のノードの再起動後に**コンピューティング ホスト エージェント**の警告が表示される問題が修正されました。

- フィルター適用時に正しくない結果が表示され、発行元フィルターで発行元の名前が重複して表示されるという、管理者ポータルでのマーケットプレース管理の問題が修正されました。 また、パフォーマンスが強化されて、結果の表示が速くなりました。

- 外部ストレージの場所へのアップロードが完了する前に、新しい利用可能なバックアップが一覧表示される、利用可能バックアップ ブレードでの問題が修正されました。 利用可能なバックアップは、ストレージの場所に正常にアップロードされた後で、一覧に表示されるようになります。 

<!-- ICM: 114819337; Task: 4408136 -->
- バックアップ操作中の回復キーの取得に関する問題が修正されました。 

<!-- Bug: 4525587 -->
- オペレーター ポータルでバージョンが "未定義" と表示される、OEM 更新プログラムでの問題が修正されました。

### <a name="security-updates-1905"></a>セキュリティ更新プログラム

Azure Stack のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack security updates](release-notes-security-updates.md)」 (Azure Stack のセキュリティ更新プログラム) をご覧ください。

## <a name="update-planning-1905"></a>更新プログラムの計画

更新プログラムを適用する前に、必ず次の情報を確認してください。

- [既知の問題](known-issues.md)
- [セキュリティ更新プログラム](release-notes-security-updates.md)
- [更新プログラム適用前後のアクティビティのチェックリスト](release-notes-checklist.md)

## <a name="download-the-update-1905"></a>更新プログラムのダウンロード

Azure Stack 1905 更新プログラム パッケージは、[Azure Stack ダウンロード ページ](https://aka.ms/azurestackupdatedownload)からダウンロードできます。 ダウンローダー ツールを使用する場合は、ダウンロード ディレクトリからキャッシュされたコピーではなく最新バージョンを使用してください。

## <a name="hotfixes-1905"></a>修正プログラム

Azure Stack では、修正プログラムが定期的にリリースされます。 Azure Stack を 1905 に更新する前に、必ず 1904 用の最新の Azure Stack 修正プログラムをインストールしてください。

Azure Stack 修正プログラムを適用できるのは Azure Stack 統合システムのみです。ASDK には修正プログラムをインストールしないでください。

### <a name="before-applying-the-1905-update"></a>1905 更新プログラムを適用する前

Azure Stack の 1905 リリースは、次の修正プログラムが適用された 1904 リリースに適用する必要があります。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack 修正プログラム 1.1904.4.45](https://support.microsoft.com/help/4505688)

### <a name="after-successfully-applying-the-1905-update"></a>1905 更新プログラムの適用に成功した後

この更新プログラムをインストールした後、適用可能な修正プログラムがあればインストールします。 詳細については、[サービス ポリシー](azure-stack-servicing-policy.md)に関する記事を参照してください。

<!-- One of these. Either no updates at all, nothing is required, or the LATEST hotfix that is required-->
- [Azure Stack 修正プログラム 1.1905.3.48](https://support.microsoft.com/help/4510078)
::: moniker-end

::: moniker range=">=azs-1905"
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

::: moniker range="<azs-1905"
[以前のバージョンの Azure Stack のリリース ノートは TechNet ギャラリー](https://aka.ms/azsarchivedrelnotes)でアクセスできます。 これらのアーカイブされたドキュメントは、参照のみを目的に提供されており、これらのバージョンのサポートを意味しているわけではありません。 Azure Stack のサポートについては、「[Azure Stack サービス ポリシー](azure-stack-servicing-policy.md)」をご覧ください。 さらにサポートが必要な場合は、Microsoft カスタマー サポート サービスにお問い合わせください。
::: moniker-end


