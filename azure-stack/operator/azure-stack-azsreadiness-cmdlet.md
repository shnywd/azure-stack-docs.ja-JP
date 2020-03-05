---
title: Start-AzsReadinessChecker コマンドレット リファレンス
description: PowerShell コマンドレットは、Azure Stack Hub 適合性チェッカー モジュールを助けるものです。
author: IngridAtMicrosoft
ms.topic: conceptual
ms.date: 01/07/2020
ms.author: inhenkel
ms.reviewer: unknown
ms.lastreviewed: 05/09/2019
ms.openlocfilehash: 588808f93ae23af3aaed37686474490bc402c566
ms.sourcegitcommit: 4ac711ec37c6653c71b126d09c1f93ec4215a489
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77695480"
---
# <a name="start-azsreadinesschecker-cmdlet-reference"></a>Start-AzsReadinessChecker コマンドレット リファレンス

モジュール:**Microsoft.AzureStack.ReadinessChecker**

このモジュールには、1 つのコマンドレットのみが含まれます。 このコマンドレットは、Azure Stack Hub に対して、デプロイ前またはサービス提供前の機能を 1 つ以上実行します。

## <a name="syntax"></a>構文

```powershell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -DeploymentDataJSONPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       -Subject <OrderedDictionary>
       -RequestType <String>
       [-IncludePaaS]
       -OutputRequestPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -PfxPassword <SecureString>
       -PfxPath <String>
       -ExportPFXPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -AADDirectoryTenantName <String>
       -IdentitySystem <String>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```powershell
Start-AzsReadinessChecker
       -ReportPath <String>
       [-ReportSections <String>]
       [-Summary]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

## <a name="description"></a>説明

**Start-AzsReadinessChecker** コマンドレットでは、証明書、Azure アカウント、Azure サブスクリプション、Azure Active Directory (Azure AD) が検証されます。 Azure Stack Hub のデプロイ前、またはシークレット ローテーションなどの Azure Stack Hub サービス アクションの前に検証を実行します。 このコマンドレットを使用して、インフラストラクチャ証明書および必要に応じて PaaS 証明書に対する証明書署名要求を生成することもできます。 最後に、コマンドレットを使用して PFX 証明書を再パッケージ化し、パッケージの一般的な問題を修復することができます。

## <a name="examples"></a>例

### <a name="example-generate-certificate-signing-request"></a>例: 証明書署名要求を生成する

```powershell
$regionName = 'east'
$externalFQDN = 'azurestack.contoso.com'
$subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"}
Start-AzsReadinessChecker -regionName $regionName -externalFQDN $externalFQDN -subject $subjectHash -IdentitySystem ADFS -requestType MultipleCSR
```

この例では、`Start-AzsReadinessChecker` によって、リージョン名が **east**、外部 FQDN が **azurestack.contoso.com** である AD FS Azure Stack Hub デプロイに適した証明書に対する複数の証明書署名要求 (CSR) が生成されます。

### <a name="example-validate-certificates"></a>例: 証明書を検証する

```powershell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
```

この例では、セキュリティのために PFX パスワードが必要です。また、`Start-AzsReadinessChecker` によって、相対フォルダー **Certificates** に、リージョン名 **east**、外部 FQDN の **azurestack.contoso.com** の Azure AD デプロイに対して有効な証明書があるかどうかがチェックされます。

### <a name="example-validate-certificates-with-deployment-data-deployment-and-support"></a>例: デプロイ データで証明書を検証する (デプロイとサポート)

```powershell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -DeploymentDataJSONPath .\deploymentdata.json
```

このデプロイとサポートの例では、セキュリティのために PFX パスワードが必要です。また、`Start-AzsReadinessChecker` によって、相対フォルダー **Certificates** に、デプロイについて生成されたデプロイ データ JSON ファイルから ID、リージョン、外部 FQDN が読み取られたデプロイに対して有効な証明書があるかどうかが確認されます。

### <a name="example-validate-paas-certificates"></a>例: PaaS 証明書を検証する

```powershell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -RegionName east -FQDN azurestack.contoso.com
```

この例では、各 PaaS 証明書に対するパスとパスワードを使ってハッシュテーブルが作成されます。 証明書は省略できます。 `Start-AzsReadinessChecker` により各 PFX パスが存在することがチェックされ、リージョン **east**、外部 FQDN **azurestack.contoso.com** を使用して検証されます。

### <a name="example-validate-paas-certificates-with-deployment-data"></a>例: デプロイ データで PaaS 証明書を検証する

```powershell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -DeploymentDataJSONPath .\deploymentdata.json
```

この例では、各 PaaS 証明書に対するパスとパスワードを使ってハッシュテーブルが作成されます。 証明書は省略できます。 `Start-AzsReadinessChecker` により各 PFX パスが存在することがチェックされ、デプロイについて生成されたデプロイ データ JSON ファイルから読み取られたリージョンと外部 FQDN を使用して検証されます。

### <a name="example-validate-azure-identity"></a>例: Azure ID を検証する

```powershell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
# Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment "<environment name>" -AzureDirectoryTenantName azurestack.contoso.com
```

この例では、セキュリティのためにサービス管理者アカウントの資格情報が必要です。また、`Start-AzsReadinessChecker` によって、Azure アカウントと Azure AD が、**azurestack.contoso.com** というテナント ディレクトリ名を持つ Azure AD デプロイに対して有効であることが確認されます。

### <a name="example-validate-azure-identity-with-deployment-data-deployment-support"></a>例: デプロイ データで Azure ID を検証する (デプロイ サポート)

```PowerShell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -DeploymentDataJSONPath .\contoso-deploymentdata.json
```

この例では、セキュリティのためにサービス管理者アカウントの資格情報が必要です。また、`Start-AzsReadinessChecker` によって、Azure アカウントと Azure AD が、デプロイ用について生成されたデプロイ データ JSON ファイルから **AzureCloud** と **TenantName** が読み取られた Azure AD デプロイに対して有効であることが確認されます。

### <a name="example-validate-azure-registration"></a>例: Azure の登録を検証する

```powershell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "<subscription ID"
# Supported values for the <environment name> parameter are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -AzureEnvironment "<environment name>"
```

この例では、セキュリティのためにサブスクリプション所有者の資格情報が必要です。また、指定したアカウントとサブスクリプションを Azure Stack Hub の登録で確実に使用できるように、`Start-AzsReadinessChecker` によって、そのアカウントとサブスクリプションに対する検証が実行されます。

### <a name="example-validate-azure-registration-with-deployment-data-deployment-team"></a>例: デプロイ データで Azure 登録を検証する (デプロイ チーム)

```powershell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "<subscription ID>"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -DeploymentDataJSONPath .\contoso-deploymentdata.json
```

この例では、セキュリティのためにサブスクリプション所有者の資格情報が必要です。また、指定したアカウントとサブスクリプションを、デプロイについて生成されたデプロイ データ JSON ファイルから詳細情報が読み取られた Azure Stack Hub の登録で確実に使用できるように、`Start-AzsReadinessChecker` によって、そのアカウントとサブスクリプションに対する検証が実行されます。

### <a name="example-importexport-pfx-package"></a>例: PFX パッケージをインポート/エクスポートする

```powershell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
```

この例では、セキュリティのために PFX パスワードが必要です。 ssl.pfx ファイルはローカル コンピューター証明書ストアにインポートされ、同じパスワードで再度エクスポートされた後、Ssl_new.pfx として保存されます。 この手順は、秘密キーで**ローカル コンピューター**属性が設定されていない、証明書チェーンが壊れている、関係のない証明書が PFX にある、証明書チェーンの順序が間違っている、といったフラグが、証明書の検証によって設定されたときに使用されます。

### <a name="example-view-validation-report-deployment-and-support"></a>例: 検証レポートを表示する (デプロイとサポート)

```powershell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json
```

この例では、デプロイまたはサポート チームが対応状況レポートを顧客 (Contoso) から受け取り、`Start-AzsReadinessChecker` を使用して、Contoso が行った検証実行の状態を表示します。

### <a name="example-view-validation-report-summary-for-certificate-validation-only-deployment-and-support"></a>例: 証明書の検証についてのみ検証レポートの概要を表示する (デプロイとサポート)

```powershell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json -ReportSections Certificate -Summary
```

この例では、デプロイまたはサポート チームが対応状況レポートを顧客 (Contoso) から受け取り、`Start-AzsReadinessChecker` を使用して、Contoso が行った証明書の検証実行のまとめられた状態を表示します。

## <a name="required-parameters"></a>必須のパラメーター

### <a name="-regionname"></a>-RegionName

Azure Stack Hub デプロイのリージョン名を指定します。

|  |  |
|----------------------------|--------------|
|型:                       |String        |
|位置:                   |named         |
|既定値:              |なし          |
|パイプライン入力を許可する:      |False         |
|ワイルドカード文字を許可する: |False         |

### <a name="-fqdn"></a>-FQDN

Azure Stack Hub デプロイの外部 FQDN (別名: **ExternalFQDN** および **ExternalDomainName**) を指定します。

|  |  |
|----------------------------|--------------|
|型:                       |String        |
|位置:                   |named         |
|既定値:              |ExternalFQDN、ExternalDomainName |
|パイプライン入力を許可する:      |False         |
|ワイルドカード文字を許可する: |False         |

### <a name="-identitysystem"></a>-IdentitySystem

Azure Stack Hub デプロイの ID システムの有効な値として、AAD (Azure Active Directory) または ADFS (Active Directory フェデレーション サービス) を指定します。

|  |  |
|----------------------------|--------------|
|型:                       |String        |
|位置:                   |named         |
|既定値:              |なし          |
|有効な値:               |"AAD"、"ADFS"  |
|パイプライン入力を許可する:      |False         |
|ワイルドカード文字を許可する: |False         |

### <a name="-pfxpassword"></a>-PfxPassword

PFX 証明書ファイルに関連付けられているパスワードを指定します。

|  |  |
|----------------------------|---------|
|型:                       |SecureString |
|位置:                   |named    |
|既定値:              |なし     |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-paascertificates"></a>-PaaSCertificates

PaaS 証明書に対するパスとパスワードを含むハッシュテーブルを指定します。

|  |  |
|----------------------------|---------|
|型:                       |Hashtable |
|位置:                   |named    |
|既定値:              |なし     |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-deploymentdatajsonpath"></a>-DeploymentDataJSONPath

Azure Stack Hub デプロイのデータ JSON 構成ファイルを指定します。 このファイルはデプロイについて生成されます。

|  |  |
|----------------------------|---------|
|型:                       |String   |
|位置:                   |named    |
|既定値:              |なし     |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-pfxpath"></a>-PfxPath

このツールの証明書の検証によって示されるように、修正にインポート/エクスポート ルーチンを必要とする、問題のある証明書へのパスを指定します。

|  |  |
|----------------------------|---------|
|型:                       |String   |
|位置:                   |named    |
|既定値:              |なし     |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-exportpfxpath"></a>-ExportPFXPath  

インポート/エクスポート ルーチンから結果 PFX ファイルの宛先パスを指定します。  

|  |  |
|----------------------------|---------|
|型:                       |String   |
|位置:                   |named    |
|既定値:              |なし     |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-subject"></a>-Subject

証明書の要求を生成する対象の順序指定されたディクショナリを指定します。

|  |  |
|----------------------------|---------|
|型:                       |OrderedDictionary   |
|位置:                   |named    |
|既定値:              |なし     |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-requesttype"></a>-RequestType

証明書要求の SAN の種類を指定します。 有効な値は **MultipleCSR**、**SingleCSR** です。

- **MultipleCSR** では、複数の証明書要求が生成されます (サービスごとに 1 つ)。
- **SingleCSR** では、すべてのサービスに対して 1 つの証明書要求が生成されます。

|  |  |
|----------------------------|---------|
|型:                       |String   |
|位置:                   |named    |
|既定値:              |なし     |
|有効な値:               |"MultipleCSR"、"SingleCSR" |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-outputrequestpath"></a>-OutputRequestPath

証明書の要求ファイルの宛先パスを指定します。 既に存在するディレクトリを指定する必要があります。

|  |  |
|----------------------------|---------|
|型:                       |String   |
|位置:                   |named    |
|既定値:              |なし     |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-aadserviceadministrator"></a>-AADServiceAdministrator

Azure Stack Hub のデプロイに使用する Azure AD サービス管理者を指定します。

|  |  |
|----------------------------|---------|
|型:                       |PSCredential   |
|位置:                   |named    |
|既定値:              |なし     |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-aaddirectorytenantname"></a>-AADDirectoryTenantName

Azure Stack Hub のデプロイに使用する Azure AD 名を指定します。

|  |  |
|----------------------------|---------|
|型:                       |String   |
|位置:                   |named    |
|既定値:              |なし     |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-azureenvironment"></a>-AzureEnvironment

Azure Stack Hub のデプロイと登録に使用するアカウント、ディレクトリ、およびサブスクリプションを含む Azure サービスのインスタンスを指定します。

|  |  |
|----------------------------|---------|
|型:                       |String   |
|位置:                   |named    |
|既定値:              |なし     |
|有効な値:               |"AzureCloud"、"AzureChinaCloud"、"AzureUSGovernment" |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-registrationaccount"></a>-RegistrationAccount

Azure Stack Hub の登録に使用する登録アカウントを指定します。

|  |  |
|----------------------------|---------|
|型:                       |String   |
|位置:                   |named    |
|既定値:              |なし     |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-registrationsubscriptionid"></a>-RegistrationSubscriptionID

Azure Stack Hub の登録に使用する登録サブスクリプション ID を指定します。

|  |  |
|----------------------------|---------|
|型:                       |Guid     |
|位置:                   |named    |
|既定値:              |なし     |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-reportpath"></a>-ReportPath

対応状況レポートのパスを指定します。既定値は現在のディレクトリと既定のレポート名です。

|  |  |
|----------------------------|---------|
|型:                       |String   |
|位置:                   |named    |
|既定値:              |All      |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

## <a name="optional-parameters"></a>省略可能なパラメーター

### <a name="-certificatepath"></a>-CertificatePath

証明書の必須証明書フォルダーのみが存在するパスを指定します。

Azure AD ID システムを使用した Azure Stack Hub デプロイの必須フォルダーは次のとおりです。

- ACSBlob、ACSQueue、ACSTable、Admin Portal、ARM Admin、ARM Public、KeyVault、KeyVaultInternal、Public Portal

Active Directory フェデレーション サービス (AD FS) ID システムを使用した Azure Stack Hub デプロイの必須フォルダーは次のとおりです。

- ACSBlob、ACSQueue、ACSTable、ADFS、Admin Portal、ARM Admin、ARM Public、Graph、KeyVault、KeyVaultInternal、Public Portal

|  |  |
|----------------------------|---------|
|型:                       |String   |
|位置:                   |named    |
|既定値:              |.\Certificates |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-includepaas"></a>-IncludePaaS  

PaaS サービス/ホスト名を証明書の要求に追加するかどうかを指定します。

|  |  |
|----------------------------|------------------|
|型:                       |SwitchParameter   |
|位置:                   |named             |
|既定値:              |False             |
|パイプライン入力を許可する:      |False             |
|ワイルドカード文字を許可する: |False             |

### <a name="-reportsections"></a>-ReportSections

レポートの概要のみを表示し、詳細を省略するかどうかを指定します。

|  |  |
|----------------------------|---------|
|型:                       |String   |
|位置:                   |named    |
|既定値:              |All      |
|有効な値:               |"Certificate"、"AzureRegistration"、"AzureIdentity"、"Jobs"、"All" |
|パイプライン入力を許可する:      |False    |
|ワイルドカード文字を許可する: |False    |

### <a name="-summary"></a>-Summary

レポートの概要のみを表示し、詳細を省略するかどうかを指定します。

|  |  |
|----------------------------|------------------|
|型:                       |SwitchParameter   |
|位置:                   |named             |
|既定値:              |False             |
|パイプライン入力を許可する:      |False             |
|ワイルドカード文字を許可する: |False             |

### <a name="-cleanreport"></a>-CleanReport

前の実行と検証の履歴を削除し、検証を新しいレポートに書き込みます。

|  |  |
|----------------------------|------------------|
|型:                       |SwitchParameter   |
|別名:                    |cf                |
|位置:                   |named             |
|既定値:              |False             |
|パイプライン入力を許可する:      |False             |
|ワイルドカード文字を許可する: |False             |

### <a name="-outputpath"></a>-OutputPath

対応状況 JSON レポートと詳細なログ ファイルを保存するカスタム パスを指定します。 パスがまだ存在しない場合、コマンドによってディレクトリの作成が試行されます。

|  |  |
|----------------------------|------------------|
|型:                       |String            |
|位置:                   |named             |
|既定値:              |$ENV:TEMP\AzsReadinessChecker  |
|パイプライン入力を許可する:      |False             |
|ワイルドカード文字を許可する: |False             |

### <a name="-confirm"></a>-Confirm

コマンドレットの実行前に確認メッセージを表示します。

|  |  |
|----------------------------|------------------|
|型:                       |SwitchParameter   |
|別名:                    |cf                |
|位置:                   |named             |
|既定値:              |False             |
|パイプライン入力を許可する:      |False             |
|ワイルドカード文字を許可する: |False             |

### <a name="-whatif"></a>-WhatIf

コマンドレットの実行時に発生する内容を示します。 コマンドレットは実行されません。

|  |  |
|----------------------------|------------------|
|型:                       |SwitchParameter   |
|別名:                    |wi                |
|位置:                   |named             |
|既定値:              |False             |
|パイプライン入力を許可する:      |False             |
|ワイルドカード文字を許可する: |False             |
