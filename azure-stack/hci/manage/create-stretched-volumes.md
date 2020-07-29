---
title: Azure Stack HCI にストレッチ クラスター用のボリュームを作成し、レプリケーションを設定する
description: Windows Admin Center と PowerShell を使用して、Azure Stack HCI にストレッチ クラスター用のボリュームを作成し、レプリケーションを設定する方法について説明します。
author: khdownie
ms.author: v-kedow
ms.topic: how-to
ms.date: 07/21/2020
ms.openlocfilehash: 647fbbc1fc0ae070955f796aee7aa102290bbc4f
ms.sourcegitcommit: 0e52f460295255b799bac92b40122a22bf994e27
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/21/2020
ms.locfileid: "86868120"
---
# <a name="create-volumes-and-set-up-replication-for-stretched-clusters"></a>ストレッチ クラスター用のボリュームを作成し、レプリケーションを設定する

> 適用対象:Azure Stack HCI バージョン 20H2

このトピックでは、Windows Admin Center と PowerShell を使用して、Azure Stack HCI にストレッチ クラスター用のボリュームを作成し、レプリケーションを設定する方法について説明します。 シングルサイト クラスター上でボリュームを作成する方法、ボリューム上のファイルの操作、およびボリューム上のデータの重複除去と圧縮を有効にする方法については、[ボリュームの作成](create-volumes.md)に関するページを参照してください。

## <a name="create-volumes-and-set-up-replication-for-stretched-clusters-using-windows-admin-center"></a>Windows Admin Center を使用してストレッチ クラスター用のボリュームを作成し、レプリケーションを設定する

では、始めましょう。

1. Windows Admin Center で、 **[ツール]** の **[ボリューム]** を選択します。
1. 右側のペインで、 **[インベントリ]** タブを選択し、 **[作成]** を選択します。
1. **[ボリュームの作成]** パネルで、 **[Replicate volume between sites]\(サイト間でボリュームをレプリケートする\)** を選択します。
1. ドロップダウン ボックスから、サイト間のレプリケーションの方向を選択します。
1. **[レプリケーション モード]** で **[非同期]** または **[同期]** を選択します。
1. [Source replication group name]\(レプリケーション元グループ名\) と [Destination replication group name]\(レプリケーション先グループ名\) を入力します。
1. ログ ボリュームに必要なサイズを入力します。
1. **[詳細]** で、必要に応じて次の手順を実行します。
     - **[Source replication group name]\(レプリケーション元グループ名\)** を入力または変更しします。
     - **[Destination replication group name]\(レプリケーション先グループ名\)** を入力または変更します。
     - **[use blocks already seeded on the target]\(ターゲットで既にシード処理されているブロックを使用する\)** するには、そのチェックボックスをオンにします。
     - **[encrypt replication traffic]\(レプリケーション トラフィックを暗号化する\)** には、そのチェックボックスをオンにします。
     - **[enable consistency groups]\(整合性グループ有効にする\)** には、そのチェックボックスをオンにします。
1. 完了したら、 **[作成]** をクリックします。
1. 右側のペインで、プライマリ (アクティブ) サイトにデータ ディスクとログ ディスクが作成され、セカンダリ (パッシブ) サイトに対応するデータとログのレプリカ ディスクが作成されていることを確認します。 双方向レプリケーションの場合、2 セットのデータとボリュームのディスクが表示されます。
1. **[ツール]** で、 **[記憶域レプリカ]** を選択します。
1. 右側のペインの **[Partnerships]\(パートナーシップ\)** で、レプリケーション パートナー シップが正常に作成されたことを確認します。

以降は、VM やその他のワークロードをデプロイする前に、サイト間でデータが正常にレプリケートされていることを確認することをお勧めします。 詳細については、[クラスターの検証](../deploy/validate.md)に関するページのレプリケーションの確認のセクションを参照してください。

## <a name="create-volumes-for-stretched-clusters-using-powershell"></a>PowerShell を使用してストレッチ クラスター用のボリュームを作成する

ボリュームの作成は、単一サイトの標準クラスターとストレッチ (2 サイト) クラスターとでは異なります。 ただし、どちらのシナリオでも、`New-Volume` コマンドレットを使用して仮想ディスクを作成し、パーティションを作成してフォーマットし、名前が一致するボリュームを作成して、クラスター共有ボリューム (CSV) に追加します。

ストレッチ クラスター用のボリュームと仮想ディスクの作成は、単一サイト クラスターの場合よりも少し複雑です。 ストレッチ クラスターには、少なくとも 4 つのボリューム (2 つのデータ ボリュームと 2 つのログ ボリューム) が必要であり、各サイトにデータとログのボリュームのペアがあります。 次に、サイトごとにレプリケーション グループを作成し、サイト間のレプリケーションを設定します。

まず、ノード間でリソース グループを移動する必要があります。 これには `Move-ClusterGroup` コマンドレットを使用します。

まず `Move-ClusterGroup` コマンドレットを使用して、"使用可能記憶域" 記憶域プール リソース グループを Site1 のノード Server1 に移動します。

```powershell
Move-ClusterGroup -Cluster ClusterS1 -Name ‘Available Storage’ -Node Server1
```

次に、サイト Site1 にノード Server1 用の最初の仮想ディスク (Disk1) を作成します。

```powershell
New-Volume -CimSession Server1 -FriendlyName Disk1 -FileSystem REFS -DriveLetter F -ResiliencySettingName Mirror -Size 10GB -StoragePoolFriendlyName "Storage Pool for Site 1"
```

ノード Server1 用の 2 つ目の仮想ディスク (Disk2) を作成します。

```powershell
New-Volume -CimSession Server1 -FriendlyName Disk2 -FileSystem REFS -DriveLetter G -ResiliencySettingName Mirror -Size 10GB -StoragePoolFriendlyName "Storage Pool for Site 1"
```

次に、"使用可能記憶域" グループをオフラインにします。

```powershell
Stop-ClusterGroup -Cluster ClusterS1 -Name 'Available Storage'
```

次に、"使用可能記憶域" グループを Site2 のノード Server3 に移動します。

```powershell
Move-ClusterGroup -Name 'Available Storage' -Node Server3
```

Site2 のノード Server3 に最初の仮想ディスク (Disk3) を作成します。

```powershell
New-Volume -CimSession Server3 -FriendlyName Disk3 -FileSystem REFS -DriveLetter H -ResiliencySettingName Mirror -Size 10GB -StoragePoolFriendlyName "Storage Pool for Site 2"
```

そして、ノード Server3 に 2 つ目の仮想ディスク (Disk4) を作成します。

```powershell
New-Volume -CimSession Server3 -FriendlyName Disk4 -FileSystem REFS -DriveLetter I -ResiliencySettingName Mirror -Size 10GB -StoragePoolFriendlyName "Storage Pool for Site 2"
```

ここで `Available Storage` グループをオフラインにし、Site1 のノードのいずれかに戻します。

```powershell
Stop-ClusterGroup -Cluster ClusterS1 -Name 'Available Storage'
```

```powershell
Move-ClusterGroup -Cluster ClusterS1 -Name 'Available Storage' -Node Server1
```

`Get-ClusterResource` コマンドレットを使用して、4 つの仮想ディスク ボリューム (各記憶域プールに 2 つ) が作成されたことを確認します。

```powershell
Get-ClusterResource -Cluster ClusterS1
```

次に、Disk1 をクラスター共有ボリュームに追加します。

```powershell
Add-ClusterSharedVolume -Name 'Cluster Virtual Disk (Disk1)'
```

## <a name="setup-replication-for-stretched-clusters-using-powershell"></a>PowerShell を使用してストレッチ クラスターのレプリケーションを設定する

PowerShell を使用してストレッチ クラスターの記憶域レプリカを設定する場合、ソース データに使用されるディスクをクラスター共有ボリューム (CSV) として追加する必要があります。 他のすべてのディスクは、使用可能記憶域グループ内の非 CSV ドライブとして保持されている必要があります。 これらのディスクは、記憶域 レプリカの作成プロセス中にクラスター共有ボリュームとして追加されます。

前の手順では、識別しやすいように、ドライブ文字を使用して仮想ディスクを追加しました。 記憶域レプリカは 1 対 1 のレプリケーションです。つまり、1 つのディスクを別の 1 つのディスクにレプリケートすることができます。

### <a name="validate-the-topology-for-replication"></a>レプリケーションのトポロジを検証する

開始する前に、`Test-SRTopology` コマンドレットを長時間 (数時間など) 実行する必要があります。 `Test-SRTopology` コマンドレットを使用すると、潜在的なレプリケーション パートナー シップを検証し、ローカル ホストから宛先のサーバーへ、またはリモートでソースと対象のサーバー間を検証することができます。

このコマンドレットでは、次のことが確認されます。

- SMB にネットワーク経由でアクセスできること。つまり、TCP ポート 445 とポート 5445 が双方向に開かれています。
- WS-MAN にネットワーク上の HTTP 経由でアクセスできること。つまり、TCP ポート 5985 と 5986 が開かれています。
- SR WMIv2 プロバイダーにアクセス可能であり、要求が受け入れられること。
- ソースと宛先のデータ ボリュームが存在し、書き込み可能であること。
- NTFS フォーマットまたは ReFS フォーマットのソースと宛先のログ ボリュームが存在し、十分な空き容量があること。
- 記憶域は MBR ではなく GPT フォーマットで初期化され、セクター サイズが一致すること。
- レプリケーションを実行できる十分な物理メモリがあること。

さらに、`Test-SRTopology` コマンドレットでは、以下の測定も行われます。

- ICMP のラウンドトリップ待機時間。平均が報告されます。
- 書き込みの入力および出力のパフォーマンス カウンター。そのボリュームで確認された平均が報告されます。
- 初期同期の推定時間。

Test-SRTopology が完了すると、Windows の Temp フォルダーに .html ファイル (日付と時刻を含む TestSrTopologyReport) が作成されます。 警告またはエラーが発生した場合は、記憶域レプリカが正しく作成されていない可能性があるため、確認する必要があります。

5 時間実行されるコマンドの例を次に示します。

```powershell
Test-SRTopology -SourceComputerName Server1 -SourceVolumeName W: -SourceLogVolumeName X: -DestinationComputerName Server3 -DestinationVolumeName Y: -DestinationLogVolumeName Z: -DurationInMinutes 300 -ResultPath c:\temp
```

### <a name="create-the-replication-partnership"></a>レプリケーション パートナーシップを作成する

`Test-SRTopology` テストが完了したので、記憶域レプリカを構成してレプリケーション パートナーシップを作成する準備は完了です。 簡単に説明すると、サイトごとにレプリケーション グループ (RG) を作成し、Site1 のソース サーバー ノード (Server1、Server2) と Site2 の宛先 (レプリケート) サーバー ノード (Server3、Server4) の両方のデータ ボリュームとログ ボリュームを指定して、記憶域レプリカを構成します。

では、始めましょう。

1. Site1 データ ディスクをクラスター共有ボリューム (CSV) として追加します。

   ```powershell
   Add-ClusterSharedVolume -Name "Cluster Virtual Disk (Site1)"
   ```

1. 使用可能記憶域グループは、現在配置されているノードによって "所有" されている必要があります。 次のコマンドを使用して、グループを Server1 に移動できます。

   ```powershell
   Move-ClusterGroup -Name “Available Storage” -Node Server1
   ```

1. レプリケーション パートナーシップを作成するには、`New-SRPartnership` コマンドレットを使用します。 このコマンドレットでは、ソース データ ボリュームとログ ボリュームの名前も指定します。

   ```powershell
   New-SRPartnership -SourceComputerName "Server1" -SourceRGName "Replication1" -SourceVolumeName "C:\ClusterStorage\Disk1\" -SourceLogVolumeName "G:" -DestinationComputerName "Server3" -DestinationRGName "Replication2" -DestinationVolumeName "H:" -DestinationLogVolumeName "I:"
   ```

`New-SRPartnership` コマンドレットを使用して、2 つのサイトの 2 つのレプリケーション グループ間にレプリケーション パートナーシップを作成します。 この例で、`Replication1` は Site1 のプライマリ ノード Server1 のレプリケーション グループであり、`Replication2` は Site2 の宛先ノード Server3 のレプリケーション グループです。

記憶域レプリカによってすべてが設定されます。 レプリケートするデータがある場合は、ここで行います。 レプリケートする必要のあるデータの量によっては、しばらく時間がかかることがあります。 このプロセスが完了するまでは、グループを移動しないことをお勧めします。

## <a name="next-steps"></a>次のステップ

関連トピックとその他の記憶域管理タスクについては、次のトピックも参照してください。

- [ストレッチ クラスターの概要](../concepts/stretched-clusters.md)
- [ボリュームの計画](../concepts/plan-volumes.md)
- [ボリュームの拡張](extend-volumes.md)
- [ボリュームの削除](delete-volumes.md)