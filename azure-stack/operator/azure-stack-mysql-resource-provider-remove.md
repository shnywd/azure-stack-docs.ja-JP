---
title: Azure Stack Hub 上の MySQL リソース プロバイダーを削除する | Microsoft Docs
description: Azure Stack Hub のデプロイから MySQL リソース プロバイダーを削除する方法について説明します。
services: azure-stack
documentationCenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 11/20/2018
ms.openlocfilehash: c1551badd87ba55432ff416aae1230abf317e1b3
ms.sourcegitcommit: 1185b66f69f28e44481ce96a315ea285ed404b66
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/09/2020
ms.locfileid: "75811041"
---
# <a name="remove-the-mysql-resource-provider-in-azure-stack-hub"></a>Azure Stack Hub 上の MySQL リソース プロバイダーを削除する

MySQL リソース プロバイダーを削除する前に、プロバイダーの依存関係をすべて削除する必要があります。 また、リソース プロバイダーのインストールに使用したデプロイ パッケージのコピーも必要になります。

> [!NOTE]
> リソース プロバイダーのインストーラーのダウンロード リンクは、[リソース プロバイダーを展開するための前提条件](./azure-stack-mysql-resource-provider-deploy.md#prerequisites)に関するページにあります。

MySQL リソース プロバイダーを削除しても、ホスティング サーバーからテナント データベースが削除されることはありません。

## <a name="dependency-cleanup"></a>依存関係のクリーンアップ

DeployMySqlProvider.ps1 スクリプトを実行してリソース プロバイダーを削除する前に、いくつかのクリーンアップ タスクを実行する必要があります。

次のクリーンアップ タスクは、Azure Stack Hub オペレーターが担当します。

* MySQL アダプターを参照しているすべてのプランを削除する。
* MySQL アダプターに関連付けられているすべてのクォータを削除する。

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
    - **DirectoryTenantID**
    - **AzCredential**:Azure Stack Hub サービス管理者アカウントの資格情報。 Azure Stack Hub のデプロイに使用したのと同じ資格情報を使用します。

## <a name="next-steps"></a>次のステップ

[App Services を PaaS として提供する](azure-stack-app-service-overview.md)
