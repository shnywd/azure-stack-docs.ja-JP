---
title: クラスター監視のセットアップ
description: クラスター監視のセットアップ方法を学習します
author: v-dasis
ms.topic: how-to
ms.date: 07/21/2020
ms.author: v-dasis
ms.reviewer: JasonGerend
ms.openlocfilehash: e660d1cc10e5d4b4e3d1ffbebeee99e421cd6dbb
ms.sourcegitcommit: a15a0f955bac922cebb7bf90a72384fd84ddfe56
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86947471"
---
# <a name="set-up-a-cluster-witness"></a>クラスター監視のセットアップ

> 適用対象: Azure Stack HCI バージョン 20H2、Windows Server 2019

監視リソースのセットアップはすべてのクラスターで必須であり、クラスターを作成した直後にセットアップする必要があります。 2 ノード クラスターでは、どちらかのサーバーがオフラインになったときに他のノードも使用できなくなることがないように監視が必要です。 3 つ以上のノードのクラスターでは、2 台のサーバーが障害が発生するかそれらがオフラインになったときに備えて監視が必要です。  

ファイル共有を監視として使用するか、Azure クラウド監視を使用することができます。 クラスター内のすべてのサーバー ノードに信頼性の高いインターネット接続がある場合は、Azure クラウド監視が推奨されます。 詳しくは、「[フェールオーバー クラスターにクラウド監視を展開する](/windows-server/failover-clustering/deploy-cloud-witness)」をご覧ください。

ファイル共有監視の場合、ファイル サーバーには要件があります。 詳細については、「[Azure Stack HCI をデプロイする前に](before-you-start.md)」を参照してください。

## <a name="set-up-a-witness-using-windows-admin-center"></a>Windows Admin Center を使用して監視をセットアップする

1. Windows Admin Center で、上部のドロップダウン矢印から **[Cluster Manager]\(クラスター マネージャー\)** を選択します。
1. **[Cluster connections]\(クラスター接続\)** の下で、クラスターを選択します。
1. **[ツール]** の **[設定]** を選択します。
1. 右側のペインで **[Witness]\(監視\)** を選択します。
1. **[Witness type]\(監視の種類\)** ボックスの一覧で、次のいずれかを選択します。
      - **[クラウド監視]** - Azure ストレージ アカウント名、キー、エンドポイントを入力します
      - **[ファイル共有監視]** - ファイル共有パス (//server/share) を入力します。

> [!NOTE]
> 3 番目のオプションである **[ディスク監視]** は、ストレッチ クラスターでの使用に適していません。

## <a name="set-up-a-witness-using-windows-powershell"></a>Windows PowerShell を使用して監視をセットアップする

PowerShell を使用してクラスター監視をセットアップするには、次のいずれかのコマンドレットを実行します。

Azure クラウド監視を作成するには、次のコマンドレットを使用します。

```powershell
Set-ClusterQuorum –Cluster Cluster1 -CloudWitness -AccountName <AzureStorageAccountName> -AccessKey <AzureStorageAccountAccessKey>
```

ファイル共有監視を作成するには、次のコマンドレットを使用します。

```powershell
Set-ClusterQuorum –Cluster Cluster1 -FileShareWitness \\fileserver\fsw
```

## <a name="next-steps"></a>次のステップ

クラスター クォーラムの詳細については、「[Azure Stack HCI のクラスターとプールのクォーラムについて](../concepts/quorum.md)」を参照してください。
