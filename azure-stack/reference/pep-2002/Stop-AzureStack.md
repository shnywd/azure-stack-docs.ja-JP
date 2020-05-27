---
title: Azure Stack Hub の Stop-AzureStack 特権エンドポイント
description: PowerShell Azure Stack の特権エンドポイントのリファレンス - Stop-AzureStack
author: mattbriggs
ms.topic: reference
ms.date: 04/27/2020
ms.author: mabrigg
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: 946d31132e4a916574e8fb051e9761f81eb7cdf2
ms.sourcegitcommit: 54f98b666bea9226c78f26dc255ddbdda539565f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82563999"
---
# <a name="stop-azurestack"></a>Stop-AzureStack

## <a name="synopsis"></a>概要
すべての Azure Stack Hub サービスを停止します。

## <a name="syntax"></a>構文

```
Stop-AzureStack [[-TimeoutInSecs] <Object>] [-AsJob]
```

## <a name="description"></a>説明
すべての Azure Stack サービスを停止し、Azure Stack Hub が実行されている物理コンピューターを停止します。

## <a name="parameters"></a>パラメーター

### <a name="-timeoutinsecs"></a>-TimeoutInSecs
実行が停止されるまでの最大時間。

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: 2400
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-asjob"></a>-AsJob


```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="next-steps"></a>次のステップ

特権エンドポイントにアクセスして使用する方法については、「[Azure Stack Hub で特権エンドポイントを使用する](https://docs.microsoft.com/azure-stack/operator/azure-stack-privileged-endpoint)」を参照してください。