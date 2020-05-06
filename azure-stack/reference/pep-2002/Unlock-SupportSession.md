---
title: Azure Stack Hub の Unlock-SupportSession 特権エンドポイント
description: PowerShell Azure Stack の特権エンドポイントのリファレンス - Unlock-SupportSession
author: mattbriggs
ms.topic: reference
ms.date: 04/27/2020
ms.author: mabrigg
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: 7f035bf5aec66bd230506477c8001b7760c8f4df
ms.sourcegitcommit: 54f98b666bea9226c78f26dc255ddbdda539565f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82563939"
---
# <a name="unlock-supportsession"></a>Unlock-SupportSession

## <a name="syntax"></a>構文

```
Unlock-SupportSession [[-ResponseToken] <Object>] [-AsJob]
```

## <a name="parameters"></a>パラメーター

### <a name="-responsetoken"></a>-ResponseToken


```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
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