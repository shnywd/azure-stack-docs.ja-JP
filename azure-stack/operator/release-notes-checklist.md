---
title: Azure Stack 更新アクティビティのチェックリスト | Microsoft Docs
description: 最新の Azure Stack の更新に備えてシステムを準備するためのチェックリスト。
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
ms.date: 08/30/2019
ms.author: sethm
ms.reviewer: hectorl
ms.lastreviewed: 08/30/2019
ms.openlocfilehash: 63b695b58a4c55f71714a5faa76d62e53f4d691e
ms.sourcegitcommit: c46d913ebfa4cb6c775c5117ac5c9e87d032a271
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2019
ms.locfileid: "71106522"
---
# <a name="azure-stack-update-activity-checklist"></a>Azure Stack 更新アクティビティのチェックリスト

*適用対象:Azure Stack 統合システム*

Azure Stack の更新を準備するために、このチェックリストを確認します。 この記事には、Azure Stack オペレーターの更新に関連するアクティビティのチェックリストを掲載しています。

## <a name="prepare-for-azure-stack-update"></a>Azure Stack の更新を準備する

| アクティビティ                     | 詳細                                                   |
|------------------------------|-----------------------------------------------------------|
| 既知の問題の確認     | [既知の問題の一覧](known-issues.md)                |
| セキュリティ更新プログラムの確認 | [セキュリティ更新プログラムの一覧](release-notes-security-updates.md)      |
| 最新の OEM パッケージの適用 | OEM に問い合わせて、ご利用のシステムのアップグレード先の Azure Stack バージョンの最小 OEM パッケージ要件を、システムが満たしていることを確認してください。 |
| Test-AzureStack を実行する | `Test-AzureStack -Group UpdateReadiness` を実行して、操作上の問題を特定します。 |
| 問題の解決 | `Test-AzureStack` によって特定された操作上の問題を解決します。 |
| 最新の修正プログラムの適用 | 現在インストールされているリリースに適用される最新の修正プログラムを適用します。 |
| 容量計画ツールの実行 | ワークロードの計画とサイズ設定を行うには、最新バージョンの [Azure Stack Capacity Planner ツール](azure-stack-capacity-planning-overview.md)を使用します。 最新バージョンではバグの修正が含まれ、Azure Stack の各更新プログラムでリリースされる新機能が提供されています。 |
| 更新プログラムが利用可能 | Azure Stack デプロイでは、接続されたシナリオに限り、セキュリティで保護されたエンドポイントが定期的にチェックされ、お客様のクラウド用の更新プログラムが入手可能かどうかが自動的に通知されます。 接続していないお客様は、[ここで説明されているプロセス](azure-stack-apply-updates.md)を使用して、新しいパッケージをダウンロードしてインポートできます。 |

## <a name="during-azure-stack-update"></a>Azure Stack の更新中

| アクティビティ | 詳細 |
|--------------------|------------------------------------------------------------------------------------------------------|
| 更新プログラムの管理 |[オペレーター ポータルを使用して Azure Stack の更新プログラムを管理します](azure-stack-updates.md)。 |
|  |  |
| 更新プログラムの監視 | オペレーター ポータルを使用できない場合は、[特権エンドポイントを使用して Azure Stack での更新プログラムをモニター](azure-stack-monitor-update.md)します。 |
|  |  |
| 更新プログラムの再開 | 失敗した更新プログラムを修正したら、[特権エンドポイントを使用して Azure Stack で更新プログラムを再開](azure-stack-monitor-update.md)します。 |

> [!Important]  
> 更新中 **Test-AzureStack** を実行しないでください。更新プロセスが停止することがあります。

## <a name="after-azure-stack-update"></a>Azure Stack の更新の後

| アクティビティ | 詳細 |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 最新の修正プログラムの適用 | 更新されたバージョンに適用可能な最新の修正プログラムを適用します。 |
| 暗号化キーの取得 | 保存データの暗号化キーを取得し、お客様の Azure Stack デプロイの外部に安全に保管します。 [キーを取得する方法の手順](azure-stack-security-bitlocker.md)に関する記事に従ってください。 |
|  |  |
| マルチテナントの再有効化 | マルチテナント Azure Stack の場合、更新の成功後に、[すべてのゲスト ディレクトリ テナントを構成してください](azure-stack-enable-multitenancy.md#configure-guest-directory)。 |

## <a name="next-steps"></a>次の手順

- [既知の問題の一覧を確認する](known-issues.md)
- [セキュリティ更新プログラムの一覧を確認する](release-notes-security-updates.md)
