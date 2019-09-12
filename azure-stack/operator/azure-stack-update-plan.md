---
title: Azure Stack の更新の計画 | Microsoft Docs
description: Azure Stack の更新の計画について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2019
ms.author: mabrigg
ms.lastreviewed: 08/23/2019
ms.reviewer: ppacent
ms.openlocfilehash: 58bfdf49492caf7f8db7f4d2a04de97a17738eac
ms.sourcegitcommit: dc633e862d49412a963daee481226c1543287e5e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70863034"
---
# <a name="plan-for-an-azure-stack-update"></a>Azure Stack の更新の計画

*適用対象:Azure Stack 統合システム*

ユーザーへの影響が最小限になるように、可能な限りスムーズに更新プロセスを進めるために Azure Stack を準備できます。 この記事では、サービスの停止の可能性をユーザーに通知する手順、および更新を準備するために実行する必要がある手順を確認します。

## <a name="notify-your-users-of-maintenance-operations"></a>メンテナンス操作についてユーザーに通知する

メンテナンス操作についてユーザーに通知し、通常のメンテナンス期間をできるだけ勤務時間外にスケジュールする必要があります。 メンテナンス操作は、テナントのワークロードとポータル操作の両方に影響を及ぼす可能性があります。

## <a name="prepare-for-an-azure-stack-update"></a>Azure Stack の更新を準備する

更新を準備するには、すべての修正プログラム、セキュリティ更新プログラム、および OEM の更新プログラムを適用し、Azure Stack インスタンスの正常性を検証して、利用可能な容量を確認し、更新プログラム パッケージを確認してください。

1. 既知の問題を確認します。 手順については、[Azure Stack の既知の問題](https://docs.microsoft.com/azure-stack/operator/azure-stack-release-notes-known-issues-1907)に関するページをご覧ください。

2. セキュリティ更新プログラムを確認します。 更新の一覧については、「[Azure Stack security updates](https://docs.microsoft.com/azure-stack/operator/azure-stack-release-notes-security-updates-1907)」(Azure Stack のセキュリティ更新プログラム) を参照してください。

3. この更新プログラムのインストールを開始する前に、[Test-AzureStack](https://docs.microsoft.com/azure-stack/operator/azure-stack-diagnostic-test) を実行して Azure Stack の状態を確認し、見つかったすべての操作上の問題 (すべての警告とエラーを含む) を解決します。 また、アクティブなアラートを確認し、アクションが必要なアラートを解決します。

    1. Azure Stack ERCS VM へのネットワーク アクセスを持つマシンから特権エンドポイント セッションを開きます。 PEP を使用する手順については、「[Azure Stack での特権エンドポイントの使用](https://docs.microsoft.com/azure-stack/operator/azure-stack-privileged-endpoint)」を参照してください。

    2. Test-AzureStack -Group UpdateReadiness

    3. 出力を確認し、正常性エラーを解決します。 詳細については、[Azure Stack の検証テストの実行](https://docs.microsoft.com/azure-stack/operator/azure-stack-diagnostic-test)に関するページをご覧ください。

4. 問題を解決します。 フラグが設定された問題の詳細については、「正常性とアラートの監視」トピックを参照できます。 手順については、「[Azure Stack での正常性およびアラートの監視](https://docs.microsoft.com/azure-stack/operator/azure-stack-monitor-health)」を参照してください。

5. 最新の修正プログラムを適用します。 最新の修正プログラムの一覧については、リリース ノートの修正プログラムのセクションを参照してください。 最新の修正プログラムの適用後、手順 3 と 4 を繰り返します。

6. OEM パッケージが、更新する Azure Stack バージョンと互換性があることを確認します。 OEM パッケージが更新先の Azure Stack バージョンと互換性がない場合は、Azure Stack 更新プログラムを実行する前に OEM パッケージの更新を実行する必要があります。 手順については、「Azure Stack に OEM (相手先ブランド供給) 更新プログラムを適用する」を参照してください。 OEM パッケージ更新プログラムの適用後、手順 3 および 4 を繰り返します。

7. 容量計画ツールを実行します。 ツールの使用の概要と手順については、「[Azure Stack のキャパシティ プランニングの概要](https://docs.microsoft.com/azure-stack/operator/azure-stack-capacity-planning-overview)」を参照してください。

8. 更新プログラム パッケージを確認します。 メンテナンス期間を計画するときは、リリース ノートに記載されているように、Microsoft からリリースされた特定の種類の更新プログラム パッケージを確認することが重要です。

## <a name="next-steps"></a>次の手順

[Azure Stack 更新プログラムを適用します](azure-stack-apply-updates.md)。
