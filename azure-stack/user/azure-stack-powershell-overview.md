---
title: Azure Stack 内の PowerShell | Microsoft Docs
description: Azure Stack 内の PowerShell には、さまざまなモジュールやコンテキストがあります。
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
ms.openlocfilehash: 8f5d97969c96e8f3546d37ffed28b6332d80dc05
ms.sourcegitcommit: 2ee75ded704e8cfb900d9ac302d269c54a5dd9a3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/30/2019
ms.locfileid: "66394418"
---
# <a name="get-started-with-powershell-in-azure-stack"></a>Azure Stack 内の PowerShell の概要

PowerShell は、コマンド ラインからリソースを管理できるように設計されています。 Azure Resource Manager モデルを使用する自動化ツールをビルドする場合に PowerShell を使用できます。 PowerShell モジュールは、特定の領域のすべての側面を管理するためにグループ化された一連の PowerShell 関数として定義できます。 Azure Stack を操作するには、一連のさまざまな PowerShell コマンドレットを使いこなす必要があります。

この記事は、Azure Stack 内で使用されるさまざまな PowerShell モジュールを習得するのに役立ちます。 Azure Stack 内で PowerShell を使用するときは、次の表に示す 4 セットの API のどれでも操作できます。

| API | PowerShell リファレンス | REST リファレンス |
| --- | --- | --- |
| グローバルな Azure Resource Manager | [Azure PowerShell モジュール](https://github.com/Azure/azure-powershell/blob/master/documentation/azure-powershell-modules.md) | [REST API ブラウザー](https://docs.microsoft.com/rest/api/) |
| Azure Stack Resource Manager | [Azure Stack での API バージョン プロファイルの管理](azure-stack-version-profiles.md) | [Azure Stack での API バージョン プロファイルの管理](azure-stack-version-profiles.md) |
| Azure Stack Administrator エンドポイント | [Azure Stack 管理モジュール](https://docs.microsoft.com/powershell/azure/azure-stack/overview) | [REST API ブラウザー - Azure Stack](https://docs.microsoft.com/rest/api/?term=Azure%20Azure%20Stack%20Admin) |
| Azure Stack の特権エンドポイント | [Azure Stack での特権エンドポイントの使用](../operator/azure-stack-privileged-endpoint.md) | |

各インターフェイスは、グローバル Azure または Azure Stack のリソース プロバイダーに接続します。 リソース プロバイダーにより、Azure の機能が有効になります。 たとえば、Azure コンピューティングのリソース プロバイダーでは、仮想マシンとそれに対応するリソースの作成および管理へのプログラムによるアクセスを提供します。

リソース プロバイダーは、リソースを管理および構成するための機能と制御の両方を提供します。 リソース プロバイダーには、Azure Resource Manager を使用してプログラムからアクセスできます。 さらに、そのインターフェイスは、PowerShell、Azure CLI、およびお客様独自の REST クライアントのサーフェイスを提供します。

## <a name="where-to-find-azure-stack-powershell"></a>Azure Stack PowerShell の場所

次のブロック図は、PowerShell モジュールのセット間の関係を示しています。 お使いのマシンから、PowerShell モジュールを読み込んで、グローバル Azure および Azure Stack の両方を管理できます。

![Azure Stack PowerShell](media/azure-stack-powershell-overview/Azure-Stack-PowerShell.png)

### <a name="global-azure"></a>グローバル Azure

Azure PowerShell には、お使いの Azure リソースの操作に Azure Resource Manager の最新バージョンを使用する一連のコマンドレットが含まれています。 Azure PowerShell では .NET Standard バージョンが使用されます。つまり、Windows、macOS、Linux で PowerShell のバージョンを使用することができます。 Azure PowerShell は、Azure Cloud Shell 上で利用することもできます。 詳細については、[Azure PowerShell の概要](https://docs.microsoft.com/powershell/azure/get-started-azureps)に関するページを参照してください。

### <a name="azure-stack-resource-manager"></a>Azure Stack Resource Manager

Azure Stack PowerShell には、Azure Resource Manager の以前のバージョンを使用する一連のコマンドレットが用意されています。 これらのコマンドレットは、Azure Stack のリソース プロバイダーと互換性があります。 Azure Stack の各リソース プロバイダーでは、グローバル Azure にあるプロバイダーの古いバージョンを使用します。 Azure Stack でサポートされている各プロバイダーのバージョンを調整しやすくするために、API プロファイルを使用できます。 Azure Stack PowerShell はPowerShell 5.1 を使用し、Windows 上でのみ利用できます。 詳細については、「[Azure Stack での API バージョンのプロファイルの管理](azure-stack-version-profiles.md)」を参照してください。

### <a name="azure-stack-administrator"></a>Azure Stack Administrator

Azure Stack では、クラウド オペレーターが Azure Stack のインストールおよび管理を行うことができるように、一連のリソース プロバイダーがオペレーターに公開されます。 グローバル Azure では、この操作がユーザーから抽象化され、Azure の一部として、バックグラウンドで処理されます。 しかし、Azure Stack では、企業はプライベート クラウドをサポートできます。 これらのタスクを実行するには、オペレーターが Azure Stack 管理 API シリーズを操作します。 詳細については、「[PowerShell for Azure Stack のインストール](../operator/azure-stack-powershell-install.md)」を参照してください。

### <a name="azure-stack-privileged-endpoint"></a>Azure Stack の特権エンドポイント

インストールのテスト、ログへのアクセスなど、Azure Stack 内でのオペレーターのアクティビティでは、オペレーターは特権エンドポイント (PEP) を操作できます。 この PEP は、あらかじめ構成されたリモート PowerShell コンソールであり、特定のタスクを実行するのに十分なアクセスをオペレーターに提供します。 エンドポイントは PowerShell Just Enough Administration (JEA) を使用して、コマンドレットの限定的なセットを公開します。 詳細については、「[Azure Stack での特権エンドポイントの使用](../operator/azure-stack-privileged-endpoint.md)」を参照してください。

### <a name="azure-stack-tools"></a>Azure Stack ツール

Azure Stack では、GitHub リポジトリの *AzureStack-Tools* でスクリプトと追加のコマンドレットが公開されています。 AzureStack-Tools は、Azure Stack へのリソースの管理とデプロイのための PowerShell モジュールをホストします。 VPN 接続を確立しようとしている場合は、これらの PowerShell モジュールを Azure Stack Development Kit か、Windows ベースの外部クライアントにダウンロードできます。 詳細については、[AzureStack-Tools](https://github.com/Azure/AzureStack-Tools) ページを参照してください。

## <a name="work-with-powershell-in-azure-stack"></a>Azure Stack 内で PowerShell を操作する

PowerShell には、Azure Resource Manager をプログラムから操作するための方法が用意されています。 対話型のコマンド プロンプトを操作するか、タスクを自動化する場合は、スクリプトを記述できます。

Azure Stack PowerShell の操作に多くの時間がかかる場合、モジュールのインストールや再インストールをすることがあります。 グローバル Azure を同時に操作している場合には、このルーチンは困難な場合があります。お客様の目的に合わせてお使いのモジュールのアンインストールや再インストールを行う必要があるためです。 

Docker コンテナーを使用して、お使いのローカル コンピューター上の PowerShell をバージョンごとに分離できます。 PowerShell モジュール セットから PowerShell モジュール セットに切り替えられるように、Docker コンテナーを使用するには、[Docker を使用した PowerShell の実行](azure-stack-powershell-user-docker.md)に関するページを参照してください。


## <a name="next-steps"></a>次の手順

- Azure Stack の [PowerShell の API プロファイル](azure-stack-version-profiles.md)についてお読みください。
- [Azure Stack PowerShell](../operator/azure-stack-powershell-install.md) をインストールします。
- クラウドの一貫性のための [Azure Resource Manager テンプレート](azure-stack-develop-templates.md)の作成についてお読みください。
