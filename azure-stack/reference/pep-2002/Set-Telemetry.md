---
title: Azure Stack Hub の Close-PrivilegedEndpoint 特権エンドポイント
description: PowerShell Azure Stack の特権エンドポイントのリファレンス - Close-PrivilegedEndpoint
author: mattbriggs
ms.topic: reference
ms.date: 04/27/2020
ms.author: mabrigg
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: 948d8771be47ca9a594da75d8e4d138ceb6b894c
ms.sourcegitcommit: 54f98b666bea9226c78f26dc255ddbdda539565f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82564011"
---
# <a name="set-telemetry"></a>Set-Telemetry

## <a name="synopsis"></a>概要
Microsoft への利用統計情報の転送を有効または無効にします。

## <a name="syntax"></a>構文

```
Set-Telemetry [-Disable] [-Enable] [-AsJob]
```

## <a name="description"></a>説明
Set-Telemetry コマンドレットを使用すると、レジストリの対応する設定を変更することにより、テレメトリ データを Microsoft に送信するかどうかを制御できるようになります。

具体的には、このコマンドレットを実行すると、ドメイン グループ ポリシーが構成され、テレメトリ レジストリ値が 0 に設定され、Windows UTC サービスがすべてのインフラストラクチャ VM およびホスト上で実行されなくなります。

## <a name="examples"></a>例

### <a name="example-1"></a>例 1
```
Set-Telemetry -Enable
```

### <a name="example-2"></a>例 2
```
Set-Telemetry -Disable
```

## <a name="parameters"></a>パラメーター

### <a name="-enable"></a>-Enable
Microsoft への利用統計情報の転送を有効にします。

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

### <a name="-disable"></a>-Disable
Microsoft への利用統計情報の転送を無効にします。

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