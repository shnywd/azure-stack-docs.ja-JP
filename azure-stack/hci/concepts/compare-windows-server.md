---
title: Azure Stack HCI と Azure Stack Hub および Windows Server の比較
description: このトピックは、Azure Stack HCI、Azure Stack Hub、または Windows Server がご自分の組織に適しているかどうかを判断するのに役立ちます。
ms.topic: conceptual
author: khdownie
ms.author: v-kedow
ms.service: azure-stack
ms.subservice: azure-stack-hci
ms.date: 12/11/2020
ms.openlocfilehash: bd1798ddfcf0897eeaedaeaae0cfb02eac1aaa1e
ms.sourcegitcommit: 79e8df69b139bfa21eb83aceb824b97e7f418c03
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2020
ms.locfileid: "97365160"
---
# <a name="compare-azure-stack-hci-to-azure-stack-hub-and-windows-server"></a>Azure Stack HCI と Azure Stack Hub および Windows Server の比較

> 適用対象:Azure Stack HCI バージョン 20H2、Windows Server 2019、Azure Stack Hub

多くのお客様は、Windows Server、Azure Stack HCI、または Azure Stack Hub のどれがご自分のニーズに合うのか迷われるでしょう。 このトピックは、ご自分の組織に適したものを判断するのに役立ちます。 

## <a name="compare-azure-stack-hci-to-windows-server"></a>Azure Stack HCI と Windows Server の比較

Windows Server と Azure Stack HCI のどちらでも、新しいリリースのロード マップに従って、同じ高品質のユーザー エクスペリエンスが提供されます。

| Windows Server | Azure Stack HCI |
| --------------- | --------------- |
| ゲストおよび従来のサーバーとして最適です | 記憶域スペース ダイレクトなど、ソフトウェアによるデータ センター用の仮想化ホストに最適です |
| 従来のソフトウェア ライセンス モデルを使用して、どこでも実行できます | 好みのベンダーのハードウェアで実行できますが、Azure サービスとして提供され、Azure アカウントに課金されます |
| 2 つのインストール オプション:デスクトップ エクスペリエンスを備えたサーバーまたはサーバー コア | わずかにカスタマイズされたサーバー コアに基づきます |

### <a name="when-to-use-windows-server"></a>Windows Server を使用する場合

| Windows Server | Azure Stack HCI |
| --------------- | --------------- |
| Windows Server は、非常に汎用性のある多目的なオペレーティング システムであり、ゲスト権限など、数十の役割と数百の機能を備えています。 | Azure Stack HCI にはゲスト権限は含まれておらず、最新のハイパーコンバージド アーキテクチャに使用することが意図されています。 |
| VM を実行する場合、または Active Directory、ファイル サービス、DNS、DHCP、インターネット インフォメーション サービス (IIS)、コンテナー ホストおよびゲスト、SQL Server、Exchange Server、ホスト ガーディアン サービス (HGS) など、従来のすべてのサーバーの役割を含むベア メタル インストール用には、Windows Server を使用します。 | Hyper-V 仮想化ホストとして意図されている Azure Stack HCI は、少数のサーバーの役割を直接実行するためだけにライセンスを付与されます。他のロールはすべて、VM 内で実行する必要があります。 |

### <a name="when-to-use-azure-stack-hci"></a>Azure Stack HCI を使用する局面

| Windows Server | Azure Stack HCI |
| --------------- | --------------- |
| Windows Server はオンプレミスまたはクラウドで実行できますが、それ自体は完全なハイパーコンバージド オファリングではありません。| オンプレミスで VM を実行し、必要に応じて 2 つのサイトに拡張して Azure ハイブリッド サービスへの接続を使用するには、Azure Stack HCI を使用します。 データ センターやブランチ オフィスを最新化してセキュリティで保護し、SQL Server データベースで業界最高のパフォーマンスを実現し、低待機時間とデータ主権のためにオンプレミスで仮想デスクトップを実行するための、簡単な方法です|
| Windows Server は、仮想化されているかどうかに関係なく、Windows Server のすべての役割を対象とする優れた多目的の "万能製品" です。 | Exchange、SharePoint、SQL Server などの従来のエンタープライズ アプリを仮想化する場合、またはファイル サーバー、DNS、DHCP、IIS、AD などの Windows Server の役割を仮想化する場合は、Azure Stack HCI を使用します。 シールドされた VM など、すべての Hyper-V 機能への無制限のアクセスが含まれます。|
| Windows Server の多くの展開は、古いハードウェアで実行されています。 | 大規模な再設計なしで、古いストレージ アレイやネットワーク アプライアンスの代わりにソフトウェアによるインフラストラクチャを使用する場合に、Azure Stack HCI を使用します。 組み込みの Hyper-V、記憶域スペース ダイレクト、およびソフトウェアによるネットワーク (SDN) は直接アクセスして管理できます。 Windows VM または Linux VM 内でアプリを実行します。|

## <a name="compare-azure-stack-hci-to-azure-stack-hub"></a>Azure Stack HCI と Azure Stack Hub の比較

お客様の組織がデジタル変革を進める中で、パブリック クラウド サービスを使用して、最新のアーキテクチャ上に構築し、従来のアプリを更新することで、より迅速に前進できることに気付くことがあります。 ただし、技術上の障害や規制の問題などの理由により、多くのワークロードをオンプレミスに残す必要があります。 次の表を使用すると、必要なところに必要なものを提供し、ワークロードの場所に関係なくそのクラウド イノベーションを実現する Microsoft ハイブリッド クラウド戦略を判断するのに役立ちます。

| Azure Stack Hub | Azure Stack HCI |
| --------------- | --------------- |
| 新しいスキル、革新的なプロセス | 同じスキル、使い慣れたプロセス |
| お客様のデータセンター内での Azure サービス | お客様のデータセンターを Azure サービスに接続する |

### <a name="when-to-use-azure-stack-hub"></a>Azure Stack Hub を使用する局面

| Azure Stack Hub | Azure Stack HCI |
| --------------- | --------------- |
| 併置された複数のテナントの強力な分離および正確な使用状況の追跡とチャージバックを実現するセルフサービスのインフラストラクチャとしてのサービス (IaaS) に Azure Stack Hub を使用します。 サービス プロバイダーやエンタープライズ プライベート クラウドに最適です。 テンプレートは Azure Marketplace にあります。 | Azure Stack HCI では、マルチテナントをネイティブに適用したり、提供したりすることはありません。 |
| オンプレミスで Web Apps、Functions、Event Hubs などのプラットフォームとしてのサービス (PaaS) サービスを利用するアプリの開発と実行に Azure Stack Hub を使用します。 これらのサービスは、Azure 内とまったく同じように Azure Stack Hub 上で実行され、一貫性のあるハイブリッドの開発およびランタイム環境を提供します。 | Azure Stack HCI では、PaaS サービスをオンプレミスで実行しません。 |
| DevOps プラクティス (コードとしてのインフラストラクチャ、継続的インテグレーションと継続的デプロイ (CI/CD) など) と便利な機能 (Azure と整合性のある VM 拡張機能など) を利用したアプリのデプロイと操作の現代化に Azure Stack Hub を使用します。 開発チームや DevOps チームに最適です。 | Azure Stack HCI には、DevOps ツールがネイティブに含まれていません。 |

### <a name="when-to-use-azure-stack-hci"></a>Azure Stack HCI を使用する局面

| Azure Stack Hub | Azure Stack HCI |
| --------------- | --------------- |
| Azure Stack Hub には、最小 4 つのノードとそれ専用のネットワーク スイッチが必要です。 | リモート オフィスやブランチの場合に占有領域を最小限に抑えるには Azure Stack HCI を使用します。 たった 2 個のサーバー ノードとスイッチレスのバックツーバック ネットワークから開始でき、ピークがわかりやすく、価格が手頃です。 ハードウェアのオファーは、ノードあたり 4 台のドライブ、64 GB のメモリ、10,000 ドル未満からです。 |
| Azure Stack Hub では、Azure との整合性を確保するために、Hyper-V の構成機能と機能セットが制限されています。 | Exchange、SharePoint、SQL Server などの従来のエンタープライズ アプリ向けの必要最低限の Hyper-V 仮想化や、ファイル サーバー、DNS、DHCP、IIS、AD などの Windows Server ロールの仮想化に、Azure Stack HCI を使用します。 シールドされた VM など、すべての Hyper-V 機能に無制限にアクセスできます。|
| Azure Stack Hub では、これらのインフラストラクチャ テクノロジは公開されません。 | 大規模な再設計なしで、古いストレージ アレイやネットワーク アプライアンスの代わりにソフトウェアによるインフラストラクチャを使用する場合に、Azure Stack HCI を使用します。 組み込みの Hyper-V、記憶域スペース ダイレクト、およびソフトウェアによるネットワーク (SDN) は直接アクセスして管理できます。 |

## <a name="next-steps"></a>次の手順

- [Azure Stack HCI とは](../overview.md)
