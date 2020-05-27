---
title: Azure Stack Hub の Register-DirectoryService 特権エンドポイント
description: PowerShell Azure Stack の特権エンドポイントのリファレンス -Register-DirectoryService
author: mattbriggs
ms.topic: reference
ms.date: 04/27/2020
ms.author: mabrigg
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: 4ac71d57c0f2e5028883a4bede539fdf89a45afb
ms.sourcegitcommit: 54f98b666bea9226c78f26dc255ddbdda539565f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82563735"
---
# <a name="register-directoryservice"></a>Register-DirectoryService

## <a name="synopsis"></a>概要
顧客の Active Directory (Azure AD) を Graph サービスに登録するスクリプト。
**CustomADGlobalCatalog** は、カスタム Active Directory のグローバル カタログです。
これは、**Get-ADForest** コマンドレットの出力の `RootDomain` 値になります。

## <a name="syntax"></a>構文

```
Register-DirectoryService [-Force] [[-TimeoutInSecs] <Object>] [-SkipRootDomainValidation]
 [[-CustomADAdminCredential] <Object>] [-ValidateParameters] [[-CustomADGlobalCatalog] <Object>] [-AsJob]
```

## <a name="description"></a>説明
顧客の Active Directory を Graph サービスに登録するスクリプト。
**CustomADGlobalCatalog** は、カスタム Active Directory のグローバル カタログです。
これは、**Get-ADForest** コマンドレットの出力の `RootDomain` 値になります。

## <a name="examples"></a>例

### <a name="example-1"></a>例 1
```
Register-DirectoryService -CustomADGlobalCatalog "contoso.com" -CustomADAdminCredential (Get-Credential) -TimeoutInSecs 1000
```

## <a name="parameters"></a>パラメーター

### <a name="-customadglobalcatalog"></a>-CustomADGlobalCatalog
 

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

### <a name="-customadadmincredential"></a>-CustomADAdminCredential
 

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-validateparameters"></a>-ValidateParameters
 

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-skiprootdomainvalidation"></a>-SkipRootDomainValidation
 

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-force"></a>-Force
 

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-timeoutinsecs"></a>-TimeoutInSecs
 

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
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