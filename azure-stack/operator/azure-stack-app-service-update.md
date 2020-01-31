---
title: Azure App Service on Azure Stack Hub を更新する
description: Azure App Service on Azure Stack Hub を更新する方法を説明します。
author: BryanLa
ms.topic: article
ms.date: 01/13/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 01/13/2019
ms.openlocfilehash: ec0b241fd1568fb6851e3ae4fad6c79a8dfeb201
ms.sourcegitcommit: fd5d217d3a8adeec2f04b74d4728e709a4a95790
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76877597"
---
# <a name="update-azure-app-service-on-azure-stack-hub"></a>Azure App Service on Azure Stack Hub を更新する

> [!IMPORTANT]
> Azure App Service 1.8 をデプロイする前に、Azure Stack Hub 統合システムに 1910 更新プログラムを適用するか、最新の Azure Stack Hub Development Kit (ASDK) をデプロイします。

この記事では、インターネットに接続された Azure Stack Hub 環境にデプロイされている [Azure App Service リソース プロバイダー](azure-stack-app-service-overview.md)をアップグレードする方法について説明します。

> [!IMPORTANT]
> アップグレードを実行する前に、[Azure App Service on Azure Stack Hub のデプロイ](azure-stack-app-service-deploy.md)を既に完了していることを確認してください。 さらに、新しい機能、修正、およびデプロイに影響を与える可能性のある既知の問題について確認できるように、1.8 リリースに付属する[リリース ノート](azure-stack-app-service-release-notes-update-eight.md)も参照してください。

## <a name="run-the-azure-app-service-resource-provider-installer"></a>Azure App Service リソース プロバイダーのインストーラーを実行する

このプロセス中に、アップグレードでは次のことを行います。

* Azure App Service の以前のデプロイを検出します。
* すべての更新プログラム パッケージとデプロイされるすべての OSS ライブラリの新しいバージョンを準備します。
* ストレージにアップロードします。
* すべての Azure App Service ロール (コントローラー、管理、フロントエンド、パブリッシャー、および worker ロール) をアップグレードする
* Azure App Service スケール セット定義を更新する
* Azure App Service リソース プロバイダー マニフェストを更新する

> [!IMPORTANT]
> Azure App Service インストーラーは、Azure Stack Hub 管理者の Azure Resource Manager エンドポイントにアクセスできるマシン上で実行する必要があります。

Azure App Service on Azure Stack Hub のデプロイをアップグレードするには、次の手順に従います。

1. [Azure App Service インストーラー](https://aka.ms/appsvcupdate8installer)をダウンロードします。

2. 管理者として appservice.exe を実行します。

    ![Azure App Service インストーラー][1]

3. **[Deploy Azure App Service or upgrade to the latest version]\(Azure App Service をデプロイするか、または最新バージョンにアップグレードする\)** をクリックします。

4. マイクロソフト ソフトウェア ライセンス条項を確認して同意し、 **[次へ]** をクリックします。

5. サード パーティのライセンス条項を確認して同意し、 **[次へ]** をクリックします。

6. Azure Stack Hub Azure Resource Manager エンドポイントと Active Directory テナントの情報が正しいことを確認します。 ASDK のデプロイ中に既定の設定を使用した場合は、ここで既定値をそのまま使用することもできます。 ただし、Azure Stack Hub のデプロイ時にオプションをカスタマイズした場合は、このウィンドウで値を編集する必要があります。 たとえば、ドメイン サフィックス *mycloud.com* を使用する場合は、Azure Stack Hub Azure Resource Manager エンドポイントを *management.region.mycloud.com* に変更する必要があります。 自分の情報を確認したら、 **[次へ]** をクリックします。

    ![Azure Stack Hub クラウドの情報][2]

7. 次のページで、次の操作を行います。

    1. 使用する接続方法 ( **[資格情報]** または **[サービス プリンシパル]** ) を選択します。
        - **資格情報**
            - Azure Active Directory (Azure AD) を使用している場合は、Azure Stack Hub のデプロイ時に指定した、Azure AD の管理者アカウントとパスワードを入力します。 **[接続]** を選択します。
            - Active Directory フェデレーション サービス (AD FS) を使用している場合は、ご自分の管理者アカウントを指定します。 たとえば、「 cloudadmin@azurestack.local 」のように入力します。 パスワードを入力し、 **[接続]** を選択します。
        - **サービス プリンシパル**
            - 使用するサービス プリンシパルには、 **[Default Provider Subscription]\(既定のプロバイダー サブスクリプション\)** に対する **[所有者]** 権限が**必要です**
            - **[サービス プリンシパル ID]** 、 **[証明書ファイル]** 、および **[パスワード]** を指定し、 **[接続]** を選択します。

    1. **[Azure Stack Hub Subscriptions]\(Azure Stack Hub サブスクリプション\)** で、 **[Default Provider Subscription]\(既定のプロバイダー サブスクリプション\)** を選択します。    Azure App Service on Azure Stack Hub は、 **[Default Provider Subscription]\(既定のプロバイダー サブスクリプション\)** にデプロイする**必要**があります。

    1. **[Azure Stack Hub Locations]\(Azure Stack Hub の場所\)** で、デプロイしているリージョンに対応する場所を選択します。 たとえば、ASDK にデプロイしている場合は、 **[ローカル]** を選びます。

    1. 既存の Azure App Service デプロイが検出された場合は、リソース グループとストレージ アカウントにデータが入力されており、使用できません。

      ![検出された Azure App Service インストール][3]

8. 概要ページで、次のことを行います。
   1. 選択した内容を確認します。 変更を加えるには、 **[前へ]** を使って前のページに戻ります。
   2. 構成が正しい場合は、チェック ボックスをオンにします。
   3. アップグレードを開始するには、 **[次へ]** をクリックします。

       ![Azure App Service アップグレードの概要][4]

9. アップグレードの進行状況ページ:
    1. アップグレードの進行状況を追跡します。 Azure App Service on Azure Stack Hub のアップグレードの期間は、デプロイされるロール インスタンスの数によって異なります。
    2. アップグレードが正常に完了したら、 **[終了]** をクリックします。

        ![Azure App Service アップグレードの進行状況][5]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update/app-service-exe.png
[2]: ./media/azure-stack-app-service-update/app-service-azure-resource-manager-endpoints.png
[3]: ./media/azure-stack-app-service-update/app-service-installation-detected.png
[4]: ./media/azure-stack-app-service-update/app-service-upgrade-summary.png
[5]: ./media/azure-stack-app-service-update/app-service-upgrade-complete.png

## <a name="next-steps"></a>次のステップ

Azure App Service on Azure Stack Hub のための追加の管理者操作を準備します。

* [追加容量の計画を立てる](azure-stack-app-service-capacity-planning.md)
* [追加容量を追加する](azure-stack-app-service-add-worker-roles.md)
