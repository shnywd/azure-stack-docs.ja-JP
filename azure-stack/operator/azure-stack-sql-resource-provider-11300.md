---
title: Azure Stack Hub SQL リソース プロバイダー 1.1.30.0 のリリース ノート
titleSuffix: Azure Stack Hub
description: Azure Stack Hub SQL リソース プロバイダー 1.1.30.0 更新プログラムのリリース ノートを確認します。
author: mattbriggs
ms.topic: article
ms.date: 1/22/2020
ms.author: mabrigg
ms.reviewer: jiahan
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: 07abcb71715c3078887e00c89f5eac53f5f98def
ms.sourcegitcommit: fd5d217d3a8adeec2f04b74d4728e709a4a95790
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76882864"
---
# <a name="sql-resource-provider-11300-release-notes"></a>SQL リソース プロバイダー 1.1.30.0 のリリース ノート

これらのリリース ノートでは、SQL リソース プロバイダー バージョン 1.1.30.0 の機能強化と既知の問題について説明します。

## <a name="build-reference"></a>ビルドのリファレンス
SQL リソース プロバイダー バイナリをダウンロードした後、自己展開ツールを実行してコンテンツを一時ディレクトリに抽出します。 リソース プロバイダーには、対応する最低限の Azure Stack Hub のビルドがあります。 SQL リソース プロバイダーのこのバージョンをインストールするために必要な Azure Stack Hub の最小リリース バージョンを次に示します。

> |Azure Stack Hub の最小バージョン|SQL リソース プロバイダーのバージョン|
> |-----|-----|
> |バージョン 1808 (1.1808.0.97)|[1.1.30.0](https://aka.ms/azurestacksqlrp11300)|
> |     |     |

> [!IMPORTANT]
> SQL リソース プロバイダーの最新バージョンをデプロイする前に、サポートされている最低限の Azure Stack Hub 更新プログラムを Azure Stack Hub 統合システムに適用します。

## <a name="new-features-and-fixes"></a>新機能と修正
このバージョンの Azure Stack Hub SQL リソース プロバイダーには、次の機能強化と修正プログラムが含まれています。

- **SQL リソース プロバイダーのデプロイに対して有効になっているテレメトリ**。 SQL リソース プロバイダーのデプロイに対してテレメトリ収集が有効になっています。 収集されるテレメトリには、リソース プロバイダーのデプロイ、開始時刻と終了時刻、終了状態、終了メッセージ、エラーの詳細 (該当する場合) が含まれています。

- **TLS 1.2 暗号化の更新**。 リソース プロバイダーと Azure Stack Hub の内部コンポーネントとの通信には、TLS 1.2 のみへのサポートが有効になっています。 

### <a name="fixes"></a>修正

- **SQL リソース プロバイダーの Azure Stack Hub PowerShell との互換性**。 SQL リソース プロバイダーは、Azure Stack Hub 2018-03-01-hybrid PowerShell プロファイルを使用でき、AzureRM 1.3.0 以降と互換性があるように更新されています。

- **SQL ログインの [パスワードの変更] ブレード**。 [パスワードの変更] ブレードでパスワードを変更できないという問題が修正されました。 パスワード変更通知からリンクを削除しました。

- **SQL ホスティング サーバーの [設定] ブレードの更新**。 [設定] ブレードのタイトルが間違って "パスワード" になっている問題を修正しました。

## <a name="known-issues"></a>既知の問題

- **SQL SKU はポータルに表示されるまで最大 1 時間かかることがあります**。 新しい SQL データベースを作成するときに、新規に作成される SKU が表示されて使用できるようになるまで、最大 1 時間かかることがあります。

    **回避策**:[なし] :

- **再利用された SQL ログイン**。 同じサブスクリプションの既存のログインと同じユーザー名で新しい SQL ログインを作成しようとすると、同じログインと既存のパスワードが再利用されます。

    **回避策**:同じサブスクリプションに新しいログインを作成するときに別のユーザー名を使用するか、同じユーザー名のログインを異なるサブスクリプションに作成します。

- **データの不整合の原因となる共有 SQL ログイン**。 SQL ログインが同じサブスクリプション下で複数の SQL データベースに対して共有されている場合、ログイン パスワードを変更するとデータの不整合が発生します。

    **回避策**:同じサブスクリプション下の異なるデータベースには常に異なるログインを使用します。

- **TLS 1.2 サポート要件**。 TLS 1.2 が有効になっていないコンピューターから SQL リソース プロバイダーをデプロイまたは更新しようとすると、操作が失敗する可能性があります。 リソース プロバイダーのデプロイまたは更新に使用するコンピューターで、次の PowerShell コマンドを実行して、TLS 1.2 がサポート対象であるかどうかを確認します。

  ```powershell
  [System.Net.ServicePointManager]::SecurityProtocol
  ```

  **Tls12** がコマンドの出力に含まれていない場合、TLS 1.2 はそのコンピューターでは有効ではありません。

    **回避策**:次の PowerShell コマンドを実行して TLS 1.2 を有効にし、同じ PowerShell セッションからリソース プロバイダーのデプロイを開始するかまたはスクリプトを更新します。

    ```powershell
    [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12
    ```

- **SQL リソース プロバイダーが SQL Server Always On リスナーの追加に失敗します**。 SQL リソース プロバイダー VM は、SQL Server Always On リスナーのリスナー IP アドレスを使用するとき、リスナーのホスト名を解決できません。

    **回避策**:リスナーのホスト名に対するリスナー IP を解決するには、DNS が正しく動作していることを確認します。

### <a name="known-issues-for-cloud-admins-operating-azure-stack-hub"></a>Azure Stack Hub を運用するクラウド管理者に関する既知の問題
[Azure Stack Hub リリース ノート](azure-stack-servicing-policy.md)内のドキュメントを参照してください。

## <a name="next-steps"></a>次のステップ
[SQL リソース プロバイダーの詳細を確認します](azure-stack-sql-resource-provider.md)。

[SQL リソースプロバイダーのデプロイを準備します](azure-stack-sql-resource-provider-deploy.md#prerequisites)。

[SQL リソース プロバイダーを以前のバージョンからアップグレードします](azure-stack-sql-resource-provider-update.md)。