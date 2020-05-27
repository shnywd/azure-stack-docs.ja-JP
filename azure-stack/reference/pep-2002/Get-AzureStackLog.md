---
title: Azure Stack Hub の Get-AzureStackLog 特権エンドポイント
description: PowerShell Azure Stack の特権エンドポイントのリファレンス - Get-AzureStackLog
author: mattbriggs
ms.topic: reference
ms.date: 04/27/2020
ms.author: mabrigg
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: d7e7ff712e1d8f7c062d7b3a7f38d97b80085e31
ms.sourcegitcommit: 54f98b666bea9226c78f26dc255ddbdda539565f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82563891"
---
# <a name="get-azurestacklog"></a>Get-AzureStackLog

## <a name="synopsis"></a>概要
タイムアウトがある Azure Stack Hub のさまざまなロールからログを取得します。
マルチノード環境または 1 ノード環境の両方で機能します。

## <a name="syntax"></a>構文

```
Get-AzureStackLog [-FilterByRole <Object>] [-CustomerConfigurationFilePath <Object>] [-OutputPath <Object>]
 [-LocalAdminCredential <Object>] [-FilterByResourceProvider <Object>] [-InputSasUri <Object>]
 [-FilterByNode <Object>] [-OutputShareCredential <Object>] [-ToDate <Object>] [-FilterByLogType <Object>]
 [-TimeOutInMinutes <Object>] [-FromDate <Object>] [-OutputSharePath <Object>] [-OutputSasUri <Object>]
 [-PerformVMLogRecovery] [-IncludeDumpFile] [-AsJob]
```

## <a name="parameters"></a>パラメーター

### <a name="-filterbyrole"></a>-FilterByRole
 

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-filterbyresourceprovider"></a>-FilterByResourceProvider
 

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-filterbynode"></a>-FilterByNode
 

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-filterbylogtype"></a>-FilterByLogType
 

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-customerconfigurationfilepath"></a>-CustomerConfigurationFilePath
 

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-fromdate"></a>-FromDate
これにより、過去 1 時間以降のログが取得されます。

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: (Get-Date).AddHours(-1)
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-todate"></a>-ToDate
これにより、現在の日付までのログが取得されます

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: (Get-Date)
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-inputsasuri"></a>-InputSasUri
ログのダウンロード元の Blob service SAS URI。

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-outputpath"></a>-OutputPath
zip ファイルを配置するルート フォルダーのパス。

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-outputsharepath"></a>-OutputSharePath
収集したログの移動先である共有パス。

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-outputsharecredential"></a>-OutputShareCredential
共有資格情報。

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-outputsasuri"></a>-OutputSasUri
収集されたログの移動先である BLOB SAS URI。

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-localadmincredential"></a>-LocalAdminCredential
 

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-timeoutinminutes"></a>-TimeOutInMinutes
ログ収集ツールのタイムアウト。

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 240
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-includedumpfile"></a>-IncludeDumpFile
既定でダンプ ファイルの収集を無効にします。

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

### <a name="-performvmlogrecovery"></a>-PerformVMLogRecovery
`Off`、`OffCritical`、`Paused`、`PausedCritical` VM の WindowsEvents ログの回復を実行します。

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