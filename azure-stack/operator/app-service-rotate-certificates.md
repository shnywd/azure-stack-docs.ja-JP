---
title: App Service on Azure Stack Hub のシークレットと証明書を交換する
description: Azure App Service on Azure Stack Hub で使用されるシークレットと証明書を交換する方法を学習します
author: BryanLa
ms.topic: article
ms.date: 01/10/2020
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 01/10/2020
ms.openlocfilehash: f1a42f5b04ea83d9ff9130fb63ba6833cd7d2914
ms.sourcegitcommit: fd5d217d3a8adeec2f04b74d4728e709a4a95790
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76876543"
---
# <a name="rotate-app-service-on-azure-stack-hub-secrets-and-certificates"></a>App Service on Azure Stack Hub のシークレットと証明書を交換する

これらの手順は、Azure App Service on Azure Stack Hub にのみ適用されます。  Azure App Service on Azure Stack Hub のシークレットの交換は、Azure Stack Hub 用の一元化されたシークレットの交換手順には含まれていません。  オペレーターは、システム内のシークレットの有効性、最後に更新された日付、およびシークレットが期限切れになるまでの残り時間を監視できます。

> [!Important]
> Azure App Service on Azure Stack Hub は Azure Stack Hub アラート サービスと統合されていないため、オペレーターが、Azure Stack Hub ダッシュボードでシークレットの有効期限に関するアラートを受け取ることはありません。  オペレーターは、Azure Stack Hub 管理者ポータルで Azure App Service on Azure Stack Hub 管理エクスペリエンスを使用して、シークレットを定期的に監視する必要があります。

このドキュメントには、次のシークレットの交換手順が含まれています。

* Azure App Service on Azure Stack Hub 内で使用される暗号化キー
* ホスティングおよび計測データベースを操作するために、Azure App Service on Azure Stack Hub で使用されるデータベース接続資格情報
* エンドポイントをセキュリティで保護するために Azure App Service on Azure Stack Hub で使用される証明書
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

## <a name="rotate-system-credentials"></a>システム資格情報を交換する

Azure App Service on Azure Stack Hub 内で使用されるシステム資格情報を交換するには、次の手順を行います。

1. Azure Stack Hub 管理者ポータルで App Service 管理エクスペリエンスにアクセスします。

1. **[シークレット]** メニュー オプションに移動します

1. [システム資格情報] セクションの **[Rotate]\(交換\)** ボタンをクリックします

1. 交換するシステム資格情報の **[スコープ]** を選択します。  オペレーターは、すべてのロールまたは個々のロールのシステム資格情報を交換することを選択できます。

1. **ローカル管理者のユーザー名**、新しい**パスワード**を指定し、**パスワード**を確認して **[OK]** をクリックします

1. 資格情報は、必要に応じて、対応する Azure App Service on Azure Stack Hub ロール インスタンス全体で交換されます。  オペレーターは、 **[状態]** ボタンを使用して、手順の状態を監視できます。



