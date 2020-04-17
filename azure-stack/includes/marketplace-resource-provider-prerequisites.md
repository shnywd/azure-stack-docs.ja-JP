---
author: BryanLa
ms.author: bryanla
ms.service: azure-stack
ms.topic: include
ms.date: 12/09/2019
ms.reviewer: bryanla
ms.lastreviewed: 12/09/2019
ms.openlocfilehash: e1e2c3c6c3d1149b2cd034c2f5e4abfdf26e712f
ms.sourcegitcommit: a630894e5a38666c24e7be350f4691ffce81ab81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "80424609"
---
既に Azure Stack Hub Marketplace からリソース プロバイダーをインストールしている場合は、一般的な前提条件を満たしていると考えられるため、このセクションは省略してもかまいません。 それ以外の場合は、まず次の前提条件を満たしておく必要があります。 

1. まだの場合は、[Azure Stack Hub インスタンスを Azure に登録します](../operator/azure-stack-registration.md)。 この手順は、Azure から Marketplace に接続して項目をダウンロードする際に必要になります。

2. Azure Stack Hub 管理者ポータルの **Marketplace Management** 機能に慣れていない場合は、[Azure から Marketplace 項目をダウンロードして Azure Stack Hub に発行する方法](../operator/azure-stack-download-azure-marketplace-item.md)に関するページを確認してください。 この記事では、Azure から Azure Stack Hub Marketplace に項目をダウンロードする手順について説明しています。 接続されている場合と接続されていない場合、両方のシナリオについての説明があります。 Azure Stack Hub インスタンスが切断されているか、部分的にしか接続されていない場合は、インストールの準備として追加の前提条件を満たす必要があります。

3. Azure Active Directory (Azure AD) ホーム ディレクトリを更新します。 ビルド 1910 以降では、新しいデプロイ リソース プロバイダー (DRP) アプリケーションを使用してホーム ディレクトリ テナントを登録する必要があります。 このアプリを使用すると、DRP でリソース プロバイダーを正常に作成して登録できます。 この手順を完了しないと、リソース プロバイダーのインストールが失敗します。 

   - Azure Stack Hub インスタンスが 1910 以上に正常に更新されたら、[Azure Stack Hub Tools リポジトリの複製またはダウンロードに関する手順](../operator/azure-stack-powershell-download.md)に従います。 
   - 次に、[(リソース プロバイダーの更新プログラムまたは新しいリソース プロバイダーのインストール後の) Azure Stack Hub Azure AD ホーム ディレクトリの更新](https://github.com/Azure/AzureStack-Tools/tree/master/Identity#updating-the-azure-stack-aad-home-directory-after-installing-updates-or-new-resource-providers)に関するページの指示に従います。 