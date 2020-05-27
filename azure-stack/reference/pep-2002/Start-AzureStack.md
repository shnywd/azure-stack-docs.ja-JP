---
title: Azure Stack Hub の Start-AzureStack 特権エンドポイント
description: PowerShell Azure Stack の特権エンドポイントのリファレンス - Start-AzureStack
author: mattbriggs
ms.topic: reference
ms.date: 04/27/2020
ms.author: mabrigg
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: 70b0992239fc2f1f5cc5bb4b7ffb4b8615c94c0d
ms.sourcegitcommit: 54f98b666bea9226c78f26dc255ddbdda539565f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82563993"
---
# <a name="start-azurestack"></a>Start-AzureStack

## <a name="synopsis"></a>概要
すべての Azure Stack Hub サービスを開始します。

## <a name="syntax"></a>構文

```
Start-AzureStack [[-TimeoutInSecs] <Object>] [-AsJob]
```

## <a name="description"></a>説明
すべての Azure Stack Hub サービスを開始します。

## <a name="examples"></a>例

### <a name="example-1"></a>例 1
```powershell
PS C:\> 
```



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