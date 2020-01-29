---
title: Azure Stack Hub への証明書署名要求を生成する | Microsoft Docs
description: Azure Stack Hub 統合システムにおいて Azure Stack Hub PKI 証明書への証明書署名要求を生成する方法について学習します。
services: azure-stack
documentationcenter: ''
author: justinha
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2019
ms.author: justinha
ms.reviewer: ppacent
ms.lastreviewed: 09/10/2019
ms.openlocfilehash: 4b966ac4faec51dac06e39fab8804e295a9a40b1
ms.sourcegitcommit: 320eddb281a36d066ec80d67b103efad7d4f33c8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2020
ms.locfileid: "76145837"
---
# <a name="generate-certificate-signing-requests-for-azure-stack-hub"></a>Azure Stack Hub への証明書署名要求を生成する

Azure Stack Hub 対応性チェッカー ツールを使用して、Azure Stack Hub のデプロイに適した証明書署名要求 (CSR) を作成できます。 証明書は、デプロイ前のテストに十分な時間を確保した上で、要求、生成、検証する必要があります。 このツールは [PowerShell ギャラリーから](https://aka.ms/AzsReadinessChecker)取得できます。

Azure Stack Hub 対応性チェッカー ツール (AzsReadinessChecker) を使用すると、次の証明書を要求できます。

- [証明書署名要求の生成](azure-stack-get-pki-certs.md#generate-certificate-signing-requests)に関する説明に従った**標準の証明書要求**。
- **サービスとしてのプラットフォーム**:「[Azure Stack Hub 公開キー インフラストラクチャ証明書の要件」の「オプションの PaaS 証明書](azure-stack-pki-certs.md#optional-paas-certificates)」で指定されているように、証明書に対するサービスとしてのプラットフォーム (PaaS) 名を要求できます。

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

## <a name="generate-certificate-signing-requests"></a>証明書の署名要求を生成する

次の手順を使って、Azure Stack Hub PKI 証明書を準備し、検証します。

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
    New-AzsCertificateSigningRequest -certificateType Deployment -RegionName $regionName -FQDN $externalFQDN -subject $subject -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ```

    他の Azure Stack Hub サービスへの証明書要求を生成するには、`-CertificateType` の値を変更します。 次に例を示します。

    ```powershell  
    # App Services
    New-AzsCertificateSigningRequest -certificateType AppServices -RegionName $regionName -FQDN $externalFQDN -subject $subject -OutputRequestPath $OutputDirectory

    # DBAdapter
    New-AzsCertificateSigningRequest -certificateType DBAdapter -RegionName $regionName -FQDN $externalFQDN -subject $subject -OutputRequestPath $OutputDirectory

    # EventHubs
    New-AzsCertificateSigningRequest -certificateType EventHubs -RegionName $regionName -FQDN $externalFQDN -subject $subject -OutputRequestPath $OutputDirectory

    # IoTHub
    New-AzsCertificateSigningRequest -certificateType IoTHub -RegionName $regionName -FQDN $externalFQDN -subject $subject -OutputRequestPath $OutputDirectory
    ```

7. または、開発/テスト環境の場合は、複数のサブジェクトの別名 (SAN) を含む 1 つの証明書要求を生成するには、 **-RequestType SingleCSR** パラメーターと値を追加します (運用環境では、推奨**されません**)。

    ```powershell  
    New-AzsCertificateSigningRequest -certificateType Deployment -RegionName $regionName -FQDN $externalFQDN -RequestType SingleCSR -subject $subject -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ```

8.  出力を確認します。

    ```powershell  
    New-AzsCertificateSigningRequest v1.1912.1082.37 started.
    Starting Certificate Request Process for Deployment
    CSR generating for following SAN(s): *.adminhosting.east.azurestack.contoso.com,*.adminvault.east.azurestack.contoso.com,*.blob.east.azurestack.contoso.com,*.hosting.east.azurestack.contoso.com,*.queue.east.azurestack.contoso.com,*.table.east.azurestack.contoso.com,*.vault.east.azurestack.contoso.com,adminmanagement.east.azurestack.contoso.com,adminportal.east.azurestack.contoso.com,management.east.azurestack.contoso.com,portal.east.azurestack.contoso.com
    Present this CSR to your Certificate Authority for Certificate Generation: C:\Users\checker\Documents\AzureStackCSR\wildcard_adminhosting_east_azurestack_contoso_com_CertRequest_20191219140359.req
    Certreq.exe output: CertReq: Request Created

    Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
    New-AzsCertificateSigningRequest Completed
    ```

9.  生成された **.REQ** ファイルを CA (内部またはパブリック) に送信します。 **New-AzsCertificateSigningRequest** の出力ディレクトリには、証明機関への送信に必要な CSR が含まれています。 また、このディレクトリには、証明書要求の生成中に使用される INF ファイルが含まれる子ディレクトリも参照用として含まれています。 CA が生成された要求を使用して、[Azure Stack Hub PKI の要件](azure-stack-pki-certs.md)を満たす証明書を生成することを確認してください。

## <a name="next-steps"></a>次のステップ

[Azure Stack Hub PKI 証明書の準備](azure-stack-prepare-pki-certs.md)
