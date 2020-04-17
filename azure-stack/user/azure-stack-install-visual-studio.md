---
title: Visual Studio をインストールして Azure Stack Hub に接続する
description: Visual Studio をインストールして Azure Stack Hub に接続する方法について説明します。
author: sethmanheim
ms.topic: article
ms.date: 01/07/2020
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 01/04/2020
ms.openlocfilehash: 9fb0cf281fb97bc5cf255fb39507869b106d0a1b
ms.sourcegitcommit: a630894e5a38666c24e7be350f4691ffce81ab81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "77702960"
---
# <a name="install-visual-studio-and-connect-to-azure-stack-hub"></a>Visual Studio をインストールして Azure Stack Hub に接続する

Visual Studio を使用して Azure Resource Manager [テンプレート](azure-stack-arm-templates.md)を作成し、Azure Stack Hub にデプロイできます。 この記事では、Visual Studio を [Azure Stack Hub](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp) または外部コンピューター ([VPN](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn) を介して Azure Stack Hub を使用する予定の場合) にインストールする手順について説明します。

## <a name="install-visual-studio"></a>Visual Studio のインストール

1. [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) をダウンロードして実行します。  

2. **Microsoft Web Platform Installer** を開きます。

3. **Visual Studio Community 2015 with Microsoft Azure SDK - 2.9.6** を検索します。 **[追加]** をクリックし、 **[インストール]** をクリックします。

4. Azure SDK の一部としてインストールされている **Microsoft Azure PowerShell** をアンインストールします。

    ![WebPI のインストール手順を示すスクリーンショット](./media/azure-stack-install-visual-studio/image1.png)

5. [PowerShell for Azure Stack Hub をインストールします](../operator/azure-stack-powershell-install.md)。

6. インストールが完了したら、オペレーティング システムを再起動します。

## <a name="connect-to-azure-stack-hub-with-azure-ad"></a>Azure AD を使用して Azure Stack Hub に接続する

1. Visual Studio を起動します。

2. **[表示]** メニューの **[Cloud Explorer]** を選択します。

3. 新しいウィンドウで **[アカウントの追加]** を選択し、Azure Active Directory (Azure AD) の資格情報を使ってサインインします。  

    ![ログインして Azure Stack Hub に接続した後の Cloud Explorer のスクリーンショット](./media/azure-stack-install-visual-studio/image2.png)

ログインしたら、[テンプレートをデプロイ](azure-stack-deploy-template-visual-studio.md)するか、使用可能なリソースの種類やリソース グループを参照して独自のテンプレートを作成することができます。  

## <a name="connect-to-azure-stack-hub-with-ad-fs"></a>AD FS を使用して Azure Stack Hub に接続する

1. Visual Studio を起動します。

2. **[ツール]** から **[オプション]** を選択します。

3. **ナビゲーション ウィンドウ**の **[環境]** を展開し、 **[アカウント]** を選択します。

4. **[追加]** を選択し、User Azure Resource Manger エンドポイントを入力します。 Azure Stack Development Kit (ASDK) の URL は `https://management.local.azurestack/external` です。  Azure Stack Hub 統合システムの URL は `https://management.[Region}.[External FQDN]` です。

    ![新しい Azure Cloud 検出エンドポイントを追加する](./media/azure-stack-install-visual-studio/image5.png)

5. **[追加]** を選択します。  

    Visual Studio は Azure Resource Manger を呼び出し、Azure Directory Federated Services (AD FS) の認証エンドポイントを含むエンドポイントを検出します。

    ![ログインして Azure Stack Hub に接続した後の Cloud Explorer のスクリーンショット](./media/azure-stack-install-visual-studio/image6.png)

6. **[表示]** メニューの **[Cloud Explorer]** を選択します。

7. **[アカウントの追加]** を選択し、AD FS の資格情報でサインインします。  

    ![Cloud Explorer で Visual Studio にサインインする](./media/azure-stack-install-visual-studio/image7.png)

    Cloud Explorer は使用できるサブスクリプションのクエリを実行します。 使用できるサブスクリプションを選択して管理できます。

    ![Cloud Explorer で管理するサブスクリプションを選択する](./media/azure-stack-install-visual-studio/image8.png)

8. 既存のリソース、リソース グループ、またはデプロイ テンプレートを参照します。

## <a name="next-steps"></a>次のステップ

- 他の Visual Studio バージョンとの[サイド バイ サイド構成](/visualstudio/install/install-visual-studio-versions-side-by-side)に関する記事を参照してください。
- [Azure Stack Hub のテンプレートを開発します](azure-stack-develop-templates.md)。
