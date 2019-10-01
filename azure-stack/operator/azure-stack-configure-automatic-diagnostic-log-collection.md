---
title: 自動 Azure Stack ログ収集の構成 | Microsoft Docs
description: Azure Stack のヘルプとサポートで自動ログ収集を構成する方法。
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
editor: ''
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 07/25/2019
ms.openlocfilehash: 4d6bc431b292fc7a124aa2b8051d0a927d736eee
ms.sourcegitcommit: 4e48f1e5af74712a104eda97757dc5f50a591936
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2019
ms.locfileid: "71224957"
---
# <a name="configure-automatic-azure-stack-diagnostic-log-collection"></a>自動 Azure Stack 診断ログ収集の構成

*適用対象:Azure Stack 統合システム*

ログ収集とカスタマー サポート エクスペリエンスを効率化するために、自動診断ログ収集機能を構成することをお勧めします。 システム正常性状態を調査する必要がある場合、Microsoft カスタマー サポートサービス (CSS) による分析のためにログを自動的にアップロードすることができます。 

## <a name="create-an-azure-blob-container-sas-url"></a>Azure BLOB コンテナー SAS URL の作成 

自動ログ収集を構成する前に、BLOB コンテナーの Shared Access Signature (SAS) を取得する必要があります。 SAS により、アカウント キーを共有することなく、ストレージ アカウントのリソースへのアクセス権を付与できます。 Azure Stack ログファイルを Azure の BLOB コンテナーに保存し、CSS がログを収集できる SAS URL を提供することができます。 

### <a name="prerequisites"></a>前提条件

Azure で新規または既存の BLOB コンテナーを使用できます。 Azure で BLOB コンテナーを作成するには、少なくとも[ストレージ BLOB 共同作成者ロール](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor)または[特定のアクセス許可](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)が必要です。 グローバル管理者にも、必要な権限があります。 

自動ログ収集ストレージ アカウントのパラメーターの選択に関するベスト プラクティスについては、「[Best practices for automatic Azure Stack log collection](azure-stack-best-practices-automatic-diagnostic-log-collection.md)」 (自動 Azure Stack ログ収集のベストプラクティス) を参照してください。 ストレージ アカウントの種類について詳しくは、「[Azure ストレージ アカウントの概要](https://docs.microsoft.com/azure/storage/common/storage-account-overview)」をご覧ください

### <a name="create-a-blob-storage-account"></a>BLOB ストレージ アカウントの作成
 
1. [Azure Portal](https://portal.azure.com) にサインインします。
1. **[ストレージ アカウント]**  >  **[追加]** をクリックします。 
1. 次の設定を使用して BLOB コンテナーを作成します。
   - **サブスクリプション**:Azure サブスクリプションを選びます
   - **[リソース グループ]** :リソース グループを指定します
   - **ストレージ アカウント名**: 一意のストレージ アカウント名を指定します
   - **[場所]** :会社のポリシーに従ってデータセンターを選択します
   - **パフォーマンス**: Standard を選択します
   - **アカウントの種類**: StorageV2 (汎用 v2) を選択します 
   - **[Replication]\(レプリケーション\)** :ローカル冗長ストレージ (LRS) を選択します
   - **アクセス層**:クールを選択します

   ![BLOB コンテナーのプロパティを示すスクリーンショット](media/azure-stack-automatic-log-collection/azure-stack-log-collection-create-storage-account.png)

1. **[確認および作成]** をクリックして、 **[作成]** をクリックします。  

### <a name="create-a-blob-container"></a>BLOB コンテナーを作成する 

1. デプロイが成功したら、 **[リソースに移動]** をクリックします。 また、簡単にアクセスできるように、ストレージ アカウントをダッシュボードにピン留めすることもできます。 
1. **[Storage Explorer (プレビュー)]** をクリックし、 **[BLOB コンテナー]** を右クリックして、 **[BLOB コンテナーの作成]** をクリックします。 
1. 新しいコンテナーの名前を入力し、 **[OK]** をクリックします。

## <a name="create-a-sas-url"></a>SAS URI を作成する

1. コンテナーを右クリックして、 **[Shared Access Signature の取得]** を選択します。 
   
   ![BLOB コンテナーの Shared Access Signature を取得する方法を示すスクリーンショット](media/azure-stack-automatic-log-collection/get-sas.png)

1. 次のプロパティを選択します。
   - 開始時刻:必要に応じて、開始時刻を戻すことができます 
   - 有効期限:2 年間
   - タイム ゾーン:UTC
   - アクセス許可:読み取り、書き込み、および一覧表示

   ![Shared Access Signature のプロパティを示すスクリーンショット](media/azure-stack-automatic-log-collection/sas-properties.png) 

1. **Create** をクリックしてください。  

[自動ログ収集を構成する](azure-stack-configure-automatic-diagnostic-log-collection.md)ときに、URL をコピーして入力します。 SAS URL の詳細については、「[Shared Access Signatures (SAS) の使用](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)」を参照してください。 


## <a name="steps-to-configure-automatic-log-collection"></a>自動ログ収集を構成する手順

次の手順に従って、SAS URL をログ収集 UI に追加します。 

1. Azure Stack 管理者ポータルにサインインします。
1. **[Help and support Overview]\(ヘルプとサポートの概要\)** を開きます。
1. **[Automatic collection settings]\(自動収集の設定\)** をクリックします。

   ![ヘルプとサポートでログ収集を有効にする場所を示すスクリーンショット](media/azure-stack-automatic-log-collection/azure-stack-automatic-log-collection.png)

1. 自動ログ収集を **[有効]** に設定します。
1. ストレージ アカウントの BLOB コンテナーの Shared Access Signature (SAS) URL を入力します。

   ![BLOB の SAS URL を示すスクリーンショット](media/azure-stack-automatic-log-collection/azure-stack-enable-automatic-log-collection.png)

>[!NOTE]
>自動ログ収集は、いつでも無効にして再度有効にすることができます。 SAS URL 構成は変更されません。 自動ログ収集を再び有効にすると、以前に入力した SAS URL で同じ検証チェックが実行され、期限切れの SAS URL が拒否されます。 

## <a name="view-log-collection"></a>ログ収集の表示

Azure Stack から収集されたログの履歴は、[ヘルプとサポート] の **[ログ収集]** ページに次の日付と時刻と共に表示されます。

- **収集時刻**:ログ収集操作が開始されたとき 
- **開始日**:収集する期間の開始
- **終了日**:期間の終了

![ログの収集を示すスクリーンショット](media/azure-stack-automatic-log-collection/azure-stack-log-collection.png)

診断ログの収集に失敗した場合、SAS URL が有効であることを確認します。 エラーが持続する場合、または複数のエラーが表示される場合は、Microsoft に電話で支援を申請してください。 

オペレーターは、自動的に収集されたログについて、ストレージ アカウントを確認することもできます。 たとえば、次のスクリーンショットは、Azure portal の Storage Explorer プレビューを使用して、ログの収集を示しています。

![ログの収集を示すスクリーンショット](media/azure-stack-automatic-log-collection/check-storage-account.png)

## <a name="automatic-diagnostic-log-collection-alerts"></a>自動診断ログ収集アラート 

有効にすると、必要な場合にのみ自動診断ログの収集が行われます。 次のアラートによってのみ、収集がトリガーされます。 

|アラートのタイトル  | FaultIdType|    
|-------------|------------|
|リモート サービスに接続できない |  UsageBridge.NetworkError|
|更新失敗 |    Urp.UpdateFailure   |          
|ストレージ リソース プロバイダーのインフラストラクチャまたは依存関係を利用できない |  StorageResourceProviderDependencyUnavailable     |     
|ノードがコントローラーに接続されていない|  ServerHostNotConnectedToController   |     
|ルート パブリケーションの失敗 |    SlbMuxRoutePublicationFailure | 
|ストレージ リソースプロバイダーの内部データ ストアを利用できない |    StorageResourceProvider. DataStoreConnectionFail     |       
|ストレージ デバイスの障害 | Microsoft.Health.FaultType.VirtualDisks.Detached   |      
|正常性コントローラーが外部ストレージ アカウントにアクセスできない | Microsoft.Health.FaultType.StorageError |    
|物理ディスクへの接続が失われた |    Microsoft.Health.FaultType.PhysicalDisk.LostCommunication    |    
|ノードで Blob service が実行されていない | StorageService.The.blob.service.is.not.running.on.a.node-Critical | 
|インフラストラクチャ ロールの異常 |    Microsoft.Health.FaultType.GenericExceptionFault |        
|Table service のエラー | StorageService.Table.service.errors-Critical |              
|ファイル共有の使用率が 80% を超えている |    Microsoft.Health.FaultType.FileShare.Capacity.Warning.Infra |       
|Scale unit node is offline\(スケール ユニットがオフラインです\) | FRP.Heartbeat.PhysicalNode |  
|インフラストラクチャ ロール インスタンスを利用できない | FRP.Heartbeat.InfraVM   |    
|インフラストラクチャ ロール インスタンスを利用できない  |    FRP.Heartbeat.NonHaVm     |        
|インフラストラクチャ ロールのディレクトリ管理で時刻同期のエラーが報告された |  DirectoryServiceTimeSynchronizationError |     
|保留中の外部証明書の有効期限 |  CertificateExpiration.ExternalCert.Warning |
|保留中の外部証明書の有効期限 |  CertificateExpiration.ExternalCert.Critical |
|メモリ容量不足のため、特定のクラスとサイズの仮想マシンをプロビジョニングできない |  AzureStack.ComputeController.VmCreationFailure.LowMemory |
|Node inaccessible for virtual machine placement\(仮想マシンを配置するためのノードにアクセスできません\) |  AzureStack.ComputeController.HostUnresponsive | 
|バックアップ失敗  | AzureStack.BackupController.BackupFailedGeneralFault |    
|失敗した操作と競合するため、スケジュールされたバックアップがスキップされた  | AzureStack.BackupController.BackupSkippedWithFailedOperationFault |   


## <a name="see-also"></a>関連項目

[Azure Stack のログおよび顧客データの処理](https://docs.microsoft.com/azure-stack/operator/azure-stack-data-collection)

[Shared Access Signatures (SAS) の使用](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)

[自動 Azure Stack ログ収集のベストプラクティス](azure-stack-best-practices-automatic-diagnostic-log-collection.md)





