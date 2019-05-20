---
title: Azure Stack へのアプリのデプロイ | Microsoft Docs
description: Azure Stack にアプリをデプロイします。
services: azure-stack
author: mattbriggs
ms.service: azure-stack
ms.topic: overview
ms.date: 04/24/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 2d9fa9472988fb7c65c8138945ad0b422c871504
ms.sourcegitcommit: 41927cb812e6a705d8e414c5f605654da1fc6952
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/25/2019
ms.locfileid: "64482249"
---
# <a name="common-deployments-for-azure-stack"></a>Azure Stack 向けの一般的なデプロイ

Azure Stack では、サービスとしてのインフラストラクチャ - およびサービスとしてのプラットフォームの両方をサポートしています。 Azure Stack へのアプリのデプロイを開始する前に、お使いになる開発用マシンを設定します。 開発者として Azure Stack を操作するための準備手順については、「[Azure Stack 上で開発環境を設定する](azure-stack-dev-start.md)」を参照してください。

## <a name="deploy-an-app-to-a-vm"></a>VM にアプリをデプロイする

次のハウツー記事では、Azure Stack 内の VM の設定と、ご使用の開発ワークステーションから Azure Stack へのアプリのデプロイについて説明します。

- [.Net Core (C#)](azure-stack-dev-start-howto-vm-dotnet.md)
- [Java](azure-stack-dev-start-howto-vm-java.md)
- [Go](azure-stack-dev-start-howto-vm-go.md)
- [Ruby](azure-stack-dev-start-howto-vm-ruby.md)
- [Python](azure-stack-dev-start-howto-vm-python.md)
- [NodeJS](azure-stack-dev-start-howto-vm-nodejs.md)

<!-- 
## Deploy an app using Azure Stack Resource Manager

The following how to article will walk you through using the Azure Stack SDK for your language to create an Azure Stack Resource Manager template to create your resources, and then deploy to those resources in Azure Stack.

- .Net Core (C#)
- Java
- Go
- Ruby
- Python

## Deploy an app to Azure Stack App service

The following how to articles will walk you deploying your app to the Azure Stack app service.

- .Net Core (C#)
- Java
- Go
- Ruby
- Python

## Deploy an app with Docker to Kubernetes

The following how to articles will walk you through deploying your Docker container to Kubernetes hosted by Azure Stack.

- .Net Core (C#)
- Java
- Go
- Ruby
- Python

-->

## <a name="next-steps"></a>次の手順

- [Azure Stack 上で開発環境](azure-stack-dev-start.md)を設定する
- [Azure Stack 上で継続的インテグレーション/継続的デリバリー (CI/CD) パイプライン](azure-stack-solution-pipeline.md)を設定する
