---
title: お客様による Azure Stack Hub の購入からデプロイ後まで | Microsoft Docs
description: 計画からデプロイ後まで、Modular Data Center (MDC) のオンサイトのデプロイを成功させるために必要なことについて説明します。
services: azure-stack
documentationcenter: ''
author: ashika789
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2020
ms.author: justinha
ms.reviewer: asganesh
ms.lastreviewed: 11/04/2020
ms.openlocfilehash: 12631a118600e67611294c87636cd2fa62bfaaea
ms.sourcegitcommit: ecd98662194d2cdb15c22f8b1f99812fc5f4c15a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/04/2020
ms.locfileid: "93344781"
---
# <a name="modular-data-center-integration-overview"></a>Modular Data Center の統合の概要

この記事では、購入からデプロイ後まで、MDC の統合についてエンドツーエンドのプロセスを説明します。 統合は、お客様と Microsoft による共同プロジェクトです。 以下のセクションでは、プロジェクトのタイムラインに沿ったさまざまなフェーズと、プロジェクト メンバーのための固有の手順について説明します。

## <a name="introduction"></a>はじめに

次の表は、デプロイのさまざまなフェーズで予想できることを示したものです。

|   |注文プロセス  |デプロイ前 |統合、検証、輸送 |オンサイト デプロイ  |配置後 |
|---|---------------|---------------|-----------------------------------|--------------------|----------------|
|Microsoft  |- 米国の場所への配送を通知する    |データセンターの要件を収集するため必要なツールとドキュメントを提供する  |- 構成成果物を検証し、検証結果を確認する<br>- ハードウェアが配送されたことを確認する  |- ラックとスタック<br>- ネットワーク統合<br>- Azure Stack Hub のデプロイ<br>- お客様に引き渡す    |登録との Marketplace シンジケーション|
|Customer   |購入を通知する   |- デプロイ ワークシートにネットワークの詳細を入力する<br>- 証明書を収集する<br>- Azure AD アカウントを取得する<br>- 提供されたすべての検証ツールを実行する    |サイトでネットワーク、電力、冷却の前提条件の準備ができていることを確認する    |- デプロイ構成成果物を準備する<br>- お客様のネットワーク エンジニアが利用できる   |     |


## <a name="order-process"></a>注文プロセス

お客様の組織は、Microsoft と協力して、割り当てられた数のシステムを発注します。 注文が済むと、Microsoft によりお客様の米国内の場所に MDC が配送されます。 Microsoft は、セキュリティで保護されたサプライチェーンのすべての要件が満たされていることを確認します。 

## <a name="hardware-delivery"></a>ハードウェア納入

Microsoft は所定の時間内にすべての必要なハードウェアが米国内の場所に到着するように、お客様と協力します。  

" *オンサイトの Microsoft デプロイ エンジニアがソリューションをデプロイするために到着する前に* "、すべての前提条件データをロックして使用可能にしておくことが **重要です** 。

- デプロイ ワークシートにすべてのデータを入力しておきます。 
- すべての証明書を検証して準備しておく必要があります。
- リージョン名を決定しておく必要があります。
- すべてのネットワーク統合パラメーターを決定しておきます。

>[!Tip]
>この情報のいずれかが変更されている場合は、社内組織と協力して、オンサイト デプロイ エンジニアが到着する前に、情報を確実に更新します。 これにより、デプロイ プロセスの遅延が回避されます。

## <a name="pre-deployment"></a>デプロイ前準備

Azure Stack Hub をデータセンターと統合する方法について決定します。 Microsoft からは、データセンターに正常に統合するために必要なすべての情報を収集する手順を案内する[デプロイ ワークシート](../operator/azure-stack-deployment-worksheet.md)が公開されています。 また、デプロイ時には、特定の証明書のセットが必要です。 これらの証明書の取得に役立つよう、Microsoft からは [Azure Stack Hub Readiness Checker](../operator/azure-stack-validation-report.md) というツールが提供されています。 このツールを使用して、内部 CA に提供する証明書署名要求 (CSR) を作成できます。 

>[!Important]
>デプロイが遅れるのを防ぐため、すべての前提条件が検証されます。 前提条件の確認には時間がかかることがあり、組織内のさまざまな部門からの調整とデータ収集が必要になります。

次の項目を選択します。

- **Azure Stack Hub の接続モデルと ID プロバイダー。** Azure Stack Hub を[インターネット (および Azure) に接続した](../operator/azure-stack-connected-deployment.md)状態でデプロイするか、[接続していない](../operator/azure-stack-disconnected-deployment.md)状態でデプロイするかを選択できます。 ハイブリッド シナリオを含めて、Azure Stack Hub から最大のメリットを得るには、Azure に接続した状態でデプロイしてください。 Active Directory フェデレーション サービス (AD FS) または Azure Active Directory (Azure AD) の選択は、デプロイ時に行う必要がある 1 回限りの決定です。 **システム全体を再デプロイせずに、後で ID プロバイダーを変更することはできません。**
- **ネットワーク統合。** [ネットワーク統合](../operator/azure-stack-network.md)は、Azure Stack Hub システムのデプロイ、操作、管理に非常に重要です。 Azure Stack Hub ソリューションに回復力があり、運用をサポートする高可用な物理インフラストラクチャがあることを確実にする、いくつかの考慮事項があります。
- **ファイアウォールの統合。** Azure Stack Hub は、[ファイアウォールを使用](../operator/azure-stack-firewall.md)してセキュリティで保護することをお勧めします。 ファイアウォールは、DDOS 攻撃の防止、侵入検出、およびコンテンツ検査に役立ちます。 ただし、Azure ストレージ サービスのスループットのボトルネックになる可能性があることに注意してください。
- **証明書の要件。** デプロイのためにデータセンターにオンサイト エンジニアが到着する前に、すべての[必要な証明書](../operator/azure-stack-pki-certs.md)が使用可能になっていることが重要です。

デプロイ ワークシートを使用して前提条件のすべての情報が収集されたら、Microsoft により、すべての検証ツールが実行されたことが確認され、お客様にさらに質問がある場合はその支援が行われます。 

## <a name="onsite-deployment"></a>オンサイト デプロイ

Azure Stack Hub をデプロイするには、Microsoft のデプロイ エンジニアがその場でデプロイを開始します。 また、オンサイトのデプロイの間は、お客様の組織のネットワーク エンジニアも対応できるようにする必要があります。

次のチェックは、デプロイの実施中にオンサイト エンジニアに期待できる事柄です。

- ハードウェアの開梱と内容確認
- 電源の接続と、ソリューションの電源オン
- 物理ハードウェアの正常性の検証
- すべてのケーブル配線と境界接続をチェックして、ソリューションが適切に構築され要件が満たされることを確認します。
- HLH (ハードウェア ライフサイクル ホスト) ソリューションを構成します。
- データセンターのネットワーク統合
- 物理ハードウェアのすべての設定が正しいことを確認します
- すべてのコンポーネントのファームウェアがソリューションにより承認済みの最新バージョンであることを確認します
- デプロイを開始する

## <a name="post-deployment"></a>配置後

ソリューションをお客様に引き渡す前に、Microsoft のデプロイ エンジニアはいくつかのステップを実行する必要があります。 このフェーズでは、システムが正しくデプロイされて動作することを確認するための検証が重要です。

Microsoft のデプロイ エンジニアが行う必要のある操作:

- 付加価値リソース プロバイダー (RP) を有効にします
- [test-azurestack](../operator/azure-stack-diagnostic-test.md) を実行します
- Azure に[登録](../operator/azure-stack-registration-role.md)します
- [Marketplace シンジケーション](../operator/azure-stack-marketplace.md)
- スイッチ構成ファイルと HLH 構成ファイルをバックアップします
- デプロイの顧客概要の準備
- ソリューションのソフトウェアが必ず最新バージョンに更新されているように、[更新プログラムを確認](../operator/azure-stack-updates.md)します

## <a name="next-steps"></a>次のステップ

[Modular Data Center のデプロイの概要](deployment-overview.md)。

