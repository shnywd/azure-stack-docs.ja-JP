---
title: クラウド ID を検索する
description: Azure Stack Hub の [ヘルプとサポート] でクラウド ID を検索する方法について説明します。
author: justinha
ms.topic: article
ms.date: 10/08/2019
ms.author: justinha
ms.reviewer: shisab
ms.lastreviewed: 10/08/2019
ms.openlocfilehash: 09b22541ed77915a2a28c4abf5212ccbbbf5fb81
ms.sourcegitcommit: e9a1dfa871e525f1d6d2b355b4bbc9bae11720d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/20/2020
ms.locfileid: "86488962"
---
# <a name="find-your-cloud-id"></a>クラウド ID を検索する

このトピックでは、管理者ポータルまたは特権エンドポイント (PEP) を使用して、クラウド ID を取得する方法について説明します。 

## <a name="use-the-administrator-portal"></a>管理者ポータルを使用する

1. 管理者ポータルを開きます。 
1. **[リージョンの管理]** をクリックします。

   ![ダッシュボードのスクリーンショット](./media/azure-stack-automatic-log-collection/dashboard.png)

1. **[プロパティ]** をクリックし、 **[Stamp Cloud ID]\(スタンプ クラウド ID\)** をコピーします。

   ![スタンプ クラウド ID を示すリージョン プロパティのスクリーンショット](media/azure-stack-automatic-log-collection/region-properties-blade-with-stamp-cloud-id.png)


## <a name="use-the-privileged-endpoint"></a>特権エンドポイントを使用する

1. 管理者特権の PowerShell セッションを開き、次のスクリプトを実行します。 必要に応じて、お使いの環境に合わせて PEP VM の IP アドレスとクラウド管理者の資格情報を置き換えます。 

   ```powershell
   $ipAddress = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.

   $password = ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
   $cred = New-Object -TypeName System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $password)
   $session = New-PSSession -ComputerName $ipAddress -ConfigurationName PrivilegedEndpoint -Credential $cred

   $stampInfo = Invoke-Command -Session $session { Get-AzureStackStampInformation }
   if ($session) {
       Remove-PSSession -Session $session
   }

   $stampInfo.CloudID
   ```

## <a name="next-steps"></a>次のステップ

* [ログを事前に送信する](./azure-stack-configure-automatic-diagnostic-log-collection.md?view=azs-2002)
* [今すぐログを送信する](./azure-stack-configure-on-demand-diagnostic-log-collection-portal.md?view=azs-2002)
