---
title: Azure Stack Hub の Reset-DatacenterIntegrationConfiguration 特権エンドポイント
description: PowerShell Azure Stack の特権エンドポイントのリファレンス - Reset-DatacenterIntegrationConfiguration
author: mattbriggs
ms.topic: reference
ms.date: 04/27/2020
ms.author: mabrigg
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: 42cf50694c333b91d110ac921be8b616bbc93107
ms.sourcegitcommit: 54f98b666bea9226c78f26dc255ddbdda539565f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82563693"
---
# <a name="reset-datacenterintegrationconfiguration"></a>Reset-DatacenterIntegrationConfiguration

## <a name="synopsis"></a>概要
データセンターの統合の変更をリセットするスクリプトです。

## <a name="syntax"></a>構文

```
Reset-DatacenterIntegrationConfiguration [[-TimeoutInSecs] <Object>] [-AsJob]
```

## <a name="description"></a>説明
データセンターの統合の変更をリセットするスクリプトです。

## <a name="examples"></a>例

### <a name="example-1"></a>例 1
```
Reset-DatacenterIntegrationConfiguration -TimeoutInSecs 2000
```

## <a name="parameters"></a>パラメーター

### <a name="-timeoutinsecs"></a>-TimeoutInSecs
 

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: 1000
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