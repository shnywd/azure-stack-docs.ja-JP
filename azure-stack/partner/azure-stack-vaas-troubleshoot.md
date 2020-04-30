---
title: サービスとしての検証のトラブルシューティング
titleSuffix: Azure Stack Hub
description: Azure Stack Hub のサービスとしての検証のトラブルシューティングを行います。
author: mattbriggs
ms.topic: article
ms.date: 04/20/2020
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 1bcca404c451190ccf1d0b82e93aea655e069044
ms.sourcegitcommit: 32834e69ef7a804c873fd1de4377d4fa3cc60fb6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81661404"
---
# <a name="troubleshoot-validation-as-a-service"></a>サービスとしての検証のトラブルシューティング

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

以下に示すのは、ソフトウェアのリリースやそのソリューションとは関係のない一般的な問題です。

## <a name="local-agent"></a>ローカル エージェント

### <a name="the-portal-shows-local-agent-in-debug-mode"></a>ポータルがデバッグ モードでローカル エージェントを表示する

この問題は、ネットワーク接続が不安定なためエージェントがハートビートをサービスに送信できないことが原因であると考えられます。 ハートビートは 5 分ごとに送信されます。 サービスが 15 分間ハートビートを受信しない場合、サービスはエージェントがアクティブでないと認識し、そのエージェントのテストはスケジュールされなくなります。 エージェントが起動されたディレクトリにある *Agenthost.log* ファイルのエラー メッセージを確認します。

> [!Note]
> エージェントで既に実行中であるテストはそのまま実行されますが、テストが終了するまでにハートビートが復活しない場合、エージェントはテスト状態の更新やログのアップロードに失敗します。 テストは常に **running** と表示され、キャンセルする必要があります。

### <a name="agent-process-on-machine-was-shut-down-while-executing-test-what-to-expect"></a>テストの実行中にコンピューター上のエージェント プロセスがシャットダウンされた。 予期される事柄

エージェント プロセスが正常ではない方法でシャットダウンされた場合、実行されていたテストは **running** と表示されたままになります。 正常でないシャットダウンの例としては、マシンが再起動されてプロセスが強制終了される場合が挙げられます (エージェント ウィンドウでの CTRL + C は正常なシャットダウンと見なされます)。 エージェントが再起動される場合、エージェントはテストの状態を **canceled** に更新します。 エージェントが再起動されない場合、テストは **running** と表示され、テストを手動でキャンセルする必要があります。

> [!Note]
> ワークフロー内のテストは、連続して実行されるようにスケジュールされます。 **保留中**のテストは、同じワークフロー内の **running** 状態のテストが完了するまで実行されません。

## <a name="vm-images"></a>VM イメージ

### <a name="handle-slow-network-connectivity"></a>低速なネットワーク接続の処理

ローカル データセンターの共有に PIR イメージをダウンロードできます。 その後、イメージを確認できます。

<!-- This is from the appendix to the Deploy local agent topic. -->

#### <a name="download-pir-image-to-local-share-in-case-of-slow-network-traffic"></a>ネットワーク トラフィックが低速な場合は、PIR イメージをローカル共有にダウンロードしてください

1. 次の場所から AzCopy をダウンロードします: [vaasexternaldependencies(AzCopy)](https://vaasexternaldependencies.blob.core.windows.net/prereqcomponents/AzCopy.zip)

2. AzCopy.zip を抽出し、`AzCopy.exe` が含まれているディレクトリに変更します。

3. 管理者特権のプロンプトから Windows PowerShell を開きます。 次のコマンドを実行します。

```powershell  
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterFullBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterCoreBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'WindowsServer2012R2DatacenterBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1404LTS.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1604-20170619.1.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'OpenLogic-CentOS-69-20180105.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Debian8_latest.vhd' /NC:12 /V:azcopylog.log /Y
```

> [!Note]  
> LocalFileShare は共有パスまたはローカル パスです。

#### <a name="verifying-pir-image-file-hash-value"></a>PIR イメージ ファイルのハッシュ値の検証

**Get-HashFile** コマンドレットを使用して、ダウンロードしたパブリック イメージ リポジトリのイメージ ファイルのハッシュ値を取得し、イメージの整合性を確認することができます。

| ファイル名 | SHA256 |
|---------------------------------------|------------------------------------------------------------------|
| Server2016DatacenterFullBYOL.vhd | 6ED58DCA666D530811A1EA563BA509BF9C29182B902D18FCA03C7E0868F733E9 |
| WindowsServer2012R2DatacenterBYOL.vhd | 9792CBF742870B1730B9B16EA814C683A8415EFD7601DDB6D5A76D0964767028 |
| Server2016DatacenterCoreBYOL.vhd | 5E80E1A6721A48A10655E6154C1B90E320DF5558487D6A0D7BFC7DCD32C4D9A5 |
| Ubuntu1404LTS.vhd | B24CDD12352AAEBC612A4558AB9E80F031A2190E46DCB459AF736072742E20E0 |
| Ubuntu1604-20170619.1.vhd | C481B88B60A01CBD5119A3F56632A2203EE5795678D3F3B9B764FFCA885E26CB |
| OpenLogic-CentOS-69-20180105.vhd | C8B874FE042E33B488110D9311AF1A5C7DC3B08E6796610BF18FDD6728C7913C |
| Debian8_latest.vhd | 06F8C11531E195D0C90FC01DFF5DC396BB1DD73A54F8252291ED366CACD996C1 |

### <a name="failure-happens-when-uploading-vm-image-in-the-vaasprereq-script"></a>`VaaSPreReq` スクリプトで VM イメージをアップロードしたときにエラーが発生する

まず、環境が正常であることを確認します。

1. DVM/ジャンプ ボックスから、管理者の資格情報を使用して管理者ポータルに正常にサインインできることを確認します。
1. アラートや警告が表示されていないことを確認します。

環境が正常である場合は、VaaS テストの実行に必要な 5 つの VM イメージを手動でアップロードします。

1. 管理者ポータルにサービス管理者としてサインインします。 管理者ポータルの URL は、ECE ストアまたはスタンプ情報ファイルで確認できます。 手順については、「[環境パラメーター](azure-stack-vaas-parameters.md#environment-parameters)」をご覧ください。
1. **[その他のサービス]**  >  **[リソース プロバイダー]**  >  **[コンピューティング]**  >  **[VM イメージ]** を選択します。
1. **[VM イメージ]** ブレードの上部にある **[+ 追加]** を選択します。
1. 最初の VM イメージの次のフィールドの値を変更または確認します。
    > [!IMPORTANT]
    > マーケットプレースの既存の項目について、すべての既定値が適切であるとは限りません。

    | フィールド  | 値  |
    |---------|---------|
    | Publisher | MicrosoftWindowsServer |
    | プラン | WindowsServer |
    | OS の種類 | Windows |
    | SKU | 2012-R2-Datacenter |
    | Version | 1.0.0 |
    | OS ディスク BLOB URI | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/WindowsServer2012R2DatacenterBYOL.vhd |

1. **[作成]** ボタンを選択します。
1. 残りの VM イメージに対してこの手順を繰り返します。

全 5 つの VM イメージのプロパティは次のとおりです。

| Publisher  | プラン  | OS の種類 | SKU | Version | OS ディスク BLOB URI |
|---------|---------|---------|---------|---------|---------|
| MicrosoftWindowsServer| WindowsServer | Windows | 2012-R2-Datacenter | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/WindowsServer2012R2DatacenterBYOL.vhd |
| MicrosoftWindowsServer | WindowsServer | Windows | 2016-Datacenter | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Server2016DatacenterFullBYOL.vhd |
| MicrosoftWindowsServer | WindowsServer | Windows | 2016-Datacenter-Server-Core | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Server2016DatacenterCoreBYOL.vhd |
| Canonical | UbuntuServer | Linux | 14.04.3-LTS | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Ubuntu1404LTS.vhd |
| Canonical | UbuntuServer | Linux | 16.04 LTS | 16.04.20170811 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Ubuntu1604-20170619.1.vhd |
| OpenLogic | CentOS | Linux | 6.9 | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/OpenLogic-CentOS-69-20180105.vhd |
| credativ | Debian | Linux | 8 | 1.0.0 | https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container/Debian8_latest.vhd |

## <a name="next-steps"></a>次のステップ

- 「[サービスとしての検証のリリース ノート](azure-stack-vaas-release-notes.md)」で、最新のリリースでの変更点を確認します。