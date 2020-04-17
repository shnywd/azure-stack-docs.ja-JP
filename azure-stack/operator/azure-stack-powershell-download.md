---
title: GitHub からの Azure Stack Hub ツールのダウンロード
description: Azure Stack Hub の操作に必要なツールをダウンロードする方法について説明します。
author: mattbriggs
ms.topic: article
ms.date: 1/22/2020
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 09/19/2019
ms.openlocfilehash: 8d2f79d85055ef15f7dc0af3e1b36434f9c63d79
ms.sourcegitcommit: a630894e5a38666c24e7be350f4691ffce81ab81
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "77698285"
---
# <a name="download-azure-stack-hub-tools-from-github"></a>GitHub からの Azure Stack Hub ツールのダウンロード

**AzureStack-Tools** は PowerShell モジュールをホストする [GitHub リポジトリ](https://github.com/Azure/AzureStack-Tools)で、リソースの管理と Azure Stack Hub へのデプロイに使用できます。 VPN 接続を確立する予定の場合は、これらの PowerShell モジュールを、Azure Stack Development Kit (ASDK) または Windows ベースの外部クライアントにダウンロードできます。 これらのツールを取得するには、GitHub リポジトリを複製するか、次のスクリプトを実行して **AzureStack-Tools** フォルダーをダウンロードします。

```powershell
# Change directory to the root directory.
cd \

# Download the tools archive.
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files.
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory.
cd AzureStack-Tools-master

```

## <a name="functionality-provided-by-the-modules"></a>モジュールで提供される機能

**AzureStack-Tools** リポジトリには、Azure Stack Hub の次の機能をサポートする PowerShell モジュールが含まれています。  

| 機能 | 説明 | このモジュールを使用できるユーザー |
| --- | --- | --- |
| [クラウド機能](../user/azure-stack-validate-templates.md) | このモジュールを使用すると、クラウドのクラウド機能を取得できます。 たとえば、API バージョンや Azure Resource Manager リソースなどのクラウド機能を取得できます。 Azure Stack Hub と Azure クラウドの VM 拡張機能を取得することもできます。 | クラウド オペレーターとユーザー |
| [Azure Stack Hub の Resource Manager ポリシー](../user/azure-stack-policy-module.md) | このモジュールを使用すると、Azure Stack Hub と同じバージョン管理とサービス可用性で、Azure サブスクリプションまたは Azure リソース グループを構成できます。 | クラウド オペレーターとユーザー |
| [Azure への登録](azure-stack-registration.md ) | このモジュールを使用して、ASDK インスタンスを Azure に登録します。 登録後、Azure Marketplace アイテムをダウンロードして Azure Stack Hub で使用できます。 | クラウド オペレーター |
| [Azure Stack Hub のデプロイ](../asdk/asdk-install.md) | このモジュールを使用すると、Azure Stack Hub の仮想ハード ディスク (VHD) イメージを使用して、Azure Stack Hub のホスト コンピューターでデプロイおよび再デプロイを行う準備ができます。 | クラウド オペレーター|
| [Azure Stack Hub への接続](azure-stack-powershell-install.md) | このモジュールを使用すると、Azure Stack Hub への VPN 接続を構成できます。 | クラウド オペレーターとユーザー |
| [テンプレートの検証](../user/azure-stack-validate-templates.md) | このモジュールを使用すると、既存のまたは新しいテンプレートを Azure Stack Hub にデプロイできるかどうかを確認できます。 | クラウド オペレーターとユーザー|

## <a name="next-steps"></a>次のステップ

- [Azure Stack Hub の PowerShell の概要](../user/azure-stack-powershell-overview.md)
- [Azure Stack Hub ユーザーの PowerShell 環境の構成](../user/azure-stack-powershell-configure-user.md)
- [VPN 経由で Azure Stack Development Kit に接続する](../asdk/asdk-connect.md)
