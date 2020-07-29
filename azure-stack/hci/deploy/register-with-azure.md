---
title: Azure Stack HCI を Azure に接続する
description: Azure Stack HCI を Azure に登録する方法。
author: khdownie
ms.author: v-kedow
ms.topic: how-to
ms.date: 07/22/2020
ms.openlocfilehash: 9b8e7e211f6c2ce21f0b00ed2a3b972418f9f9bd
ms.sourcegitcommit: 16ff77f7157e5b04a8cd401b095f7b71f51d5a11
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86949612"
---
# <a name="connect-azure-stack-hci-to-azure"></a>Azure Stack HCI を Azure に接続する

> Azure Stack HCI v20H2、Windows Server 2019 に適用されます

Azure Stack HCI は Azure サービスとして提供され、Azure オンライン サービス条件に従ってインストールから 30 日以内に登録する必要があります。 このトピックでは、監視、サポート、課金、およびハイブリッド サービスのために、Azure Stack HCI クラスターを [Azure Arc](https://azure.microsoft.com/services/azure-arc/) に登録する方法について説明します。 登録時に、オンプレミスの個々の Azure Stack HCI クラスターを表す Azure Resource Manager リソースが作成されます。これにより、Azure 管理プレーンが Azure Stack HCI に効果的に拡張されます。 Azure リソースとオンプレミス クラスターの間で情報が定期的に同期されます。 

## <a name="prerequisites-for-registration"></a>登録の前提条件

Azure Stack HCI クラスターを作成しないと、Azure に登録することはできません。 ノードは物理マシンまたは仮想マシンにすることができますが、Unified Extensible Firmware Interface (UEFI) である必要があります。つまり、Hyper-V 第 1 世代の仮想マシンを使用することはできません。 Azure Arc 登録は Azure Stack HCI のネイティブ機能であるため、エージェントは必要ありません。

### <a name="internet-access"></a>インターネットへのアクセス

Azure Stack HCI ノードは、Azure に接続するためにクラウドに接続する必要があります。 たとえば、次の送信 ping は成功します。

```PowerShell
C:\> ping bing.com
```

### <a name="azure-subscription"></a>Azure サブスクリプション

まだ Azure アカウントを持っていない場合は[作成します](https://azure.microsoft.com/)。 

以下のいずれかの種類の既存のサブスクリプションを使用できます。
- [学生](https://azure.microsoft.com/free/students/)または [Visual Studio サブスクライバー](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)向けの Azure クレジットが付いた無料アカウント
- クレジット カードを使用した[従量課金制](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/)サブスクリプション
- マイクロソフトエンタープライズ契約 (EA) を通じて取得されたサブスクリプション
- クラウド ソリューション プロバイダー (CSP) プログラムを通じて取得されたサブスクリプション

### <a name="azure-active-directory-permissions"></a>Azure Active Directory のアクセス許可

登録プロセスを完了するには、Azure Active Directory のアクセス許可が必要です。 まだお持ちでない場合は、Azure AD 管理者に依頼して、アクセス許可を付与または委任してもらいます。 詳しくは、「[Azure の登録を管理する](../manage/manage-azure-registration.md#azure-active-directory-permissions)」をご覧ください。

## <a name="register-using-powershell"></a>PowerShell を使用して登録する

Azure Stack HCI オペレーティング システムを実行しているクラスター内の 1 つ以上のサーバーで、次の PowerShell コマンドを実行して、Azure Stack HCI 用の PowerShell モジュールをインストールします。

```PowerShell
Install-WindowsFeature RSAT-Azure-Stack-HCI -ComputerName Server1
```

管理 PC またはクラスター ノードに必要なコマンドレットをインストールします。

```PowerShell
Install-Module Az.StackHCI
```
   > [!NOTE]
   > 1. "今すぐ PowerShellGet で NuGet プロバイダーをインストールしてインポートしますか?" などのプロンプトが表示される場合があります。 これには [はい] (Y) と答える必要があります。
   > 2. "'PSGallery' からモジュールをインストールしますか? " というメッセージが表示され、[はい] (Y) と答える必要があります。
   > 3. 最後に、**Az** モジュール全体をインストールすると **StackHCI** サブモジュールが含められ、それが長期的に正しいと思われるかもしれませんが、 標準の Azure PowerShell 規約に従い、プレビュー段階のサブモジュールは自動的には含められません。そうではなく、これらを明示的に指定する必要があります。 したがって、ここでは上記のように **Az.StackHCI** を明示的に要求する必要があります。

登録します (これにより、Azure のログインが求められます)。

```PowerShell
Register-AzStackHCI  -SubscriptionId "e569b8af-6ecc-47fd-a7d5-2ac7f23d8bfe" [-ResourceName] [-ResourceGroupName] [-ComputerName –Credential]
```

最小構文では、Azure サブスクリプション ID のみが必要です。 上記のコマンドを実行するユーザーには Azure Active Directory のアクセス許可が必要です。そうでない場合は登録プロセスが完了しないことに注意してください。

   > [!NOTE]
   > 登録時に以下のメッセージのようなエラーが表示された場合は、**24 から 48 時間後に再び登録してみてください**。 Azure の統合は、リージョンをまたいでロールアウトが進行中です。 引き続き評価を続行することは可能であり、いかなる機能にも影響はありません。 後で戻って登録するようにしてください。
   >
   > `Register-AzStackHCI : Azure Stack HCI is not yet available in region <regionName>`

## <a name="authenticate-with-azure"></a>Azure での認証
依存関係がインストールされ、パラメーターが検証されたら、Azure アカウントを使用して認証 (サインイン) する必要があります。 登録を続行するには、指定された Azure サブスクリプションへのアクセス権がアカウントに必要です。

Windows 10 などのデスクトップ エクスペリエンスを備えたオペレーティング システムから Register-AzStackHCI をリモートで実行した場合は、対話型の Azure ログイン ウィンドウがポップアップ表示されます。 表示される正確なプロンプトは、セキュリティ設定 (2 要素認証など) によって異なります。 画面の指示に従ってログインします。

対話型の Azure ログイン ウィンドウを表示できない Azure Stack HCI オペレーティング システムで Register-AzStackHCI をローカルで実行した場合は、別のデバイス (PC や電話など) で microsoft.com/devicelogin にアクセスし、コードを入力してサインインするように求められます。 これは、入力方式が限定されている他のデバイス (Xbox など) に対して Microsoft が使用するのと同じエクスペリエンスです。

登録ワークフローではログインしたことが検出され、完了に進みます。 その後、Azure portal でクラスターを確認できるようになります。

## <a name="next-steps"></a>次のステップ

これで以下の準備が整いました。

- [クラスターの検証](validate.md)
- [ボリュームの作成](../manage/create-volumes.md)
