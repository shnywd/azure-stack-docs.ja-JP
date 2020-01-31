---
title: 自動 Azure Stack Hub ログ収集のベスト プラクティス
description: Azure Stack Hub のヘルプとサポートの自動ログ収集のベスト プラクティス
author: justinha
ms.topic: article
ms.date: 07/25/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 07/25/2019
ms.openlocfilehash: 56af62717b4cd32d7a5130d1d324bd40bf61adb9
ms.sourcegitcommit: fd5d217d3a8adeec2f04b74d4728e709a4a95790
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76878430"
---
# <a name="best-practices-for-automatic-azure-stack-hub-log-collection"></a>自動 Azure Stack Hub ログ収集のベスト プラクティス 

このトピックでは、Azure Stack Hub の自動診断ログ収集を管理するためのベスト プラクティスについて説明します。 

## <a name="collecting-logs-from-multiple-azure-stack-hub-systems"></a>複数の Azure Stack Hub システムからのログの収集

ログを収集する Azure Stack Hub スケールユニットごとに 1 つの BLOB コンテナーを設定します。 BLOB コンテナーを構成する方法の詳細については、「[自動 Azure Stack Hub 診断ログ収集の構成](azure-stack-configure-automatic-diagnostic-log-collection.md)」を参照してください。 ベスト プラクティスとして、単一の BLOB コンテナー内の同じ Azure Stack Hub スケール ユニットからの診断ログのみを保存します。 

## <a name="retention-policy"></a>保持ポリシー

Azure Blob Storage [ライフサイクル管理ルール](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts)を作成して、ログの保持ポリシーを管理します。 診断ログは 30 日間保持することをお勧めします。 Azure storage でライフサイクル管理ルールを作成するには、Azure portal にサインインし、 **[ストレージ アカウント]** をクリックし、BLOB コンテナーをクリックし、 **[Blob service]** の **[ライフサイクル管理]** をクリックします。

![Azure portal のライフサイクル管理を示すスクリーンショット](media/azure-stack-automatic-log-collection/blob-storage-lifecycle-management.png)


## <a name="sas-token-expiration"></a>SAS トークンの有効期限

SAS URL の有効期限を 2 年に設定します。 ストレージ アカウント キーを更新する場合は、必ず SAS URL を再生成してください。 SAS トークンは、ベストプラクティスに従って管理する必要があります。 詳細については、「[SAS を使用する際のベスト プラクティス](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#best-practices-when-using-sas)」を参照してください。


## <a name="bandwidth-consumption"></a>帯域幅の消費

診断ログ収集の平均サイズは、ログ収集がオンデマンドであるか自動であるかによって異なります。 

オンデマンドのログ収集の場合、ログ収集のサイズは、収集される時間数によって異なります。 過去 7 日間の 1 時間から 4 時間のスライディング ウィンドウを選択できます。 

自動診断ログ収集が有効になっている場合、サービスによって重大なアラートが監視されます。 重大なアラートが発生し、約 30 分間持続すると、サービスによって適切なログが収集され、アップロードされます。 このログ収集のサイズは、平均で約 2 GB です。 修正プログラムや更新のエラーが発生した場合、自動ログ収集は、重大なアラートが発生し、約 30 分間持続している場合のみ開始されます。 [修正プログラムと更新の監視に関するガイダンス](azure-stack-updates.md)に従うことをお勧めします。
アラートの監視、ログ収集、およびアップロードは、ユーザーに対して透過的です。 



正常なシステムでは、ログはまったく収集されません。 異常なシステムでは、ログ収集が 1 日に 2、3 回実行される場合がありますが、通常は 1 回だけ実行されます。 多くて、最悪のシナリオで 1 日に最大 10 回実行される可能性があります。  

次の表は、Azure への接続が制限されている環境で、自動ログ収集を有効にする影響を考慮する場合に役立つ可能性があります。

| ネットワーク接続 | 影響 |
|--------------------|--------|
| 低帯域幅/待機時間が長い接続 | ログのアップロードは、完了までに長時間かかります | 
| 共有接続 | アップロードは、ネットワーク接続を共有する他のアプリケーションやユーザーに影響を与える場合があります |
| 従量制課金接続 | 追加のネットワーク使用に対して、ISP からの追加料金が発生する場合があります |


## <a name="managing-costs"></a>コストの管理

Azure [BLOB ストレージの料金](https://azure.microsoft.com/pricing/details/storage/blobs/)は、毎月保存されるデータの量と、データの冗長性などのその他の要因によって異なります。 既存のストレージアカウントがない場合は、Azure portal にサインインし、 **[ストレージ アカウント]** をクリックし、手順に従って、[Azure BLOB コンテナーの SAS URL を作成](azure-stack-configure-automatic-diagnostic-log-collection.md)します。

ベスト プラクティスとして、Azure BLOB ストレージ [ライフサイクル管理ポリシー](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts)を作成して、継続的なストレージ コストを最小限に抑えます。 ストレージ アカウントを設定する方法の詳細については、「[自動 Azure Stack Hub 診断ログ収集の構成](azure-stack-configure-automatic-diagnostic-log-collection.md)」を参照してください

## <a name="see-also"></a>参照

[自動 Azure Stack Hub ログ収集の構成](azure-stack-best-practices-automatic-diagnostic-log-collection.md)

