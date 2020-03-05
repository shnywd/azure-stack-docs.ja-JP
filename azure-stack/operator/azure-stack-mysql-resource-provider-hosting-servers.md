---
title: Azure Stack Hub に MySQL ホスティング サーバーを追加する
description: MySQL アダプター リソース プロバイダーを使用したプロビジョニングのために MySQL ホスティング サーバーを追加する方法について説明します。
author: bryanla
ms.topic: article
ms.date: 11/06/2019
ms.author: bryanla
ms.reviewer: xiaofmao
ms.lastreviewed: 11/06/2019
ms.openlocfilehash: a0b0e654ac2698c1b9611924cca853bd1ff525de
ms.sourcegitcommit: 4ac711ec37c6653c71b126d09c1f93ec4215a489
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77698761"
---
# <a name="add-mysql-hosting-servers-in-azure-stack-hub"></a>Azure Stack Hub に MySQL ホスティング サーバーを追加する

MySQL リソース プロバイダーが SQL インスタンスに接続できる限り、その MySQL ホスティング サーバー インスタンスを、[Azure Stack Hub](azure-stack-overview.md) 内の仮想マシン (VM) またはご利用の Azure Stack Hub 環境の外部にある VM でホストできます。

> [!NOTE]
> MySQL リソース プロバイダーは既定のプロバイダー サブスクリプションに作成する必要がありますが、MySQL ホスティング サーバーは課金対象のユーザー サブスクリプションに作成する必要があります。 リソース プロバイダー サーバーは、ユーザー データベースをホストするためには使用しないでください。

ホスティング サーバーには、MySQL バージョン 5.6、5.7、および 8.0 を使用できます。 MySQL RP では、caching_sha2_password 認証はサポートされません。これは、次のリリースで追加される予定です。 mysql_native_password を使用するには、MySQL 8.0 サーバーを構成する必要があります。 MariaDB もサポートされています。

## <a name="configure-external-access-to-the-mysql-hosting-server"></a>MySQL ホスティング サーバーへの外部アクセスを構成する

MySQL サーバーを Azure Stack Hub MySQL Server ホストとして追加する前に、外部アクセスを有効にする必要があります。 Azure Stack Hub マーケットプレースで入手できる BitNami MySQL を例にとると、次の手順を実行して外部アクセスを構成できます。

1. SSH クライアントを使用して (この例では [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) を使用)、パブリック IP にアクセスできるコンピューターから MySQL サーバーにログインします。

    パブリック IP を使用して、ユーザー名 **bitnami** と、前に作成した特殊文字を含まないアプリケーション パスワードを入力して、VM にログインします。

   ![LinuxLogin](media/azure-stack-tutorial-mysqlrp/bitnami1.png)

2. SSH クライアント ウィンドウで、次のコマンドを使用して、bitnami サービスがアクティブで実行中であることを確認します。 プロンプトが表示されたら、もう一度 bitnami パスワードを入力します。

   `sudo service bitnami status`

   ![bitnami サービスを確認する](media/azure-stack-tutorial-mysqlrp/bitnami2.png)

3. Azure Stack Hub MySQL ホスティング サーバーが MySQL に接続し、SSH クライアントを終了するために使用するリモート アクセス ユーザー アカウントを作成します。

    前に作成したルート パスワードを使用して、次のコマンドを実行して MySQL に root としてログインします。 新しい管理者ユーザーを作成し、環境に合わせて *\<username\>* および *\<password\>* を置き換えます。 この例では、作成されたユーザーの名前は **sqlsa** であり、強力なパスワードが使用されています。

   ```mysql
   mysql -u root -p
   create user <username>@'%' identified by '<password>';
   grant all privileges on *.* to <username>@'%' with grant option;
   flush privileges;
   ```

   ![管理者ユーザーを作成する](media/azure-stack-tutorial-mysqlrp/bitnami3.png)

4. 新しい MySQL ユーザー情報をメモします。

このユーザー名とパスワードは、Azure Stack Hub オペレーターがこの MySQL サーバーを使用して MySQL ホスティング サーバーを作成するときに使用されます。

## <a name="connect-to-a-mysql-hosting-server"></a>MySQL ホスティング サーバーに接続する

システム管理特権を持つアカウントの資格情報があることを確認してください。

> [!NOTE]
> MySQL 8.0 以降のバージョンでは、リモート アクセスは既定では有効になっていません。 ホスティング サーバーとして追加する前に、新しいユーザー アカウントを作成し、このユーザー アカウントにリモート アクセスの特権を付与する必要があります。

ホスティング サーバーを追加するには、次の手順に従います。

1. Azure Stack Hub 管理ポータルにサービス管理者としてサインインします。
2. **[すべてのサービス]** を選択します。
3. **[管理リソース]** カテゴリで **[MySQL ホスティング サーバー]**  >  **[+追加]** を選択します。 次の画面キャプチャに示される **[MySQL ホスティング サーバーの追加]** ダイアログが開きます。

   ![MySQL ホスティング サーバーを構成する](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

4. MySQL サーバー インスタンスの接続詳細を指定します。

   * **[MySQL Hosting Server Name]\(MySQL ホスティング サーバー名\)** では、完全修飾ドメイン名 (FQDN) または有効な IPv4 アドレスを指定します。 短い VM 名は使用しないでください。
   * Azure Stack Hub Marketplace で利用可能な Bitnami MySQL イメージ用の既定の管理者**ユーザー名**は *root* です。
   * root の**パスワード**がわからない場合は、[Bitnami のドキュメント](https://docs.bitnami.com/azure/faq/#how-to-find-application-credentials)で取得方法を確認してください。
   * 既定の MySQL インスタンスが指定されていないため、 **[ホスティング サーバーのサイズ (GB)]** を指定する必要があります。 データベース サーバーの容量に近いサイズを入力します。
   * **[サブスクリプション]** の既定の設定のままにします。
   * **[リソース グループ]** で、新しいリソース グループを作成するか、既存のグループを使用します。

   > [!NOTE]
   > テナントと管理者の Azure Resource Manager から MySQL インスタンスにアクセスできる場合は、そのインスタンスをリソース プロバイダーの管理下に置くことができます。 ただし、MySQL インスタンスは、リソース プロバイダーに排他的に割り当てる**必要があります**。

5. **[SKU]** を選択して **[Create SKU]\(SKU の作成\)** ダイアログを開きます。

   ![MySQL SKU を作成する](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)

   ユーザーが自分のデータベースを適切な SKU にデプロイできるように、SKU の **[名前]** には SKU のプロパティが反映される必要があります。

6. **[OK]** を選択して SKU を作成します。
   > [!NOTE]
   > SKU はポータルに表示されるまで最大 1 時間かかることがあります。 SKU がデプロイされて実行されるまで、データベースを作成することはできません。

7. **[Add a MySQL Hosting Server]\(MySQL ホスティング サーバーの追加\)** の下で、 **[作成]** を選択します。

サーバーを追加する際に、サービス内容を区別するために新規または既存の SKU に割り当てます。 たとえば、増加したデータベースと自動バックアップを提供する MySQL エンタープライズ インスタンスを使用できます。 この高パフォーマンス サーバーは、組織内のさまざまな部門用に予約できます。

## <a name="security-considerations-for-mysql"></a>MySQL のセキュリティに関する考慮事項

次の情報は、RP および MySQL ホスティング サーバーに適用されます。

* すべてのホスティング サーバーが TLS 1.1 を使用した通信用に構成されていることを確認します。 [暗号化された接続を使用するための MySQL の構成](https://dev.mysql.com/doc/refman/5.7/en/using-encrypted-connections.html)に関するページを参照してください。
* [Transparent Data Encryption](https://dev.mysql.com/doc/mysql-secure-deployment-guide/5.7/en/secure-deployment-data-encryption.html) を採用します。
* MySQL RP では、caching_sha2_password 認証はサポートされていません。

## <a name="increase-backend-database-capacity"></a>バックエンド データベース容量を増やす

バックエンド データベース容量を増加するには、Azure Stack Hub ポータルで追加で MySQL サーバーをデプロイします。 これらのサーバーを新規または既存の SKU に追加します。 サーバーを既存の SKU に追加する場合は、サーバーの特性が SKU 内の他のサーバーと同じであることを確認してください。

## <a name="sku-notes"></a>SKU に関する注意
使用する SKU 名は、SKU 内のサーバーの機能 (容量やパフォーマンス) を示すものにしてください。 この名前は、該当する SKU にユーザーがデータベースをデプロイするときの助けになります。 たとえば、SKU 名を使用して、サービス内容を次の特性によって区別することもできます。
  
* 大容量
* ハイ パフォーマンス
* 高可用性

ベスト プラクティスとして、SKU 内のすべてのホスティング サーバーに同じリソースとパフォーマンス特性を持たせてください。

SKU は、特定のユーザーやグループに割り当てることはできません。

SKU を編集するには、 **[すべてのサービス]**  >  **[MySQL アダプター]**  >  **[SKU]** に移動します。 変更する SKU を選択し、必要な変更を加えた後、 **[保存]** をクリックして変更を保存します。 

不要になった SKU を削除するには、 **[すべてのサービス]**  >  **[MySQL アダプター]**  >  **[SKU]** に移動します。 SKU 名を右クリックし、 **[削除]** を選択して削除します。

> [!IMPORTANT]
> 新しい SKU がユーザー ポータルで使用可能になるまでに最大で 1 時間かかることがあります。

## <a name="make-mysql-database-servers-available-to-your-users"></a>ユーザーが MySQL データベース サーバーを使用できるようにする

ユーザーが MySQL データベース サーバーを使用できるようにプランとオファーを作成します。 プランに Microsoft.MySqlAdapter サービスを追加し、新しいクォータを作成します。 MySQL では、データベースのサイズの制限が許可されません。

> [!IMPORTANT]
> ユーザー ポータルで新しいクォータが使用可能になるまで、または変更されたクォータが適用されるまでに最大で 2 時間かかることがあります。

## <a name="next-steps"></a>次のステップ

[MySQL データベースの作成](azure-stack-mysql-resource-provider-databases.md)
