---
title: シークレットの切り替え
titleSuffix: Azure Stack Hub
description: Azure Stack Hub でシークレットをローテーションする方法について説明します。
author: IngridAtMicrosoft
ms.topic: article
ms.date: 12/13/2019
ms.reviewer: ppacent
ms.author: inhenkel
ms.lastreviewed: 12/13/2019
monikerRange: '>=azs-1802'
ms.openlocfilehash: 9eaa0ecdf3b8d544e916f5f94e106bc76dbe0e55
ms.sourcegitcommit: 4ac711ec37c6653c71b126d09c1f93ec4215a489
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77697724"
---
# <a name="rotate-secrets-in-azure-stack-hub"></a>Azure Stack Hub でシークレットをローテーションする

"*この記事の説明は、Azure Stack Hub 統合システムのバージョン 1803 以降に対してのみ適用されます。1802 以前のバージョンの Azure Stack Hub でシークレットのローテーションを試みないでください。* "

シークレットは、Azure Stack Hub インフラストラクチャのリソースとサービス間のセキュリティ保護された通信を維持するのに役立ちます。

## <a name="rotate-secrets-overview"></a>シークレットのローテーションの概要

1. シークレットのローテーションで使用される証明書を準備します。
2. Azure Stack Hub の[公開キー インフラストラクチャ証明書の要件](https://docs.microsoft.com/azure-stack/operator/azure-stack-pki-certs)を確認します。
3. [特権エンドポイントを使用し](azure-stack-privileged-endpoint.md)、**Test-azurestack** を実行して、すべてに問題がないことを確認します。  
4. [シークレット ローテーションの事前手順](#pre-steps-for-secret-rotation)を再確認します。
5. [Azure Stack Hub PKI 証明書を検証](https://docs.microsoft.com/azure-stack/operator/azure-stack-validate-pki-certs)します。 パスワードに `*` や `)` などの特殊文字が含まれないことを確認します。
6. PFX 暗号化が **TripleDES-SHA1** であることを確認します。 問題が発生した場合は、「[Azure Stack Hub PKI 証明書に関する一般的な問題を修復する](https://docs.microsoft.com/azure-stack/operator/azure-stack-remediate-certs#pfx-encryption)」を参照してください。
7. フォルダー構造を準備します。  例については、「[外部シークレットのローテーション](https://docs.microsoft.com/azure-stack/operator/azure-stack-rotate-secrets#rotating-external-secrets)」セクションを参照してください。
8. [シークレットのローテーションを開始します](#use-powershell-to-rotate-secrets)。

## <a name="rotate-secrets"></a>シークレットの切り替え

Azure Stack Hub では、さまざまなシークレットを使って、Azure Stack Hub インフラストラクチャのリソースとサービス間のセキュリティ保護された通信が維持されます。

- **内部シークレット**

    Azure Stack Hub オペレーターの介入なしに Azure Stack Hub インフラストラクチャによって使用されるすべての証明書、パスワード、セキュリティで保護された文字列、キー。

- **外部シークレット**

    Azure Stack Hub オペレーターによって提供される、外部向けサービス用のインフラストラクチャ サービス証明書。 外部シークレットには、次のサービスの証明書が含まれます。

    - 管理者ポータル
    - パブリック ポータル
    - 管理者の Azure Resource Manager
    - グローバルな Azure Resource Manager
    - 管理者の Key Vault
    - Key Vault
    - 管理者拡張機能ホスト
    - ACS (Blob、Table、Queue Storage を含みます)
    - ADFS*
    - Graph*
    
    \* 環境の ID プロバイダーが Active Directory フェデレーション サービス (AD FS) の場合にのみ適用されます。

> [!Note]
> BMC やスイッチ パスワードなどの他のすべてのセキュリティで保護されたキーと文字列、ユーザーアカウントと管理者アカウントのパスワードは、依然として管理者が手動で更新します。

> [!Important]
> Azure Stack Hub の 1811 リリース以降、シークレットのローテーションは、内部の証明書と外部の証明書に分けられます。

Azure Stack Hub インフラストラクチャの整合性を維持するため、オペレーターは、組織のセキュリティ要件と一致する頻度でインフラストラクチャのシークレットを定期的にローテーションできる必要があります。

### <a name="rotating-secrets-with-external-certificates-from-a-new-certificate-authority"></a>新しい証明書機関からの外部証明書を使用してシークレットをローテーションする

Azure Stack Hub では、次のようなコンテキストで、新しい証明書機関 (CA) からの外部証明書を使用したシークレットのローテーションがサポートされます。

|インストール済みの証明書 CA|ローテーション先の CA|サポートされています|サポートされる Azure Stack Hub のバージョン|
|-----|-----|-----|-----|
|自己署名済みから|Enterprise へ|サポートされています|1903 以降|
|自己署名済みから|自己署名済みへ|サポートされていません||
|自己署名済みから|公開へ<sup>*</sup>|サポートされています|1803 以降|
|Enterprise から|Enterprise へ|サポートされています。 1803 から 1903: 顧客がデプロイ時に使用したのと同じエンタープライズ CA を使用している限りサポートされます|1803 以降|
|Enterprise から|自己署名済みへ|サポートされていません||
|Enterprise から|公開へ<sup>*</sup>|サポートされています|1803 以降|
|公開から<sup>*</sup>|Enterprise へ|サポートされています|1903 以降|
|公開から<sup>*</sup>|自己署名済みへ|サポートされていません||
|公開から<sup>*</sup>|公開へ<sup>*</sup>|サポートされています|1803 以降|

<sup>*</sup>ここでの公開証明機関は、Windows の信頼されたルート プログラムに含まれているものです。 「[Microsoft Trusted Root Certificate Program: Participants (as of June 27, 2017) (Microsoft の信頼されたルート証明書プログラム: 参加者 (2017 年 6 月 27 日時点)](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca) に完全な一覧があります。

## <a name="fixing-alerts"></a>アラートの修正

シークレットの有効期限まで 30 日以内になると、管理者ポータルで次のアラートが生成されます。

- 保留中のサービス アカウントのパスワードの有効期限
- 保留中の内部証明書の有効期限
- 保留中の外部証明書の有効期限

以下の手順を使ってシークレットのローテーションを実行すると、これらのアラートは修正されます。

> [!Note]
> 1811 より前の Azure Stack Hub 環境では、保留中の内部証明書やシークレットの有効期限のアラートが表示される場合があります。 これらのアラートは正確ではないため、内部シークレットのローテーションを実行せずに無視する必要があります。 正しくない内部シークレットの有効期限切れアラートは、1811 で解決される既知の問題です。 環境が 2 年間アクティブになっていない限り、内部シークレットの有効期限が切れることはありません。

## <a name="pre-steps-for-secret-rotation"></a>シークレット ローテーションの事前手順

   > [!IMPORTANT]
   > シークレットのローテーションが Azure Stack Hub 環境で既に実行されている場合は、シークレットのローテーションを再度実行する前に、システムをバージョン 1811 以降に更新する必要があります。 シークレットのローテーションは、[特権エンドポイント](azure-stack-privileged-endpoint.md)を使用して実行する必要があり、Azure Stack Hub オペレーターの資格情報が必要です。 ご使用の環境の Azure Stack Hub オペレーターが、その環境内でシークレットのローテーションが実行されているかどうかを把握していない場合は、シークレットのローテーションを再度実行する前に、1811 に更新します。

1. Azure Stack Hub インスタンスをバージョン 1811 に更新することを強くお勧めします。

    > [!Note]
    > 1811 より前のバージョンの場合は、拡張機能ホストの証明書を追加するためにシークレットのローテーションを行う必要はありません。 拡張機能ホストの証明書を追加するには、「[Azure Stack Hub の拡張機能ホストを準備する](azure-stack-extension-host-prepare.md)」の手順に従ってください。

2. オペレーターが、Azure Stack Hub シークレットのローテーション中に、アラートが開いて自動的に閉じることがあります。  これは予期される動作であり、アラートは無視してもかまいません。  オペレーターは、**Test-AzureStack** を実行することで、このアラートの有効性を確認できます。  System Center Operations Manager を使用して Azure Stack Hub システムを監視しているオペレーターの場合、システムをメンテナンス モードにすると、これらのアラートは ITSM システムに到達しなくなりますが、Azure Stack Hub システムに到達できなくなっても引き続きアラートは生成されます。

3. メンテナンス操作をユーザーに通知します。 可能な限り非営業時間中に、通常のメンテナンス期間をスケジュールします。 メンテナンス操作は、ユーザーのワークロードとポータル操作の両方に影響を及ぼす可能性があります。

    > [!Note]
    > 次の手順は、Azure Stack Hub 外部シークレットのローテーションに対してのみ適用されます。

4. シークレットをローテーションする前に、 **[Test-AzureStack](azure-stack-diagnostic-test.md)** を実行して、すべてのテスト出力が正常であることを確認します。
5. 代わりの外部証明書の新しいセットを準備します。 新しいセットは、「[Azure Stack Hub 公開キー インフラストラクチャ証明書の要件](azure-stack-pki-certs.md)」で説明されている証明書の仕様と一致します。 新しい証明書を購入または作成するための証明書署名要求 (CSR) を作成するには、[PKI 証明書の生成](azure-stack-get-pki-certs.md)に関する記事で説明されている手順を使用します。ご使用の Azure Stack Hub 環境で使用できるようそれらを準備するには、[Azure Stack Hub の PKI 証明書の準備](azure-stack-prepare-pki-certs.md)に関する記事の手順を使用します。 準備する証明書は、[PKI 証明書の検証](azure-stack-validate-pki-certs.md)に関する記事の手順で必ず検証してください。
6. ローテーションに使われる証明書のバックアップを安全なバックアップ場所に格納します。 ローテーションを実行して失敗した場合は、ローテーションを再実行する前に、ファイル共有内の証明書をバックアップ コピーに置き換えます。 バックアップ コピーはセキュリティで保護されたバックアップ場所に保存してください。
7. ERCS VM からアクセスできるファイル共有を作成します。 ファイル共有は、**CloudAdmin** ID で読み書きできる必要があります。
8. ファイル共有にアクセスできるコンピューターから PowerShell ISE コンソールを開きます。 ファイル共有に移動します。
9. **[CertDirectoryMaker.ps1](https://www.aka.ms/azssecretrotationhelper)** を実行して、外部証明書に必要なディレクトリを作成します。

> [!IMPORTANT]
> CertDirectoryMaker スクリプトでは、次のようなフォルダー構造が作成されます。
>
> **.\Certificates\AAD** または ***.\Certificates\ADFS*** のいずれであるかは、Azure Stack Hub に使用される ID プロバイダーによって決まります。
>
> 最も重要なことは、フォルダー構造が **AAD** または **ADFS** フォルダーで終了し、すべてのサブディレクトリがこの構造内にあることです。そうでないと、**Start-SecretRotation** は次のようになります。
>
> ```powershell
> Cannot bind argument to parameter 'Path' because it is null.
> + CategoryInfo          : InvalidData: (:) [Test-Certificate], ParameterBindingValidationException
> + FullyQualifiedErrorId : ParameterArgumentValidationErrorNullNotAllowed,Test-Certificate
> + PSComputerName        : xxx.xxx.xxx.xxx
> ```
>
> エラー メッセージではファイル共有へのアクセスに問題があることが指示されていますが、実際には、ここに適用されるのはフォルダー構造です。 Microsoft AzureStack の適合性チェッカー [PublicCertHelper モジュール](https://www.powershellgallery.com/packages/Microsoft.AzureStack.ReadinessChecker/1.1811.1101.1/Content/CertificateValidation%5CPublicCertHelper.psm1)で、さらに詳しい情報がわかります。
>
> また、ファイル共有のフォルダー構造が **Certificates** フォルダーで始まっていることも重要です。そうでない場合も、検証が失敗します。
> ファイル共有のマウントは、 **\\\\\<IP アドレス>\\\<共有名>\\** のようになっている必要があり、内部に **Certificates\AAD** または **Certificates\ADFS** フォルダーが含まれる必要があります。
>
> 次に例を示します。
> - Fileshare = **\\\\\<IP アドレス>\\\<共有名>\\**
> - CertFolder = **Certificates\AAD**
> - FullPath = **\\\\\<IP アドレス>\\\<共有名>\Certificates\AAD**

## <a name="rotating-external-secrets"></a>外部シークレットのローテーション

外部シークレットのローテーションを行うには、次の手順に従います。

1. 前の手順で新しく作成した **\Certificates\\\<ID プロバイダー>** ディレクトリに、「[Azure Stack Hub PKI 証明書の要件](azure-stack-pki-certs.md#mandatory-certificates)」の **「必須の証明書**」セクションに記載されている形式に従ったディレクトリ構造で、交換用の外部証明書の新しいセットを配置します。

    Azure AD ID プロバイダーのフォルダー構造の例:
    ```powershell
        <ShareName>
        │   │
        │   └───Certificates
        │         └───AAD
        │             ├───ACSBlob
        │             │       <CertName>.pfx
        │             │
        │             ├───ACSQueue
        │             │       <CertName>.pfx
        │             │
        │             ├───ACSTable
        │             │       <CertName>.pfx
        │             │
        │             ├───Admin Extension Host
        │             │       <CertName>.pfx
        │             │
        │             ├───Admin Portal
        │             │       <CertName>.pfx
        │             │
        │             ├───ARM Admin
        │             │       <CertName>.pfx
        │             │
        │             ├───ARM Public
        │             │       <CertName>.pfx
        │             │
        │             ├───KeyVault
        │             │       <CertName>.pfx
        │             │
        │             ├───KeyVaultInternal
        │             │       <CertName>.pfx
        │             │
        │             ├───Public Extension Host
        │             │       <CertName>.pfx
        │             │
        │             └───Public Portal
        │                     <CertName>.pfx

    ```

2. **CloudAdmin** アカウントを使用して[特権エンドポイント](azure-stack-privileged-endpoint.md)で PowerShell セッションを作成し、セッションを変数として格納します。 次の手順で、この変数をパラメーターとして使います。

    > [!IMPORTANT]  
    > セッションに入らないでください。 セッションを変数として格納してください。

3. **[Invoke-Command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/Invoke-Command?view=powershell-5.1)** を実行します。 特権エンドポイントの PowerShell セッション変数を **Session** パラメーターとして渡します。

4. 次のパラメーターで **Start-SecretRotation** を実行します。
    - **PfxFilesPath**  
    前に作成した証明書ディレクトリへのネットワーク パスを指定します。  
    - **PathAccessCredential**  
    共有に対する資格情報の PSCredential オブジェクトです。
    - **CertificatePassword**  
    作成されるすべての pfx 証明書ファイルに使われるパスワードのセキュリティで保護された文字列です。

5. シークレットのローテーションが済むまで待ちます。 外部シークレットのローテーションには、約 1 時間かかります。

    シークレットのローテーションが正常に完了すると、コンソールに **[Overall action status: Success]\(全体的なアクションの状態: 成功\)** と表示されます。

    > [!Note]
    > シークレットのローテーションが失敗した場合は、エラー メッセージの指示に従い、 **-ReRun** パラメーターを指定して **Start-SecretRotation** を再実行します。

    ```powershell
    Start-SecretRotation -ReRun
    ```

    シークレットのローテーションの失敗が繰り返される場合は、サポートにお問い合わせください。

6. シークレットのローテーションが正常に完了した後、事前手順で作成した共有から証明書を削除し、セキュリティで保護されたバックアップ場所に保存します。

## <a name="use-powershell-to-rotate-secrets"></a>PowerShell を使用してシークレットをローテーションする

次の PowerShell の例では、シークレットのローテーションを行うために実行するコマンドレットとパラメーターを示します。

```powershell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Run Secret Rotation
$CertPassword = ConvertTo-SecureString "<CertPasswordHere>" -AsPlainText -Force
$CertShareCreds = Get-Credential
$CertSharePath = "<NetworkPathOfCertShare>"
Invoke-Command -Session $PEPSession -ScriptBlock {
    Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCreds -CertificatePassword $using:CertPassword
}
Remove-PSSession -Session $PEPSession
```

## <a name="rotating-only-internal-secrets"></a>内部シークレットのみのローテーション

> [!Note]
> 内部シークレットのローテーションは、内部シークレットのセキュリティが悪意のあるエンティティによって侵害された疑いがある場合、または内部証明書の有効期限が近づいていることを示すアラートを受け取った場合 (ビルド 1811 以降) にのみ実行する必要があります。 1811 より前の Azure Stack Hub 環境では、保留中の内部証明書やシークレットの有効期限のアラートが表示される場合があります。 これらのアラートは正確ではないため、内部シークレットのローテーションを実行せずに無視する必要があります。 正しくない内部シークレットの有効期限切れアラートは、1811 で解決される既知の問題です。 環境が 2 年間アクティブになっていない限り、内部シークレットの有効期限が切れることはありません。

1. [特権エンドポイント](azure-stack-privileged-endpoint.md)で PowerShell セッションを作成します。
2. 特権エンドポイント セッションで、**Start-SecretRotation -Internal** を実行します。

    > [!Note]
    > 1811 より前の Azure Stack Hub 環境では、 **-Internal** フラグは必要ありません。 **Start-SecretRotation** により、内部シークレットのみがローテーションされます。

3. シークレットのローテーションが済むまで待ちます。

   シークレットのローテーションが正常に完了すると、コンソールに **[Overall action status: Success]\(全体的なアクションの状態: 成功\)** と表示されます。
    > [!Note]
    > シークレット ローテーションが失敗した場合は、エラー メッセージ内の指示に従い、 **-Internal** パラメーターと **-Rerun** パラメーターを付けて **Start-SecretRotation** を再実行します。  

```powershell
Start-SecretRotation -Internal -ReRun
```

シークレットのローテーションの失敗が繰り返される場合は、サポートにお問い合わせください。

## <a name="start-secretrotation-reference"></a>Start-SecretRotation のリファレンス

Azure Stack Hub システムのシークレットのローテーションを行います。 Azure Stack Hub の特権エンドポイントに対してのみ実行されます。

### <a name="syntax"></a>構文

#### <a name="for-external-secret-rotation"></a>外部シークレットのローテーションの場合

```powershell
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential <PSCredential>] [-CertificatePassword <SecureString>]  
```

#### <a name="for-internal-secret-rotation"></a>内部シークレットのローテーションの場合

```powershell
Start-SecretRotation [-Internal]  
```

#### <a name="for-external-secret-rotation-rerun"></a>外部シークレットのローテーションの再実行の場合

```powershell
Start-SecretRotation [-ReRun]
```

#### <a name="for-internal-secret-rotation-rerun"></a>内部シークレットのローテーションの再実行の場合

```powershell
Start-SecretRotation [-ReRun] [-Internal]
```

### <a name="description"></a>説明

**Start-SecretRotation** コマンドレットは、Azure Stack Hub システムのインフラストラクチャ シークレットのローテーションを行います。 既定では、すべての外部ネットワーク インフラストラクチャ エンドポイントの証明書のみのローテーションを行います。 -Internal フラグを付けて使用した場合は、内部インフラストラクチャ シークレットのローテーションが行われます。 外部ネットワーク インフラストラクチャ エンドポイントのローテーションを行う場合は、**Invoke-Command** スクリプト ブロックで **Start-SecretRotation** を実行し、Azure Stack Hub 環境の特権エンドポイント セッションを **Session** パラメーターで渡す必要があります。

### <a name="parameters"></a>パラメーター

| パラメーター | Type | 必須 | [位置] | Default | 説明 |
| -- | -- | -- | -- | -- | -- |
| `PfxFilesPath` | String  | False  | named  | なし  | すべての外部ネットワーク エンドポイント証明書を含む **\Certificates** ディレクトリへのファイル共有パスです。 外部シークレットのローテーションを行う場合にのみ必要です。 最後のディレクトリは **\Certificates** にする必要があります。 |
| `CertificatePassword` | SecureString | False  | named  | なし  | -PfXFilesPath で提供されているすべての証明書のパスワード。 外部のシークレットのローテーションを行うときに PfxFilesPath を指定する場合は、必須の値です。 |
| `Internal` | String | False | named | なし | Azure Stack Hub オペレーターが内部インフラストラクチャ シークレットのローテーションを行うときは、常に Internal フラグを使用する必要があります。 |
| `PathAccessCredential` | PSCredential | False  | named  | なし  | すべての外部ネットワーク エンドポイント証明書を含む **\Certificates** ディレクトリへのファイル共有の PowerShell 資格情報。 外部シークレットのローテーションを行う場合にのみ必要です。  |
| `ReRun` | SwitchParameter | False  | named  | なし  | ReRun は、試行が失敗した後でシークレット ローテーションが再試行されるとき、常に使用する必要があります。 |

### <a name="examples"></a>例

#### <a name="rotate-only-internal-infrastructure-secrets"></a>内部インフラストラクチャ シークレットのみのローテーション

このコマンドは、Azure Stack Hub [環境の特権エンドポイント](azure-stack-privileged-endpoint.md)を使用して実行する必要があります。

```powershell
PS C:\> Start-SecretRotation -Internal
```

このコマンドを使用すると、Azure Stack Hub 内部ネットワークに公開されているすべてのインフラストラクチャ シークレットのローテーションが行われます。

#### <a name="rotate-only-external-infrastructure-secrets"></a>外部インフラストラクチャ シークレットのみのローテーション  

```powershell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Create Credentials for the fileshare
$CertPassword = ConvertTo-SecureString "<CertPasswordHere>" -AsPlainText -Force
$CertShareCreds = Get-Credential
$CertSharePath = "<NetworkPathOfCertShare>"
# Run Secret Rotation
Invoke-Command -Session $PEPSession -ScriptBlock {  
    Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCreds -CertificatePassword $using:CertPassword
}
Remove-PSSession -Session $PEPSession
```

このコマンドは、Azure Stack Hub の外部ネットワーク インフラストラクチャ エンドポイントで使用する TLS 証明書のローテーションを行います。

#### <a name="rotate-internal-and-external-infrastructure-secrets-pre-1811-only"></a>内部と外部のインフラストラクチャ シークレットのローテーション (**1811 より前**のみ)

> [!IMPORTANT]
> ローテーションは内部証明書と外部証明書に分けられたので、このコマンドは **1811 より前**の Azure Stack Hub にのみ適用されます。
>
> **"*1811 以降*" 、内部証明書と外部証明書の両方をローテーションすることはできなくなりました。**

```powershell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Create Credentials for the fileshare
$CertPassword = ConvertTo-SecureString "<CertPasswordHere>" -AsPlainText -Force
$CertShareCreds = Get-Credential
$CertSharePath = "<NetworkPathOfCertShare>"
# Run Secret Rotation
Invoke-Command -Session $PEPSession -ScriptBlock {
    Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCreds -CertificatePassword $using:CertPassword
}
Remove-PSSession -Session $PEPSession
```

このコマンドは、Azure Stack Hub 内部ネットワークに公開されているすべてのインフラストラクチャ シークレットと、Azure Stack Hub の外部ネットワーク インフラストラクチャ エンドポイントに使われる TLS 証明書のローテーションを行います。 Start-SecretRotation は、スタックで生成されたすべてのシークレットのローテーションを行い、証明書が指定されているため、外部エンドポイント証明書のローテーションも行われます。  

## <a name="update-the-baseboard-management-controller-bmc-credential"></a>ベースボード管理コントローラー (BMC) の資格情報の更新

ベースボード管理コントローラー (BMC) は、サーバーの物理的な状態を監視します。 BMC のユーザー アカウント名とパスワードを更新する手順については、OEM (Original Equipment Manufacturer) ハードウェア ベンダーを参照してください。

>[!NOTE]
> OEM から追加の管理アプリが提供されている場合があります。 他の管理アプリのユーザー名またはパスワードを更新しても、BMC のユーザー名またはパスワードには影響しません。

1. **1910 より前のバージョン**:OEM の指示に従って、Azure Stack Hub の物理サーバー上で BMC を更新します。 環境内の各 BMC のユーザー名とパスワードは同じである必要があります。 BMC のユーザー名は、16 文字を超えることはできません。

   **バージョン 1910 以降**:OEM の指示に従って Azure Stack Hub 物理サーバーの BMC 資格情報を最初に更新する必要がなくなりました。 環境内の各 BMC のユーザー名とパスワードは同じである必要があります。 BMC のユーザー名は、16 文字を超えることはできません。

    | パラメーター | 説明 | State |
    | --- | --- | --- |
    | BypassBMCUpdate | このパラメーターを使用すると、BMC の資格情報は更新されません。 更新されるのは、Azure Stack Hub 内部データストアのみです。 | 省略可能 |

2. Azure Stack Hub セッションで特権エンドポイントを開きます。 手順については、「[Azure Stack Hub で特権エンドポイントを使用する](azure-stack-privileged-endpoint.md)」を参照してください。

3. PowerShell プロンプトが **[IP アドレスまたは ERCS VM 名]:PS>** または **[azs-ercs01]:PS>** に環境に応じて変更されたら、`Set-BmcCredential` を `Invoke-Command` によって実行します。 パラメーターとして特権エンドポイントのセッション変数を渡します。 次に例を示します。

    ```powershell
    # Interactive Version
    $PEPIp = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEPCreds = Get-Credential "<Domain>\CloudAdmin" -Message "PEP Credentials"
    $NewBmcPwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString
    $NewBmcUser = Read-Host -Prompt "Enter New BMC user name"

    $PEPSession = New-PSSession -ComputerName $PEPIp -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

    Invoke-Command -Session $PEPSession -ScriptBlock {
        # Parameter BmcPassword is mandatory, while the BmcUser parameter is optional.
        Set-BmcCredential -BmcPassword $using:NewBmcPwd -BmcUser $using:NewBmcUser
    }
    Remove-PSSession -Session $PEPSession
    ```

    パスワードをコード行として含む静的な PowerShell バージョンを使用することもできます。

    ```powershell
    # Static Version
    $PEPIp = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEPUser = "<Privileged Endpoint user for example Domain\CloudAdmin>"
    $PEPPwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PEPCreds = New-Object System.Management.Automation.PSCredential ($PEPUser, $PEPPwd)
    $NewBmcPwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force
    $NewBmcUser = "<New BMC User name>"

    $PEPSession = New-PSSession -ComputerName $PEPIp -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

    Invoke-Command -Session $PEPSession -ScriptBlock {
        # Parameter BmcPassword is mandatory, while the BmcUser parameter is optional.
        Set-BmcCredential -BmcPassword $using:NewBmcPwd -BmcUser $using:NewBmcUser
    }
    Remove-PSSession -Session $PEPSession
    ```

## <a name="next-steps"></a>次のステップ

[Azure Stack Hub のセキュリティについて詳しく学習する](azure-stack-security-foundations.md)
