---
title: Azure Stack MySQL リソース プロバイダー 1.1.47.0 のリリース ノート | Microsoft Docs
description: リリース ノートを参照して、Azure Stack MySQL リソース プロバイダー 1.1.47.0 更新プログラムの新機能を確認してください。
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
ms.openlocfilehash: 979aa822c981b4d34cb430e69dc15f80661818fe
ms.sourcegitcommit: 11e0c2d9abbc0a2506f992976b3c9f8ca4e746b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74823523"
---
# <a name="mysql-resource-provider-11470-release-notes"></a>MySQL リソース プロバイダー 1.1.47.0 のリリース ノート

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

これらのリリース ノートでは、MySQL リソース プロバイダー バージョン 1.1.47.0 の機能強化と既知の問題について説明します。

## <a name="build-reference"></a>ビルドのリファレンス
MySQL リソース プロバイダー バイナリをダウンロードした後、自己展開ツールを実行してコンテンツを一時ディレクトリに展開します。 リソース プロバイダーには、対応する最低限の Azure Stack ビルドがあります。 MySQL リソース プロバイダーのこのバージョンをインストールするために必要な Azure Stack の最小リリース バージョンを次に示します。

> |最小の Azure Stack バージョン|MySQL リソース プロバイダーのバージョン|
> |-----|-----|
> |バージョン 1910 (1.1910.0.58)|[MySQL RP バージョン 1.1.47.0](https://aka.ms/azurestackmysqlrp11470)|  
> |     |     |

> [!IMPORTANT]
> MySQL リソース プロバイダーの最新バージョンをデプロイする前に、サポートされている最低限の Azure Stack 更新プログラムを Azure Stack 統合システムに適用するか、最新の Azure Stack Development Kit (ASDK) をデプロイします。

## <a name="new-features-and-fixes"></a>新機能と修正

このバージョンの Azure Stack MySQL リソース プロバイダーは、リソース プロバイダーが 1910 更新プログラムの最新のポータル変更の一部との互換性を持つようにするための修正プログラム リリースであり、新機能はありません。

現時点で最新の Azure Stack API バージョン プロファイル 2019-03-01-hybrid と Azure Stack PowerShell モジュール 1.8.0 もサポートされます。 このため、デプロイおよび更新時にモジュールの特定の履歴バージョンをインストールする必要はありません。

Azure Stack が 1910 リリースにアップグレードされた後に MySQL リソース プロバイダー修正プログラム 1.1.47.0 を適用することをお勧めします。

## <a name="known-issues"></a>既知の問題

なし。

## <a name="next-steps"></a>次の手順
[MySQL リソース プロバイダーの詳細を確認します](azure-stack-mysql-resource-provider.md)。

[MySQL リソースプロバイダーのデプロイを準備します](azure-stack-mysql-resource-provider-deploy.md#prerequisites)。

[MySQL リソース プロバイダーを以前のバージョンからアップグレードします](azure-stack-mysql-resource-provider-update.md)。 
