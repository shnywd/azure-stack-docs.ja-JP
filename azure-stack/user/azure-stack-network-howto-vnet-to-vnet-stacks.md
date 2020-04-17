---
title: Fortinet FortiGate NVA を使用して Azure Stack Hub に VNET 間接続を確立する方法
description: Fortinet FortiGate NVA を使用して Azure Stack Hub に VNET 間接続を確立する方法
author: mattbriggs
ms.topic: how-to
ms.date: 1/22/2020
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 10/03/2019
ms.openlocfilehash: 243baa06b2374fb42ffcd70fe7cb194e193d9e34
ms.sourcegitcommit: a630894e5a38666c24e7be350f4691ffce81ab81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "77702212"
---
# <a name="establish-a-vnet-to-vnet-connection-in-azure-stack-hub-with-fortinet-fortigate-nva"></a>Fortinet FortiGate NVA を使用して Azure Stack Hub に VNET 間接続を確立する

この記事では、Fortinet FortiGate NVA (ネットワーク仮想アプライアンス) を使用して、ある Azure Stack Hub 内の VNET を別の Azure Stack Hub 内の VNET に接続します。

この記事では、現在の Azure Stack Hub に適用されている、テナントで 2 つの環境にわたって 1 つの VPN 接続しか設定できない制限事項に対処します。 ユーザーは、異なる Azure Stack Hub 間で複数の VPN 接続を許可するカスタム ゲートウェイを Linux 仮想マシンに設定する方法について学びます。 この記事の手順では、各 VNET に 1 つの FortiGate NVA を含む 2 つの VNET をデプロイします。Azure Stack Hub 環境ごとに 1 つデプロイされます。 また、2 つの VNET 間に IPSec VPN を設定するために必要な変更についても詳しく説明します。 この記事の手順は、各 Azure Stack Hub 内の各 VNET に対して繰り返してください。 

## <a name="prerequisites"></a>前提条件

-  Azure Stack Hub 統合システムへのアクセスと、このソリューションで求められるコンピューティング要件、ネットワーク要件、リソース要件をデプロイするために必要とされる空き容量。 

    > [!Note]  
    > これらの手順は、Azure Stack Development Kit (ASDK) のネットワーク制限により、ASDK では使用**できません**。 詳細については、「[ASDK の要件と考慮事項](https://docs.microsoft.com/azure-stack/asdk/asdk-deploy-considerations)」を参照してください。

-  ネットワーク仮想アプライアンス (NVA) ソリューションがダウンロードされ、Azure Stack Hub Marketplace に発行されていること。 NVA は、境界ネットワークから他のネットワークまたはサブネットへのネットワーク トラフィックのフローを制御します。 この手順では、「[Fortinet FortiGate の次世代ファイアウォールの単一の VM ソリューション](https://azuremarketplace.microsoft.com/marketplace/apps/fortinet.fortinet-FortiGate-singlevm)」を使用します。

-  FortiGate NVA をアクティブにするための、2 つ以上の有効な FortiGate ライセンス ファイル。 これらのライセンスを取得する方法については、Fortinet ドキュメント ライブラリの「[ライセンスの登録とダウンロード](https://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/19071/registering-and-downloading-your-license)」の記事を参照してください。

    この手順では、「[単一の FortiGate-VM デプロイ](ttps://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/632940/single-FortiGate-vm-deployment)」を使用します。 FortiGate NVA を Azure Stack Hub VNET に接続する手順については、オンプレミス ネットワーク内で確認できます。

    アクティブ/パッシブ (HA) 設定で FortiGate ソリューションをデプロイする方法については、Fortinet ドキュメント ライブラリの「[Azure 上の FortiGate-VM の HA](https://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/983245/ha-for-FortiGate-vm-on-azure)」の記事を参照してください。

## <a name="deployment-parameters"></a>展開のパラメーター

次の表には、これらのデプロイで使用されているパラメーターが参照用にまとめられています。

### <a name="deployment-one-forti1"></a>デプロイ 1:Forti1

| FortiGate のインスタンス名 | Forti1 |
|-----------------------------------|---------------------------|
| BYOL ライセンス/バージョン | 6.0.3 |
| FortiGate の管理ユーザー名 | fortiadmin |
| リソース グループ名 | forti1-rg1 |
| 仮想ネットワーク名 | forti1vnet1 |
| VNET のアドレス空間 | 172.16.0.0/16* |
| パブリック VNET サブネット名 | forti1-PublicFacingSubnet |
| パブリック VNET アドレス プレフィックス | 172.16.0.0/24* |
| VNET 内部サブネット名 | forti1-InsideSubnet |
| VNET 内部サブネット プレフィックス | 172.16.1.0/24* |
| FortiGate NVA の VM サイズ | 標準 F2s_v2 |
| パブリック IP アドレス名 | forti1-publicip1 |
| パブリック IP アドレスの種類 | 静的 |

### <a name="deployment-two-forti2"></a>デプロイ 2:Forti2

| FortiGate のインスタンス名 | Forti2 |
|-----------------------------------|---------------------------|
| BYOL ライセンス/バージョン | 6.0.3 |
| FortiGate の管理ユーザー名 | fortiadmin |
| リソース グループ名 | forti2-rg1 |
| 仮想ネットワーク名 | forti2vnet1 |
| VNET のアドレス空間 | 172.17.0.0/16* |
| パブリック VNET サブネット名 | forti2-PublicFacingSubnet |
| パブリック VNET アドレス プレフィックス | 172.17.0.0/24* |
| VNET 内部サブネット名 | Forti2-InsideSubnet |
| VNET 内部サブネット プレフィックス | 172.17.1.0/24* |
| FortiGate NVA の VM サイズ | 標準 F2s_v2 |
| パブリック IP アドレス名 | Forti2-publicip1 |
| パブリック IP アドレスの種類 | 静的 |

> [!Note]
> \* 上記が何らかの形 (いずれかの Azure Stack Hub の VIP プールなど) でオンプレミス ネットワーク環境と重複する場合は、別のセットのアドレス空間とサブネット プレフィックスを選択してください。 また、アドレス範囲が互いに重複していないことを確認してください。**

## <a name="deploy-the-fortigate-ngfw-marketplace-items"></a>FortiGate NGFW Marketplace 項目をデプロイする

両方の Azure Stack Hub 環境に対して、この手順を繰り返します。 

1. Azure Stack Hub ユーザー ポータルを開きます。 少なくともサブスクリプションに対する共同作成者権限を持つ資格情報を使用してください。

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image5.png)

1. **[リソースの作成]** を選択し、`FortiGate` を検索します。

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image6.png)

2. **[FortiGate NGFW]** を選択し、 **[作成]** を選択します。

3. 「[デプロイで使用されるパラメーター](#deployment-parameters)」の表にあるパラメーターを使用して、 **[基本]** を入力します。

    フォームには次のように情報を入力してください。

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image7.png)

4. **[OK]** を選択します。

5. 「[デプロイで使用されるパラメーター](#deployment-parameters)」を使用して、仮想ネットワーク、サブネット、VM サイズの詳細を指定します。

    別の名前や範囲を使用する場合は、他の Azure Stack Hub 環境内の他の VNET や FortiGate のリソースと競合するパラメーターは使用しないよう注意してください。 VNET 内の VNET IP 範囲やサブネットの範囲を設定するときは特に気を付けてください。 作成した他の VNET の IP 範囲と重複していないことを確認します。

6. **[OK]** を選択します。

7. FortiGate NVA で使用されるパブリック IP を次のように構成します。

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image8.png)

8. **[OK]** を選択し、 **[OK]** を選択します。

9. **［作成］** を選択します

デプロイには約 10 分かかります。 これでその手順を繰り返して、他の Azure Stack Hub 環境で他の FortiGate NVA と VNET のデプロイを作成できるようになりました。

## <a name="configure-routes-udrs-for-each-vnet"></a>各 VNET のルート (UDR) を構成する

forti1-rg1 と forti2-rg1 の両方のデプロイで、次の手順を実行します。

1. Azure Stack Hub ポータルで、forti1-rg1 リソース グループに移動します。

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image9.png)

2. "forti1-forti1-InsideSubnet-routes-xxxx" リソースを選択します。

3. **[設定]** で、 **[ルート]** を選択します。

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image10.png)

4. **[to-Internet]\(インターネットへ\)** ルートを削除します。

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image11.png)

5. **[はい]** を選択します。

6. **[追加]** を選択します。

7. **[ルート]** に `to-forti1` または `to-forti2` という名前を付けます。 別の IP 範囲を使用している場合は、その IP 範囲を使用します。

8. 次のように入力します。
    - forti1: `172.17.0.0/16`  
    - forti2: `172.16.0.0/16`  

    別の IP 範囲を使用している場合は、その IP 範囲を使用します。

9. **[次ホップの種類]** として **[仮想アプライアンス]** を選択します。
    - forti1: `172.16.1.4`  
    - forti2: `172.17.0.4`  

    別の IP 範囲を使用している場合は、その IP 範囲を使用します。

    ![](./media/azure-stack-network-howto-vnet-to-vnet-stacks/image12.png)

10. **[保存]** を選択します。

各リソース グループについて、各 **InsideSubnet** ルートに対して同じ手順を繰り返します。

## <a name="activate-the-fortigate-nvas-and-configure-an-ipsec-vpn-connection-on-each-nva"></a>FortiGate NVA をアクティブにして各 NVA で IPSec VPN 接続を構成する

 各 FortiGate NVA をアクティブにするには、Fortinet が提供する有効なライセンス ファイルが必要です。 各 NVA がアクティブになるまで、NVA は機能**しません**。 ライセンス ファイルを取得する方法と NVA をアクティブにする手順については、Fortinet ドキュメント ライブラリの「[ライセンスの登録とダウンロード](https://docs2.fortinet.com/vm/azure/FortiGate/6.2/azure-cookbook/6.2.0/19071/registering-and-downloading-your-license)」の記事を参照してください。

2 つのライセンス ファイル (NVA ごとに 1 つ) を取得する必要があります。

## <a name="create-an-ipsec-vpn-between-the-two-nvas"></a>2 つの NVA 間に IPSec VPN を作成する

NVA がアクティブになったら、次の手順に従って、2 つの NVA 間に IPSec VPN を作成します。

forti1 NVA と forti2 NVA の両方について、次の手順に従います。

1.  fortiX VM の概要ページに移動して、割り当てられたパブリック IP アドレスを取得します。

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image13.png)

2.  割り当てられた IP アドレスをコピーし、ブラウザーを開いてアドレス バーにそのアドレスを貼り付けます。 セキュリティ証明書が信頼されていないという警告がブラウザーに表示される場合があります。 そのまま続行します。

4.  デプロイ中に指定した FortiGate の管理ユーザー名とパスワードを入力します。

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image14.png)

5.  **[System]\(システム\)**  >  **[Firmware]\(ファームウェア\)** を選択します。

6.  最新のファームウェアを示すボックスを選択します (例: `FortiOS v6.2.0 build0866`)。

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image15.png)

7.  **[Backup config and upgrade]\(コンフィグをバックアップしてアップグレード\)** を選択し、プロンプトが表示されたら [Continue]\(続行\) を選択します。

8.  NVA でファームウェアが最新のビルドに更新され、再起動されます。 このプロセスには約 5 分かかります。 FortiGate Web コンソールにもう一度ログインします。

10.  **[VPN]**  >  **[IPSec Wizard]\(IPSec ウィザード\)** をクリックします。

11. VPN の名前を入力します。たとえば、 **[VPN Creation Wizard]\(VPN 作成ウィザード\)** で「`conn1`」と入力します。

12. **[This site is behind NAT]\(このサイトは NAT の背後にある\)** を選択します。

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image16.png)

13. **[次へ]** を選択します。

14. 接続先となるオンプレミスの VPN デバイスのリモート IP アドレスを入力します。

15. **[Outgoing Interface]\(送信インターフェイス\)** として **[port1]** を選択します。

16. **[Pre-shared Key]\(事前共有キー\)** を選択し、事前共有キーを入力 (および記録) します。 

    > [!Note]  
    > このキーは、オンプレミスの VPN デバイスで接続を設定するために必要であるため、"*正確に*" 一致する必要があります。

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image17.png)

17. **[次へ]** を選択します。

18. **[Local Interface]\(ローカル インターフェイス\)** として **[port2]** を選択します。

19. 次のローカル サブネットの範囲を入力します。
    - forti1:172.16.0.0/16
    - forti2:172.17.0.0/16

    別の IP 範囲を使用している場合は、その IP 範囲を使用します。

20. オンプレミスの VPN デバイスを介して接続する、オンプレミス ネットワークを表す適切なリモート サブネットを入力します。
    - forti1:172.16.0.0/16
    - forti2:172.17.0.0/16

    別の IP 範囲を使用している場合は、その IP 範囲を使用します。

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image18.png)

21. **[作成]**

22. **[Network]\(ネットワーク\)**  >  **[Interfaces]\(インターフェイス\)** を選択します。

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image19.png)

23. **[port2]** をダブルクリックします。

24. **[Role]\(ロール\)** の一覧から **[LAN]** を、アドレス指定モードとして **[DHCP]** を選択します。

25. **[OK]** を選択します。

他の NVA に対して同じ手順を繰り返します。


## <a name="bring-up-all-phase-2-selectors"></a>すべてのフェーズ 2 セレクターを表示する 

"**両方**" の NVA で上記が完了したら、次のようにします。

1.  forti2 の FortiGate Web コンソールで、 **[Monitor]\(モニター\)**  >  **[IPsec Monitor]\(IPsec モニター\)** を選択します。 

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image20.png)

2.  `conn1` を強調表示し、 **[Bring Up]\(表示\)**  >  **[All Phase 2 Selectors]\(すべてのフェーズ 2 セレクター\)** を選択します。

    ![](./media/azure-stack-network-howto-vnet-to-vnet/image21.png)


## <a name="test-and-validate-connectivity"></a>接続のテストと検証

これで、FortiGate NVA を介して各 VNET 間をルーティングできるようになりました。 接続を検証するには、各 VNET の InsideSubnet に Azure Stack Hub VM を作成します。 Azure Stack Hub VM の作成は、ポータル、CLI、または PowerShell を使用して行うことができます。 VM を作成する際は、次の手順に従います。

-   Azure Stack Hub VM は、各 VNET の **InsideSubnet** に配置されます。

-   VM の作成時には、NSG は適用**しません** (ポータルから VM を作成している場合は、既定で追加される NSG を削除してください)。

-   接続をテストするために使用する通信が VM のファイアウォール規則によって許可されていることを確認します。 テスト目的の場合は、可能な限り、OS 内でファイアウォールを完全に無効にすることをお勧めします。

## <a name="next-steps"></a>次のステップ

[Azure Stack Hub ネットワークの違いと考慮事項](azure-stack-network-differences.md)  
[Fortinet FortiGate を使用して Azure Stack Hub にネットワーク ソリューションを提供する](../operator/azure-stack-network-solutions-enable.md)  