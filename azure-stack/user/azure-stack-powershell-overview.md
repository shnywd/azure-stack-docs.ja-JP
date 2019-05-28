---
title: Azure Stack 上の PowerShell | Microsoft Docs
description: Azure Stack 上の PowerShell には、さまざまなモジュールやコンテキストがあります。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: article
ms.date: 04/25/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 0cef39147fdbc62fe0652b1e387aa23f5ecb8487
ms.sourcegitcommit: 889fd09e0ab51ad0e43552a800bbe39dc9429579
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/16/2019
ms.locfileid: "65782882"
---
# <a name="get-started-with-powershell-on-azure-stack"></a>Azure Stack 上の PowerShell の概要

PowerShell は、コマンド ラインからリソースを管理できるように設計されています。 Azure Resource Manager モデルを使用する自動化ツールをビルドする場合に PowerShell を使用できます。 PowerShell モジュールは、特定の領域のすべての側面を管理するためにグループ化された一連の PowerShell 関数として定義できます。 Azure Stack を操作するには、一連のさまざまな PowerShell コマンドレットを使いこなす必要があります。

この記事は、Azure Stack で使用されるこれらのさまざまな PowerShell モジュールを習得するのに役立ちます。 Azure Stack で PowerShell を使用するときに操作できる 4 セットの API があります。

| API | PowerShell リファレンス | REST リファレンス |
| --- | --- | --- |
| 1.グローバルな Azure Resource Manager | [Azure PowerShell モジュール](https://github.com/Azure/azure-powershell/blob/master/documentation/azure-powershell-modules.md) | [REST API ブラウザー](https://docs.microsoft.com/rest/api/) |
| 2.Azure Stack Resource Manager | [Azure Stack での API バージョン プロファイルの管理](azure-stack-version-profiles.md) | [Azure Stack での API バージョン プロファイルの管理](azure-stack-version-profiles.md) |
| 手順 3.Azure Stack Administrator エンドポイント | [Azure Stack 管理モジュール](https://docs.microsoft.com/powershell/azure/azure-stack/overview) | [REST API ブラウザー - Azure Stack](https://docs.microsoft.com/rest/api/?term=Azure%20Azure%20Stack%20Admin) |
| 4.Azure Stack の特権エンドポイント | [Azure Stack での特権エンドポイントの使用](../operator/azure-stack-privileged-endpoint.md) | |

各インターフェイスは、グローバル Azure または Azure Stack のリソース プロバイダーに接続します。 リソース プロバイダーにより、Azure の機能が有効になります。 たとえば、Azure コンピューティングのリソース プロバイダーでは、仮想マシンとそれに対応するリソースの作成および管理へのプログラム的なアクセスを提供します。

リソース プロバイダーは、機能を提供するだけでなく、リソースを管理および構成するための制御も提供します。 Azure Resource Manager を使用して、リソース プロバイダーにプログラム的にアクセスでき、そのインターフェイスは、PowerShell、Azure CLI、およびお客様独自の REST クライアントのサーフェスを提供します。

## <a name="where-to-find-azure-stack-powershell"></a>Azure Stack PowerShell の場所

次のブロック図は、PowerShell モジュールの各セットの関係を示しています。 お使いのマシンから、PowerShell モジュールを読み込んで、グローバル Azure および Azure Stack の両方を管理できます。

![Azure Stack PowerShell](media/azure-stack-powershell-overview/Azure-Stack-PowerShell.png)

### <a name="global-azure"></a>グローバル Azure

Azure PowerShell には、Azure リソースの操作に Azure Resource Manager の現行バージョンを使用する一連のコマンドレットが含まれています。 Azure PowerShell は、.NET Standard を使用します。 つまり、Windows、macOS、Linux で PowerShell のバージョンを使用することができます。 Azure PowerShell は、Azure Cloud Shell 上で利用することもできます。 詳細については、[Azure PowerShell の概要](https://docs.microsoft.com/powershell/azure/get-started-azureps)に関するページを参照してください。

### <a name="azure-stack-resource-manager"></a>Azure Stack Resource Manager

Azure Stack PowerShell には、Azure Resource Manager の以前のバージョンを使用する一連のコマンドレットが用意されています。 これらのコマンドレットは、Azure Stack のリソース プロバイダーと互換性があります。 Azure Stack の各リソース プロバイダーでは、グローバル Azure にあるプロバイダーの古いバージョンを使用します。 Azure Stack でサポートされている各プロバイダーのバージョンを調整しやすくするために、API プロファイルを使用できます。 Azure Stack PowerShell ではPowerShell 5.1 を使用し、Windows でのみ使用できます。 詳細については、「[Azure Stack での API バージョンのプロファイルの管理](azure-stack-version-profiles.md)」を参照してください。

### <a name="azure-stack-administrator"></a>Azure Stack Administrator

Azure Stack では、Azure Stack のインストールおよび管理を行うために、一連のリソース プロバイダーをクラウド オペレーターに公開します。 グローバル Azure では、この操作がユーザーから抽象化され、Azure の一部として、バックグラウンドで処理されます。 しかし、Azure Stack では、プライベート クラウドをサポートする機能を企業に提供します。 これらのタスクを実行するため、オペレーターは、Azure Stack 管理 API と対話します。 詳細については、「[PowerShell for Azure Stack のインストール](../operator/azure-stack-powershell-install.md)」を参照してください。

### <a name="azure-stack-privileged-endpoint"></a>Azure Stack の特権エンドポイント

インストールのテスト、ログへのアクセスなど、Azure Stack 上でのオペレーターのアクティビティでは、オペレーターは特権エンドポイント (PEP) と対話できます。 この PEP は、あらかじめ構成されたリモート PowerShell コンソールであり、特定のタスクの実行を容易にするのに十分なアクセスのみをオペレーターに提供します。 エンドポイントは PowerShell JEA (Just Enough Administration) を使用して、コマンドレットの限定的なセットを公開します。 詳細については、「[Azure Stack での特権エンドポイントの使用](../operator/azure-stack-privileged-endpoint.md)」を参照してください。

### <a name="azurestack-tools"></a>AzureStack-Tools

また、Azure Stack は、GitHub リポジトリの Azure Stack ツールで使用可能なスクリプトと追加のコマンドレットを提供します。 AzureStack-Tools は、Azure Stack へのリソースの管理とデプロイのための PowerShell モジュールをホストします。 VPN 接続を確立しようとしている場合は、これらの PowerShell モジュールを Azure Stack Development Kit にダウンロードするか、Windows ベースの外部クライアントにダウンロードできます。 詳細については、[AzureStack-Tools](https://github.com/Azure/AzureStack-Tools) に関するページを参照してください。

## <a name="working-with-powershell-on-azure-stack"></a>Azure Stack の PowerShell を操作する

PowerShell には、Azure Resource Manager と対話するためのプログラム的な手法が用意されています。 タスクを自動化する場合は、対話型のコマンド プロンプトを操作するか、スクリプトを記述できます。

Azure Stack PowerShell の操作に多くの時間がかかる場合、モジュールのインストールや再インストールをすることがあります。 これは、グローバル Azure を同時に操作している場合には困難な可能性があります。お客様の目的に合わせてお使いのモジュールのアンインストールや再インストールを行う必要があるためです。 Docker コンテナーを使用して、お使いのローカル コンピューター上の PowerShell をバージョンごとに分離できます。 「[Docker を使用した PowerShell の実行](azure-stack-powershell-user-docker.md)」では、PowerShell モジュール セットから PowerShell モジュール セットへ切り替えられるように、Docker コンテナーを使用する方法について説明しています。


## <a name="next-steps"></a>次の手順

- Azure Stack の [PowerShell の API プロファイル](azure-stack-version-profiles.md)に関するページについてお読みください。
- [Azure Stack PowerShell のインストール](../operator/azure-stack-powershell-install.md)
- クラウドの一貫性のための [Azure Resource Manager テンプレート](azure-stack-develop-templates.md)の作成についてお読みください。