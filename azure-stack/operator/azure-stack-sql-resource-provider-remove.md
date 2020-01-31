---
title: SQL リソース プロバイダーの削除
titleSuffix: Azure Stack Hub
description: Azure Stack Hub のデプロイから SQL リソース プロバイダーを削除する方法について説明します。
author: mattbriggs
ms.topic: article
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 11/20/2018
ms.openlocfilehash: 5d8335e69c35c209e444d88f41ce76a3fae94bb0
ms.sourcegitcommit: fd5d217d3a8adeec2f04b74d4728e709a4a95790
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76882798"
---
# <a name="remove-the-sql-resource-provider"></a>SQL リソース プロバイダーの削除

SQL リソース プロバイダーを削除する前に、プロバイダーの依存関係をすべて削除する必要があります。 また、リソース プロバイダーのインストールに使用したデプロイ パッケージのコピーも必要になります。

> [!NOTE]
> リソース プロバイダーのインストーラーのダウンロード リンクは、[リソース プロバイダーを展開するための前提条件](./azure-stack-sql-resource-provider-deploy.md#prerequisites)に関するページにあります。

SQL リソース プロバイダーを削除しても、ホスティング サーバーからテナント データベースが削除されることはありません。

## <a name="dependency-cleanup"></a>依存関係のクリーンアップ

DeploySqlProvider.ps1 スクリプトを実行してリソース プロバイダーを削除する前に、いくつかのクリーンアップ タスクを実行する必要があります。

次のクリーンアップ タスクは、Azure Stack Hub オペレーターが担当します。

* SQL アダプターを参照しているすべてのプランを削除する。
* SQL アダプターに関連付けられているすべてのクォータを削除する。

## <a name="to-remove-the-sql-resource-provider"></a>SQL リソース プロバイダーを削除するには

1. 既存の SQL リソース プロバイダーの依存関係がすべて削除されていることを確認します。

   > [!NOTE]
   > 依存リソースがリソース プロバイダーを現在使用している場合でも、SQL リソース プロバイダーのアンインストールは続行されます。
  
2. SQL リソース プロバイダーのインストール パッケージのコピーを入手し、自己展開形式ファイルを実行してコンテンツを一時ディレクトリに展開します。

3. 新しい管理者特権の PowerShell コンソール ウィンドウを開き、SQL リソース プロバイダーのインストール ファイルを抽出したディレクトリに変更します。

4. 次のパラメーターを使用して、DeploySqlProvider.ps1 スクリプトを実行します。

    * **Uninstall**:リソース プロバイダーと関連付けられているすべてのリソースを削除します。
    * **PrivilegedEndpoint**:特権エンドポイントの IP アドレスまたは DNS 名。
    * **AzureEnvironment**:Azure Stack Hub のデプロイに使用する Azure 環境。 Azure AD のデプロイでのみ必須です。
    * **CloudAdminCredential**:特権エンドポイントへのアクセスに必要な、クラウド管理者の資格情報。
    * **AzCredential**:Azure Stack Hub サービス管理者アカウントの資格情報。 Azure Stack Hub のデプロイに使用したのと同じ資格情報を使用します。

## <a name="next-steps"></a>次のステップ

[App Services を PaaS として提供する](azure-stack-app-service-overview.md)
