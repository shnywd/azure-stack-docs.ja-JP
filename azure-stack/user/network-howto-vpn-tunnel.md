---
title: Azure Stack Hub で複数サイト間 VPN トンネルを設定する方法 | Microsoft Docs
description: Azure Stack Hub で複数サイト間 VPN トンネルを設定する方法について説明します。
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: how-to
ms.date: 09/19/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 09/19/2019
ms.openlocfilehash: 4593898a1ea70b2001c252f885b12db2f16e922e
ms.sourcegitcommit: d450dcf5ab9e2b22b8145319dca7098065af563b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75883050"
---
# <a name="how-to-set-up-a-multiple-site-to-site-vpn-tunnel-in-azure-stack-hub"></a>Azure Stack Hub で複数サイト間 VPN トンネルを設定する方法

この記事では、Azure Stack Hub Resource Manager テンプレートを使用してソリューションをデプロイする方法について説明します。 このソリューションでは、仮想ネットワークに関連付けられた複数のリソース グループと、これらのシステムの接続方法を作成します。

テンプレートは [Azure Intelligent Edge Patterns](https://github.com/Azure-Samples/azure-intelligent-edge-patterns) GitHub リポジトリにあります。 このテンプレートは、**rras-gre-vnet-vnet** フォルダーにあります。 

## <a name="scenarios"></a>シナリオ

![](./media/azure-stack-network-howto-vpn-tunnel/scenarios.png)

## <a name="create-multiple-vpn-tunnels"></a>複数の VPN トンネルを作成する

![](./media/azure-stack-network-howto-vpn-tunnel/image1.png)

-  3 層アプリケーション (Web、アプリ、および DB) をデプロイします。

-  最初の 2 つのテンプレートを別の Azure Stack Hub インスタンスにデプロイします。

-  **WebTier** は PPE1 にデプロイされ、**AppTier** は PPE2 にデプロイされます。

-  IKE トンネルを使用して **WebTier** と **AppTier** を接続します。

-  **AppTier** を、**DBTier** を呼び出すオンプレミス システムに接続します。

## <a name="steps-to-deploy-multiple-vpns"></a>複数の VPN をデプロイする手順

これは複数の手順があるプロセスです。 このソリューションでは、Azure Stack Hub ポータルを使用します。 ただし、PowerShell、Azure CLI、またはその他のコードとしてのインフラストラクチャ ツール チェーンを使用して出力をキャプチャし、入力として使用することができます。

![alt text](./media/azure-stack-network-howto-vpn-tunnel/image2.png)

## <a name="walkthrough"></a>チュートリアル

### <a name="deploy-web-tier-to-azure-stack-hub-instances-ppe1"></a>Web 層を Azure Stack Hub インスタンス PPE1 にデプロイする

1.  Azure Stack Hub ユーザー ポータルを開き、 **[リソースの作成]** を選択します。

2.  **[テンプレートのデプロイ]** を選択します。

    ![](./media/azure-stack-network-howto-vpn-tunnel/image3.png)

3.  **azure-intelligent-edge-patterns/rras-vnet-vpntunnel** リポジトリから azuredeploy.json の内容をコピーし、テンプレート ウィンドウに貼り付けます。 テンプレート内に含まれているリソースを確認し、 **[保存]** を選択します。

    ![](./media/azure-stack-network-howto-vpn-tunnel/image4.png)

4.  **[リソース グループ]** の名前を入力し、パラメーターを確認します。

    > ![注]  
    > WebTier のアドレス空間は **10.10.0.0/16** なので、リソース グループの場所は **PPE1** であることがわかります。

    ![](./media/azure-stack-network-howto-vpn-tunnel/image5.png)

### <a name="deploy-app-tier-to-the-second-azure-stack-hub-instances"></a>2 つ目の Azure Stack Hub インスタンスにアプリ層をデプロイする

次に示すように、**WebTier** と同じプロセスを使用できますが、パラメーターは異なります。

> [!Note]  
> AppTier のアドレス空間は **10.20.0.0/16** なので、リソース グループの場所は **WestUS2** であることがわかります。

![](./media/azure-stack-network-howto-vpn-tunnel/image6.png)

### <a name="review-the-deployments-for-web-tier-and-app-tier-and-capture-outputs"></a>Web 層とアプリケーション層のデプロイとキャプチャの出力を確認する

1.  デプロイが正常に完了したことを確認します。 **[出力]** を選択します

    ![](./media/azure-stack-network-howto-vpn-tunnel/image7.png)

3.  最初の 4 つの値をメモ帳アプリにコピーします。

    ![](./media/azure-stack-network-howto-vpn-tunnel/image8.png)

5.  **AppTier** のデプロイに対してこの手順を繰り返します。

![](./media/azure-stack-network-howto-vpn-tunnel/image9.png)

![](./media/azure-stack-network-howto-vpn-tunnel/image10.png)

### <a name="create-tunnel-from-web-tier-to-app-tier"></a>Web 層からアプリ層へのトンネルを作成する

1.  Azure Stack Hub ユーザー ポータルを開き、 **[リソースの作成]** を選択します。

2.  **[テンプレートのデプロイ]** を選択します。

3.  **azuredeploy.tunnel.ike.json** の内容を貼り付けます。

4.  **[パラメーターの編集]** を選択します。

![](./media/azure-stack-network-howto-vpn-tunnel/image11.png)

### <a name="create-tunnel-from-app-tier-to-web-tier"></a>アプリ層から web 層へのトンネルを作成する

1.  Azure Stack Hub ユーザー ポータルを開き、 **[リソースの作成]** を選択します。

2.  **[テンプレートのデプロイ]** を選択します。

3.  **azuredeploy.tunnel.ike.json** の内容を貼り付けます。

4.  **[パラメーターの編集]** を選択します。

![](./media/azure-stack-network-howto-vpn-tunnel/image12.png)

### <a name="viewing-tunnel-deployment"></a>トンネルのデプロイの表示

カスタム スクリプト拡張機能からの出力を表示すると、トンネルが作成されていることを確認できます。また、その状態が表示されます。 一方には、もう一方の準備が整うまで待つ間、**接続中**と表示され、もう一方には、デプロイされると**接続済み**と表示されます。

![](./media/azure-stack-network-howto-vpn-tunnel/image13.png)

![](./media/azure-stack-network-howto-vpn-tunnel/image14.png)

![](./media/azure-stack-network-howto-vpn-tunnel/image15.png)

### <a name="troubleshooting-on-the-rras-vm"></a>RRAS VM でのトラブルシューティング

1.  RDP 規則を **Deny** から **Allow** に変更します。

2.  デプロイ時に設定した資格情報を使用して、リモート デスクトップ (DRP) クライアントでシステムに接続します。

3.  管理者特権のプロンプトで PowerShell を開き、`get-VPNS2SInterface` を実行します。

    ![](./media/azure-stack-network-howto-vpn-tunnel/image16.png)

5.  **RemoteAccess** コマンドレットを使用してシステムを管理します。

    ![](./media/azure-stack-network-howto-vpn-tunnel/image17.png)

### <a name="install-rras-on-a-on-premises-vm-db-tier"></a>オンプレミスの VM DB 層に RRAS をインストールする

1.  ターゲットは Windows 2016 イメージです。

2.  リポジトリから `Add-Site2SiteIKE.ps1` スクリプトをコピーしてローカルで実行すると、スクリプトによって **WindowsFeature** と **RemoteAccess** がインストールされます。

    > [!Note]
    > 環境によっては、システムの再起動が必要になる場合があります。

    詳細については、オンプレミスのマシンのネットワーク構成を参照してください。

    ![](./media/azure-stack-network-howto-vpn-tunnel/image18.png)

3.  AppTier テンプレートのデプロイから記録された **Output** パラメーターを追加してスクリプトを実行します。

    ![](./media/azure-stack-network-howto-vpn-tunnel/image19.png)

5.  これでトンネルが構成され、AppTier 接続の待機状態になります。

### <a name="configure-app-tier-to-db-tier"></a>アプリ層から DB 層を構成する

1.  Azure Stack Hub ユーザー ポータルを開き、 **[リソースの作成]** を選択します。

2.  **[テンプレートのデプロイ]** を選択します。

3.  **azuredeploy.tunnel.ike.json** の内容を貼り付けます。

4.  **[パラメーターの編集]** を選択します。

    ![](./media/azure-stack-network-howto-vpn-tunnel/image20.png)

5.  AppTier が選択され、リモート内部ネットワークが 10.99.0.1 に設定されていることを確認します。

### <a name="confirm-tunnel-between-app-tier-and-db-tier"></a>アプリ層と DB 層の間のトンネルを確認する

1.  VM にログインせずにトンネルを確認するには、カスタム スクリプト拡張機能を実行します。

2.  RRAS VM (AppTier) にアクセスします。

3.  **[拡張機能]** を選択し、**カスタム スクリプト拡張機能を実行**します。

4.  **azure-intelligent-edge-patterns/rras-vnet-vpntunnel** リポジトリのスクリプト ディレクトリを参照します。 **Get-VPNS2SInterfaceStatus.ps1** を選択します。

    ![](./media/azure-stack-network-howto-vpn-tunnel/image21.png)

5.  RDP を有効にしてサインインし、`get-vpns2sinterface` で PowerShell を開いて実行すると、トンネルが接続されていることがわかります。

    **DBTier**

    ![](./media/azure-stack-network-howto-vpn-tunnel/image22.png)

    **AppTier**

    ![](./media/azure-stack-network-howto-vpn-tunnel/image23.png)

    > [!Note]  
    > 1 台目のマシンから 2 台目へ、および 2 台目から 1 台目の両方で RDP をテストできます。

    > [!Note]  
    > このソリューションをオンプレミスで実装するには、Azure Stack Hub リモート ネットワークへのルートをスイッチング インフラストラクチャに、または少なくとも特定の VM にデプロイする必要があります。

### <a name="deploying-a-gre-tunnel"></a>GRE トンネルのデプロイ

このチュートリアルのこのテンプレートでは、[IKE テンプレート](network-howto-vpn-tunnel-ipsec.md)を使用しました。 ただし、[GRE トンネル](network-howto-vpn-tunnel-gre.md)をデプロイすることもできます。 このトンネルにより、スループットがさらに高くなります。

プロセスはほぼ同じです。 ただし、トンネル テンプレートを既存のインフラストラクチャにデプロイする場合、最初の 3 つの入力に他のシステムからの出力を使用する必要があります。 接続対象のリソース グループではなく、デプロイ先のリソース グループの **LOCALTUNNELGATEWAY** を知る必要があります。

![](./media/azure-stack-network-howto-vpn-tunnel/image24.png)

## <a name="next-steps"></a>次のステップ

[Azure Stack Hub ネットワークの違いと考慮事項](azure-stack-network-differences.md)  
[GRE を使用して VPN トンネルを作成する方法](network-howto-vpn-tunnel-gre.md)  
[IPSEC を使用して VPN トンネルを作成する方法](network-howto-vpn-tunnel-ipsec.md)