---
title: Azure Stack 1906 リリース ノート | Microsoft Docs
description: 新機能、既知の問題、更新プログラムをダウンロードする場所など、Azure Stack 統合システムの 1906 更新プログラムについて説明します。
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
ms.date: 07/15/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 07/15/2019
ms.openlocfilehash: 5330136bd1d4950ace26fa3f232be039c90ab144
ms.sourcegitcommit: 12aef286fc14ab27ed737a17e2761c38f054b9b3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/15/2019
ms.locfileid: "67885349"
---
# <a name="azure-stack-1906-update"></a>Azure Stack 1906 更新プログラム

*適用対象:Azure Stack 統合システム*

この記事では、1906 更新プログラム パッケージの内容について説明します。 この更新プログラムには、このリリースの Azure Stack に対する新機能、機能強化、および修正が含まれています。

> [!IMPORTANT]  
> 更新プログラム パッケージは、Azure Stack 統合システム専用です。 Azure Stack Development Kit にこの更新プログラム パッケージは適用しないでください。

## <a name="build-reference"></a>ビルドのリファレンス

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

## <a name="security-updates"></a>セキュリティ更新プログラム

Azure Stack のこの更新でのセキュリティ更新プログラムについては、「[Azure Stack security updates](azure-stack-release-notes-security-updates-1906.md)」 (Azure Stack のセキュリティ更新プログラム) をご覧ください。

## <a name="update-planning"></a>計画の更新

更新プログラムを適用する前に、必ず次の情報を確認してください。

- [既知の問題](azure-stack-release-notes-known-issues-1906.md)
- [セキュリティ更新プログラム](azure-stack-release-notes-security-updates-1906.md)
- [更新プログラム適用前後のアクティビティのチェックリスト](azure-stack-release-notes-checklist.md)

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
- [Azure Stack 修正プログラム 1.1906.11.52](https://support.microsoft.com/help/4513119)

## <a name="automatic-update-notifications"></a>自動更新通知

インフラストラクチャ ネットワークからインターネットにアクセスできるシステムをご利用の場合は、オペレーター ポータルに "**利用可能な更新プログラムがあります**" というメッセージが表示されます。 インターネットにアクセスできないシステムでは、対応する .xml を含む .zip ファイルをダウンロードしてインポートできます。

> [!TIP]  
> 以下の *RSS* または *Atom* フィードに登録して、Azure Stack 修正プログラムの最新情報を入手してください。
>
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

## <a name="archived-release-notes"></a>アーカイブされたリリース ノート

[以前のバージョンの Azure Stack のリリース ノートは TechNet ギャラリー](https://aka.ms/azsarchivedrelnotes)で確認できます。 これらのアーカイブされたリリース ノートは参照のみを目的に提供されており、これらのバージョンのサポートを意味しているわけではありません。 Azure Stack のサポートについては、「[Azure Stack サービス ポリシー](azure-stack-servicing-policy.md)」をご覧ください。 さらにサポートが必要な場合は、Microsoft カスタマー サポート サービスにお問い合わせください。

## <a name="next-steps"></a>次の手順

- Azure Stack での更新プログラム管理の概要については、「[Azure Stack での更新プログラムの管理概要](azure-stack-updates.md)」を参照してください。  
- Azure Stack に更新プログラムを適用する方法については、「[Azure Stack で更新を適用する](azure-stack-apply-updates.md)」を参照してください。
- Azure Stack 統合システムのサービス ポリシーについて、およびサポートを受けられる状態にシステムを維持するために必要な作業について確認するには、「[Azure Stack サービス ポリシー](azure-stack-servicing-policy.md)」を参照してください。  
- 特権エンドポイント (PEP) を使用して更新プログラムを監視および再開するには、「[特権エンドポイントを使用して Azure Stack での更新プログラムをモニターする](azure-stack-monitor-update.md)」をご覧ください。  
