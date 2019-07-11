---
title: Azure Stack セキュリティ コントロールを構成する
description: Azure Stack でセキュリティ コントロールを構成する方法について説明します
services: azure-stack
author: PatAltimore
ms.service: azure-stack
ms.topic: article
ms.date: 06/17/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 06/17/2019
ms.openlocfilehash: b36a6d826dc7249f10b4785b27511096e45923a9
ms.sourcegitcommit: 7348876a97e8bed504b5f5d90690ec8d1d9472b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67557861"
---
# <a name="configure-azure-stack-security-controls"></a>Azure Stack セキュリティ コントロールを構成する

*適用対象:Azure Stack 統合システム*

この記事では、Azure Stack で変更できるセキュリティ コントロールについて説明し、可能な場合はそのトレードオフについて重点的に説明します。

Azure Stack アーキテクチャは、侵害の想定と既定での強化という 2 つのセキュリティ原則の柱に基づいて構築されています。 Azure Stack セキュリティの詳細については、「[Azure Stack インフラストラクチャのセキュリティ体制](azure-stack-security-foundations.md)」を参照してください。 Azure Stack の既定のセキュリティ体制は実稼働可能ですが、追加の強化を必要とするいくつかのデプロイ シナリオがあります。

## <a name="tls-version-policy"></a>TLS バージョン ポリシー

トランスポート層セキュリティ (TLS) プロトコルは、ネットワーク経由で暗号化された通信を確立するために広く採用されている暗号プロトコルです。 TLS は、時間の経過と共に進化し、複数のバージョンがリリースされています。 Azure Stack インフラストラクチャでは、そのすべての通信に TLS 1.2 のみを使用しています。 外部インターフェイスには、Azure Stack では現在、TLS 1.2 が既定で使用されています。 ただし、下位互換性のため、TLS 1.1 および 1\.0 のネゴシエートもサポートしています。 TLS クライアントが、TLS 1.1 または TLS 1.0 経由の通信を要求すると、Azure Stack は下位の TLS バージョンとネゴシエートして要求を受け入れます。 クライアントが TLS 1.2 を要求すると、Azure Stack は TLS 1.2 を使用して TLS 接続を確立します。

TLS 1.0 と 1.1 は組織やコンプライアンス基準で徐々に非推奨または禁止になっているため、1906 更新プログラムからは、Azure Stack で TLS ポリシーを構成できるようになりました。 TLS 1.2 より前のバージョンで TLS セッションを確立する試みを許可せずに拒否する TLS 1.2 のみのポリシーを適用できます。

> [!IMPORTANT]
> Microsoft では、Azure Stack の運用環境には TLS 1.2 のみのポリシーを使用することをお勧めします。

## <a name="get-tls-policy"></a>TLS ポリシーを取得する

[特権エンドポイント (PEP)](azure-stack-privileged-endpoint.md) を使用して、すべての Azure Stack エンドポイントの TLS ポリシーを表示します。

```powershell
Get-TLSPolicy
```

出力例:

    TLS_1.2

## <a name="set-tls-policy"></a>TLS ポリシーを設定する

[特権エンドポイント (PEP)](azure-stack-privileged-endpoint.md) を使用して、すべての Azure Stack エンドポイントの TLS ポリシーを設定します。

```powershell
Set-TLSPolicy -Version <String>
```

*Set-TLSPolicy* コマンドレットのパラメーター:

| パラメーター | 説明 | データ型 | 必須 |
|---------|---------|---------|---------|
| *バージョン* | Azure Stack で許可される TLS のバージョン | string | はい|

すべての Azure Stack エンドポイントに対して許可された TLS バージョンを構成するには、次のいずれかの値を使用します。

| バージョンの値 | 説明 |
|---------|---------|
| *TLS_All* | Azure Stack の TLS エンドポイントでは TLS 1.2 がサポートされていますが、TLS 1.1 と TLS 1.0 のダウン ネゴシエーションは許可されています。 |
| *TLS_1.2* | Azure Stack の TLS エンドポイントでは、TLS 1.2 のみがサポートされます。 | 

TLS ポリシーの更新は、完了するまで数分かかります。

### <a name="enforce-tls-12-configuration-example"></a>TLS 1.2 構成例を適用する

この例では、TLS 1.2 のみを適用するように TLS ポリシーを設定します。

```powershell
Set-TLSPolicy -Version TLS_1.2
```

出力例:

    VERBOSE: Successfully setting enforce TLS 1.2 to True
    VERBOSE: Invoking action plan to update GPOs
    VERBOSE: Create Client for execution of action plan
    VERBOSE: Start action plan
    <...>
    VERBOSE: Verifying TLS policy
    VERBOSE: Get GPO TLS protocols registry 'enabled' values
    VERBOSE: GPO TLS applied with the following preferences:
    VERBOSE:     TLS protocol SSL 2.0 enabled value: 0
    VERBOSE:     TLS protocol SSL 3.0 enabled value: 0
    VERBOSE:     TLS protocol TLS 1.0 enabled value: 0
    VERBOSE:     TLS protocol TLS 1.1 enabled value: 0
    VERBOSE:     TLS protocol TLS 1.2 enabled value: 1
    VERBOSE: TLS 1.2 is enforced

### <a name="allow-all-versions-of-tls-12-11-and-10-configuration-example"></a>TLS のすべてのバージョン (1.2、1.1、1.0) の構成例を許可する

この例では、TLS のすべてのバージョン (1.2、1.1、1.0) を許可するように TLS ポリシーを設定します。

```powershell
Set-TLSPolicy -Version TLS_All
```

出力例:

    VERBOSE: Successfully setting enforce TLS 1.2 to False
    VERBOSE: Invoking action plan to update GPOs
    VERBOSE: Create Client for execution of action plan
    VERBOSE: Start action plan
    <...>
    VERBOSE: Verifying TLS policy
    VERBOSE: Get GPO TLS protocols registry 'enabled' values
    VERBOSE: GPO TLS applied with the following preferences:
    VERBOSE:     TLS protocol SSL 2.0 enabled value: 0
    VERBOSE:     TLS protocol SSL 3.0 enabled value: 0
    VERBOSE:     TLS protocol TLS 1.0 enabled value: 1
    VERBOSE:     TLS protocol TLS 1.1 enabled value: 1
    VERBOSE:     TLS protocol TLS 1.2 enabled value: 1
    VERBOSE: TLS 1.2 is not enforced

## <a name="next-steps"></a>次の手順

- [Azure Stack インフラストラクチャのセキュリティ体制について学習する](azure-stack-security-foundations.md)
- [Azure Stack でシークレットをローテーションする方法を確認する](azure-stack-rotate-secrets.md)
- [Azure Stack 上で Windows Defender ウイルス対策を更新する](azure-stack-security-av.md)
