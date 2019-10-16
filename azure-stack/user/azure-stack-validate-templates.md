---
title: Azure Stack でのテンプレート検証ツールの使用 | Microsoft Docs
description: テンプレート検証ツールを使用して、Azure Stack にデプロイするテンプレートをチェックします。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: a5e86f0f0719e30ef693c736ac4c70f05830c3bb
ms.sourcegitcommit: b2d19e12a50195bb8925879ee75c186c9604f313
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2019
ms.locfileid: "71961667"
---
# <a name="use-the-template-validation-tool-in-azure-stack"></a>Azure Stack でのテンプレート検証ツールの使用

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

テンプレート検証ツールを使用して、Azure Stack に Azure Resource Manager [テンプレート](azure-stack-arm-templates.md)をデプロイする準備ができているかどうかをチェックします。 テンプレート検証ツールは Azure Stack ツールの GitHub リポジトリの一部として使用できます。 Azure Stack ツールをダウンロードするには、「[GitHub からの Azure Stack ツールのダウンロード](../operator/azure-stack-powershell-download.md)」で説明されている手順を行います。

## <a name="overview"></a>概要

テンプレートを検証するには、最初にクラウド機能ファイルを作成してから検証ツールを実行する必要があります。 Azure Stack ツールから、次の PowerShell モジュールを使用します。

- **CloudCapabilities** フォルダー内:**AzureRM.CloudCapabilities.psm1** は、Azure Stack クラウド内のサービスとバージョンを表すクラウド機能 JSON ファイルを作成します。
- **TemplateValidator** フォルダー内:**AzureRM.TemplateValidator.psm1** は、クラウド機能 JSON ファイルを使用して、Azure Stack へのデプロイ用テンプレートをテストします。

## <a name="build-the-cloud-capabilities-file"></a>クラウド機能ファイルをビルドする

テンプレート検証ツールを使用する前に、**AzureRM.CloudCapabilities** PowerShell モジュールを実行して JSON ファイルを構築します。

>[!NOTE]
> 統合システムを更新する場合や、新しいサービスまたは仮想拡張機能を追加する場合は、このモジュールの再実行が必要になります。

1. Azure Stack への接続があることを確認してください。 これらの手順は Azure Stack Development Kit (ASDK) ホストから実行するか、または、[VPN](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn) を使用してワークステーションから接続できます。
2. **AzureRM.CloudCapabilities** PowerShell モジュールをインポートします。

    ```powershell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ```

3. **Get-CloudCapabilities** コマンドレットを使用して、サービスのバージョンを取得し、クラウド機能 JSON ファイルを作成します。 `-OutputPath` を指定しない場合、ファイル **AzureCloudCapabilities.json** は現在のディレクトリに作成されます。 実際の Azure の場所を使用してください。

    ```powershell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```

## <a name="validate-templates"></a>テンプレートの検証

次の手順を使用して、**AzureRM.TemplateValidator** PowerShell モジュールを使用してテンプレートを検証します。 独自のテンプレートを使用するか、[Azure Stack クイック スタート テンプレート](https://github.com/Azure/AzureStack-QuickStart-Templates)を使用できます。

1. **AzureRM.TemplateValidator.psm1** PowerShell モジュールをインポートします。

    ```powershell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2. テンプレート検証ツールを実行します。

    ```powershell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

テンプレート検証の警告やエラーが、PowerShell コンソールに表示され、ソース ディレクトリ内の HTML ファイルに書き込まれます。 以下のスクリーンショットは、検証レポートの例です。

![テンプレートの検証レポート](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>parameters

テンプレート検証コマンドレットでは、次のパラメーターがサポートされます。

| パラメーター | 説明 | 必須 |
| ----- | -----| ----- |
| `TemplatePath` | Azure Resource Manager テンプレートを再帰的に検索するパスを指定します。 | はい |
| `TemplatePattern` | 照合するテンプレート ファイルの名前を指定します。 | いいえ |
| `CapabilitiesPath` | クラウド機能 JSON ファイルへのパスを指定します。 | はい |
| `IncludeComputeCapabilities` | VM サイズや VM 拡張機能などの IaaS リソースの評価が含まれます。 | いいえ |
| `IncludeStorageCapabilities` | SKU の種類などのストレージ リソースの評価が含まれます。 | いいえ |
| `Report` | 生成される HTML レポートの名前を指定します。 | いいえ |
| `Verbose` | エラーと警告をコンソールに記録します。 | いいえ|

### <a name="examples"></a>例

この例では、ローカル ストレージにダウンロードした [Azure Stack クイック スタート テンプレート](https://github.com/Azure/AzureStack-QuickStart-Templates)をすべて検証します。 また、この例では、仮想マシン (VM) のサイズと ASDK 機能に対する拡張機能も検証します。

```powershell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json `
-TemplatePattern MyStandardTemplateName.json `
-IncludeComputeCapabilities `
-Report TemplateReport.html
```

## <a name="next-steps"></a>次の手順

- [テンプレートを Azure Stack にデプロイする](azure-stack-arm-templates.md)
- [Azure Stack のテンプレートの開発](azure-stack-develop-templates.md)
