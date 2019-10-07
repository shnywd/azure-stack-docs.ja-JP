---
title: Azure Stack の Key Vault の概要 | Microsoft Docs
description: Azure Stack での Key Vault によるキーとシークレットの管理方法について説明します
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 70f1684a-3fbb-4cd1-bf29-9f9882e98fe9
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: sethm
ms.lastreviewed: 05/21/2019
ms.openlocfilehash: 2ad70754a63660a2ad374a324e8778c405446d29
ms.sourcegitcommit: bbf3edbfc07603d2c23de44240933c07976ea550
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71714675"
---
# <a name="introduction-to-key-vault-in-azure-stack"></a>Azure Stack での Key Vault の概要

## <a name="prerequisites"></a>前提条件

* Azure Key Vault サービスを含むオファーをサブスクライブします。  
* PowerShell をインストールし、[Azure Stack で使うように構成します](azure-stack-powershell-configure-user.md)。

## <a name="key-vault-basics"></a>Key Vault の基礎

Azure Stack の Key Vault には、クラウド アプリやクラウド サービスで使用される暗号化キーとシークレットを保護する機能があります。 Key Vault を使用すると、キーやシークレットを暗号化することができます。その例を次に示します。

* 認証キー
* ストレージ アカウント キー
* データ暗号化キー
* .pfx ファイル
* パスワード

Key Vault は、キー管理プロセスを合理化し、データにアクセスして暗号化するキーの制御を維持できます。 開発者は、開発やテスト用のキーを数分で作成し、それらをシームレスに実稼働キーに移行できます。 セキュリティ管理者は、必要に応じて、キーに権限を付与する (取り消す) ことができます。

Azure Stack サブスクリプションを持つユーザーはだれでも、Key Vault を作成して使うことができます。 Key Vault は開発者とセキュリティ管理者にとってメリットがありますが、組織の他の Azure Stack サービスを管理するオペレーターが実装して管理できます。 たとえば、Azure Stack オペレーターは、Azure Stack サブスクリプションを使用してサインインし、キーを格納する組織のコンテナーを作成できます。 完了すると、次のことができるようになります。

* キーやシークレットの作成やインポート。
* キーやシークレットの取り消しや削除。
* ユーザーまたはアプリがキーとシークレットを管理または使用できるように、キー コンテナーへのアクセスを許可する。
* キーの使用状況の構成 (署名や暗号化など)。

オペレーターは、アプリから呼び出す Uniform Resource Identifier (URI) を開発者に提供できるようになります。 オペレーターは、セキュリティ管理者にキーの使用状況のログ情報を提供することもできます。

開発者は、API を使用してキーを直接管理することもできます。 詳細については、「[Key Vault 開発者ガイド](/azure/key-vault/key-vault-developers-guide)」を参照してください。

## <a name="scenarios"></a>シナリオ

次のシナリオでは、Key Vault が開発者やセキュリティ管理者のニーズを満たすのに役立つシナリオについて説明します。

### <a name="developer-for-an-azure-stack-app"></a>Azure Stack アプリの開発者

**問題**:キーを使って署名と暗号化を行う Azure Stack アプリを作成したい。 ソリューションが地理的に分散したアプリに合うように、これらのキーをアプリの外部に設定したい。

**説明:** キーはコンテナーに格納され、必要に応じて URI で呼び出されます。

### <a name="developer-for-software-as-a-service-saas"></a>サービスとしてのソフトウェア (SaaS) の開発者

**問題**:顧客のキーやシークレットに対して義務や潜在的責任を負いたくない。 顧客がキーを自分で所有して管理すれば、オペレーターは自分の専門分野である、中心的なソフトウェア機能を提供することに集中できます。

**説明:** 顧客は Azure Stack で独自のキーをインポートして管理できます。

### <a name="chief-security-officer-cso"></a>最高セキュリティ責任者 (CSO)

**問題**:組織が、キーのライフサイクルを管理し、キーの使用状況を確実に監視できるようにしたい。

**説明:** Key Vault は、Microsoft がキーを確認または抽出しないように作られています。 アプリが顧客のキーを使って暗号化操作を実行する必要がある場合は、Key Vault がアプリに代わってこれを行います。 アプリからは顧客のキーが見えません。 複数の Azure Stack サービスとリソースを使っていますが、Azure Stack の 1 つの場所からキーを管理したいと望んでいます。 コンテナーは、Azure Stack にあるコンテナーの数、サポートするリージョン、使うアプリに関係なく、1 つのインターフェイスを提供します。

## <a name="next-steps"></a>次の手順

* [ポータルを使って Azure Stack の Key Vault を管理する](azure-stack-key-vault-manage-portal.md)  
* [PowerShell を使用した Azure Stack での Key Vault の管理](azure-stack-key-vault-manage-powershell.md)
