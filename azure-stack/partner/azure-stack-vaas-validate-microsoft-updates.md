---
title: Azure Stack のサービスとしての検証で Microsoft のソフトウェア更新プログラムを検証する | Microsoft Docs
description: Microsoft のソフトウェア更新プログラムをサービスとしての検証で検証する方法について説明します。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/29/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 10/29/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 3399288b801114f076afe3b68d3f9790bfe2af29
ms.sourcegitcommit: 08d2938006b743b76fba42778db79202d7c3e1c4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2019
ms.locfileid: "74954487"
---
# <a name="validate-software-updates-from-microsoft"></a>Microsoft のソフトウェア更新プログラムの検証

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft は、Azure Stack ソフトウェアの更新プログラムを定期的にリリースします。 これらの更新プログラムは、Azure Stack 共同エンジニアリング パートナーに提供されます。 更新プログラムの一般公開より前に提供されます。 独自のソリューションに対して更新プログラムのチェックを行い、Microsoft にフィードバックを返すことができます。

Azure Stack を対象とした Microsoft のソフトウェア更新プログラムは名前付け規則を使って指定されます。たとえば 1803 は、2018 年 3 月の更新プログラムであることを示します。 Azure Stack の更新ポリシー、周期、リリース ノートについては、「[Azure Stack サービス ポリシー](../operator/azure-stack-servicing-policy.md)」を参照してください。

## <a name="prerequisites"></a>前提条件

VaaS で月次更新プログラムのプロセスを実行する前に、以下の項目に理解しておく必要があります。

- [サービスとしての検証の主要概念](azure-stack-vaas-key-concepts.md)

## <a name="required-tests"></a>必須のテスト

月次ソフトウェア検証のためには、以下のテストを以下の順序で実行する必要があります。

- OEM 検証ワークフロー

## <a name="validating-software-updates"></a>ソフトウェア更新プログラムの検証

1. 新しい**パッケージの検証**ワークフローを作成します。
1. 上記の必須テストについては、「[Run Package Validation tests (パッケージ検証テストの実行)](azure-stack-vaas-validate-oem-package.md#run-package-validation-tests)」の手順に従ってください。 **Monthly Azure Stack Update Verification (月次 Azure Stack 更新プログラムの検証)** テストの詳細な手順については、後のセクションを参照してください。

質問や懸念事項がある場合は、[VaaS ヘルプ](mailto:vaashelp@microsoft.com)にお問い合わせください。

## <a name="next-steps"></a>次の手順

- [VaaS ポータルでのテストの監視と管理](azure-stack-vaas-monitor-test.md)