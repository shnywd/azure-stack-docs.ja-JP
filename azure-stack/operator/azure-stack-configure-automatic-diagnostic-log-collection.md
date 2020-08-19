---
title: Azure Stack Hub での事前診断ログの収集
description: Azure Stack Hub のヘルプとサポートで事前診断ログ収集を構成する方法について説明します。
author: justinha
ms.topic: article
ms.date: 06/16/2020
ms.author: justinha
ms.reviewer: shisab
ms.lastreviewed: 06/16/2020
ms.openlocfilehash: fe1ae4c0c979d579df99b6e440d62dd16a9df2e9
ms.sourcegitcommit: 52b33ea180c38a5ecce150f5a9ea4a026344cc3d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2020
ms.locfileid: "88074164"
---
# <a name="proactive-diagnostic-log-collection-in-azure-stack-hub"></a>Azure Stack Hub での事前診断ログの収集

::: moniker range=">= azs-2002"

Azure Stack Hub でアラートが発生したときに診断ログを事前に収集することで、カスタマー サポートに要する時間を節約します。

システム正常性状態を調査する必要がある場合、サポート ケースを開く前に Microsoft サポートにより分析のためにログを自動的にアップロードできます。

>[!NOTE]
>インターネットに接続していない場合、またはローカルでのみログを保存する場合は、[Get-AzureStackLog](azure-stack-get-azurestacklog.md) メソッドを使用してログを送信します。 

## <a name="steps-to-configure-proactive-log-collection"></a>事前ログ収集を構成する手順

事前ログ収集を構成するには、次の手順に従います。 事前ログ収集は、いつでも無効にして再度有効にすることができます。  

1. Azure Stack Hub 管理者ポータルにサインインします。
1. **[Help and support Overview]\(ヘルプとサポートの概要\)** を開きます。
1. バナーが表示されたら、 **[Enable proactive log collection]\(事前ログ収集を有効にする\)** を選択します。 =

   ![ヘルプとサポートでログ収集を有効にする場所を示すスクリーンショット](media/azure-stack-help-and-support/banner-enable-automatic-log-collection.png)

   または、 **[設定]** を選択して **[Proactive log collection]\(事前ログ収集\)** を **[有効]** にし、 **[保存]** を選択することもできます。

   ![ヘルプとサポートでログ収集を有効にする場所を示すスクリーンショット](media/azure-stack-help-and-support/settings-enable-automatic-log-collection.png)

::: moniker-end
::: moniker range="<= azs-1910"

ログ収集とカスタマー サポート エクスペリエンスを効率化するために、自動診断ログ収集機能を構成することをお勧めします。

システム正常性状態を調査する必要がある場合、Microsoft サポートによる分析のためにログを自動的にアップロードすることができます。

## <a name="create-an-azure-blob-container-sas-url"></a>Azure BLOB コンテナー SAS URL の作成

自動ログ収集を構成する前に、BLOB コンテナーの Shared Access Signature (SAS) を取得する必要があります。 SAS により、アカウント キーを共有することなく、ストレージ アカウントのリソースへのアクセス権を付与できます。

Azure Stack Hub ログ ファイルを Azure の BLOB コンテナーに保存し、Microsoft サポートがログを収集できる SAS URL を提供することができます。

### <a name="prerequisites"></a>前提条件

Azure で新規または既存の BLOB コンテナーを使用できます。 Azure で BLOB コンテナーを作成するには、少なくとも[ストレージ BLOB 共同作成者ロール](/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor)または[特定のアクセス許可](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)が必要です。 グローバル管理者にも、必要な権限があります。

自動ログ収集ストレージ アカウントのパラメーターの選択に関するベスト プラクティスについては、「[自動 Azure Stack Hub ログ収集のベスト プラクティス](./azure-stack-overview.md?view=azs-2002)」を参照してください。 ストレージ アカウントの種類について詳しくは、「[Azure ストレージ アカウントの概要](/azure/storage/common/storage-account-overview)」をご覧ください。

### <a name="create-a-blob-storage-account"></a>BLOB ストレージ アカウントの作成

1. [Azure portal](https://portal.azure.com) にサインインします。
1. **[ストレージ アカウント]** > **[追加]** を選択します。
1. 次の設定を使用して BLOB コンテナーを作成します。

   - **サブスクリプション**:Azure サブスクリプションを選択します。
   - **[リソース グループ]** :リソース グループを指定します。
   - **ストレージ アカウント名**: 一意のストレージ アカウント名を指定します。
   - **[場所]** :会社のポリシーに従ってデータセンターを選択します。
   - **パフォーマンス**: Standard。
   - **アカウントの種類** StorageV2 (汎用 v2)。
   - **[Replication]\(レプリケーション\)** :ローカル冗長ストレージ (LRS)。
   - **アクセス層**:クール。

   ![BLOB コンテナーのプロパティ](media/azure-stack-automatic-log-collection/azure-stack-log-collection-create-storage-account.png)

1. **[確認および作成]** を選択し、次に **[作成]** を選択します。  

### <a name="create-a-blob-container"></a>BLOB コンテナーを作成する

1. デプロイが成功したら、 **[リソースに移動]** を選択します。 また、簡単にアクセスできるように、ストレージ アカウントをダッシュボードにピン留めすることもできます。
1. **[Storage Explorer (プレビュー)]** を選択し、 **[BLOB コンテナー]** を右クリックして、 **[BLOB コンテナーの作成]** を選択します。
1. 新しいコンテナーの名前を入力し、 **[OK]** を選択します。

## <a name="create-a-sas-url"></a>SAS URI を作成する

1. コンテナーを右クリックしてから、 **[Shared Access Signature の取得]** を選択します。
   
   ![BLOB コンテナーの Shared Access Signature を取得する方法](media/azure-stack-automatic-log-collection/get-sas.png)

1. 次のプロパティを選択します。

   - 開始時刻:必要に応じて、開始時刻を戻すことができます
   - 有効期限:2 年間
   - タイム ゾーン:UTC
   - 権限:読み取り、書き込み、および一覧表示

   ![Shared Access Signature のプロパティ](media/azure-stack-automatic-log-collection/sas-properties.png) 

1. **［作成］** を選択します  

[自動ログ収集を構成する](?view=azs-2002)ときに、URL をコピーして入力します。 SAS URL の詳細については、「[Shared Access Signatures (SAS) の使用](/azure/storage/common/storage-dotnet-shared-access-signature-part-1)」を参照してください。

## <a name="steps-to-configure-automatic-log-collection"></a>自動ログ収集を構成する手順

次の手順に従って、SAS URL をログ収集 UI に追加します。

1. Azure Stack Hub 管理者ポータルにサインインします。
1. **[Help and support Overview]\(ヘルプとサポートの概要\)** を開きます。
1. **[Automatic collection settings]\(自動収集の設定\)** をクリックします。

   ![ヘルプとサポートでログ収集を有効にする場所](media/azure-stack-automatic-log-collection/azure-stack-automatic-log-collection.png)

1. 自動ログ収集を **[有効]** に設定します。
1. ストレージ アカウントの BLOB コンテナーの Shared Access Signature (SAS) URL を入力します。

   ![BLOB SAS URL](media/azure-stack-automatic-log-collection/azure-stack-enable-automatic-log-collection.png)

>[!NOTE]
>自動ログ収集は、いつでも無効にして再度有効にすることができます。 SAS URL 構成は変更されません。 自動ログ収集を再び有効にすると、以前に入力した SAS URL で同じ検証チェックが実行され、期限切れの SAS URL が拒否されます。

::: moniker-end

>[!NOTE]
>ログの場所の設定がローカル ファイル共有に対して構成されている場合、共有ストレージがそのサイズ クォータに達するのを、ライフサイクル管理ポリシーが未然に防ぎます。 Azure Stack Hub は、ローカル ファイル共有を監視せず、アイテム保持ポリシーを強制することもしません。

## <a name="view-log-collection"></a>ログ収集の表示

Azure Stack Hub から収集されたログの履歴は、 **[ヘルプとサポート]** の **[Log collection]\(ログ収集\)** ページに次の日付と時刻と共に表示されます。

- **収集された時刻**:ログ収集操作が開始されたとき。
- **状態**: 進行中または完了。
- **ログの開始**:収集する期間の開始。
- **ログの終了**:期間の終了。
- **[種類]** :手動ログ収集または事前ログ収集のいずれか。

![ヘルプとサポートのログ収集](media/azure-stack-help-and-support/azure-stack-log-collection.png)

## <a name="proactive-diagnostic-log-collection-alerts"></a>事前診断ログ収集アラート

有効にすると、次のいずれかのイベントが発生した場合にのみ、事前ログ収集によってログがアップロードされます。

たとえば、**更新失敗**は、事前診断ログ収集をトリガーするアラートです。 これが有効になっていると、Microsoft サポートによる問題のトラブルシューティングに役立つように、更新エラーの間に前もって診断ログがキャプチャされます。 診断ログは、**更新失敗**のアラートが発生した場合にだけ収集されます。

| アラートのタイトル | FaultIdType |
|---|---|
|リモート サービスに接続できない | UsageBridge.NetworkError|
|更新失敗 | Urp.UpdateFailure |
|ストレージ リソース プロバイダーのインフラストラクチャまたは依存関係を利用できない |    StorageResourceProviderDependencyUnavailable |
|ノードがコントローラーに接続されていない| ServerHostNotConnectedToController |  
|ルート パブリケーションの失敗 | SlbMuxRoutePublicationFailure |
|ストレージ リソースプロバイダーの内部データ ストアを利用できない |    StorageResourceProvider. DataStoreConnectionFail |
|ストレージ デバイスの障害 | Microsoft.Health.FaultType.VirtualDisks.Detached |
|正常性コントローラーが外部ストレージ アカウントにアクセスできない | Microsoft.Health.FaultType.StorageError |
|物理ディスクへの接続が失われた | Microsoft.Health.FaultType.PhysicalDisk.LostCommunication |
|ノードで Blob service が実行されていない | StorageService.The.blob.service.is.not.running.on.a.node-Critical |
|インフラストラクチャ ロールの異常 | Microsoft.Health.FaultType.GenericExceptionFault |
|Table service のエラー | StorageService.Table.service.errors-Critical |
|ファイル共有の使用率が 80% を超えている | Microsoft.Health.FaultType.FileShare.Capacity.Warning.Infra |
|Scale unit node is offline\(スケール ユニットがオフラインです\) | FRP.Heartbeat.PhysicalNode |
|インフラストラクチャ ロール インスタンスを利用できない | FRP.Heartbeat.InfraVM |
|インフラストラクチャ ロール インスタンスを利用できない  | FRP.Heartbeat.NonHaVm |
|インフラストラクチャ ロールのディレクトリ管理で時刻同期のエラーが報告された | DirectoryServiceTimeSynchronizationError |
|保留中の外部証明書の有効期限 | CertificateExpiration.ExternalCert.Warning |
|保留中の外部証明書の有効期限 | CertificateExpiration.ExternalCert.Critical |
|メモリ容量不足のため、特定のクラスとサイズの仮想マシンをプロビジョニングできない | AzureStack.ComputeController.VmCreationFailure.LowMemory |
|Node inaccessible for virtual machine placement\(仮想マシンを配置するためのノードにアクセスできません\) | AzureStack.ComputeController.HostUnresponsive |
|バックアップ失敗  | AzureStack.BackupController.BackupFailedGeneralFault |
|失敗した操作と競合するため、スケジュールされたバックアップがスキップされた    | AzureStack.BackupController.BackupSkippedWithFailedOperationFault |

## <a name="see-also"></a>関連項目

[Azure Stack Hub でのログおよび顧客データの処理](azure-stack-data-collection.md)
