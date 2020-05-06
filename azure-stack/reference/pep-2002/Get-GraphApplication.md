---
title: Azure Stack Hub の Get-GraphApplication 特権エンドポイント
description: PowerShell Azure Stack の特権エンドポイントのリファレンス - Get-GraphApplication
author: mattbriggs
ms.topic: reference
ms.date: 04/27/2020
ms.author: mabrigg
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: baccf889139b2a91b23b55ef9bb86387991558da
ms.sourcegitcommit: 54f98b666bea9226c78f26dc255ddbdda539565f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82563549"
---
# <a name="get-graphapplication"></a>Get-GraphApplication

## <a name="synopsis"></a>概要
Get-GraphApplication は、指定されたアプリケーションの名前または識別子のグラフ アプリケーション情報を取得するためのラッパー関数です。

## <a name="syntax"></a>構文

```
Get-GraphApplication [[-ApplicationIdentifier] <Object>] [-ApplicationName <Object>] [-AsJob]
```

## <a name="description"></a>説明
ADFS マシン上で Get-GraphApplicationGroup を呼び出して、グラフ アプリケーション情報を取得します。

## <a name="examples"></a>例

### <a name="example-1"></a>例 1
```
Get-GraphApplication -ApplicationName $ApplicationName
```

## <a name="parameters"></a>パラメーター

### <a name="-applicationidentifier"></a>-ApplicationIdentifier
AD FS のアプリケーション レジスタのアプリケーション識別子。

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

### <a name="-applicationname"></a>-ApplicationName
ADFS に登録されているアプリケーションのアプリケーション名。

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