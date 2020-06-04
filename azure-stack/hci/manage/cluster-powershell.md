---
title: PowerShell を使用して Azure Stack HCI クラスターを管理する
description: PowerShell を使用して Azure Stack HCI でクラスターを管理する方法について説明します
author: v-dasis
ms.topic: article
ms.date: 05/21/2020
ms.author: v-dasis
ms.reviewer: JasonGerend
ms.openlocfilehash: 922a6188c1529d3461288ac28ff542db6fa9a527
ms.sourcegitcommit: f2203f070c17e19ba1a41d681662bac3ab50ed12
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/26/2020
ms.locfileid: "83855705"
---
# <a name="manage-azure-stack-hci-clusters-using-powershell"></a>PowerShell を使用して Azure Stack HCI クラスターを管理する

> 適用対象: Windows Server 2019

Windows PowerShell を使用して、Azure Stack HCI クラスター上のリソースを管理し、機能を構成することができます。

通常、クラスターは、クラスター内のホスト サーバーではなく、Windows 10 を実行しているリモート コンピューターから管理します。 このリモート コンピューターは、管理コンピューターと呼ばれます。

> [!NOTE]
> 管理コンピューターから PowerShell コマンドを実行する場合は、管理しているクラスターの名前を指定した `-Name` パラメーターまたは `-Cluster` パラメーターを含めます。 さらに、サーバー ノードに対して `-ComputerName` パラメーターを使用する場合は、完全修飾ドメイン名 (FQDN) を指定する必要があります。

PowerShell を使用したクラスターの管理に関する完全なリファレンス ドキュメントについては、[FailoverCluster リファレンス](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)を参照してください。

## <a name="using-windows-powershell"></a>Windows PowerShell を使用する

この記事のすべてのタスクを実行するには、Windows PowerShell を使用します。 利便性のために、このアプリをタスク バーにピン留めすることをお勧めします。

以下のコマンドレットが PowerShell セッションで使用できない場合は、PowerShell コマンド `Add-WindowsFeature RSAT-Clustering-PowerShell` を使用して、Windows PowerShell 機能の `Failover Cluster` モジュールを追加することが必要な場合があります。

> [!NOTE]
> Windows 10 October 2018 Update 以降では、RSAT は Windows 10 から直接 "オンデマンド機能" のセットとして含まれています。 **[設定] > [アプリ] > [アプリと機能] > [オプション機能] > [RSAT: フェールオーバー クラスタリング ツール]** に移動し、 **[インストール]** を選択します。 インストールの進行状況を確認するには、[戻る] ボタンをクリックして、[オプション機能の管理] ページに状態を表示します。 インストールされた機能は、Windows 10 のバージョンのアップグレードをまたがって保持されます。

## <a name="view-cluster-settings-and-resources"></a>クラスター設定およびリソースを表示する

Cluster1 という名前のクラスターに関する情報を取得します。

```powershell
Get-Cluster -Name Cluster1
```
Cluster1 の 1 つ以上のノード (サーバー) に関する情報を取得します。

```powershell
Get-ClusterNode -Cluster Cluster1
```

クラスター ノードにインストールされている Windows 機能を表示するには、`Get-WindowsFeature` コマンドレットを使用します。 次に例を示します。

```powershell
Get-WindowsFeature -ComputerName Server1
```

ネットワーク アダプターとそれらのプロパティ (名前、IPv4 アドレス、VLAN ID など) を表示するには、次を実行します。

```powershell
Get-NetAdapter -CimSession Server1 | Where Status -Eq "Up" | Sort InterfaceAlias | Format-Table Name, InterfaceDescription, Status, LinkSpeed, VLANID, MacAddress
```

Hyper-V 仮想スイッチと、物理ネットワーク アダプターのチーミング方法を表示するには、次を実行します。

```powershell
Get-VMSwitch -ComputerName Server1
```

ホスト仮想ネットワークアダプターを表示するには、次を実行します。

```powershell
Get-VMNetworkAdapter -ComputerName Server1
```

記憶域スペース ダイレクトが有効になっているかどうかを確認するには、次を実行します。

```powershell
Get-CimSession -ComputerName Server1 | Get-ClusterStorageSpacesDirect
```

## <a name="start-or-stop-a-cluster"></a>クラスターを開始または停止する

クラスターに対してサーバー ノードを追加または削除するには、`Start-Cluster` と `Stop-Cluster` のコマンドレットを使用します。 その他の例と使用方法に関する情報については、「[Start-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/start-cluster?view=win10-ps)」と「[Stop-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/stop-cluster?view=win10-ps)」のリファレンス ドキュメントを参照してください。

まだクラスター サービスが開始されていないクラスターのすべてのサーバー ノードでそのサービスを開始します。

```powershell
Start-Cluster -Name Cluster1
```

次の例では、Cluster1 という名前のクラスター内のすべてのノードでクラスター サービスを停止します。これにより、クラスターに構成されているすべてのサービスおよびアプリケーションが停止します。

```powershell
Stop-Cluster -Name Cluster1
```

## <a name="add-or-remove-a-server"></a>サーバーを追加または削除する

クラスターに対してサーバー ノードを追加または削除するには、`Add-ClusterNode` と `Remove-ClusterNode` のコマンドレットを使用します。 その他の例と使用方法に関する情報については、「[Add-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusternode?view=win10-ps)」と「[Remove-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode?view=win10-ps)」のリファレンス ドキュメントを参照してください。

この例では、Node4 という名前のサーバーを Cluster1 という名前のクラスターに追加します。 最初に、サーバーの電源がオンになっていて、クラスター ネットワークに接続されていることを確認してください。

```powershell
Add-ClusterNode -Cluster Cluster1 -Name Node4
```

次の例では、Node4 という名前のノードをクラスター Cluster1 から削除します。

```powershell
Remove-ClusterNode -Cluster Cluster1 -Name Node4
```

## <a name="set-quorum-options"></a>クォーラム オプションを設定する

`Set-ClusterQuorum` コマンドレットを使用して、クラスターのクォーラム オプションを設定します。 その他の例と使用方法に関する情報については、「[Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum?view=win10-ps)」のリファレンス ドキュメントを参照してください。

次の例では、クラスター Cluster1 でクラウド監視を使用するようにクォーラム構成を変更します。

```powershell
Set-ClusterQuorum -Cluster Cluster1 -CloudWitness
```

この例では、ファイル共有監視用に \\fileserver\fsw のディスク リソースを使用して、クラスター Cluster1 でクォーラム構成をノードおよびファイル共有マジョリティに変更します。

```powershell
Set-ClusterQuorum -Cluster Cluster1 -NodeAndFileShareMajority \\fileserver\fsw
```

## <a name="enable-storage-spaces-direct"></a>記憶域スペース ダイレクトを有効にする

クラスターで記憶域スペース ダイレクトを有効にするには、`Enable-ClusterStorageSpacesDirect` コマンドレットを使用します。 その他の例と使用方法に関する情報については、「[Enable-ClusterStorageSpacesDirect](https://docs.microsoft.com/powershell/module/failoverclusters/enable-clusterstoragespacesdirect?view=win10-ps)」のリファレンス ドキュメントを参照してください。

次の例では、Server1 で記憶域スペース ダイレクトを有効にします。

```powershell
Enable-ClusterStorageSpacesDirect -CimSession Cluster1
```

## <a name="configure-a-hyper-v-host"></a>Hyper-V ホストを構成する

VHD および VM のパス、ライブ マイグレーション、記憶域移行、認証、NUMA ノードにまたがるメモリ割り当てなど、さまざまな Hyper-V ホスト設定を構成するには、`Set-VMHost` コマンドレットを使用します。 その他の例と使用方法に関する情報については、「[Enable-ClusterStorageSpacesDirect](https://docs.microsoft.com/powershell/module/hyper-v/Set-VMHost?view=win10-ps)」のリファレンス ドキュメントを参照してください。

次の例では、ホスト サーバー Server1 上の仮想ハード ディスクおよび VM の新しい既定の場所を指定します。

```powershell
Set-VMHost -ComputerName Server1 -VirtualHardDiskPath "C:\Hyper-V\Virtual Hard Disks" -VirtualMachinePath "C:\Hyper-V\Configuration Files"
```

次の例では、ライブ マイグレーションと記憶域移行を同時に 10 個まで許可するように、ホスト サーバー Server1 を構成します。

```powershell
Set-VMHost -ComputerName Server1 -MaximumVirtualMachineMigrations 10 -MaximumStorageMigrations 10
```

次の例では、Kerberos を使用して受信ライブ マイグレーションを認証するように、ホスト サーバー Server1 を構成します。

```powershell
Set-VMHost -ComputerName Server1 -VirtualMachineMigrationAuthenticationType Kerberos
```

## <a name="validate-a-cluster"></a>クラスターを検証する

クラスターで検証テストを実行するには、`Test-Cluster` コマンドレットを使用します。 その他の例と使用方法に関する情報については、「[Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)」のリファレンス ドキュメントを参照してください。

次の例では、Cluster1 という名前のクラスターで、適用可能なすべてのクラスター検証テストを実行します。

```powershell
Test-Cluster -Cluster Cluster1
```

次の例では、クラスター検証のすべてのテストおよびカテゴリの名前を一覧表示します。 これらのテスト名を *Ignore* または *Include* パラメーターで指定して、特定のテストを実行します。


```powershell
Test-Cluster -Cluster Cluster1 -List
Category                                DisplayName                             Description
--------                                -----------                             -----------
Cluster Configuration                   List Cluster Core Groups                List information about the available...
Cluster Configuration                   List Cluster Network Information        List cluster-specific network settin...
Cluster Configuration                   List Cluster Resources                  List the resources that are configur...
Cluster Configuration                   List Cluster Volumes                    List information for the volumes in ...
Cluster Configuration                   List Clustered Roles                    List information about clustered roles.
Cluster Configuration                   Validate Quorum Configuration           Validate that the current quorum con...
Cluster Configuration                   Validate Resource Status                Validate that cluster resources are ...
Cluster Configuration                   Validate Service Principal Name         Validate that a Service Principal Na...
Cluster Configuration                   Validate Volume Consistency             If any volumes are flagged as incons...
Hyper-V Configuration                   List Information About Servers Runni... List Hyper-V related information on ...
Hyper-V Configuration                   Validate Compatibility of Virtual Fi... Validate that all specified nodes sh...
Hyper-V Configuration                   Validate Hyper-V Memory Resource Poo... Validate that all specified nodes sh...
Hyper-V Configuration                   Validate Hyper-V Network Resource Po... Validate that all specified nodes sh...
Hyper-V Configuration                   Validate Hyper-V Processor Resource ... Validate that all specified nodes sh...
Hyper-V Configuration                   Validate Hyper-V Role Installed         Validate that all the nodes have the...
Hyper-V Configuration                   Validate Hyper-V Storage Resource Po... Validate that all specified nodes sh...
Hyper-V Configuration                   Validate Matching Processor Manufact... Validate that all specified nodes sh...
Hyper-V Configuration                   List Hyper-V Virtual Machine Informa... List Hyper-V virtual machine informa...
Hyper-V Configuration                   Validate Hyper-V Integration Service... Validate that the Hyper-V integratio...
Hyper-V Configuration                   Validate Hyper-V Virtual Machine Net... Validate that all virtual machines o...
Hyper-V Configuration                   Validate Hyper-V Virtual Machine Sto... Validate that all virtual machines o...
Inventory                               List Fibre Channel Host Bus Adapters    List Fibre Channel host bus adapters...
Inventory                               List iSCSI Host Bus Adapters            List iSCSI host bus adapters on each...
Inventory                               List SAS Host Bus Adapters              List Serial Attached SCSI (SAS) host...
Inventory                               List BIOS Information                   List BIOS information from each node...
Inventory                               List Environment Variables              List environment variables set on ea...
Inventory                               List Memory Information                 List memory information for each node.
Inventory                               List Operating System Information       List information about the operating...
Inventory                               List Plug and Play Devices              List Plug and Play devices on each n...
Inventory                               List Running Processes                  List the running processes on each n...
Inventory                               List Services Information               List information about the services ...
Inventory                               List Software Updates                   List software updates that have been...
Inventory                               List System Drivers                     List the system drivers on each node.
Inventory                               List System Information                 List system information such as comp...
Inventory                               List Unsigned Drivers                   List the unsigned drivers on each node.
Network                                 List Network Binding Order              List the order in which networks are...
Network                                 Validate Cluster Network Configuration  Validate the cluster networks that w...
Network                                 Validate IP Configuration               Validate that IP addresses are uniqu...
Network                                 Validate Multiple Subnet Properties     For clusters using multiple subnets,...
Network                                 Validate Network Communication          Validate that servers can communicat...
Network                                 Validate Windows Firewall Configuration Validate that the Windows Firewall i...
Storage                                 List Disks                              List all disks visible to one or mor...
Storage                                 List Potential Cluster Disks            List disks that will be validated fo...
Storage                                 Validate CSV Network Bindings           Validate that network bindings requi...
Storage                                 Validate CSV Settings                   Validate that settings and configura...
Storage                                 Validate Disk Access Latency            Validate acceptable latency for disk...
Storage                                 Validate Disk Arbitration               Validate that a node that owns a dis...
Storage                                 Validate Disk Failover                  Validate that a disk can fail over s...
Storage                                 Validate File System                    Validate that the file system on dis...
Storage                                 Validate Microsoft MPIO-based disks     Validate that disks that use Microso...
Storage                                 Validate Multiple Arbitration           Validate that in a multiple-node arb...
Storage                                 Validate SCSI device Vital Product D... Validate uniqueness of inquiry data ...
Storage                                 Validate SCSI-3 Persistent Reservation  Validate that storage supports the S...
Storage                                 Validate Simultaneous Failover          Validate that disks can fail over si...
System Configuration                    Validate Active Directory Configuration Validate that all the nodes have the...
System Configuration                    Validate All Drivers Signed             Validate that tested servers contain...
System Configuration                    Validate Cluster Service and Driver ... Validate startup settings used by se...
System Configuration                    Validate Memory Dump Settings           Validate that none of the nodes curr...
System Configuration                    Validate Operating System Edition       Validate that all tested servers are...
System Configuration                    Validate Operating System Installati... Validate that the operating systems ...
System Configuration                    Validate Operating System Version       Validate that the operating systems ...
System Configuration                    Validate Required Services              Validate that services required for ...
System Configuration                    Validate Same Processor Architecture    Validate that all servers run as 64-...
System Configuration                    Validate Service Pack Levels            Validate that all servers with same ...
System Configuration                    Validate Software Update Levels         Validate that all tested servers hav...
System Configuration                    Validate System Drive Variable          Validate that all nodes have the sam...
```

## <a name="remove-cluster-and-resources"></a>クラスターとリソースを削除する

クラスター上の 1 つまたはすべてのリソースを削除するには、`Remove-ClusterResource` コマンドレットを使用します。 その他の例と使用方法に関する情報については、「[Remove-ClusterResource](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusterresource?view=win10-ps)」のリファレンス ドキュメントを参照してください。

> [!NOTE]
> クラスターを削除するには、Credential Security Service Provider (CredSSP) 認証を一時的に有効にする必要があります。 詳細については、「[Enable-WSManCredSSP](https://docs.microsoft.com/powershell/module/microsoft.wsman.management/enable-wsmancredssp?view=powershell-7)」を参照してください。

次の例では、クラスター Cluster1 上のクラスター リソースを名前で削除します。

```powershell
Remove-ClusterResource -Cluster Cluster1 -Name "Cluster Disk 4"
```

次の例では、`Remove-Cluster` コマンドレットを使用して、クラスター Cluster1 を完全に削除します。

```powershell
Remove-Cluster -Cluster Cluster1
```

## <a name="next-steps"></a>次のステップ

Windows Admin Center を使用してクラスターを管理する方法を確認します。 [Windows Admin Center を使用した Azure Stack HCI でのクラスターの管理](cluster.md)に関する記事を参照してください。