---
title: Infrastructure Backup サービスを使用した Azure Stack のデータの回復 | Microsoft Docs
description: Infrastructure Backup サービスを使用して、Azure Stack の構成とサービス データをバックアップおよび復元する方法について学習します。
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: justinha
ms.reviewer: hectorl
ms.lastreviewed: 05/16/2019
ms.openlocfilehash: 832d2146e79f3724c5f1b3bf9da1776388636da8
ms.sourcegitcommit: 245a4054a52e54d5989d6148fbbe386e1b2aa49c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2019
ms.locfileid: "70974699"
---
# <a name="recover-data-in-azure-stack-with-the-infrastructure-backup-service"></a>Infrastructure Backup サービスを使用した Azure Stack のデータの回復

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

Azure Stack の Infrastructure Backup サービスを使用して、構成とサービス データをバックアップおよび復元できます。 Azure Stack の各インストールには、サービスのインスタンスが含まれています。 Azure Stack Cloud を再デプロイし、ID、セキュリティ、Azure Resource Manager データを復元するには、サービスによって作成されたバックアップを使用できます。

クラウドを実稼働に移行する準備ができたら、バックアップを有効にします。 テストと検証を長期間実行する予定の場合は、バックアップを有効にしないでください。

バックアップ サービスを有効にする前に、[要件が満たされている](#verify-requirements-for-the-infrastructure-backup-service)ことを確認してください。

> [!Note]  
> Infrastructure Backup サービスには、ユーザー データとアプリは含まれません。 IaaS VM ベースのアプリケーションを保護する方法の詳細については、「[Azure Stack にデプロイされた VM の保護](../user/azure-stack-manage-vm-protect.md)」を参照してください。 Azure Stack 上でアプリを保護する方法を包括的に理解するには、[事業継続とディザスター リカバリーのための Azure Stack の考慮事項に関するホワイトペーパー](https://aka.ms/azurestackbcdrconsiderationswp)を参照してください。

## <a name="the-infrastructure-backup-service"></a>インフラストラクチャ バックアップ サービス

このサービスには以下の機能が含まれています。

| 機能                                            | 説明                                                                                                                                                |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| バックアップ インフラストラクチャ サービス                     | Azure Stack でインフラストラクチャ サービスのサブセット全体のバックアップを調整します。 災害が発生した場合、再デプロイの一環としてデータを復元できます。 |
| エクスポートされるバックアップ データの圧縮と暗号化 | バックアップ データは、システムによって圧縮および暗号化されてから、管理者が提供する外部ストレージの場所にエクスポートされます。                |
| バックアップ ジョブの監視                              | バックアップ ジョブが失敗した場合、システムによって問題の解決方法が通知されます。                                                                                                |
| 管理エクスペリエンスのバックアップ                       | バックアップ RP はバックアップの有効化をサポートしています。                                                                                                                         |
| クラウドの回復                                     | 致命的なデータ損失が発生した場合、バックアップを使用して、デプロイの一環として Azure Stack のコア情報を復元できます。                                 |

## <a name="verify-requirements-for-the-infrastructure-backup-service"></a>インフラストラクチャ バックアップ サービスの要件を確認する

- **保存場所**  
  7 個のバックアップを保存できる、Azure Stack からアクセス可能なファイル共有が必要です。 各バックアップは約 10 GB です。 共有には 140 GB のバックアップを保存できる必要があります。 Infrastructure Backup サービスの保存場所の選択の詳細については、「[バックアップ コントローラーの要件](azure-stack-backup-reference.md#backup-controller-requirements)」を参照してください。
- **資格情報**  
  ドメイン ユーザー アカウントと資格情報が必要です。 たとえば、Azure Stack 管理者の資格情報を使用できます。
- **暗号化証明書**  
  バックアップ ファイルは証明書内の公開キーを使用して暗号化されます。 必ず、この証明書は安全な場所に保存してください。 


## <a name="next-steps"></a>次の手順

[管理者ポータルで Azure Stack のバックアップを有効にする](azure-stack-backup-enable-backup-console.md)方法を学習します。

[PowerShell で Azure Stack のバックアップを有効にする](azure-stack-backup-enable-backup-powershell.md)方法を学びます。

[Azure Stack のバックアップ](azure-stack-backup-back-up-azure-stack.md)方法を学習します。

[致命的なデータの損失からの復旧](azure-stack-backup-recover-data.md)方法を学習します。
