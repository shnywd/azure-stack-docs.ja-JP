---
title: 'Azure App Service をオフラインで更新する '
description: Azure App Service on Azure Stack Hub をオフラインで更新するための詳細なガイダンス
author: bryanla
ms.topic: article
ms.date: 01/13/2020
ms.author: anwestg
ms.reviewer: anwe
ms.openlocfilehash: 65fb8a0adebb73fa8740f41d50eb83f869ac534a
ms.sourcegitcommit: a630894e5a38666c24e7be350f4691ffce81ab81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "77700801"
---
# <a name="offline-update-of-azure-app-service-on-azure-stack-hub"></a>Azure App Service on Azure Stack Hub のオフライン更新

> [!IMPORTANT]
> Azure App Service 1.8 をデプロイする前に、Azure Stack Hub 統合システムに 1910 以降の更新プログラムを適用するか、最新の Azure Stack Development Kit をデプロイします。

この記事の手順に従うことによって、次のような Azure Stack Hub 環境にデプロイされた [Azure App Service リソース プロバイダー](azure-stack-app-service-overview.md)をアップグレードできます。

* インターネットに接続されていない
* Active Directory フェデレーション サービス (AD FS) によって保護されている

> [!IMPORTANT]
> アップグレードを実行する前に、[Azure App Service on Azure Stack Hub リソース プロバイダーのデプロイ](azure-stack-app-service-deploy-offline.md)を既に完了していること、および 1.8 リリースに付随している[リリース ノート](azure-stack-app-service-release-notes-update-eight.md)を読んで、新しい機能、修正、およびデプロイに影響を与える可能性のある既知の問題を把握していることを確認してください。

## <a name="run-the-app-service-resource-provider-installer"></a>App Service リソースプロバイダーのインストーラーを実行する

Azure Stack Hub 環境内の App Service リソース プロバイダーをアップグレードするには、次のタスクを完了する必要があります。

1. [Azure App Service インストーラー](https://aka.ms/appsvcupdate8installer)をダウンロードします。
2. オフライン アップグレード パッケージを作成する。
3. App Service インストーラー (appservice.exe) を実行し、アップグレードを完了します。

このプロセス中に、アップグレードでは次のことを行います。

* App Service の以前のデプロイを検出する
* ストレージにアップロードする
* すべての App Service ロール (コントローラー、管理、フロントエンド、パブリッシャー、および worker ロール) をアップグレードする
* App Service スケール セット定義を更新する
* App Service リソース プロバイダー マニフェストを更新する

## <a name="create-an-offline-upgrade-package"></a>オフライン アップグレード パッケージを作成する

切断された環境で App Service をアップグレードするには、まずインターネットに接続されたマシン上でオフライン アップグレード パッケージを作成する必要があります。

1. 管理者として appservice.exe を実行する

    ![Azure App Service インストーラー][1]

2. **[詳細]**  >  **[Create offline package] (オフライン パッケージを作成する)** をクリックします。

    ![Azure App Service インストーラーの [詳細]][2]

3. Azure App Service インストーラーによってオフライン アップグレード パッケージが作成され、そのパスが表示されます。  **[フォルダーを開く]** をクリックすると、エクスプローラーでフォルダーが開きます。

4. インストーラー (AppService.exe) とオフライン アップグレード パッケージを Azure Stack Hub ホスト マシンにコピーします。

## <a name="complete-the-upgrade-of-app-service-on-azure-stack-hub"></a>App Service on Azure Stack Hub のアップグレードを完了する

> [!IMPORTANT]
> Azure App Service インストーラーは、Azure Stack Hub Administrator Azure Resource Manager エンドポイントに到達できるマシン上で実行する必要があります。

1. 管理者として appservice.exe を実行します。

    ![Azure App Service インストーラー][1]

2. **[詳細]**  >  **[Complete offline installation or upgrade] (オフライン インストールまたはアップグレードを完了する)** をクリックします。

    ![Azure App Service インストーラーの [詳細]][2]

3. 前に作成したオフライン アップグレード パッケージの場所を参照し、 **[次へ]** をクリックします。

4. マイクロソフト ソフトウェア ライセンス条項を確認して同意し、 **[次へ]** をクリックします。

5. サード パーティのライセンス条項を確認して同意し、 **[次へ]** をクリックします。

6. Azure Stack Hub Azure Resource Manager エンドポイントと Active Directory テナントの情報が正しいことを確認します。 Azure Stack Development Kit の展開中に既定の設定を使用した場合は、ここで既定値を受け入れることができます。 ただし、Azure Stack Hub のデプロイ時にオプションをカスタマイズした場合は、このウィンドウで値を編集する必要があります。 たとえば、ドメイン サフィックス *mycloud.com* を使用する場合は、Azure Stack Hub Azure Resource Manager エンドポイントを *management.region.mycloud.com* に変更する必要があります。 自分の情報を確認したら、 **[次へ]** をクリックします。

    ![Azure Stack Hub クラウドの情報][3]

7. 次のページで、次の操作を行います。

   1. 使用する接続方法 ( **[資格情報]** または **[サービス プリンシパル]** ) を選択します。
        - **資格情報**
            - Azure Active Directory (Azure AD) を使用している場合は、Azure Stack Hub のデプロイ時に指定した、Azure AD の管理者アカウントとパスワードを入力します。 **[接続]** を選択します。
            - Active Directory フェデレーション サービス (AD FS) を使用している場合は、ご自分の管理者アカウントを指定します。 たとえば、「 cloudadmin@azurestack.local 」のように入力します。 パスワードを入力し、 **[接続]** を選択します。
        - **サービス プリンシパル**
            - 使用するサービス プリンシパルには、 **[Default Provider Subscription]\(既定のプロバイダー サブスクリプション\)** に対する **[所有者]** 権限が**必要です**
            - **[サービス プリンシパル ID]** 、 **[証明書ファイル]** 、および **[パスワード]** を指定し、 **[接続]** を選択します。

   1. **[Azure Stack Hub Subscriptions]\(Azure Stack Hub サブスクリプション\)** で、 **[Default Provider Subscription]\(既定のプロバイダー サブスクリプション\)** を選択します。  Azure App Service on Azure Stack Hub は、 **[Default Provider Subscription]\(既定のプロバイダー サブスクリプション\)** にデプロイする**必要**があります。

   1. **[Azure Stack Hub Locations]\(Azure Stack Hub の場所\)** で、デプロイしているリージョンに対応する場所を選択します。 たとえば、ASDK にデプロイしている場合は、 **[ローカル]** を選びます。
   
   1. 既存の App Service デプロイが検出された場合は、リソース グループとストレージ アカウントにデータが入力され、灰色表示されます。

      ![検出された Azure App Service インストール][4]
8. 概要ページで、次のことを行います。
   1. 選択した内容を確認します。 変更を加えるには、 **[前へ]** を使って前のページに戻ります。
   2. 構成が正しい場合は、チェック ボックスをオンにします。
   3. アップグレードを開始するには、 **[次へ]** をクリックします。

       ![Azure App Service アップグレードの概要][5]

9. アップグレードの進行状況ページ:
    1. アップグレードの進行状況を追跡します。 App Service on Azure Stack Hub のアップグレードの期間は、デプロイされるロール インスタンスの数によって異なります。
    2. アップグレードが正常に完了したら、 **[終了]** をクリックします。

        ![Azure App Service アップグレードの進行状況][6]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update-offline/app-service-exe.png
[2]: ./media/azure-stack-app-service-update-offline/app-service-exe-advanced.png
[3]: ./media/azure-stack-app-service-update-offline/app-service-azure-resource-manager-endpoints.png
[4]: ./media/azure-stack-app-service-update-offline/app-service-installation-detected.png
[5]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-summary.png
[6]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-complete.png

## <a name="next-steps"></a>次のステップ

Azure App Service on Azure Stack Hub のための追加の管理者操作を準備します

* [追加容量の計画を立てる](azure-stack-app-service-capacity-planning.md)
* [追加容量を追加する](azure-stack-app-service-add-worker-roles.md)
