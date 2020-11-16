---
title: Modular Data Center の概要 |
description: Modular Data Center は、一時的または固定の指令拠点における大規模な戦闘活動を支援するための移植可能かつ迅速にデプロイ可能なデータセンターです。
author: BryanLa
ms.author: bryanla
ms.service: azure-stack
ms.topic: overview
ms.date: 11/06/2020
ms.reviewer: prchint
ms.lastreviewed: 11/06/2020
ms.openlocfilehash: eaa84fb673863935dc4778f34a6fce592590a7a8
ms.sourcegitcommit: ce864e1d86ad05a03fe896721dea8f0cce92085f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/09/2020
ms.locfileid: "94383406"
---
# <a name="modular-data-center-overview"></a>Modular Data Center の概要 

Modular Data Center (MDC) は Azure Stack Hub に基づいています。 MDC は、一時的または固定の指令拠点における大規模な戦闘活動を支援するための移植可能かつ迅速にデプロイ可能なデータセンターです。

Azure Stack Hub は水平方向および垂直方向のスケーラビリティを備えたソリューションで、IaaS および PaaS サービス用のマルチテナントのネイティブ ハイブリッド クラウド機能をエッジで環境に提供し、一時的および固定の指令拠点や遠征軍におけるさまざまなモジュール式のシナリオをサポートします。 Azure Stack Hub はノード スケール ユニットを使用したスケーリングに基づくさまざまな容量で販売されている統合されたハードウェアおよびソフトウェア アプライアンスであり、汎用グラフィックス処理装置 (GPU) 対応の構成や拡張可能な外部ストレージなどの拡張機能と共に提供されています。

## <a name="how-you-can-use-the-mdc"></a>MDC の使用方法

Azure Stack は、モジュール式エッジのシナリオ向けの Azure の機能に整合し、それを拡張する 4 つの原則を実現します。 

### <a name="develop-and-deliver-apps-with-a-common-devops-model-including-api-symmetry-with-azure"></a>Azure との API の対称性を含む、共通の DevOps モデルを使用してアプリを開発および提供する

Azure と Azure Stack の間の整合性により、ソリューションがいったん開発され、さまざまなユースケースをサポートするためにデプロイされると、キー管理用の Azure Key Vault やリソース監視および管理用の Azure Monitor などの共通のツール一式を使用してセキュリティ保護され、維持されます。 Azure Stack は、オンプレミス データ、アプリ、DevOps ツール、セキュリティで保護された操作 (キー管理など) と相互運用できます。

### <a name="deliver-azure-services-on-premises"></a>Azure サービスをオンプレミスで提供する

Azure Stack は、悪条件 (競合、混雑、拒否) の通信環境でも堅牢な通信環境でも実行でき、重要なアプリの実行やローカル操作の有効化において Azure への接続に依存しません。 

### <a name="use-integrated-hardware-and-software-delivery-experience"></a>統合されたハードウェアおよびソフトウェアの提供エクスペリエンスを使用する

モジュール式エッジでは、ベースライン コンピューティングやストレージ サービスだけでなく、機械学習、AI、分析の高度な機能を提供する幅広い機能が必要です。 モジュール式エッジからセキュアなクラウドに接続する、または逆に、それとは個別に制限された環境から操作できる機能は、意思決定に必要なデータにアクセスする上で必要不可欠です。

### <a name="keep-your-datacenter-secure-and-available-with-hybrid-cloud-security-operations"></a>ハイブリッド クラウド セキュリティ運用によりデータセンターの安全性と可用性を維持する

Azure Stack のクラウド ネイティブな設計では、管理者が修正プログラムを適用するタイミングを選択できることと、インストール プロセスおよびシステムにネイティブに組み込まれた Azure Stack 管理ファブリックを使用して運用全体を調整することにより、従来の仮想環境の運用上の複雑さが解消されています。

Azure Stack デバイスに対する Microsoft 製または Microsoft が管理するオファリングによる現場でのリモート支援管理は、管理ポータル、特権管理エンドポイント、またはコマンド ライン インターフェイスを使用してユーザーに適切なアクセス権を付与することで有効にできます。 これにより、修正プログラムの適用やその他の管理および監視アクティビティをすべて Microsoft が実行できるようになります。 Azure Stack は、セキュリティで保護された管理ポータルまたはコマンド ライン インターフェイス (CLI) コマンドを使用して現場でアップグレードおよびアップデート可能です。これらは、両方とも、必要に応じてローカルまたはリモート ネットワークから Azure を使用して商用パリティを維持するために、セキュリテ更新プログラムや IaaS および PaaS 機能の更新プログラムを適用します。 

## <a name="benefits-of-using-the-mdc"></a>MDC を使用する利点

MDC は、条件の悪い通信環境において Azure を使用して一貫した環境をサポートします。
 - モジュール式オペレーション センター (TOC) で大規模な分析アプリを実行するために、Azure クラウド サービスを使用して迅速にデプロイできる静的なモジュール式データ センターです。
 - Azure のモジュール式エッジ オファリングでは、仮想マシン、ストレージ、仮想ネットワークなどの IaaS プリミティブをサポートするための 1 つのアプローチを使用して、クラウドとエッジの間の整合性を維持します
 - Azure Active Directory とロールベースのアクセス制御のサポート
 - 共通の管理インターフェイス
 - API の対称性と Microsoft、サードパーティ、およびオープンソースの DevOps ツールのサポート
 - Azure Log Analytics および Azure Security Center を使用した管理と監視
 - ハイブリッド クラウドを有効にすることにより、クラウド コンピューティングのアジリティをオンプレミス環境とエッジにもたらします。<br>次のようにすることができます。
     - コードを再利用し、Azure とオンプレミス環境全体でクラウド ネイティブ アプリを一貫した方法で実行する。
     - Azure サービスへのオプション接続を使用して、従来の仮想化されたワークロードを実行する。
     - データをクラウドに転送するか、専用のデータセンターに保管してコンプライアンスを維持する。
     - ハードウェア アクセラレータによる機械学習、コンテナー化、または仮想化されたワークロードをすべてインテリジェント エッジで実行する。

## <a name="next-steps"></a>次のステップ

[Azure Stack Hub のキャパシティ プランニング](../operator/azure-stack-capacity-planning-overview.md)
