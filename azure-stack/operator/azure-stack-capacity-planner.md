---
title: Azure Stack Capacity Planner | Microsoft Docs
description: Azure Stack デプロイのための容量計画について説明します。
services: azure-stack
documentationcenter: ''
author: prchint
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2019
ms.author: justinha
ms.reviewer: prchint
ms.lastreviewed: 05/31/2019
ms.openlocfilehash: aec1d6fab044c731501433e6578257e8b09570eb
ms.sourcegitcommit: 80775f5c5235147ae730dfc7e896675a9a79cdbe
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/03/2019
ms.locfileid: "66461084"
---
# <a name="azure-stack-capacity-planner"></a>Azure Stack のキャパシティ プランニング ツール

Azure Stack Capacity Planner は、コンピューティング リソースのさまざまな割り当てがハードウェア オファリングのさまざまな選択に対してどのようにフィットするかを示すスプレッドシートです。 

## <a name="worksheet-descriptions"></a>ワークシートの説明
以下の表に Azure Stack Capacity Planner の各ワークシートの説明を示します。Azure Stack Capacity Planner は [https://aka.ms/azstackcapacityplanner](https://aka.ms/azstackcapacityplanner) からダウンロードできます。 

|タブ名|説明|
|-----|-----|
|Version-Disclaimer|この計算ツールの目的、バージョン番号、およびリリース日。|
|Instructions|VM のコレクションのキャパシティ プランニングをモデル化するための詳細な手順。|
|DefinedSolutionSKUs|最大 5 つのハードウェア定義を含む表。 エントリは例です。 検討中のシステム構成に合わせて詳細を変更してください。|
|DefineByVMFootprint|VM のサイズと数が異なる構成を比較して適切なハードウェア SKU を見つけます。|
|DefineByWorkloadFootprint|Azure Stack のワークロードのコレクションを作成して適切なハードウェア SKU を検索します。|
|  |  |

## <a name="definedsolutionskus-instructions"></a>DefinedSolutionSKUs の手順
このワークシートには、最大 5 つのハードウェア定義の例が含まれています。 検討中のシステム構成に合わせて詳細を変更してください。

### <a name="hardware-selections-provided-by-authorized-hardware-partners"></a>ハードウェアの認定パートナーによって提供されるハードウェアの選択
Azure Stack は、ソリューション パートナーによってインストールされたソフトウェアが組み込まれたシステムとして提供されます。 ソリューション パートナーは、Azure Stack のキャパシティ プランニング ツールの独自の権限のあるバージョンを提供しています。 ソリューション容量の最終的な検討には、それらのツールを使用してください。

### <a name="multiple-ways-to-model-computing-resources"></a>コンピューティング リソースをモデル化する複数の方法
Azure Stack プランニング ツール内のリソース モデリングでは、さまざまなサイズの Azure Stack VM が利用されています。 VM のサイズ範囲は、最小の Basic 0 から最大の Standard_Fsv2 までです。 2 つの異なる方法でコンピューティング リソースの割り当てをモデル化できます。

- 特定のハードウェア オファリングを選択し、さまざまなリソースのどの組み合わせがフィットするかを確認します。 

- VM 割り当ての特定の組み合わせを作成し、使用可能などのハードウェア SKU がこの VM 構成をサポートできるかを Azure Resource 計算ツールに表示させます。

このツールには、VM リソースを割り当てる 2 つの方法 (VM リソース割り当ての 1 つのコレクションとして、または最大 6つの異なるワークロード構成のコレクションとして) が用意されています。 各ワークロード構成は、使用可能な VM リソースの異なる割り当てを含むことができます。 次のセクションでは、これらの各割り当てモデルを作成して使用するための詳細な手順を説明します。 非バック グラウンドのシェードされたセルに格納されている値、またはこのワークシートの SKU プルダウン リスト内の値のみ変更する必要があります。 シェードされたセル内で行われた変更によってリソースの計算が損なわれる場合があります。


## <a name="definebyvmfootprint-instructions"></a>DefineByVMFootprint の手順
さまざまなサイズと数の VM の 1 つのコレクションを使用してモデルを作成するには、[DefineByVMFootprint] タブを選択し、次の手順に従います。

1. このワークシートの右上隅で、指定されたプルダウン リスト ボックス コントロールを使用して、各ハードウェア システム (SKU) にインストールするサーバーの初期の数 (4 ～ 16) を選択します。 このサーバーの数は、リソース割り当てモデルの使用可能なリソース全体への影響を確認するために、モデリング プロセス中にいつでも変更できます。
2. 1 つの特定のハードウェア構成に対してさまざまな VM リソース割り当てをモデル化する場合、ページの右上隅の「Current SKU」(現在の SKU) ラベルの真下にある青のプルダウン リスト ボックスを探します。 このリスト ボックスをプルダウンし、目的のハードウェア SKU を選択します。
3. さまざまなサイズの VM のモデルへの追加を開始する準備が整いました。 特定の VM タイプを含めるには、その VM エントリの左側の青枠で囲まれたボックスに数量の値を入力します。

   > [!NOTE]
   > VM ストレージの合計とは、VM のデータ ディスクの総容量 (サポートされているディスク数に単一ディスクの最大容量 (1 TB) を乗算したもの) のことです。 構成評価指標に基づいて使用可能なストレージ構成テーブルは設定済みなので、各 Azure Stack に必要なレベルの VM のストレージ リソースを選択できます。 ただし重要な点として、使用可能なストレージ構成テーブルは必要に応じて追加または変更できることに注意してください。<br><br>各 VM は、最初に割り当てられたローカルの一時ストレージから始まります。 一時ストレージのシン プロビジョニングを反映するには、可能な最大一時ストレージ容量を含むドロップダウン メニューのいずれかにローカルの一時的な数を変更します。

4. VM を追加すると、使用可能な SKU リソースの変更を示すグラフが表示されます。 これにより、モデリング プロセス中にさまざまなサイズと VM の数量を追加することの効果を確認できます。 変更の影響を把握する別の方法は、使用可能な VM の真下にリストされる消費された数とまだ使用可能な数を確認することです。 これらの数値は、現在選択されているハードウェア SKU に基づく推定値を反映しています。
5. 一連の VM を作成した後は、ページの右上隅の「Current SKU」(現在の SKU) ラベルの真下にある「Suggested SKU」(推奨 SKU) ボタンをクリックして、推奨ハードウェア SKU を検索できます。 このボタンを使用すると、VM 構成を変更し、どのハードウェアがそれぞれの構成をサポートするかを確認できます。


## <a name="definebyworkloadfootprint-instructions"></a>DefineByWorkloadFootprint の手順
Azure Stack のワークロードのコレクションを使用してモデルを作成するには、[DefineByWorkloadFootprint] タブを選択し、この一連の手順に従います。 Azure Stack のワークロードは、使用可能な VM リソースを使用して作成されます。   

> [!TIP]
> Azure Stack VM に指定されたストレージ サイズを変更するには、前のセクションの手順 3 のメモを参照してください。

1. このページの右上隅で、指定されたプルダウン リスト ボックス コントロールを使用して、各ハードウェア システム (SKU) にインストールするサーバーの初期の数 (4 ～ 16) を選択します。
2. 1 つの特定のハードウェア構成に対してさまざまな VM リソース割り当てをモデル化する場合、ページの右上隅の「Current SKU」(現在の SKU) ラベルの真下にある青のプルダウン リスト ボックスを探します。 このリスト ボックスをプルダウンし、目的のハードウェア SKU を選択します。
3. 上記の DefineByVMFootprint の手順の手順 3 で説明しているように、DefineByVMFootprint ページの目的の Azure Stack VM それぞれに対して適切なストレージ サイズを選択します。 VM あたりのストレージ サイズは、DefineByVMFootprint シートで定義されます。
4. DefineByWorkloadFootprint ページの左上から、ワークロード内に含まれている各 VM タイプの数量を入力して、最大 6 つの異なるワークロード タイプの構成を作成します。 これは、該当のワークロードの名前の真下にある列に数値を入力することによって実行します。 ワークロード名を変更して、この特定の構成でサポートされるワークロードのタイプを反映できます。
5. 「Quantity」(数量) ラベルの真下にある該当する列の下部に値を入力することによって、それぞれのワークロード タイプの特定の数量を含めることができます。
6. ワークロード タイプと数量が作成されてから、ページの右上隅の「Current SKU」(現在の SKU) ラベルの真下にある「Suggested SKU」(推奨 SKU) ボタンをクリックすると、ワークロードの構成全体をサポートするのに十分なリソースを含む最小の SKUが表示されます。
7. ハードウェア SKU に選択したサーバーの数を変更する、または VM の割り当てまたはワークロード構成内の数量を変更することによって、さらなるモデリングを実行できます。 関連付けられているグラフに、変更が全体的なリソースの消費量にどのように影響するかを示すフィードバックがすぐに表示されます。
8. 変更に満足してから、 **[Suggested SKU]\(推奨 SKU\)** ボタンをもう一度クリックすると、新しい構成に推奨される SKU が表示されます。 ドロップダウン メニューをクリックして目的の SKU を選択することもできます。

## <a name="next-steps"></a>次の手順
[Azure Stack のデータ センター統合に関する考慮事項](azure-stack-datacenter-integration.md)
