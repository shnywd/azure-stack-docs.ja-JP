---
title: App Service on Azure Stack Hub のシークレットと証明書を交換する
description: Azure App Service on Azure Stack Hub で使用されるシークレットと証明書を交換する方法を学習します
author: apwestgarth
ms.topic: article
ms.date: 04/09/2020
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 04/09/2020
ms.openlocfilehash: edbc0a0e9d0382f53503dddf4362a371b6369ebc
ms.sourcegitcommit: a630894e5a38666c24e7be350f4691ffce81ab81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "81005401"
---
# <a name="rotate-app-service-on-azure-stack-hub-secrets-and-certificates"></a>App Service on Azure Stack Hub のシークレットと証明書を交換する

これらの手順は、Azure App Service on Azure Stack Hub にのみ適用されます。  Azure App Service on Azure Stack Hub のシークレットの交換は、Azure Stack Hub 用の一元化されたシークレットの交換手順には含まれていません。  オペレーターは、システム内のシークレットの有効性、最後に更新された日付、およびシークレットが期限切れになるまでの残り時間を監視できます。

> [!Important]
> Azure App Service on Azure Stack Hub は Azure Stack Hub アラート サービスと統合されていないため、オペレーターが、Azure Stack Hub ダッシュボードでシークレットの有効期限に関するアラートを受け取ることはありません。  オペレーターは、Azure Stack Hub 管理者ポータルで Azure App Service on Azure Stack Hub 管理エクスペリエンスを使用して、シークレットを定期的に監視する必要があります。

このドキュメントには、次のシークレットの交換手順が含まれています。

* Azure App Service on Azure Stack Hub 内で使用される暗号化キー
* ホスティングおよび計測データベースを操作するために、Azure App Service on Azure Stack Hub で使用されるデータベース接続資格情報
* Azure Active Directory (Azure AD) または Active Directory フェデレーション サービス (AD FS) で ID アプリケーション証明書のエンドポイントとローテーションをセキュリティで保護するために、Azure Stack Hub の Azure App Service によって使用される証明書
* Azure App Service on Azure Stack Hub インフラストラクチャ ロールのシステム資格情報

## <a name="rotate-encryption-keys"></a>暗号化キーを交換する

Azure App Service on Azure Stack Hub 内で使用される暗号化キーを交換するには、次の手順を行います。

1. Azure Stack Hub 管理者ポータルで App Service 管理エクスペリエンスにアクセスします。

1. **[シークレット]** メニュー オプションに移動します

1. [暗号化キー] セクションで **[Rotate]\(交換\)** ボタンをクリックします

1. **[OK]** をクリックして、交換手順を開始します。

1. 暗号化キーが交換され、すべてのロール インスタンスが更新されます。 オペレーターは、 **[状態]** ボタンを使用して、手順の状態を監視できます。

## <a name="rotate-connection-strings"></a>接続文字列を交換する

App Service ホスティングおよび計測データベース用のデータベース接続文字列の資格情報を更新するには、次の手順を行います。

1. Azure Stack Hub 管理者ポータルで App Service 管理エクスペリエンスにアクセスします。

1. **[シークレット]** メニュー オプションに移動します

1. [接続文字列] セクションの **[Rotate]\(交換\)** ボタンをクリックします

1. **SQL SA ユーザー名**と**パスワード**を指定し、 **[OK]** をクリックして交換手順を開始します。 

1. 資格情報は、Azure App Service ロール インスタンス全体で交換されます。 オペレーターは、 **[状態]** ボタンを使用して、手順の状態を監視できます。

## <a name="rotate-certificates"></a>証明書のローテーション

Azure App Service on Azure Stack Hub 内で使用される証明書を交換するには、次の手順を行います。

1. Azure Stack Hub 管理者ポータルで App Service 管理エクスペリエンスにアクセスします。

1. **[シークレット]** メニュー オプションに移動します

1. [証明書] セクションの **[Rotate]\(交換\)** ボタンをクリックします

1. 交換する証明書の**証明書ファイル**と関連付けられている**パスワード**を指定し、 **[OK]** をクリックします。

1. 証明書は、必要に応じて、Azure App Service on Azure Stack Hub ロール インスタンス全体で交換されます。  オペレーターは、 **[状態]** ボタンを使用して、手順の状態を監視できます。

ID アプリケーション証明書がローテーションされるときは、AAD または ADFS の対応するアプリケーションも、新しい証明書で更新する必要があります。

> [!IMPORTANT]
> 新しい証明書で ID アプリケーションを更新しないと、ローテーション後、Azure Functions のユーザー ポータル エクスペリエンスが中断され、ユーザーは KUDU 開発者ツールを使用できなくなり、管理者は App Service の管理エクスペリエンスからワーカー層のスケール セットを管理できなくなります。

### <a name="rotate-credential-for-the-azure-ad-identity-application"></a>Azure AD ID アプリケーションの資格情報をローテーションする 

ID アプリケーションは、Azure Stack Hub への Azure App Service のデプロイの前に、オペレーターによって作成されます。  アプリケーション ID が不明な場合は、次の手順に従って見つけます。

1. **Azure Stack Hub 管理ポータル**にアクセスします

1. **[サブスクリプション]** に移動し、 **[既定のプロバイダー サブスクリプション]** を選択します

1. **[アクセス制御 (IAM)]** を選択し、 **[App Service]** アプリケーションを選択します

1. **APP ID** をメモしておきます。この値は、Azure Active Directory で更新する必要がある ID アプリケーションのアプリケーション ID です。

アプリケーションの証明書を Active Directory でローテーションするには、次の手順を実行します。

1. **Azure portal** にアクセスし、Azure Stack Hub のデプロイに使用したグローバル管理者を使用してサインインします

1. **[Azure Active Directory]** にアクセスし、 **[アプリの登録]** に移動します

1. **アプリケーション ID** を検索し、ID アプリケーション ID を指定します

1. アプリケーションを選択してから、 **[証明書とシークレット]** に移動します

1. **[証明書のアップロード]** をクリックして、ID アプリケーションの新しい証明書 (.cer、.pem、.crt のいずれかのファイルの種類) をアップロードします

1. **拇印**が、Azure Stack Hub 管理ポータルの App Service 管理エクスペリエンスに一覧表示されているものと一致することを確認します

1. 古い証明書を削除します

### <a name="rotate-certificate-for-adfs-identity-application"></a>ADFS ID アプリケーションの証明書をローテーションする

ID アプリケーションは、Azure Stack Hub への Azure App Service のデプロイの前に、オペレーターによって作成されます。  アプリケーションのオブジェクト ID が不明な場合は、次の手順に従って見つけます。

1. **Azure Stack Hub 管理ポータル**にアクセスします

1. **[サブスクリプション]** に移動し、 **[既定のプロバイダー サブスクリプション]** を選択します

1. **[アクセス制御 (IAM)]** を選択し、"**AzureStack-AppService-<guid>** " アプリケーションを選択します

1. **オブジェクト ID** をメモしておきます。この値は、ADFS で更新する必要があるサービス プリンシパルの ID です。

ADFS でアプリケーションの証明書をローテーションするには、特権エンドポイント (PEP) にアクセスできる必要があります。その後、PowerShell を使用して証明書の資格情報を更新し、次のプレースホルダーを独自の値に置き換えます。

| プレースホルダー | 説明 | 例 |
| ----------- | ----------- | ------- |
| \<PepVM\> | Azure Stack Hub インスタンス上にある特権エンドポイント VM の名前。 | "AzS-ERCS01" |
| \<CertificateFileLocation\> | ディスク上の X509 証明書の場所。 | "d:\certs\sso.cer" |
| \<ApplicationObjectId\> | ID アプリケーションに割り当てられた識別子。 | "S-1-5-21-401916501-2345862468-1451220656-1451" |

 1. Windows PowerShell セッションを管理者特権で開き、次のスクリプトを実行します

    ```powershell
    # Sign in to PowerShell interactively, using credentials that have access to the VM running the Privileged Endpoint
    $Creds = Get-Credential

    # Create a new Certificate object from the identity application certificate exported as .cer file
    $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2("<CertificateFileLocation>")

    # Create a new PSSession to the PrivelegedEndpoint VM
    $Session = New-PSSession -ComputerName "<PepVm>" -ConfigurationName PrivilegedEndpoint -Credential $Creds

    # Use the privileged endpoint to update the certificate thumbprint, used by the service principal associated with the App Service identity application
    $SpObject = Invoke-Command -Session $Session -ScriptBlock {Set-GraphApplication -ApplicationIdentifier "<ApplicationObjectId>" -ClientCertificates $using:Cert}
    $Session | Remove-PSSession

    # Output the updated service principal details
    $SpObject

    ```

1. このスクリプトが完了すると、更新後のアプリ登録の情報が表示され、この中には証明書の拇印も含まれます

    ```shell
    ApplicationIdentifier : S-1-5-21-401916501-2345862468-1451220656-1451
    ClientId              : 
    Thumbprint            : FDAA679BF9EDDD0CBB581F978457A37BFD73CA3B
    ApplicationName       : Azurestack-AppService-d93601c2-1ec0-4cac-8d1c-8ccde63ef308
    ClientSecret          : 
    PSComputerName        : AzS-ERCS01
    RunspaceId            : cb471c79-a0d3-40ec-90ba-89087d104510
    ```


## <a name="rotate-system-credentials"></a>システム資格情報を交換する

Azure App Service on Azure Stack Hub 内で使用されるシステム資格情報を交換するには、次の手順を行います。

1. Azure Stack Hub 管理者ポータルで App Service 管理エクスペリエンスにアクセスします。

1. **[シークレット]** メニュー オプションに移動します

1. [システム資格情報] セクションの **[Rotate]\(交換\)** ボタンをクリックします

1. 交換するシステム資格情報の **[スコープ]** を選択します。  オペレーターは、すべてのロールまたは個々のロールのシステム資格情報を交換することを選択できます。

1. **新しいローカル管理者のユーザー名**、新しい**パスワード**を指定し、**パスワード**を確認して **[OK]** をクリックします

1. 資格情報は、必要に応じて、対応する Azure App Service on Azure Stack Hub ロール インスタンス全体で交換されます。  オペレーターは、 **[状態]** ボタンを使用して、手順の状態を監視できます。



