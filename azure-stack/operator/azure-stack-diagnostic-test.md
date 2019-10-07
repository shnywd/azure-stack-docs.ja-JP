---
title: Azure Stack 検証ツールを使用してシステム状態を検証する |Microsoft Docs
description: Azure Stack 検証ツールを使用してシステム状態を検証する方法について説明します。
services: azure-stack
author: justinha
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 06/26/2019
ms.author: justinha
ms.reviewer: adshar
ms.lastreviewed: 12/03/2018
ms.openlocfilehash: 194af241480cce42273ff81d91213a63b1b9fd59
ms.sourcegitcommit: 28c8567f85ea3123122f4a27d1c95e3f5cbd2c25
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829166"
---
# <a name="validate-azure-stack-system-state"></a>Azure Stack システムの状態を検証する

*適用対象: Azure Stack 統合システムと Azure Stack Development Kit*

Azure Stack オペレーターは、システムの正常性と状態をオンデマンドで判断できることが不可欠です。 Azure Stack の検証ツール (**Test-azurestack**) は PowerShell コマンドレットであり、システム上で一連のテストを実行して、障害があればそれを特定できます。 通常このツールは、問題について Microsoft カスタマー サービス サポート (CSS) に問い合わせるときに、[特権エンドポイント (PEP)](azure-stack-privileged-endpoint.md) から実行するよう求められます。 手元にシステム全体の正常性および状態情報があれば、CSS は詳細ログを収集して分析し、エラーが発生した領域に焦点を当て、お客様と連携して問題を修正できます。

## <a name="running-the-validation-tool-and-accessing-results"></a>検証ツールを実行して結果にアクセスする

前述のとおり、この検証ツールは PEP 経由で実行されます。 各テストは、PowerShell ウィンドウで**合格/不合格**のいずれかの状態を返します。 次に示すのは、エンド ツー エンド検証テスト プロセスの概要です。

1. PEP にアクセスします。 PEP セッションを確立するために、次のコマンドを実行します。

   ```powershell
   Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
   ```

   > [!TIP]
   > Azure Stack Development Kit (ASDK) ホスト コンピューターで PEP にアクセスするには、-ComputerName に対して AzS-ERCS01 を使用します。

2. PEP にアクセスしたら、次を実行します。

   ```powershell
   Test-AzureStack
   ```

   詳細については、「[パラメーターに関する考慮事項](azure-stack-diagnostic-test.md#parameter-considerations)」と「[ユース ケースの例](azure-stack-diagnostic-test.md#use-case-examples)」を参照してください。

3. いずれかのテストで **FAIL** が報告された場合は、`Get-AzureStackLog` を実行します。 統合システムでの手順については、「[Azure Stack 統合システムで Get-AzureStackLog を実行するには](azure-stack-configure-on-demand-diagnostic-log-collection.md#to-run-get-azurestacklog-on-azure-stack-integrated-systems)」を参照し、ASDK での手順については、「[ASDK システムで Get-AzureStackLog を実行する](azure-stack-configure-on-demand-diagnostic-log-collection.md#run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system)」をご覧ください。

   このコマンドレットは、Test-azurestack によって生成されたログを収集します。 テストで **WARN** が報告される場合は、ログを収集せずに CSS に連絡することをお勧めします。

4. CSS により検証ツールを実行するように指示された場合、CSS の担当者から、問題のトラブルシューティングを続行するために、お客様が収集したログの提出が求められます。

## <a name="tests-available"></a>利用可能なテスト

検証ツールを使用すると、一連のシステム レベルのテストと基本的なクラウド シナリオを実行して、現在の状態を把握し、システムの問題を修正できます。

### <a name="cloud-infrastructure-tests"></a>クラウド インフラストラクチャのテスト

これらの影響度が低いテストは、インフラストラクチャ レベルで動作し、さまざまなシステム コンポーネントや機能に関する情報を提供します。 現時点では、テストは、次のカテゴリにグループ化されます。

| テスト カテゴリ                                        | -Include および -Ignore の引数 |
| :--------------------------------------------------- | :-------------------------------- |
| Azure Stack の ACS の概要                              | AzsAcsSummary                     |
| Azure Stack Active Directory の概要                 | AzsAdSummary                      |
| Azure Stack のアラートの概要                            | AzsAlertSummary                   |
| Azure Stack アプリケーションのクラッシュの概要                | AzsApplicationCrashSummary        |
| Azure Stack バックアップ共有のアクセシビリティの概要       | AzsBackupShareAccessibility       |
| Azure Stack の BMC の概要                              | AzsStampBMCSummary                |
| Azure Stack クラウド ホスティング インフラストラクチャの概要     | AzsHostingInfraSummary            |
| Azure Stack クラウド ホスティング インフラストラクチャの利用 | AzsHostingInfraUtilization        |
| Azure Stack のコントロール プレーンの概要                    | AzsControlPlane                   |
| Azure Stack Defender の概要                         | AzsDefenderSummary                |
| Azure Stack のホスティング インフラストラクチャ ファームウェアの概要  | AzsHostingInfraFWSummary          |
| Azure Stack インフラストラクチャの容量                  | AzsInfraCapacity                  |
| Azure Stack インフラストラクチャのパフォーマンス               | AzsInfraPerformance               |
| Azure Stack インフラストラクチャ ロールの概要              | AzsInfraRoleSummary               |
| Azure Stack ネットワーク インフラストラクチャ                            | AzsNetworkInfra                   |
| Azure Stack ポータルおよび API の概要                   | AzsPortalAPISummary               |
| Azure Stack スケール ユニットの VM イベント                     | AzsScaleUnitEvents                |
| Azure Stack スケール ユニットの VM リソース                  | AzsScaleUnitResources             |
| Azure Stack シナリオ                                | AzsScenarios                      |
| Azure Stack SDN 検証の概要                   | AzsSDNValidation                  |
| Azure Stack サービス ファブリック ロールの概要              | AzsSFRoleSummary                  |
| Azure Stack Storage のデータ プレーン                       | AzsStorageDataPlane               |
| Azure Stack ストレージ サービスの概要                 | AzsStorageSvcsSummary             |
| Azure Stack SQL ストアの概要                        | AzsStoreSummary                   |
| Azure Stack 更新プログラムの概要                           | AzsInfraUpdateSummary             |
| Azure Stack の VM 配置の概要                     | AzsVmPlacement                    |

### <a name="cloud-scenario-tests"></a>クラウド シナリオのテスト

上記のインフラストラクチャ テストに加えて、クラウド シナリのオテストを実行して、インフラストラクチャ コンポーネント全体の機能を確認することもできます。 リソースのデプロイが関係するため、これらのテストを実行するは、クラウド管理者の資格情報が必要です。

> [!NOTE]
> 現時点では、Active Directory フェデレーション サービス (AD FS) の資格情報を使用してクラウド シナリオのテストを実行することはできません。

検証ツールでは、次のクラウドのシナリオがテストされます。
- リソース グループの作成
- プランの作成
- オファーの作成
- ストレージ アカウントの作成
- 仮想マシン (VM) の作成
- Blob ストレージの操作
- キュー ストレージの操作
- テーブル ストレージの操作

## <a name="parameter-considerations"></a>パラメーターに関する考慮事項

- パラメーター **List** は、利用可能なすべてのテスト カテゴリを表示するために使用できます。

- パラメーター **Include** と **Ignore** は、テスト カテゴリを含めたり除外したりするために使用できます。 これらの引数の詳細については、次のセクションを参照してください。

  ```powershell
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
  ```

  ```powershell
  Test-AzureStack -Ignore AzsInfraPerformance
  ```

- テナント VM はクラウド シナリオ テストの一部としてデプロイされます。 この VM のデプロイを無効にするには、**DoNotDeployTenantVm** を使用します。

- クラウド シナリオ テストを実行するには、**ServiceAdminCredential** パラメーターを指定する必要があります。これについては「[ユース ケースの例](azure-stack-diagnostic-test.md#use-case-examples)」のセクションで説明しています。

- **BackupSharePath** と **BackupShareCredential** は、インフラストラクチャ バックアップ設定をテストするときに使用されます。これについては「[ユース ケースの例](azure-stack-diagnostic-test.md#use-case-examples)」セクションで説明しています。

- **DetailedResults** を使用すると、実行全体だけでなく、各テストの合格/不合格/警告の情報を取得できます。 指定しないと、失敗がない場合は **$true** が、失敗がある場合は **$false** が **Test-AzureStack** から返されます。
- **TimeoutSeconds** を使用すると、各グループでの完了に対する特定の時刻を設定できます。

- 検証ツールは、Verbose、Debug、ErrorAction、ErrorVariable、WarningAction、WarningVariable、OutBuffer、PipelineVariable、および OutVariable などの一般的な PowerShell パラメーターもサポートします。 詳細については、「[About Common Parameters (一般的なパラメーターについて)](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。  

## <a name="use-case-examples"></a>ユース ケースの例

### <a name="run-validation-without-cloud-scenarios"></a>クラウド シナリオがない検証の実行

クラウド シナリオ テストの実行をスキップするには、**ServiceAdminCredential** パラメーターを指定せずに検証ツールを実行します。 

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred
Test-AzureStack
```

### <a name="run-validation-with-cloud-scenarios"></a>クラウド シナリオがある検証の実行

検証ツールに **ServiceAdminCredentials** パラメーターを指定すると、既定でクラウド シナリオ テストが実行されます。 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" 
```

残りのテストを実行せずにクラウド シナリオのみを実行したい場合は、**Include** パラメーターを使用してそのようにすることができます。 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" -Include AzsScenarios   
```

クラウド管理者のユーザー名は UPN 形式serviceadmin@contoso.onmicrosoft.com (Azure AD) で入力する必要があります。 入力を求められたら、クラウド管理者のアカウントのパスワードを入力します。

### <a name="groups"></a>グループ

オペレーターの操作性を向上させるため、複数のテスト カテゴリを同時に実行できるように **Group** パラメーターが有効になっています。 現時点では、次の 3 つのグループが定義されています。**既定**、**UpdateReadiness**、および **SecretRotationReadiness**。

- **既定**:**Test-AzureStack** の標準実行と見なされます。 他のグループが選択されていない場合、このグループが既定で実行されます。
- **UpdateReadiness**:Azure Stack インスタンスが更新可能かどうかを確認するためのチェック。 **UpdateReadiness** グループが実行されると、コンソール出力にエラーとして警告が表示されます。これは更新の妨げと見なします。 次のカテゴリは、**UpdateReadiness** グループの一部です。

  - **AzsAcsSummary**
  - **AzsDefenderSummary**
  - **AzsHostingInfraSummary**
  - **AzsInfraCapacity**
  - **AzsInfraRoleSummary**
  - **AzsPortalAPISummary**
  - **AzsSFRoleSummary**
  - **AzsStoreSummary**

- **SecretRotationReadiness**:Azure Stack インスタンスが、シークレット ローテーションを実行できる状態にあるかどうかを確認するためのチェック。 **SecretRotationReadiness** グループが実行されると、コンソール出力にエラーとして警告が表示されます。これはシークレット ローテーションの妨げと見なします。 次のカテゴリは、SecretRotationReadiness グループの一部です。

  - **AzsAcsSummary**
  - **AzsDefenderSummary**
  - **AzsHostingInfraSummary**
  - **AzsInfraCapacity**
  - **AzsInfraRoleSummary**
  - **AzsPortalAPISummary**
  - **AzsSFRoleSummary**
  - **AzsStorageSvcsSummary**
  - **AzsStoreSummary**

#### <a name="group-parameter-example"></a>Group パラメーターの例

次の例では、**Group** を使用して更新プログラムまたは修正プログラムをインストールする前に、**Test-AzureStack** を実行してシステムの準備状況をテストします。 更新プログラムまたは修正プログラムのインストールを開始する前に、**Test-AzureStack** を実行して Azure Stack の状態を確認します。

```powershell
Test-AzureStack -Group UpdateReadiness
```

お使いの Azure Stack で 1811 より前のバージョンが実行されている場合、次の PowerShell コマンドを使用して **Test-AzureStack** を実行してください。

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
```

### <a name="run-validation-tool-to-test-infrastructure-backup-settings"></a>インフラストラクチャのバックアップ設定をテストするには、検証ツールを実行します。

インフラストラクチャのバックアップを構成する*前に*、**AzsBackupShareAccessibility** テストを使用して、バックアップ共有パスと資格情報をテストできます。

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupSharePath "\\<fileserver>\<fileshare>" -BackupShareCredential $using:backupcred
  ```

バックアップを構成した*後*、PEP セッションの実行から **AzsBackupShareAccessibility** を実行して、ERCS から共有にアクセスできることを検証できます。

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility
  ```

構成済みのバックアップ共有で新しい資格情報をテストするには、以下を実行します。 

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupShareCredential "<PSCredential for backup share>"
  ```

### <a name="run-validation-tool-to-test-network-infrastructure"></a>検証ツールを実行してネットワーク インフラストラクチャをテストする

このテストでは、Azure Stack ソフトウェア定義ネットワーク (SDN) をバイパスすることによって、ネットワーク インフラストラクチャの接続が確認されます。 パブリック VIP から構成済みの DNS フォワーダー、NTP サーバー、および認証エンドポイントへの接続が示されます。 これには、ID プロバイダーとして Azure AD を使用する場合の Azure への接続、または ID プロバイダーとして AD FS を使用する場合のフェデレーション サーバーへの接続が含まれます。

コマンドの詳細な出力を取得するには、debug パラメーターを含めます。

```powershell
Test-AzureStack -Include AzsNetworkInfra -Debug
```

## <a name="next-steps"></a>次の手順

Azure Stack 診断ツールと問題のログ記録の詳細については、「[Azure Stack の診断ツール](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep-to-collect-diagnostic-logs)」を参照してください。

トラブルシューティングの詳細については、「[Microsoft Azure Stack のトラブルシューティング](azure-stack-troubleshooting.md)」を参照してください。
