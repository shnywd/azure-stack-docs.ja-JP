---
title: Azure Stack HCI の既知の問題
description: このトピックでは、Azure Stack HCI の既知の問題について説明します。
author: myoungerman
ms.author: v-myoung
ms.topic: troubleshooting
ms.date: 10/27/2020
ms.openlocfilehash: 05884ce83f6f35f8d75c632f67dabd38fa3c2814
ms.sourcegitcommit: 75603007badd566f65d01ac2eacfe48ea4392e58
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92688314"
---
# <a name="known-issues-for-azure-stack-hci"></a>Azure Stack HCI の既知の問題

>適用対象:Azure Stack HCI バージョン 20H2

このトピックでは、Azure Stack HCI の既知の問題について説明します

- 2020 年 10 月 20 日のプレビュー更新プログラム (KB4580388) では、いずれかの仮想マシン (VM) がクラスター対応更新 (CAU) の間にライブ マイグレーションを実行することが予想される場合、CAU 操作が失敗することがあります。 これを回避するには、CAU でライブ マイグレーションの代わりにクイック マイグレーションが使用されるように、既定の動作を一時的に変更します。 詳しくは、[リリース ノート](release-notes.md#october-20-2020-preview-update-kb4580388)のページをご覧ください。
- オンプレミス クラスターと Azure Stack HCI クラウド サービスの間の接続では、[Azure Private Link](https://azure.microsoft.com/services/private-link) はまだサポートされていません。
- Azure Stack HCI クラウド サービスは、現在、選択したリージョンでのみ使用できます。
- オペレーティング システムに対話形式でログインしたときに、"Azure Stack HCI へようこそ" というようこそ画面は、チェコ語、ハンガリー語、オランダ語、ポーランド語、スウェーデン語、およびトルコ語 (ロケール コード cs-cz、hu-hu、nl-nl、pl-pl、pt-pt、sv-se、tr-tr) ではまだローカライズされていません。 さらに、英語以外のすべての言語では、"[Y]es/[N]o" などの入力プロンプトは値 "Y" と "N" のみを受け入れます。このプロンプト自体が、誤解を招くようなローカライズされた形 (たとえば、フランス語では [O]ui/[N]on、ドイツ語では [J]a/[N]ein) で表示される場合も同様です。
- 入れ子になった仮想化を使用して Azure Stack HCI を評価する場合、仮想化ベースのセキュリティに対する Azure Stack HCI の依存関係が原因で、"仮想化サポートが有効になっていない Hyper-V をインストールできません" というようなエラーが表示されることがあります。 次の 2 つの回避策を使用できます。(1) Hyper-V 第 1 世代の VM を代わりに使用します。または、(2) Hyper-V 機能を VM の VHDX にオフラインで挿入します。 Azure Stack HCI ノードとして機能する各 VM の電源がオフになっている状態で、ホストから次の PowerShell コマンドを実行します。 **Install-WindowsFeature -Vhd \<path> -Name Hyper-V, RSAT-Hyper-V-Tools, Hyper-V-Powershell**
