---
title: Microsoft Azure Stack Development Kit のリリース ノート | Microsoft Docs
description: Azure Stack Development Kit の機能強化、修正、既知の問題。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 06/04/2019
ms.openlocfilehash: 2ca85da5d9fde42fb06eef149e7304ab08bc32ee
ms.sourcegitcommit: 7f39bdc83717c27de54fe67eb23eb55dbab258a9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2019
ms.locfileid: "66691197"
---
# <a name="asdk-release-notes"></a>ASDK リリース ノート

この記事では、Azure Stack Development Kit (ASDK) の変更、修正、および既知の問題に関する情報を提供します。 実行しているバージョンが不明な場合は、[ポータルを使用して確認](../operator/azure-stack-updates.md#determine-the-current-version)できます。

[![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS フィード](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)をサブスクライブして、ASDK の新着情報を常に把握するようにしてください。

## <a name="build-11905040"></a>ビルド 1.1905.0.40

<!-- ### Changes -->

### <a name="new-features"></a>新機能

- このリリースでの新機能の一覧については、Azure Stack リリース ノートの[このセクション](../operator/azure-stack-release-notes-1905.md#whats-in-this-update)を参照してください。

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

- 登録スクリプトの実行中にサービス プリンシパルのタイムアウトが起こるため、正常に [ASDK を登録](asdk-register.md)するには、**RegisterWithAzure.psm1** PowerShell スクリプトを編集する必要があります。 以下の手順を実行します。

  1. ASDK ホスト コンピューター上で、管理者特権のアクセス許可を使用してエディターでファイル **C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1** を開きます。
  2. 1249 行目の末尾に、`-TimeoutInSeconds 1800` パラメーターを追加します。 これが必要なのは、登録スクリプトの実行中にサービス プリンシパルのタイムアウトが起こるためです。 1249 行目は次のようになります。

     ```powershell
      $servicePrincipal = Invoke-Command -Session $PSSession -ScriptBlock { New-AzureBridgeServicePrincipal -RefreshToken $using:RefreshToken -AzureEnvironment $using:AzureEnvironmentName -TenantId $using:TenantId -TimeoutInSeconds 1800 }
      ```

- このリリースで修正されたその他の Azure Stack の問題の一覧については、Azure Stack リリース ノートの[このセクション](../operator/azure-stack-release-notes-1905.md#fixes)を参照してください。
- 既知の問題の一覧については、[この記事](../operator/azure-stack-release-notes-known-issues-1905.md)を参照してください。
- [使用可能な Azure Stack 修正プログラム](../operator/azure-stack-release-notes-1905.md#hotfixes)が Azure Stack ASDK には適用できないことに注意してください。

## <a name="build-11904036"></a>ビルド 1.1904.0.36

<!-- ### Changes -->

### <a name="new-features"></a>新機能

- このリリースでの新機能の一覧については、Azure Stack リリース ノートの[このセクション](../operator/azure-stack-release-notes-1904.md#whats-in-this-update)を参照してください。

### <a name="fixed-and-known-issues"></a>修正された問題と既知の問題

- 登録スクリプトの実行中にサービス プリンシパルのタイムアウトが起こるため、正常に [ASDK を登録](asdk-register.md)するには、**RegisterWithAzure.psm1** PowerShell スクリプトを編集する必要があります。 以下の手順を実行します。

  1. ASDK ホスト コンピューター上で、管理者特権のアクセス許可を使用してエディターでファイル **C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1** を開きます。
  2. 1249 行目の末尾に、`-TimeoutInSeconds 1800` パラメーターを追加します。 これが必要なのは、登録スクリプトの実行中にサービス プリンシパルのタイムアウトが起こるためです。 1249 行目は次のようになります。

     ```powershell
      $servicePrincipal = Invoke-Command -Session $PSSession -ScriptBlock { New-AzureBridgeServicePrincipal -RefreshToken $using:RefreshToken -AzureEnvironment $using:AzureEnvironmentName -TenantId $using:TenantId -TimeoutInSeconds 1800 }
      ```

- [このリリース 1902](#known-issues) で特定された VPN 接続の問題が修正されました。

- このリリースで修正されたその他の Azure Stack の問題の一覧については、Azure Stack リリース ノートの[このセクション](../operator/azure-stack-release-notes-1904.md#fixes)を参照してください。
- 既知の問題の一覧については、[この記事](../operator/azure-stack-release-notes-known-issues-1904.md)を参照してください。
- [使用可能な Azure Stack 修正プログラム](../operator/azure-stack-release-notes-1904.md#hotfixes)が Azure Stack ASDK には適用できないことに注意してください。

## <a name="build-1903"></a>ビルド 1903

1903 ペイロードに、ASDK リリースは含まれません。

## <a name="build-11902069"></a>ビルド 1.1902.0.69

### <a name="new-features"></a>新機能

- 1902 ビルドでは、Azure Stack の管理者ポータル上にプラン、オファー、クォータ、アドオン プランの作成のための新しいユーザー インターフェイスを導入しています。 スクリーンショットを含めた詳細については、[プラン、オファー、クォータの作成](../operator/azure-stack-create-plan.md)に関するページを参照してください。

- このリリースでのその他の変更点および機能強化点の一覧は、Azure Stack リリース ノートの[このセクション](../operator/azure-stack-update-1902.md#improvements)を参照してください。

<!-- ### New features

- For a list of new features in this release, see [this section](../operator/azure-stack-update-1902.md#new-features) of the Azure Stack release notes.

### Fixed and known issues

- For a list of issues fixed in this release, see [this section](../operator/azure-stack-update-1902.md#fixed-issues) of the Azure Stack release notes. For a list of known issues, see [this section](../operator/azure-stack-update-1902.md#known-issues-post-installation).
- Note that [available Azure Stack hotfixes](../operator/azure-stack-update-1902.md#azure-stack-hotfixes) are not applicable to the Azure Stack ASDK. -->

### <a name="known-issues"></a>既知の問題

- [この記事](asdk-connect.md)の手順を使用して別のホストから ASDK へのVPN 接続を確立する際に問題が発生します。 VPN クライアントから ASDK 環境に接続しようとすると、正しいアカウントを使用し、パスワードを正しく入力した場合でも、 **[ユーザー名またはパスワードが正しくありません]** というエラーが表示されます。 この問題は、お使いの資格情報ではなく、ASDK の VPN 接続に使用される認証プロトコルの変更で発生します。 この問題を回避するには、次の手順を実行します。

   まず、ASDK のサーバー側で使用される認証プロトコルを変更します。

   1. ASDK ホストへの RDP を実行します。
   2. 管理者特権の PowerShell セッションを開き、デプロイ時に設定したパスワードを使用して、AzureStack\AzureStackAdmin としてサインインします。
   3. 次のコマンドを実行します。

      ```powershell
      netsh nps set np name = "Connections to Microsoft Routing and Remote Access server" profileid = "0x100a" profiledata = "1A000000000000000000000000000000" profileid = "0x1009" profiledata = "0x5"
      restart-service remoteaccess -force
      ```

   次に、クライアント側の接続スクリプトを変更します。 これを行う最も簡単な方法は、C:\AzureStack-Tools-master\connect\azurestack.connect.psm1 スクリプト モジュールを直接変更することです。

   1. **Add-AzsVpnConnection** コマンドレットを変更して、`AuthenticationMethod` パラメーターを `MsChapv2` から `EAP` に変更します。

      ```powershell
      $connection = Add-VpnConnection -Name $ConnectionName -ServerAddress $ServerAddress -TunnelType L2tp -EncryptionLevel Required -AuthenticationMethod Eap -L2tpPsk $PlainPassword -Force -RememberCredential -PassThru -SplitTunneling
      ```

   2. EAP では対話型サインインが必要なため、**Connect-AzsVpn** コマンドレットを `rasdial @ConnectionName $User $PlainPassword` の使用から `rasphone` の使用に変更します。

      ```powershell
      rasphone $ConnectionName
      ```

   3. 変更を保存してから、**azurestack.connect.psm1** モジュールを再インポートします。
   4. [この記事](asdk-connect.md#set-up-vpn-connectivity)の手順に従ってください。
   5. VPN 経由で ASDK に接続する場合、タスクバーから接続するのではなく、Windows の **[ネットワークとインターネットの設定]** 、 **[VPN]** の順に移動して接続し、認証情報を求められるようにします。

- 内部ロード バランサー (ILB) を宛先としたパケットが 1,450 バイトを上回る場合にドロップする問題が発見されています。 この問題は、ホストの MTU の設定が、ロールを走査するカプセル化された VXLAN パケットに対応できないほど低い値になっていることが原因となって発生します。この設定は、1901 の時点でホストに移行されました。 この問題が発生することが確認されているシナリオは少なくとも 2 つあります。

  - 内部ロード バランサー (ILB) の背後にある SQL Always-On に SQL クエリを実行し、そのクエリが 660 バイトを超えるとき。
  - 複数のマスターを有効にしようとした場合に、Kubernetes のデプロイに失敗したとき。  

  この問題は、同じ仮想ネットワークにあるものの、サブネットが異なる VM と ILB の間に通信がある場合に発生します。 ASDK ホスト上で管理者特権でのコマンド プロンプトを起動し、次のコマンドを実行すると、この問題を回避できます。

  ```shell
  netsh interface ipv4 set sub "hostnic" mtu=1660
  netsh interface ipv4 set sub "management" mtu=1660
  ```
