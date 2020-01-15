---
title: Azure Stack Hub 更新アクティビティのチェックリスト | Microsoft Docs
description: 最新の Azure Stack Hub の更新に備えてシステムを準備するためのチェックリスト。
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
ms.date: 12/10/2019
ms.author: sethm
ms.reviewer: ppacent
ms.lastreviewed: 12/10/2019
ms.openlocfilehash: a626b50a742d3219d7934e4cc3f77cd890ea1b93
ms.sourcegitcommit: c3be6b2e962c5905eb3c54f9555e13095f6b4d40
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/20/2019
ms.locfileid: "75303733"
---
# <a name="azure-stack-hub-update-activity-checklist"></a>Azure Stack Hub 更新アクティビティのチェックリスト

*適用対象:Azure Stack Hub 統合システム*

Azure Stack Hub の更新を準備するために、このチェックリストを確認します。 この記事には、Azure Stack Hub オペレーターに向けた、更新関連のアクティビティのチェックリストが掲載されています。

## <a name="prepare-for-azure-stack-hub-update"></a>Azure Stack Hub の更新を準備する

| アクティビティ                     | 詳細                                                   |
|------------------------------|-----------------------------------------------------------|
| 既知の問題の確認     | [既知の問題の一覧](known-issues.md)                |
| セキュリティ更新プログラムの確認 | [セキュリティ更新プログラムの一覧](release-notes-security-updates.md)      |
| 最新の OEM パッケージの適用 | お使いのシステムがアップグレードされる予定の Azure Stack Hub バージョンに関して OEM に問い合わせて、システムが最小 OEM パッケージ要件を満たしていることを確認してください。 OEM パッケージが、更新する Azure Stack Hub バージョンと互換性があることを確認します。 OEM パッケージが更新後の Azure Stack Hub バージョンと互換性がない場合は、Azure Stack Hub 更新プログラムを実行する前に OEM パッケージの更新を実行する必要があります。 手順については、「Azure Stack Hub に OEM (相手先ブランド供給) の更新プログラムを適用する」を参照してください。 |
| 省略可能:自動ログ収集の構成 | サポート チケットを開く必要がある場合にシステム ログを収集するプロセスを効率化するため、Azure Stack Hub 環境で自動ログ収集を構成することをお勧めします。 自動ログ収集を構成するには、「[自動 Azure Stack Hub 診断ログ収集の構成](azure-stack-configure-automatic-diagnostic-log-collection.md)」を参照してください。 |
| 最新の修正プログラムの適用 | 現在インストールされているリリースに適用される最新の修正プログラムを適用します。 最新の修正プログラムの一覧については、リリース ノートの修正プログラムのセクションを参照してください。 |
| 容量計画ツールの実行 | ワークロードの計画とサイズ設定を行うには、最新バージョンの [Azure Stack Hub キャパシティ プランニング ツール](azure-stack-capacity-planning-overview.md)を使用します。 最新バージョンにはバグの修正プログラムが含まれていて、Azure Stack Hub の各更新プログラムでリリースされる新機能が提供されます。 |
| Test-AzureStack を実行する | `Test-AzureStack -Group UpdateReadiness` を実行して、操作上の問題を特定します。 コマンドレットには、特権エンドポイント セッション (PEP) を使用してアクセスできます。 詳細については、「[Azure Stack Hub システムの状態を検証する](azure-stack-diagnostic-test.md)」を参照してください。 |
| 問題の解決 | `Test-AzureStack` によって特定された操作上の問題を解決します。 |
| 更新プログラムが利用可能 | Azure Stack Hub デプロイでは、接続されたシナリオでのみ、セキュリティで保護されたエンドポイントが定期的にチェックされ、お客様のクラウド用の更新プログラムが入手可能かどうかが自動的に通知されます。 接続していないお客様は、[ここで説明されているプロセス](azure-stack-apply-updates.md)を使用して、新しいパッケージをダウンロードしてインポートできます。 |
| ユーザーへの通知 | メンテナンス操作についてユーザーに通知し、通常のメンテナンス期間をできるだけ勤務時間外にスケジュールする必要があります。 メンテナンス操作は、テナントのワークロードとポータル操作の両方に影響を及ぼす可能性があります。 |

## <a name="during-azure-stack-hub-update"></a>Azure Stack Hub の更新中

| アクティビティ | 詳細 |
|--------------------|------------------------------------------------------------------------------------------------------|
| 更新プログラムの管理 |[オペレーター ポータルを使用して Azure Stack Hub の更新プログラムを管理します](azure-stack-updates.md)。 |
|  |  |
| 更新プログラムの監視 | オペレーター ポータルを使用できない場合は、[特権エンドポイントを使用して Azure Stack Hub の更新プログラムを監視](azure-stack-monitor-update.md)します。 |
|  |  |
| 更新プログラムの再開 | 失敗した更新プログラムを修正したら、[特権エンドポイントを使用して Azure Stack Hub で更新プログラムを再開](azure-stack-monitor-update.md)します。 |

> [!IMPORTANT]  
> 更新中 **Test-AzureStack** を実行しないでください。更新プロセスが停止することがあります。

## <a name="after-azure-stack-hub-update"></a>Azure Stack Hub の更新後

| アクティビティ | 詳細 |
|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 最新の修正プログラムの適用 | 更新されたバージョンに適用可能な最新の修正プログラムを適用します。 |
| 暗号化キーの取得 | 保存データの暗号化キーを取得し、お使いの Azure Stack Hub デプロイの外部にそれらを安全に保管します。 [キーを取得する方法の手順](azure-stack-security-bitlocker.md)に関する記事に従ってください。 |
|  |  |
| マルチテナントの再有効化 | マルチテナント Azure Stack Hub の場合は、更新の成功後、[すべてのゲスト ディレクトリ テナントを構成していることを確認します](azure-stack-enable-multitenancy.md#configure-guest-directory)。 |

## <a name="next-steps"></a>次のステップ

- [既知の問題の一覧を確認する](known-issues.md)
- [セキュリティ更新プログラムの一覧を確認する](release-notes-security-updates.md)
