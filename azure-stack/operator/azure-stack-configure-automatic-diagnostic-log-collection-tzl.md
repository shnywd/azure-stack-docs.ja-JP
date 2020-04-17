---
title: Azure Stack Hub でアラートが発生したときに診断ログを事前に収集する
description: Azure Stack Hub のヘルプとサポートで事前ログ収集を構成する方法。
author: justinha
ms.topic: article
ms.date: 02/12/2020
ms.author: justinha
ms.reviewer: shisab
ms.lastreviewed: 02/12/2020
ms.openlocfilehash: 0c6bfe4dba2b3795e1069419d90624c8e3b55cda
ms.sourcegitcommit: a630894e5a38666c24e7be350f4691ffce81ab81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "79520544"
---
# <a name="send-azure-stack-hub-diagnostic-logs-proactively"></a>Azure Stack Hub 診断ログを事前に送信する

Azure Stack Hub でアラートが発生したときに診断ログを事前に収集することで、カスタマー サポートに要する時間を節約することができます。
システム正常性状態を調査する必要がある場合、サポート ケースを開く前に Microsoft カスタマー サポートサービス (CSS) により分析のためにログを自動的にアップロードできます。 

## <a name="steps-to-configure-proactive-log-collection"></a>事前ログ収集を構成する手順

事前ログ収集を構成するには、次の手順に従います。 自動ログ収集は、いつでも無効にして再度有効にすることができます。  

1. Azure Stack Hub 管理者ポータルにサインインします。
1. **[Help and support Overview]\(ヘルプとサポートの概要\)** を開きます。
1. バナーが表示されたら、 **[Enable proactive log collection]\(事前ログ収集を有効にする\)** をクリックします。 

   ![ヘルプとサポートでログ収集を有効にする場所を示すスクリーンショット](media/azure-stack-help-and-support/banner-enable-automatic-log-collection.png)


   または、 **[設定]** をクリックして **[Proactive log collection]\(事前ログ収集\)** を **[有効]** にし、 **[保存]** をクリックすることもできます。

   ![ヘルプとサポートでログ収集を有効にする場所を示すスクリーンショット](media/azure-stack-help-and-support/settings-enable-automatic-log-collection.png)


## <a name="view-log-collection"></a>ログ収集の表示

Azure Stack Hub から収集されたログの履歴は、[ヘルプとサポート] の**ログ収集**ページに次の日付と時刻と共に表示されます。

- **収集された時刻**:ログ収集操作が開始されたとき
- **状態**: 進行中または完了
- **ログの開始**:収集する期間の開始
- **ログの終了**:期間の終了
- **[種類]** :手動ログ収集または事前ログ収集のいずれか 


![ログの収集を示すスクリーンショット](media/azure-stack-help-and-support/azure-stack-log-collection.png)


## <a name="proactive-diagnostic-log-collection-alerts"></a>事前診断ログ収集アラート 

有効にすると、次のいずれかのイベントが発生した場合にのみ、事前ログ収集によってログがアップロードされます。 

たとえば、**更新失敗**は、事前診断ログ収集をトリガーするアラートです。 これが有効になっていると、CSS による問題のトラブルシューティングに役立つように、更新エラーの間に前もって診断ログがキャプチャされます。 診断ログは、**更新失敗**のアラートが発生した場合にだけ収集されます。 

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

[Azure Stack Hub でのログおよび顧客データの処理](azure-stack-data-collection.md)





