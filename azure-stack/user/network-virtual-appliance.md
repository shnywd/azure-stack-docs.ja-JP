---
title: Azure Stack Hub でのネットワーク仮想アプライアンスに関する問題のトラブルシューティング
description: Microsoft Azure Stack Hub でサードパーティのネットワーク仮想アプライアンス (NVA) を使用しているときに発生する、VM または VPN の接続の問題を解決します。
author: sethmanheim
ms.author: sethm
ms.date: 05/12/2020
ms.topic: article
ms.reviewer: sranthar
ms.lastreviewed: 05/12/2020
ms.openlocfilehash: 5e42f9e1b099df2ab1dc1063b3bf2710f59ad66f
ms.sourcegitcommit: 999c6cd0ab64cd2d695feb8405a9c720c9ae755b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/13/2020
ms.locfileid: "83342946"
---
# <a name="troubleshoot-network-virtual-appliance-issues"></a>ネットワーク仮想アプライアンスに関する問題のトラブルシューティング

Microsoft Azure Stack Hub でサードパーティのネットワーク仮想アプライアンス (NVA) を使用しているときに、VM または VPN の接続の問題が発生する可能性があります。 この記事では、NVA 構成に対する基本の Azure Stack Hub Platform の要件を確認するのに役立つ基本的な手順を示します。

サードパーティの NVA および Azure Stack Hub プラットフォームとの統合向けのテクニカル サポートは、NVA のベンダーによって提供されます。

> [!NOTE]
> NVA に関する接続性やルーティングの問題がある場合は、直接 [NVA のベンダーにお問い合わせ](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines)いただく必要があります。

この記事で Azure Stack Hub NVA 問題が解決されない場合、[Azure Stack Hub サポート チケット](../operator/azure-stack-manage-basics.md#where-to-get-support)を作成してください。

## <a name="checklist-for-troubleshooting-with-nva-vendor"></a>NVA のベンダーと共にトラブルシューティングするためのチェックリスト

- NVA の VM ソフトウェア用のソフトウェア更新プログラム。
- サービス アカウントのセットアップと機能。
- NVA に直接通信する仮想ネットワークのサブネット上のユーザー定義のルート (UDR)。
- NVA から直接通信する仮想ネットワークのサブネット上の UDR。
- NVA 内のテーブルとルールのルーティング (例: NIC1 から NIC2)。
- ネットワーク トラフィックの送受信を確認するための NVA NIC のトレース。

## <a name="basic-troubleshooting-steps"></a>基本的なトラブルシューティングの手順

- 基本構成の確認。
- NVA のパフォーマンスの確認。
- 高度なネットワークのトラブルシューティング。

## <a name="check-the-minimum-configuration-requirements-for-nvas-on-azure"></a>Azure 上の NVA に対する最小構成要件の確認

各 NVA には Azure Stack Hub 上で正常に機能するための基本構成要件があります。 次のセクションでは、これらの基本構成を確認するための手順を示します。 詳細については、[NVA のベンダーにお問い合わせください](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines)。

> [!IMPORTANT]
> S2S トンネルを使用する場合、パケットは追加のヘッダーでさらにカプセル化されます。 このカプセル化により、パケットの全体的なサイズが増加します。 これらのシナリオでは、TCP **MSS** を **1350** で固定する必要があります。 お使いの VPN デバイスで MSS クランプがサポートされていない場合、代わりにトンネル インターフェイスの MTU を **1400** バイトに設定できます。 詳細については、[仮想ネットワークの TCPIP パフォーマンス チューニング](/azure/virtual-network/virtual-network-tcpip-performance-tuning)に関する記事を参照してください。

### <a name="check-whether-ip-forwarding-is-enabled-on-nva"></a>NVA 上で IP 転送が有効かどうかを確認する

#### <a name="use-the-azure-stack-hub-portal"></a>Azure Stack Hub ポータルを使用する

- Azure Stack Hub ポータルで NVA リソースを検索し、 **[ネットワーク]** を選択して、ネットワーク インターフェイスを選択します。
- **[ネットワーク インターフェイス]** ページで、 **[IP 構成]** を選択します。
- IP 転送が有効になっていることを確認します。

#### <a name="use-powershell"></a>PowerShell の使用

- 次のコマンドを実行します (かっこで囲まれた値をご自分の情報に置き換えます)。

   ```powershell
   Get-AzureRMNetworkInterface -ResourceGroupName <ResourceGroupName> -Name <NIC name>
   ```

- **EnableIPForwarding** プロパティを確認します。
- IP 転送が有効になっていない場合は、次のコマンドを実行して有効にします。

   ```powershell
   $nic2 = Get-AzureRMNetworkInterface -ResourceGroupName <ResourceGroupName> -Name <NIC name>
   $nic2.EnableIPForwarding = 1
   Set-AzureRMNetworkInterface -NetworkInterface $nic2
   Execute: $nic2 #and check for an expected output:
   EnableIPForwarding   : True
   NetworkSecurityGroup : null
   ```

### <a name="check-whether-the-traffic-can-be-routed-to-the-nva"></a>トラフィックを NVA にルーティングできるかどうかを確認する。

- NVA にトラフィックをリダイレクトするように構成されている VM を見つける
- Windows の場合は `Tracert <Private IP of NVA>` または `Traceroute <Private IP of NVA>` を実行し、次ホップとして NVA があるか確認します。
- NVA が次ホップとして一覧表示されていない場合、Azure Stack Hub ルート テーブルを確認して更新します。

ゲストレベルのオペレーティング システムによっては、ICMP トラフィックをブロックするためのファイアウォール ポリシーが配置されていないことがあります。 前のコマンドが機能するには、このようなファイアウォール ルールを更新する必要があります。

### <a name="check-whether-the-traffic-can-reach-the-nva"></a>トラフィックで NVA に到達できるかどうかを確認する

- NVA に接続する必要がある VM を見つけます。
- Windows の場合は `ping (ICMP)` または `Test-NetConnection (TCP) <Private IP of NVA>` を実行し、Linux の場合は `Tcpping <Private IP of NVA>` を実行し、ネットワーク セキュリティ グループによってトラフィックがブロックされるかどうかを確認します。
- トラフィックがブロックされている場合、NSG を変更してトラフィックを許可します。

### <a name="check-whether-nva-and-vms-are-listening-for-expected-traffic"></a>NVA および VM が予想されるトラフィックをリッスンしているかどうかを確認する

- RDP または SSH を使用して NVA に接続して、次のコマンドを実行します。

   **Windows**:
   ```shell
   netstat -an
   ```

   **Linux**:
   ```shell
   netstat -an | grep -i listen
   ```

- 結果に一覧されている NVA ソフトウェアによって使用される TCP ポートが表示されない場合は、それらのポートに到達するトラフィックをリッスンして応答するように、NVA と VM 上にアプリケーションを構成する必要があります。 [必要に応じて、NVA のベンダーにお問い合わせください](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines)。

## <a name="check-nva-performance"></a>NVA のパフォーマンスの確認

### <a name="validate-vm-cpu"></a>VM CPU を検証する

CPU 使用率が 100 パーセントに近い場合は、影響するネットワーク パケットが低下する問題が発生する可能性があります。 CPU のスパイク時に、高い CPU の原因となっているゲスト VM 上のプロセスを調査して、可能であれば、そのプロセスを軽減させます。 また、場合によっては、VM の SKU サイズを増やすか、仮想マシン スケール セットであれば、インスタンス数を増やす必要があります。 これらの問題のいずれかについては、[必要に応じて、NVA のベンダーにお問い合わせください](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines)。

### <a name="validate-vm-network-statistics"></a>VM ネットワークの統計を検証する

VM ネットワークでスパイクを使用したり、高い使用率の期間を示したりする場合は、スループット機能をさらに高めるために、VM の SKU サイズを増加させる必要もある可能性があります。

## <a name="advanced-network-administrator-troubleshooting"></a>高度なネットワーク管理者のトラブルシューティング

### <a name="capture-network-trace"></a>ネットワーク トレースのキャプチャ

[**PsPing**](/sysinternals/downloads/psping) または **Nmap** を実行しながら、ソース VM、NVA、宛先の VM 上で同時ネットワーク追跡をキャプチャしてから、追跡を停止します。

- 同時ネットワーク追跡をキャプチャするには、次のコマンドを実行します。

   **Windows**

   ```shell
   netsh trace start capture=yes tracefile=c:\server_IP.etl scenario=netconnection
   ```

   **Linux**

   ```shell
   sudo tcpdump -s0 -i eth0 -X -w vmtrace.cap
   ```

- ソース VM から宛先 VM に対して **PsPing** または **Nmap** を使用します (例: `PsPing 10.0.0.4:80` または `Nmap -p 80 10.0.0.4`)。

- **tcpdump** か自分で選んだパケット アナライザーを利用し、宛先 VM からネットワーク トレースを開きます。 `IPv4.address==10.0.0.4` (Windows netmon) または `tcpdump -nn -r vmtrace.cap src` か `dst host 10.0.0.4` (Linux) など、**PsPing** または **Nmap** を実行したソース VM の IP に表示フィルターを適用します。

### <a name="analyze-traces"></a>トレースの分析

バックエンド VM トレースにパケットが入ってこない場合、NSG または UDR により妨害されているか、NVA ルーティング テーブルが正しくない可能性があります。

パケットを受信しているのに応答がない場合は、VM アプリケーションまたはファイアウォールに関する問題に対処する必要があります。 これらの問題のいずれかについては、[必要に応じて、NVA のベンダーにお問い合わせください](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines)。

### <a name="create-a-support-ticket"></a>サポート チケットの作成

上記の手順で問題が解決しない場合、[サポート チケット](../operator/azure-stack-manage-basics.md#where-to-get-support)を作成し、[オンデマンド ログ収集ツール](../operator/azure-stack-configure-on-demand-diagnostic-log-collection.md)を使用してログを提供してください。
