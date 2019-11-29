---
title: Azure Stack に使用量と課金のためのテナントを追加する | Microsoft Docs
description: Azure Stack に使用量と課金のためのテナントを追加する方法について説明します。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 09/17/2019
ms.openlocfilehash: a146a99476912e97c72e7a37ffc5224158feaffc
ms.sourcegitcommit: 0b783e262ac87ae67929dbd4c366b19bf36740f0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74310148"
---
# <a name="add-tenant-for-usage-and-billing-to-azure-stack"></a>Azure Stack に使用量と課金のためのテナントを追加する

*適用対象: Azure Stack 統合システム*

この記事では、クラウド ソリューション プロバイダー (CSP) によって管理される Azure Stack デプロイにテナントを追加する方法について説明します。 新しいテナントによってリソースが使用されると、Azure Stack から CSP サブスクリプションに使用量が報告されます。

多くの場合、CSP は Azure Stack デプロイ上で複数のエンド カスタマー (テナント) にサービスを提供します。 Azure Stack の登録にテナントを追加すると、各テナントの使用量が報告され、対応する CSP サブスクリプションに課金されます。 この記事の手順を完了しなかった場合、テナントの使用量は、Azure Stack の初期登録で使用したサブスクリプションに課金されます。 使用量の追跡とテナントの管理のために Azure Stack にエンド カスタマーを追加する前に、Azure Stack を CSP として構成する必要があります。 手順とリソースについては、「[クラウド ソリューション プロバイダーとして Azure Stack の使用状況と課金を管理する](azure-stack-add-manage-billing-as-a-csp.md)」を参照してください。

次の図は、新しいエンド カスタマーが Azure Stack を使用できるようにし、また、そのカスタマーの使用量の追跡を設定するために、CSP が従う必要がある手順を示しています。 エンド カスタマーを追加すると、Azure Stack 内のリソースも管理できるようになります。 リソースの管理には 2 つのオプションがあります。

- エンド カスタマーを管理し、ローカルの Azure Stack サブスクリプション用の資格情報をエンド カスタマーに提供できます。  
- エンド カスタマーが自身のサブスクリプションをローカルで管理し、所有者アクセス許可を持つゲストとして CSP を追加できます。

## <a name="add-an-end-customer"></a>エンド カスタマーを追加する

エンド カスタマーを追加する前に、登録でマルチテナントの課金を有効にする必要があります。 マルチテナントの課金を有効にするには、登録サブスクリプション ID、リソース グループ名、および登録名を `azstcsp@microsoft.com` に送信します。 マルチテナントを有効にするには、通常 1 から 2 営業日かかります。

次の図に示すように、以下の手順を実行してエンド カスタマーを追加します。

![使用状況の追跡とエンド カスタマー アカウントの管理のためにクラウド ソリューション プロバイダーを設定する](media/azure-stack-csp-enable-billing-usage-tracking/process-csp-enable-billing.png)

### <a name="create-a-new-customer-in-partner-center"></a>パートナー センターに新しい顧客を作成する

パートナー センターで、顧客の新しい Azure サブスクリプションを作成します。 手順については、「[新しい顧客の追加](/partner-center/add-a-new-customer)」を参照してください。

### <a name="create-an-azure-subscription-for-the-end-customer"></a>エンド カスタマーの Azure サブスクリプションを作成する

パートナー センターで顧客のレコードを作成したら、その顧客にカタログ内の製品のサブスクリプションを販売できます。 手順については、「[顧客サブスクリプションの作成、停止、キャンセル](/partner-center/create-a-new-subscription)」を参照してください。

### <a name="create-a-guest-user-in-the-end-customer-directory"></a>エンド カスタマー ディレクトリにゲスト ユーザーを作成する

既定では、CSP (あなた) はエンド カスタマーの Azure Stack サブスクリプションにアクセスできません。 ただし、顧客があなたにリソースの管理を望む場合、顧客はあなたのアカウントを所有者/共同作成者として自分の Azure Stack サブスクリプションに追加できます。 これを行うには、Azure AD テナントにゲスト ユーザーとしてご自身のアカウントを追加する必要があります。 Azure CSP アカウントとは別のアカウントを使用して顧客の Azure Stack サブスクリプションを管理し、顧客の Azure サブスクリプションへのアクセスが失われないようにすることをお勧めします。

### <a name="update-the-registration-with-the-end-customer-subscription"></a>エンド カスタマーのサブスクリプションで登録を更新する

エンド カスタマーの新しいサブスクリプションで登録を更新します。 Azure は、パートナー センターの顧客 ID を使用して顧客の使用量をレポートします。 この手順により、各顧客の使用量が、その顧客それぞれの CSP サブスクリプションごとに報告されるようになります。 これで、使用量の追跡と課金が簡単になります。 この手順を実行するためには、まず [Azure Stack に登録する](azure-stack-registration.md)必要があります。

1. 管理者特権のプロンプトで Windows PowerShell を開き、次のコマンドを実行します。  

   ```powershell
   Add-AzureRmAccount
   ```

   >[!Note]
   > ご自分のセッションの期限が切れたり、ご自分のパスワードが変更になったり、または単純にアカウントを切り替えたい場合は、サインイン前に次のコマンドレットを Add-AzureRmAccount: `Remove-AzureRmAccount-Scope Process` を使用して実行します。

2. Azure の資格情報を入力します。
3. PowerShell セッションで、次のコマンドを実行します。

   ```powershell
   New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
   ```

### <a name="new-azurermresource-powershell-parameters"></a>PowerShell の New-AzureRmResource パラメーター

次のセクションでは、**New-AzureRmResource** コマンドレットのパラメーターについて説明します。

| パラメーター | 説明 |
| --- | --- |
|registrationSubscriptionID | Azure Stack の初期登録に使用された Azure サブスクリプション。|
| customerSubscriptionID | 登録される顧客の Azure サブスクリプション (Azure Stack ではありません)。 CSP のオファー内で作成する必要があります。 実際には、パートナー センターを介することを意味します。 顧客が複数の Azure Active Directory テナントを持っている場合は、Azure Stack へのログインに使用されるテナントでこのサブスクリプションを作成する必要があります。 顧客サブスクリプション ID には、小文字を使用する必要があります。 |
| resourceGroup | 登録が格納されている Azure 内のリソース グループ。 |
| registrationName | Azure Stack の登録名。 Azure に格納されているオブジェクトです。 

> [!NOTE]  
> テナントには、使用する各 Azure Stack を登録する必要があります。 Azure Stack のデプロイが 2 つあり、テナントでその両方を使用する場合は、各デプロイの初期登録をテナントのサブスクリプションで更新する必要があります。

### <a name="onboard-tenant-to-azure-stack"></a>Azure Stack でテナントの利用を開始する

複数の Azure AD テナントのユーザーが Azure Stack のサービスを使用できるように Azure Stack を構成します。 手順については、「[Azure Stack でのマルチテナントの有効化](azure-stack-enable-multitenancy.md)」を参照してください。

### <a name="create-a-local-resource-in-the-end-customer-tenant-in-azure-stack"></a>Azure Stack 内のエンド カスタマーのテナントにローカル リソースを作成する

Azure Stack に新しいカスタマーを追加するか、エンド カスタマーのテナントで所有者特権を持つゲスト アカウントを有効化したら、そのテナントにリソースを作成できることを確認します。 たとえば、[Azure Stack ポータルで Windows 仮想マシンを作成する](../user/azure-stack-quick-windows-portal.md)ことができます。

## <a name="next-steps"></a>次の手順

- 登録プロセス中にエラーが発生した場合にエラー メッセージを確認する方法については、[テナント登録時のエラー メッセージ](azure-stack-registration-errors.md)に関するページを参照してください。
- Azure Stack からリソース使用量情報を取得する方法ついて詳しくは、「[Azure Stack での使用量と請求](azure-stack-billing-and-chargeback.md)」をご覧ください。
- エンド カスタマーが CSP を Azure Stack テナントの管理者として追加する方法を確認するには、[クラウド ソリューション プロバイダーが Azure Stack サブスクリプションを管理できるようにする](../user/azure-stack-csp-enable-billing-usage-tracking.md)方法に関するページを参照してください。
