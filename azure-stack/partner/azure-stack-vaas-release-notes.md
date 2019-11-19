---
title: Azure Stack のサービスとしての検証のリリース ノート | Microsoft Docs
description: Azure Stack のサービスとしての検証のリリース ノート。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 10/28/2019
ms.openlocfilehash: aa85310314a09db47f10424e84fe40e355bacb25
ms.sourcegitcommit: ed44d477b9fd11573d1e0d1ed3a3c0ef4512df53
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2019
ms.locfileid: "73846238"
---
# <a name="release-notes-for-validation-as-a-service"></a>サービスとしての検証のリリース ノート

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

この記事には、Azure Stack のサービスとしての検証のリリース ノートが含まれています。

## <a name="version-4353"></a>バージョン 4.3.5.3

2019 月 11 月 7 日

- テスト コンテンツの更新
  - Monthly Azure Stack Update Verification (月次 Azure Stack 更新プログラムの検証) (バージョン 5.1.46.0 -> 5.1.49.0)
  - OEM Extension Package Verification (OEM 拡張機能パッケージの検証) (バージョン 5.1.46.0 -> 5.1.49.0)
  - 5\.1.46.0 の結果は保持されています。 5\.1.46.0 で正常に実行された場合は、結果を送信するときに vaashelp@microsoft.com に通知します。

- バグの修正
  - 更新プログラムの .zip に特殊文字が含まれていた場合に月次 Azure Stack 更新プログラムの検証が実行に失敗する問題を修正しました。

- 既知の問題
  - mstest.exe が検出されない場合、VaaS テストが失敗します。 対処法:
    1. PowerShell ウィンドウでエージェントの CTRL + C を実行します。
    1. 「mstest.exe」と入力して、mstest.exe が認識されるプログラムであることを確認します。
    1. mstest.exe が認識されない場合は、現在の PowerShell ウィンドウを閉じます。
    1. [スタート] (タスク バーの PowerShell ではない) をクリックし、PowerShell を見つけ、管理者として開きます。
    1. 「mstest.exe」と入力し、それがコマンドとして使用可能なことを確認します。
    1. エージェントを再起動し、テストを再実行します。
  - 場合によって、クラウド シミュレーション エンジンが \*vm テストでエラーを報告することがあります。 再実行を試みる前に vaashelp@microsoft.com にお問い合わせください。 


2019 年 10 月 29 日

- 月次 Azure Stack 更新プログラム ワークフローと OEM パッケージの検証ワークフローのオンライン ドキュメントが更新されました。

    更新されたドキュメント「OEM パッケージの検証」と「Microsoft のソフトウェア更新プログラムの検証」を確認してください
- VaaS ワークフローの更新:月次 Azure Stack 更新プログラム (バージョン 5.1.30.0 -> 5.1.46.0) - 月次 Azure Stack 更新プログラムの検証テスト フローが更新されました。

    ワークフローは手動での介入を必要とせず、シームレスに実行されるようにスケジュールできるようになりました。
- VaaS ワークフローの更新:OEM パッケージの検証  (バージョン 5.1.30.0 -> 5.1.46.0)  –  OEM パッケージの検証ワークフローが更新されました。

    ワークフローは手動での介入を必要とせず、シームレスに実行されるようにスケジュールできるようになりました。
- OEM パッケージの検証ワークフロー (バージョン 5.1.30.0 -> 5.1.46.0) のクラウド シミュレーション エンジンが更新され、検証時間が短縮されました。実行時間が 1 時間に短縮されました。
- OEM パッケージの検証ワークフローと Azure Stack 更新プログラム ワークフロー (バージョン 5.1.30.0 -> 5.1.46.0) のクラウド シミュレーション エンジンでは、検証対象の更新プログラムは、子フォルダーに他の更新プログラムが含まれていない 2 つの別々の親フォルダーに入っている必要があります。
- OEM パッケージの検証ワークフローと Azure Stack 更新プログラム ワークフロー (バージョン 5.1.30.0 -> 5.1.46.0) のクラウド シミュレーション エンジンでは、テストは、月次の Azure Stack 更新プログラムの検証テスト、OEM 拡張機能パッケージの検証テスト、そして最後にクラウド シミュレーション エンジンの順序でスケジュールする必要があります。
- VaaS エージェントの更新:更新された VaaS エージェントは、Azure Stack のクラウド管理者の資格情報を使ってスタンプのクエリを実行し、ワークフローに自動入力するためのスタンプ情報を取得するようになりました。 

    この更新では、すべてのエージェントを更新して再起動する必要があります。 VaaS エージェントの更新方法に関する次の説明を参照してください: https://docs.microsoft.com/en-us/azure-stack/partner/azure-stack-vaas-local-agent
- VaaS ポータル UI の更新:テストを容易にするために、エージェント選択テーブルはテスト スケジュール ペインの上に移動されました。

    VaaS エージェントが正しく更新されていれば、ジョブをスケジュールするときに、スタンプ情報を入力する必要はなくなりました。


## <a name="version-405"></a>バージョン 4.0.5

2019 年 6 月 7 日

- パッケージの検証ワークフローのクラウド シミュレーション エンジンが更新され、検証時間が短縮されました。  
    実行時間: 6 時間に短縮  
    バージョン:5.1.13.0 -> 5.1.22.0  


2019 年 1 月 17 日

- Disk Identification Test (ディスク識別テスト) がストレージ プールの不整合を解決するために更新されました。 バージョン:5.1.14.0 -> 5.1.15.0
- Azure Stack 月間更新検証が、承認済みソフトウェアおよびコンテンツの検証の不整合を解決するために更新されました。 バージョン:5.1.14.0 -> 5.1.17.0
- OEM Extension Package Verification (OEM 拡張機能パッケージの検証) が、Azure Stack の更新手順の前に必要なチェックを実行するよう更新されました。 バージョン:5.1.14.0 -> 5.1.16.0
- 内部のバグ修正

## <a name="version-402"></a>バージョン 4.0.2

2019 年 1 月 7 日

Azure Stack 月間更新検証ワークフローを実行しているとき、ご利用の OEM 更新プログラム パッケージのバージョンが 1810 以降でない場合、OEM 更新手順に進んだときにエラーが表示されます。 これはバグです。 修正プログラムが開発されています。 軽減手順は次のとおりです。

1. OEM の更新プログラムを通常どおり実行します。
2. パッケージが正常に適用されたら Test-AzureStack を実行し、出力を保存します。
3. テストを取り消します。
4. 保存した出力を VaaSHelp@microsoft.com に送信し、実行の合格結果を受け取ります。

## <a name="version-402"></a>バージョン 4.0.2

2018 年 11 月 30 日

- 内部のバグ修正

## <a name="version-401"></a>バージョン 4.0.1

2018 年 10 月 8 日

- VaaS の前提条件

    `Install-VaaSPrerequisites` に、クラウド管理者の資格情報は不要になりました。 このコマンドレットの最新バージョンを実行する場合、前提条件をインストールするための変更後のコマンドについては、「[ローカル エージェントのダウンロードとインストール](azure-stack-vaas-local-agent.md#download-and-install-the-local-agent)」を参照してください。 コマンドは次のとおりです。

    ```powershell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region
    ```

## <a name="version-400"></a>バージョン 4.0.0

2018 年 8 月 29 日

- VaaS の前提条件と VHD の更新

    パッケージ検証時の問題に対処するために、`Install-VaaSPrerequisites` に、クラウド管理者の資格情報が必要になりました。 「[ローカル エージェントのダウンロードとインストール](azure-stack-vaas-local-agent.md#download-and-install-the-local-agent)」のドキュメントが次のように更新されました。

    ```powershell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    $CloudAdminCreds = New-Object System.Management.Automation.PSCredential "<cloudAdminDomain\username>", (ConvertTo-SecureString "<cloudAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region `
                              -CloudAdminCredentials $CloudAdminCreds
    ```
    > [!NOTE]
    > スクリプトで必要とされる `$CloudAdminCreds` は、検証対象の Azure Stack インスタンス用です。 VaaS テナントが使用する Azure Active Directory の資格情報ではありません。

- ローカル エージェントの更新

    以前のバージョンのローカル エージェントは、現在の 4.0.0 リリースのサービスと互換性がありません。 すべてのユーザーが、ローカル エージェントを更新する必要があります。 最新のエージェントをインストールする手順については、「[ローカル エージェントのダウンロードとインストール](azure-stack-vaas-local-agent.md#download-and-install-the-local-agent)」を参照してください。

- PowerShell オートメーションの更新

    `LaunchVaaSTests` PowerShell スクリプトに、最新バージョンのスクリプト パッケージを必要とする変更が加えられました。 最新バージョンのスクリプト パッケージをインストールする手順については、「[テスト パス ワークフローの起動](azure-stack-vaas-automate-with-powershell.md#launch-the-test-pass-workflow)」をご覧ください。

- サービスとしての検証ポータル

  - パッケージの署名の通知

    パッケージ検証ワークフローの一環として OEM カスタマイズ パッケージが提出されると、パッケージ形式が検証され、公開されている仕様に従っているかどうかが確認されます。 パッケージが準拠していない場合、実行は失敗します。 テナントの登録済みの Azure Active Directory 連絡先の電子メール アドレスに電子メール通知が送信されます。

  - 対話型テスト カテゴリ

    **対話型**テスト カテゴリが追加されました。 これらのテストにより、自動化されていない対話型の Azure Stack シナリオが実行されます。

  - 対話型の機能検証

    テスト成功ワークフローで、特定の機能に焦点を合わせたフィードバックを提供する機能を使用できるようになりました。 `OEM Update on Azure Stack 1806 RC Validation 5.1.4.0` テストでは、特定の更新プログラムが正しく適用されたかどうかを確認し、フィードバックを収集します。

## <a name="next-steps"></a>次の手順

- [サービスとしての検証のトラブルシューティング](azure-stack-vaas-troubleshoot.md)
