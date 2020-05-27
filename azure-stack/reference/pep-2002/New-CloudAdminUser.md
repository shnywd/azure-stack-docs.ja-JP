---
title: Azure Stack Hub の New-CloudAdminUser 特権エンドポイント
description: PowerShell Azure Stack の特権エンドポイントのリファレンス - New-CloudAdminUser
author: mattbriggs
ms.topic: reference
ms.date: 04/27/2020
ms.author: mabrigg
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: 99ac0d7b96d040979861cf0a6fe22db6b30fc596
ms.sourcegitcommit: 54f98b666bea9226c78f26dc255ddbdda539565f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82563807"
---
# <a name="new-cloudadminuser"></a>New-CloudAdminUser

## <a name="syntax"></a>構文

```
New-CloudAdminUser [[-Password] <Object>] [[-UserName] <Object>] [-AsJob]
```

## <a name="parameters"></a>パラメーター

### <a name="-password"></a>-Password
 

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-username"></a>-UserName
 

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