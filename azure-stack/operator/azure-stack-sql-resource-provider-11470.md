---
title: Azure Stack SQL リソース プロバイダー 1.1.47.0 のリリース ノート | Microsoft Docs
description: 既知の問題やダウンロード場所など、最新の Azure Stack SQL リソース プロバイダー更新プログラムについて説明します。
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
ms.topic: article
ms.date: 11/26/2019
ms.author: justinha
ms.reviewer: xiaofmao
ms.lastreviewed: 11/26/2019
ms.openlocfilehash: 703edc0cc2c5229c3301db0b68a400507fe4c872
ms.sourcegitcommit: 11e0c2d9abbc0a2506f992976b3c9f8ca4e746b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74823529"
---
# <a name="sql-resource-provider-11470-release-notes"></a>SQL リソース プロバイダー 1.1.47.0 のリリース ノート

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

これらのリリース ノートでは、SQL リソース プロバイダー バージョン 1.1.47.0 の機能強化と既知の問題について説明します。

## <a name="build-reference"></a>ビルドのリファレンス
SQL リソース プロバイダー バイナリをダウンロードした後、自己展開ツールを実行してコンテンツを一時ディレクトリに抽出します。 リソース プロバイダーには、対応する最低限の Azure Stack ビルドがあります。 SQL リソース プロバイダーのこのバージョンをインストールするために必要な Azure Stack の最小リリース バージョンを次に示します。

> |最小の Azure Stack バージョン|SQL リソース プロバイダーのバージョン|
> |-----|-----|
> |バージョン 1910 (1.1910.0.58)|[SQL RP バージョン 1.1.47.0](https://aka.ms/azurestacksqlrp11470)|  
> |     |     |

> [!IMPORTANT]
> SQL リソース プロバイダーの最新バージョンをデプロイする前に、サポートされている最低限の Azure Stack 更新プログラムを Azure Stack 統合システムに適用するか、最新の Azure Stack Development Kit (ASDK) をデプロイします。

## <a name="new-features-and-fixes"></a>新機能と修正

このバージョンの Azure Stack SQL リソース プロバイダーは、リソース プロバイダーが 1910 更新プログラムの最新のポータル変更の一部との互換性を持つようにするための修正プログラム リリースであり、新機能はありません。

現時点で最新の Azure Stack API バージョン プロファイル 2019-03-01-hybrid と Azure Stack PowerShell モジュール 1.8.0 もサポートされます。 このため、デプロイおよび更新時にモジュールの特定の履歴バージョンをインストールする必要はありません。

Azure Stack が 1910 更新プログラムにアップグレードされた後、リソース プロバイダーの更新プロセスに従って、SQL リソース プロバイダーの修正プログラム 1.1.47.0 を適用してください。 SQL リソース プロバイダーの容量監視で読み込み処理が続くという管理者ポータルの既知の問題に対処することができます。

## <a name="known-issues"></a>既知の問題

なし。

## <a name="next-steps"></a>次の手順
[SQL リソース プロバイダーの詳細を確認します](azure-stack-sql-resource-provider.md)。

[SQL リソースプロバイダーのデプロイを準備します](azure-stack-sql-resource-provider-deploy.md#prerequisites)。

[SQL リソース プロバイダーを以前のバージョンからアップグレードします](azure-stack-sql-resource-provider-update.md)。 
