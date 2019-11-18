---
title: Azure Stack の AKS エンジンのサポート ポリシー | Microsoft Docs
description: このトピックでは Azure Stack の AKS エンジンのサポート ポリシーについて説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na (Kubernetes)
ms.devlang: nav
ms.topic: article
ms.date: 09/14/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 09/14/2019
ms.openlocfilehash: 8c83e0b006904482634e50bdf217ec5d236e4e5c
ms.sourcegitcommit: 5ef433aa6b75cdfb557fab0ef9308ff2118e66e5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2019
ms.locfileid: "73594951"
---
# <a name="support-policies-for-aks-engine-on-azure-stack"></a>Azure Stack の AKS エンジンのサポート ポリシー

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

この記事では、Azure Stack の AKS エンジンのテクニカル サポート ポリシーおよび制限事項の詳細について説明します。 また、Kubernetes Marketplace の項目、サード パーティ製オープン ソース コンポーネント、およびセキュリティまたは修正プログラムの管理についても説明します。 

> [!IMPORTANT]
> AKS エンジンは、現在、パブリック プレビュー段階にあります。
> このプレビュー バージョンはサービス レベル アグリーメントなしで提供されています。運用環境のワークロードに使用することはお勧めできません。 特定の機能はサポート対象ではなく、機能が制限されることがあります。 詳しくは、[Microsoft Azure プレビューの追加使用条件](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)に関するページをご覧ください。

## <a name="self-managed-kubernetes-clusters-on-azure-stack-with-aks-engine"></a>AKS エンジンを使用した Azure Stack の自己管理型 Kubernetes クラスター

コンピューティング コンポーネントやネットワーク コンポーネントなどのサービスとしてのインフラストラクチャ (IaaS) クラウド コンポーネントでは、低レベルの制御とカスタマイズのオプションへのアクセス権がユーザーに付与されます。 AKS エンジンを使用すると、ユーザーはこれらの IaaS コンポーネントを使用して Kubernetes クラスターを透過的に展開できます。ユーザーは、デプロイのすべての側面にアクセスして影響を与えることができます。

クラスターが作成されると、お客様は AKS エンジンで作成される Kubernetes マスターおよびワーカー ノードを定義します。 お客様のワークロードは、これらのノードで実行されます。 お客様はこれらのマスターおよびワーカー ノードを所有し、表示または変更できます。 ノードが不注意に変更された場合、データとワークロードの損失が発生し、クラスターが機能しない状態になる可能性があります。 また、アップグレードやスケールなどの AKS エンジン操作により、バインドされていない変更が上書きされます。 たとえば、クラスターに静的ポッドがある場合、これらは AKS エンジンのアップグレード操作後は保持されません。

お客様のクラスター ノードではプライベート コードが実行され、機密データが格納されるため、Microsoft サポートはこれらのノードへのアクセスを制限されています。 Microsoft サポートは、お客様による明確な許可または支援なしに、これらのノードにサインインしたり、これらのノードでコマンドを実行したり、これらのノードのログを表示することができません。

## <a name="aks-engine-supported-areas"></a>AKS エンジンのサポートされている領域

Microsoft は、以下に関するテクニカル サポートを提供します。

-  AKS エンジン コマンドに関する問題: デプロイ、生成、アップグレード、およびスケール。 このツールは、Azure での動作と一致している必要があります。
-  「[AKS エンジンの概要](azure-stack-kubernetes-aks-engine-overview.md)」に従ってデプロイされた Kubernetes クラスターに関する問題。
-  他の Azure Stack サービスへの接続に関する問題 
-  Kubernetes API 接続に関する問題
-  Azure Resource Manager を使用した Azure Stack Kubernetes プロバイダーの機能と接続に関する問題
-  ロード バランサー、ネットワーク セキュリティ グループ、VNet、サブネット、ネットワーク インターフェイス、ルート テーブル、可用性セット、パブリック IP アドレス、ストレージ アカウント、VM マシンなどの Azure Stack ネイティブな成果物の、AKS エンジンによって生成された構成に関する問題 
-  ネットワーク パフォーマンスと待機時間に関する問題
-  切断されたデプロイで AKS エンジンによって使用される AKS 基本イメージに関する問題 

## <a name="aks-engine-areas-not-supported"></a>AKS エンジンのサポートされていない領域

以下に関するテクニカル サポートは、Microsoft からは提供されません。

-  Azure での AKS エンジンの使用。
-  Azure Stack Kubernetes Marketplace の項目。
-  次の AKS エンジン クラスター定義オプションとアドオンの使用。
    -  サポートされていないアドオン:  
            -  AAD Pod Identity  
            -  ACI Connector  
            -  Blobfuse Flex Volume  
            -  Cluster Autoscaler  
            -  Container Monitoring  
            -  KeyVault Flex Volume  
            -  NVIDIA Device Plugin  
            -  Rescheduler  
            -  SMB Flex Volume  
        
    -  サポートされていないクラスター定義オプション:  
            -  KubernetesConfig の下:  
                    -  cloudControllerManagerConfig  
                    -  enableDataEncryptionAtRest  
                    -  enableEncryptionWithExternalKms  
                    -  enablePodSecurityPolicy  
                    -  etcdEncryptionKey  
                    -  useInstanceMetadata  
                    -  useManagedIdentity  
                    -  azureCNIURLLinux  
                    -  azureCNIURLWindows  
            -  masterProfile の下:  
                    -  availabilityZones  
            -  agentPoolProfiles の下:  
                    -  availabilityZones  
                    -  singlePlacementGroup  
                    -  scaleSetPriority  
                    -  scaleSetEvictionPolicy  
                    -  acceleratedNetworkingEnabled  
                    -  acceleratedNetworkingEnabledWindows

-  Kubernetes 構成の変更は、Kubernetes 構成ストアの etcd の外部で保持されます。 たとえば、クラスターのノードで実行されている静的ポッドなどです。
-  Kubernetes の使用方法に関する質問。 たとえば、Microsoft サポートでは、カスタムのイングレス コント ローラーの作成方法、アプリケーションのワークロードの使用方法、サード パーティまたはオープン ソースのソフトウェア パッケージやツールの適用方法に対するアドバイスは提供していません。
-  AKS エンジンによってデプロイされた Kubernetes クラスターの一部として提供されない、サードパーティのオープンソース プロジェクト。 これらのプロジェクトには、Kubeadm、Kubespray、Native、Istio、Helm、Envoy などが含まれる場合があります。
-  「[AKS エンジンでサポートされるシナリオ](azure-stack-kubernetes-aks-engine-overview.md#supported-scenarios-with-the-aks-engine)」で指定されていないユースケース シナリオでの AKS エンジンの使用。
-  サードパーティ製ソフトウェア。 このソフトウェアには、セキュリティ スキャン ツール、およびネットワーク デバイスまたはソフトウェアが含まれる場合があります。
-  マルチクラウドまたはマルチベンダーの構築に関する問題。 たとえば、Microsoft では、フェデレーション マルチパブリック クラウド ベンダーのソリューションの実行に関連する問題はサポートしていません。
-  「[AKS エンジンのサポートされている領域](#aks-engine-supported-areas)」に記載されている以外のネットワークのカスタマイズ。

##  <a name="security-issues-and-patching"></a>セキュリティの問題と修正プログラムの適用

AKS エンジンまたは Azure Stack 用 Kubernetes プロバイダーの 1 つ以上のコンポーネントでセキュリティ上の欠陥が見つかった場合、Microsoft は問題を軽減するために、影響を受けるクラスターを修正するための修正プログラムを提供します。 または、チームがユーザーにアップグレード ガイダンスを示します。 修正プログラムではクラスターのダウンタイムが必要になる場合があることに注意してください。 再起動が必要な場合、Microsoft はこの要件をお客様に通知します。 ユーザーが Microsoft のガイダンスに従って修正プログラムを適用しない場合、クラスターはセキュリティ問題に対して脆弱なままになります。

## <a name="kubernetes-marketplace-item"></a>Kubernetes Marketplace 項目

ユーザーは、Kubernetes Marketplace アイテムをダウンロードできます。これにより、ユーザーは、AKS エンジンを使用して Kubernetes クラスターを、Azure Stack ユーザー ポータルのテンプレートを通じて間接的にデプロイできます。これは、AKS エンジンを直接使用するよりも簡単です。 これは、デモンストレーション、テスト、および開発のためにクラスターをすばやく設定するのに便利なツールです。 Microsoft によってサポートされる項目のセットに含まれていないため、運用環境向けではありません。

## <a name="preview-features"></a>プレビュー機能

拡張テストとユーザー フィードバックを必要とする機能の場合、Microsoft では、新しいプレビュー機能または機能フラグの背後にある機能をリリースします。 これらの機能は、プレリリースまたはベータ機能としてお考えください。 プレビュー機能または機能フラグは、運用環境向けではありません。 機能の継続的な変更と動作、バグの修正、その他の変更により、クラスターが不安定になったり、ダウンタイムが生じたりする可能性があります。 これらの機能は Microsoft ではサポートされていません。

## <a name="next-steps"></a>次の手順

- [Azure Stack の AKS エンジン](azure-stack-kubernetes-aks-engine-overview.md)に関するページを読む