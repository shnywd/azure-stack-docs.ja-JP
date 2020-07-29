---
title: Azure Stack Hub でのデプロイ用に証明書署名要求を取得する
description: Azure Stack Hub 統合システムにおいて Azure Stack Hub PKI 証明書の証明書署名要求を取得する方法について学習します。
author: IngridAtMicrosoft
ms.topic: article
ms.date: 09/10/2019
ms.author: inhenkel
ms.reviewer: ppacent
ms.lastreviewed: 09/10/2019
ms.openlocfilehash: 37f308a9b554453a1f7c10219d68b1255c23cbf0
ms.sourcegitcommit: 09fbc4e8fc53828647d515bfb556dfe42df28c19
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/16/2020
ms.locfileid: "86419271"
---
# <a name="get-certificate-signing-requests-for-deployment-in-azure-stack-hub"></a>Azure Stack Hub でのデプロイ用に証明書署名要求を取得する

Azure Stack Hub 対応性チェッカー ツールを使用して、Azure Stack Hub のデプロイに適した証明書署名要求 (CSR) を作成できます。 証明書は、デプロイ前のテストに十分な時間を確保した上で、要求、生成、検証する必要があります。 このツールは [PowerShell ギャラリーから](https://aka.ms/AzsReadinessChecker)取得できます。

Azure Stack Hub 対応性チェッカー ツール (AzsReadinessChecker) を使用すると、次の証明書を要求できます。

- [証明書署名要求の生成](azure-stack-get-pki-certs.md)に関する説明に従った**標準の証明書要求**。
- **サービスとしてのプラットフォーム**:「[Azure Stack Hub 公開キー インフラストラクチャ証明書の要件」の「オプションの PaaS 証明書](azure-stack-pki-certs.md)」で指定されているように、証明書に対するサービスとしてのプラットフォーム (PaaS) 名を要求できます。

## <a name="prerequisites"></a>前提条件

Azure Stack Hub デプロイのための PKI 証明書に対する CSR を生成する前に、システムは次の前提条件を満たしている必要があります。

- Microsoft Azure Stack Hub 適合性チェッカー
- 証明書の属性:
  - リージョン名
  - 外部完全修飾ドメイン名 (FQDN)
  - サブジェクト
- Windows 10 または Windows Server 2016 以降

  > [!NOTE]  
  > 証明機関から証明書が送り返されたら、[Azure Stack Hub PKI 証明書の準備](azure-stack-prepare-pki-certs.md)に関するページの手順を同じシステムで完了する必要があります。

## <a name="generate-certificate-signing-requests-for-new-deployments"></a>新しいデプロイのための証明書署名要求を生成する

新しい Azure Stack Hub PKI 証明書のための証明書署名要求を準備するには、次の手順を使用します。

1. 次のコマンドレットを実行して、PowerShell プロンプト (5.1 以上) から AzsReadinessChecker をインストールします。

    ```powershell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ```

2. **サブジェクト**を宣言します。 次に例を示します。

    ```powershell  
    $subject = "C=US,ST=Washington,L=Redmond,O=Microsoft,OU=Azure Stack Hub"
    ```

    > [!NOTE]  
    > 共通名 (CN) を指定すると、すべての証明書要求で CN が構成されます。 CN を省略すると、証明書要求に対して Azure Stack Hub サービスの最初の DNS 名が構成されます。

3. 既に存在する出力ディレクトリを宣言します。 次に例を示します。

    ```powershell  
    $outputDirectory = "$ENV:USERPROFILE\Documents\AzureStackCSR"
    ```

4. ID システムを宣言します。

    Azure Active Directory (Azure AD):

    ```powershell
    $IdentitySystem = "AAD"
    ```

    Active Directory フェデレーション サービス (AD FS):

    ```powershell
    $IdentitySystem = "ADFS"
    ```
    > [!NOTE]  
    > このパラメーターが必要なのは CertificateType が Deployment の場合のみです。

5. Azure Stack Hub のデプロイのために**リージョン名**と**外部 FQDN** を宣言します。

    ```powershell
    $regionName = 'east'
    $externalFQDN = 'azurestack.contoso.com'
    ```

    > [!NOTE]  
    > `<regionName>.<externalFQDN>` は、Azure Stack Hub のすべての外部 DNS 名が作成される基礎となります。 この例では、ポータルは `portal.east.azurestack.contoso.com` となります。  

6. デプロイ用に証明書署名要求を生成するため、次を実行します。

    ```powershell  
    New-AzsHubDeploymentCertificateSigningRequest -RegionName $regionName -FQDN $externalFQDN -subject $subject -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ```

    他の Azure Stack Hub サービスへの証明書要求を生成するには、`-CertificateType` の値を変更します。 次に例を示します。

    ```powershell  
    # App Services
    New-AzsHubAppServicesCertificateSigningRequest -RegionName $regionName -FQDN $externalFQDN -subject $subject -OutputRequestPath $OutputDirectory

    # DBAdapter
    New-AzsHubDbAdapterCertificateSigningRequest -RegionName $regionName -FQDN $externalFQDN -subject $subject -OutputRequestPath $OutputDirectory

    # EventHubs
    New-AzsHubEventHubsCertificateSigningRequest -RegionName $regionName -FQDN $externalFQDN -subject $subject -OutputRequestPath $OutputDirectory

    # IoTHub
    New-AzsHubIoTHubCertificateSigningRequest -RegionName $regionName -FQDN $externalFQDN -subject $subject -OutputRequestPath $OutputDirectory
    ```

7. または、開発/テスト環境の場合は、複数のサブジェクトの別名 (SAN) を含む 1 つの証明書要求を生成するには、 **-RequestType SingleCSR** パラメーターと値を追加します (運用環境では、推奨**されません**)。

    ```powershell  
    New-AzsHubDeploymentCertificateSigningRequest -RegionName $regionName -FQDN $externalFQDN -RequestType SingleCSR -subject $subject -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ```

8.  出力を確認します。

    ```powershell  
    Starting Certificate Request Process for Deployment
    CSR generating for following SAN(s): *.adminhosting.east.azurestack.contoso.com,*.adminvault.east.azurestack.contoso.com,*.blob.east.azurestack.contoso.com,*.hosting.east.azurestack.contoso.com,*.queue.east.azurestack.contoso.com,*.table.east.azurestack.contoso.com,*.vault.east.azurestack.contoso.com,adminmanagement.east.azurestack.contoso.com,adminportal.east.azurestack.contoso.com,management.east.azurestack.contoso.com,portal.east.azurestack.contoso.com
    Present this CSR to your Certificate Authority for Certificate Generation: C:\Users\[*redacted*]\Documents\AzureStackCSR\Deployment_east_azurestack_contoso_com_SingleCSR_CertRequest_20200710165538.req
    Certreq.exe output: CertReq: Request Created
    ```

9.  生成された **.REQ** ファイルを CA (内部またはパブリック) に送信します。 **New-AzsCertificateSigningRequest** の出力ディレクトリには、証明機関への送信に必要な CSR が含まれています。 また、このディレクトリには、証明書要求の生成中に使用される INF ファイルが含まれる子ディレクトリも参照用として含まれています。 CA が生成された要求を使用して、[Azure Stack Hub PKI の要件](azure-stack-pki-certs.md)を満たす証明書を生成することを確認してください。

## <a name="generate-certificate-signing-requests-for-certificate-renewal"></a>証明書更新のための証明書署名要求を生成する

既存の Azure Stack Hub PKI 証明書を更新するための証明書署名要求を準備するには、次の手順を使用します。

1. 次のコマンドレットを実行して、PowerShell プロンプト (5.1 以上) から AzsReadinessChecker をインストールします。

    ```powershell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ```

2. **stampEndpoint** を宣言します。 次に例を示します。

    ```powershell  
    $stampEndpoint = 'portal.east.azurestack.contoso.com'
    ```

    > [!NOTE]  
    > 上記のエンドポイントには、HTTPS 接続が必要です。
    > 上記のエンドポイントは、証明書の種類に必要な証明書の 1 つと一致する必要があります。たとえば、デプロイ証明書の場合は、portal.region.domain エンドポイント、AppServices の場合は、sso.appservices.region.domain が必要です。エンドポイントにバインドされた証明書は、サブジェクト、キーの長さ、署名アルゴリズムなどの属性を複製するために使用されます。  必要な既存のエンドポイントは 1 つだけであり、すべての署名要求により、必要なすべての証明書が作成されます。

3. 既に存在する出力ディレクトリを宣言します。 次に例を示します。

    ```powershell  
    $outputDirectory = "$ENV:USERPROFILE\Documents\AzureStackCSR"
    ```

4. デプロイ用に証明書署名要求を生成するため、次を実行します。

    ```powershell  
    New-AzsHubDeploymentCertificateSigningRequest -StampEndpoint $stampEndpoint -OutputRequestPath $OutputDirectory
    ```

    他の Azure Stack Hub サービスへの証明書要求を生成するには、次を使用します。

    ```powershell  
    # App Services
    New-AzsHubAppServicesCertificateSigningRequest -StampEndpoint $stampEndpoint -OutputRequestPath $OutputDirectory

    # DBAdapter
    New-AzsHubDBAdapterCertificateSigningRequest -StampEndpoint $stampEndpoint -OutputRequestPath $OutputDirectory

    # EventHubs
    New-AzsHubEventHubsCertificateSigningRequest -StampEndpoint $stampEndpoint -OutputRequestPath $OutputDirectory

    # IoTHub
    New-AzsHubIotHubCertificateSigningRequest -StampEndpoint $stampEndpoint -OutputRequestPath $OutputDirectory
    ```

5. または、開発/テスト環境の場合は、複数のサブジェクトの別名 (SAN) を含む 1 つの証明書要求を生成するには、 **-RequestType SingleCSR** パラメーターと値を追加します (運用環境では、推奨**されません**)。

    ```powershell  
    New-AzsHubDeploymentCertificateSigningRequest -StampEndpoint $stampendpoint -OutputRequestPath $OutputDirectory -RequestType SingleCSR
    ```

6.  出力を確認します。

    ```powershell  
    Querying StampEndpoint portal.east.azurestack.contoso.com for existing certificate
    Starting Certificate Request Process for Deployment
    CSR generating for following SAN(s): *.adminhosting.east.azurestack.contoso.com,*.adminvault.east.azurestack.contoso.com,*.blob.east.azurestack.contoso.com,*.hosting.east.azurestack.contoso.com,*.queue.east.azurestack.contoso.com,*.table.east.azurestack.contoso.com,*.vault.east.azurestack.contoso.com,adminmanagement.east.azurestack.contoso.com,adminportal.east.azurestack.contoso.com,management.east.azurestack.contoso.com,portal.east.azurestack.contoso.com
    Present this CSR to your Certificate Authority for Certificate Generation: C:\Users\[*redacted*]\Documents\AzureStackCSR\Deployment_east_azurestack_contoso_com_SingleCSR_CertRequest_20200710122723.req
    Certreq.exe output: CertReq: Request Created
    ```

7.  生成された **.REQ** ファイルを CA (内部またはパブリック) に送信します。 **New-AzsCertificateSigningRequest** の出力ディレクトリには、証明機関への送信に必要な CSR が含まれています。 また、このディレクトリには、証明書要求の生成中に使用される INF ファイルが含まれる子ディレクトリも参照用として含まれています。 CA が生成された要求を使用して、[Azure Stack Hub PKI の要件](azure-stack-pki-certs.md)を満たす証明書を生成することを確認してください。

## <a name="next-steps"></a>次のステップ

[Azure Stack Hub PKI 証明書の準備](azure-stack-prepare-pki-certs.md)
