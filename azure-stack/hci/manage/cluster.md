---
title: Windows Admin Center を使用して Azure Stack HCI クラスターを管理する
description: Azure Stack HCI 上のクラスターを Windows Admin Center を使用して管理する方法について説明します。
ms.topic: how-to
author: v-dasis
ms.author: v-dasis
ms.reviewer: jgerend
ms.date: 05/20/2020
ms.openlocfilehash: 63290b49558d85e0869664c6ca760ae492718c34
ms.sourcegitcommit: 76af742a42e807c400474a337e29d088ede8a60d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/22/2020
ms.locfileid: "85196479"
---
# <a name="manage-azure-stack-hci-clusters-using-windows-admin-center"></a>Windows Admin Center を使用して Azure Stack HCI クラスターを管理する

> 適用対象:Windows Server 2019

Azure Stack HCI で Windows Admin Center を使用してクラスターを管理できます。 具体的には、Windows Admin Center のクラスター マネージャー拡張機能を使用して、クラスターを管理します。

## <a name="view-the-cluster-dashboard"></a>クラスター ダッシュボードを表示する

クラスター ダッシュボードには、クラスターの正常性とパフォーマンスに関する情報が表示されます。

:::image type="content" source="media/manage-cluster/cluster-dashboard.png" alt-text="クラスター ダッシュボード画面":::

この情報を表示するには、 **[すべての接続]** の下のクラスター名を選択し、左側の **[ツール]** の下の **[ダッシュボード]** を選択します。 次の情報を表示できます。

- クラスター イベント アラート
- クラスターに参加しているサーバーの一覧
- クラスターで実行されている仮想マシンの一覧
- クラスターで使用可能なディスク ドライブの一覧
- クラスターで使用可能なボリュームの一覧
- クラスターの合計クラスター CPU 使用率
- クラスターの合計クラスター メモリ使用量
- クラスターの合計クラスター ストレージ使用率
- クラスター入力/出力操作の合計数/秒 (IOPS)
- クラスターの平均待機時間 (ミリ秒)

## <a name="change-cluster-general-settings"></a>クラスターの全般設定を変更する

クラスターに適用できる全般設定は 5 つあります。

1. Windows Admin Center で、上部のドロップダウン矢印から **[クラスター マネージャー]** をクリックします。
1. **[ツール]** の下の **[設定]** をクリックします。
1. クラスター名を変更するには、 **[アクセス ポイント]** を選択し、新しい名前を入力します。

    :::image type="content" source="media/manage-cluster/cluster-settings-access.png" alt-text="クラスターの [アクセス ポイント] 画面":::

1. ノードのシャットダウン動作を制御するには、 **[ノードのシャットダウン動作]** を選択し、チェックボックスがオンになっていることを確認します。 これにより、ノードから任意の仮想マシンを先に移動して、ノードのグレースフル シャットダウンが可能になります。

    :::image type="content" source="media/manage-cluster/cluster-settings-shutdown.png" alt-text="クラスターの [ノードのシャットダウン動作] 画面":::

1. クラスター ノード間でデータを送信するために使用される SMB 接続を暗号化するには、 **[Cluster traffic encryption]\(クラスターのトラフィックの暗号化\)** を選択し、次のドロップダウン ボックスから **[暗号化]** を選択します。

   - **コア トラフィック** - NetFT (クラスター仮想アダプター) を介してポート 3343 で送信されるトラフィックを暗号化します。

   - **サーバー トラフィック** - クラスター共有ボリューム (CSV) とストレージ バス層 (SBL) のトラフィックを暗号化します。

        :::image type="content" source="media/manage-cluster/cluster-settings-encryption.png" alt-text="クラスターの [Cluster traffic encryption]\(クラスターのトラフィックの暗号化\) 画面":::

1. クラスター全体で仮想マシンの負荷を自動的に分散するには、 **[Virtual machine load balancing]\(仮想マシンの負荷分散\)** を選択し、次の手順を実行します。

   - **[Balance virtual machines]\(仮想マシンのバランスを取る\)** で、適切なアクションを選択します
   - **[強度]** で、適切な動作を選択します。

     このしくみの詳細については、「[仮想マシンの負荷分散の概要](https://docs.microsoft.com/windows-server/failover-clustering/vm-load-balancing-overview)」を参照してください。

        :::image type="content" source="media/manage-cluster/cluster-settings-vm-load.png" alt-text="クラスターの [Balance virtual machines]\(仮想マシンのバランスを取る\) 画面":::

1. クォーラム監視の種類を選択するには **[監視]** を選択した後、次のいずれかを選択します。

   - **クラウド監視** - Azure クラウド リソースを監視として使用します。
   - **ディスク監視** - ディスク リソースを監視として使用します。
   - **ファイル共有監視** - ファイル共有を監視として使用します。

        詳細については、「[クォーラムを構成および管理する](https://docs.microsoft.com/windows-server/failover-clustering/manage-cluster-quorum)」を参照してください。

        :::image type="content" source="media/manage-cluster/cluster-settings-witness.png" alt-text="クラスターの [監視] 画面":::

## <a name="change-cluster-hyper-v-settings"></a>クラスターの Hyper-V 設定を変更する

クラスターに適用できる Hyper-V ホスト設定は 5 つあります。

1. Windows Admin Center で、上部のドロップダウン矢印から **[クラスター マネージャー]** をクリックします。
1. **[ツール]** の下の **[設定]** をクリックします。
1. **[全般]** を選択した後、次の設定を使用します。

   - **仮想ハード ディスクのパス** - 仮想ハード ディスクのファイルを格納するための既定のフォルダーを指定します。

   - **仮想マシンのパス** - 仮想マシンの構成ファイルを格納するための既定のフォルダーを指定します。

   - **ハイパーバイザー スケジューラの種類** - **[コア スケジューラ]** または **[クラシック スケジューラ]** を選択します。 これにより、同時マルチスレッド (SMT またはハイパースレッディングとも呼ばれます) を使用する物理プロセッサで実行される仮想プロセスが、ハイパーバイザーによってスケジュールされる方法が決まります。

        :::image type="content" source="media/manage-cluster/cluster-settings-hyperv.png" alt-text="クラスターの Hyper-V の [全般設定] 画面":::

1. 仮想マシンからローカル デバイスとリソースをリダイレクトできるようにするには、 **[拡張セッション モード]** を選択します。 拡張セッション モード接続では、サポートされているゲスト オペレーティング システムが必要であることに注意してください。

    :::image type="content" source="media/manage-cluster/cluster-settings-session.png" alt-text="クラスターの Hyper-V の [拡張セッション モード] 画面":::

1. バーチャル マシンが物理 NUMA ノードにまたがることを許可するには、 **[NUMA Spanning]\(NUMA スパニング\)** を選択します。 不均一メモリ アーキテクチャ (NUMA) のスパニングでは、単一の NUMA ノードで使用可能なメモリよりも多くのメモリを持つ仮想マシンを提供できます。

    :::image type="content" source="media/manage-cluster/cluster-settings-numa.png" alt-text="クラスターの [NUMA Spanning]\(NUMA スパニング\) 画面":::

1. 実行中に同時に移動 (ライブ マイグレーション) できる VM の数を指定するには、 **[ライブ マイグレーション]** を選択し、数値を選択してから、次のように指定します。 **[ライブ マイグレーション]** を選択し、数値を選択してから、次のように指定します。

   - **[認証プロトコル]** で、 **[CredSSP]** または **[Kerberos]** を選択します。

   - **[パフォーマンス オプション]** で、 **[圧縮]** または **[SMB]** を選択します。 圧縮データは、TCP/IP 接続を介して送信されます。

   - ノード上の使用可能な任意のネットワークを使用して移行を実行するには、 **[Use any network]\(任意のネットワークを使用する\)** チェックボックスをオンにします。

        :::image type="content" source="media/manage-cluster/cluster-settings-liv-migration.png" alt-text="クラスターの [ライブ マイグレーション] 画面":::

1. 同時に実行できる記憶域の移行の数を指定するには、 **[記憶域の移行]** を選択してから、数値を選択します。

    :::image type="content" source="media/manage-cluster/cluster-settings-sto-migration.png" alt-text="クラスターの [記憶域の移行] 画面":::

## <a name="change-cluster-storage-settings"></a>クラスターのストレージ設定を変更する

クラスターに適用できる記憶域スペース ダイレクトに関連して変更できる設定が 2 つあります。

1. Windows Admin Center で、上部のドロップダウン矢印から **[クラスター マネージャー]** をクリックします。
1. **[ツール]** の一番下にある **[設定]** をクリックします。
1. 記憶域キャッシュを構成するには、 **[記憶域スペース ダイレクト]** を選択し、次のように構成します。

   - **[Persistent cache]\(永続的なキャッシュ\)** で、 **[有効]** または **[無効]** のいずれかを選択します。

   - **[Cache mode for HDD]\(HDD のキャッシュ モード\)** で、 **[読み取り専用]** 、 **[書き込みのみ]** 、または **[読み取り/書き込み]** を選択します。

   - **[Cache mode for SSD]\(SSD のキャッシュ モード\)** で、 **[読み取り専用]** 、 **[書き込みのみ]** 、または **[読み取り/書き込み]** を選択します。

        :::image type="content" source="media/manage-cluster/cluster-settings-ssd.png" alt-text="クラスターの [記憶域スペース ダイレクト] 画面":::

1. サーバー メモリを使用して頻度が高い読み取りをキャッシュするには、 **[インメモリ キャッシュ]** を選択し、サーバーごとに使用される最大メモリを指定します。 「[記憶域スペース ダイレクトと CSV インメモリ読み取りキャッシュの使用](https://docs.microsoft.com/windows-server/storage/storage-spaces/csv-cache)」も参照してください。

    :::image type="content" source="media/manage-cluster/cluster-settings-memory.png" alt-text="クラスターの [インメモリ キャッシュ] 画面":::

## <a name="next-steps"></a>次のステップ

- クラスターのパフォーマンスを監視するには、「[クラスターのパフォーマンスを監視する](https://docs.microsoft.com/azure-stack/hci/get-started#monitor-cluster-performance-with-the-windows-admin-center-dashboard)」を参照してください。