---
title: Azure Stack リリース ノート - 更新アクティビティ チェックリスト | Microsoft Docs
description: 最新の Azure Stack の更新用にシステムを準備するクイック チェックリストを示します。
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
ms.date: 06/27/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 06/27/2019
ms.openlocfilehash: c5108dc8b24fec7569d54964217f146a5cefa5e6
ms.sourcegitcommit: bcaad8b7db2ea596018d973cb29283d8c6daebfb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2019
ms.locfileid: "67419571"
---
# <a name="azure-stack-update-activity-checklist"></a>Azure Stack 更新アクティビティのチェックリスト

この記事には、Azure Stack オペレーターの更新に関連するアクティビティのチェックリストを掲載しています。 Azure Stack に更新プログラムを適用する準備を行う場合、この情報を確認できます。

## <a name="prepare-for-azure-stack-update"></a>Azure Stack の更新を準備する

| アクティビティ              | 詳細                                                                          |
|-----------------------|----------------------------------------------------------------------------------|
| 既知の問題の確認   | [既知の問題の一覧](azure-stack-release-notes-known-issues-1906.md)                |
| セキュリティ更新プログラムの確認 | [セキュリティ更新プログラムの一覧](azure-stack-release-notes-security-updates-1906.md)      |
| Test-AzureStack を実行する   | `Test-AzureStack -Group UpdateReadiness` を実行して、操作上の問題を特定します。      |
| 問題の解決        | **Test-AzureStack** によって特定された操作上の問題を解決します。                |
| 最新の修正プログラムの適用 | 現在インストールされているリリースに適用される最新の修正プログラムを適用します。         |
| 容量計画ツールの実行 | ワークロードの計画とサイズ設定を行うには、最新バージョンの [Azure Stack Capacity Planner](https://aka.ms/azstackcapacityplanner)  ツールを使用します。 最新バージョンではバグの修正が含まれ、Azure Stack の各更新プログラムでリリースされる新機能が提供されています。 |
| 更新プログラムが利用可能       | Azure Stack デプロイでは、接続されたシナリオに限り、セキュリティで保護されたエンドポイントが定期的にチェックされ、お客様のクラウド用の更新プログラムが入手可能かどうかが自動的に通知されます。 接続していないお客様は、[ここで説明されているプロセス](azure-stack-apply-updates.md)を使用して、新しい 1906 パッケージをダウンロードしてインポートできます。               |

## <a name="during-azure-stack-update"></a>Azure Stack の更新中

| アクティビティ              | 詳細                                                                          |
|-----------------------|----------------------------------------------------------------------------------|
| 更新プログラムの管理         | [オペレーター ポータルを使用して Azure Stack の更新プログラムを管理します](azure-stack-updates.md)。 |
| 更新プログラムの監視        | オペレーター ポータルを使用できない場合は、[特権エンドポイントを使用して Azure Stack での更新プログラムをモニター](azure-stack-monitor-update.md)します。 |
| 更新プログラムの再開            | 失敗した更新プログラムを修正したら、[特権エンドポイントを使用して Azure Stack で更新プログラムを再開](azure-stack-monitor-update.md)します。 |

> [!IMPORTANT]  
> 更新中 **Test-AzureStack** を実行しないでください。更新プロセスが停止することがあります。

## <a name="after-azure-stack-update"></a>Azure Stack の更新の後

| アクティビティ              | 詳細                                                                          |
|-----------------------|----------------------------------------------------------------------------------|
| 最新の修正プログラムの適用 | 更新されたバージョンに適用可能な最新の修正プログラムを適用します。                          |
| 暗号化キーの取得 | 保存データの暗号化キーを取得し、お客様の Azure Stack デプロイの外部に安全に保管します。 [キーを取得する方法の手順](azure-stack-security-bitlocker.md)に関する記事に従ってください。 |

## <a name="next-steps"></a>次の手順

- [既知の問題の一覧を確認する](azure-stack-release-notes-known-issues-1906.md)
- [セキュリティ更新プログラムの一覧を確認する](azure-stack-release-notes-security-updates-1906.md)
