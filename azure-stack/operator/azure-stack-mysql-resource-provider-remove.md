---
title: Azure Stack Hub 上の MySQL リソース プロバイダーを削除する
description: Azure Stack Hub のデプロイから MySQL リソース プロバイダーを削除する方法について説明します。
author: bryanla
ms.topic: article
ms.date: 1/22/2020
ms.author: bryanla
ms.reviewer: xiaofmao
ms.lastreviewed: 11/20/2
ms.openlocfilehash: 60f8ab926bb868517c7b9feeb1c5c4325212607c
ms.sourcegitcommit: 1969a13d378cc3b8c58fea11dca8cf8c06136acd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2020
ms.locfileid: "80302817"
---
# <a name="remove-the-mysql-resource-provider-in-azure-stack-hub"></a>Azure Stack Hub 上の MySQL リソース プロバイダーを削除する

MySQL リソース プロバイダーを削除する前に、プロバイダーの依存関係をすべて削除する必要があります。 また、リソース プロバイダーのインストールに使用したデプロイ パッケージのコピーも必要になります。

> [!NOTE]
> リソース プロバイダーのインストーラーのダウンロード リンクは、[リソース プロバイダーを展開するための前提条件](./azure-stack-mysql-resource-provider-deploy.md#prerequisites)に関するページにあります。

MySQL リソース プロバイダーを削除すると、オペレーターが管理する関連プランとクォータが削除されます。 ただし、ホスティング サーバーからテナント データベースは削除されません。

## <a name="to-remove-the-mysql-resource-provider"></a>MySQL リソースプロバイダーを削除するには

1. 既存の MySQL リソース プロバイダーの依存関係がすべて削除されていることを確認します。

   > [!NOTE]
   > 依存リソースがリソース プロバイダーを現在使用している場合でも、MySQL リソース プロバイダーのアンインストールは続行されます。
  
2. MySQL リソース プロバイダーのインストール パッケージのコピーを入手し、自己展開形式ファイルを実行してコンテンツを一時ディレクトリに展開します。
3. 新しい管理者特権の PowerShell コンソール ウィンドウを開き、MySQL リソース プロバイダーのインストール ファイルを抽出したディレクトリに変更します。
4. 次のパラメーターを使用して、DeployMySqlProvider.ps1 スクリプトを実行します。
    - **Uninstall**:リソース プロバイダーと関連付けられているすべてのリソースを削除します。
    - **PrivilegedEndpoint**:特権エンドポイントの IP アドレスまたは DNS 名。
    - **AzureEnvironment**:Azure Stack Hub のデプロイに使用する Azure 環境。 Azure AD のデプロイでのみ必須です。
    - **CloudAdminCredential**:特権エンドポイントへのアクセスに必要な、クラウド管理者の資格情報。
    - **AzCredential**:Azure Stack Hub サービス管理者アカウントの資格情報。 Azure Stack Hub のデプロイに使用したのと同じ資格情報を使用します。

## <a name="next-steps"></a>次のステップ

[App Services を PaaS として提供する](azure-stack-app-service-overview.md)
