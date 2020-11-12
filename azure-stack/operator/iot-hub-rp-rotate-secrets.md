---
title: Azure Stack Hub リソース プロバイダーで IoT Hub のシークレットをローテーションする方法
description: Azure Stack Hub リソース プロバイダーで IoT Hub のシークレットをローテーションする方法について説明します
author: BryanLa
ms.author: bryanla
ms.service: azure-stack
ms.topic: how-to
ms.date: 10/20/2020
ms.reviewer: bryanla
ms.lastreviewed: 10/20/2020
ms.openlocfilehash: f648aaf0b8a8d90ca8f4789cb6dd58bf5c9fb8c9
ms.sourcegitcommit: 0e3296fb27b9dabbc2569bf85656c4c7b1d58ba9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93050335"
---
# <a name="how-to-rotate-secrets-for-iot-hub-on-azure-stack-hub"></a>Azure Stack Hub で IoT Hub のシークレットをローテーションする方法

[!INCLUDE [preview-banner](../includes/iot-hub-preview.md)]

この記事では、IoT Hub リソース プロバイダーで使用されるシークレットをローテーションする方法について説明します。

## <a name="overview-and-prerequisites"></a>概要と前提条件

[!INCLUDE [prereqs](../includes/resource-provider-va-rotate-secrets-prereqs.md)]

## <a name="prepare-a-new-tls-certificate"></a>新しい TLS 証明書を準備する

[!INCLUDE [prepare](../includes/resource-provider-va-rotate-secrets-prepare.md)]

## <a name="rotate-secrets"></a>シークレットの切り替え

[!INCLUDE [rotate](../includes/resource-provider-va-rotate-secrets-rotate.md)]

## <a name="troubleshooting"></a>トラブルシューティング

シークレットのローテーションは、エラーが発生することなく正常に完了する必要があります。 管理者ポータルで次のいずれかの症状が発生した場合は、[サポート リクエストを開いて](azure-stack-manage-basics.md#where-to-get-support)ください。

   - 認証の問題 (IoT Hub リソース プロバイダーへの接続の問題を含む)。
   - リソース プロバイダーをアップグレードできない、または構成パラメーターを編集できない。
   - 使用状況のメトリックが表示されない。
   - 請求書が生成されない。
   - バックアップが実行されない。

## <a name="next-steps"></a>次のステップ

Azure Stack Hub インフラストラクチャのシークレットのローテーションについて詳しくは、「[Azure Stack Hub でシークレットをローテーションする](azure-stack-rotate-secrets.md)」を参照してください。