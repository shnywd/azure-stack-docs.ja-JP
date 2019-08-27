---
title: Azure Stack スイッチ構成で特定の設定を変更する |Microsoft Docs
description: Azure Stack スイッチ構成でカスタマイズできるものについて説明します。 相手先ブランド供給 (OEM) によって構成が作成されたら、OEM または Microsoft Azure Stack エンジニアリング チームからの同意なしに変更しないでください。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: Femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2019
ms.author: mabrigg
ms.reviewer: wamota
ms.lastreviewed: 08/09/2019
ms.openlocfilehash: 8ed7b3c71c1a9a2098fa125caeb03c949a332f50
ms.sourcegitcommit: 54448d4518c7d9145b8755f51b18d554b3190810
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69559649"
---
#  <a name="modify-specific-settings-on-your-azure-stack-switch-configuration"></a>Azure Stack スイッチ構成の特定の設定を変更する

Azure Stack スイッチ構成について、いくつかの環境設定を変更できます。 相手先ブランド供給 (OEM) によって作成されたテンプレートで、どの設定を変更できるかを特定できます。 この記事では、それらのカスタマイズ可能な各設定と、変更が Azure Stack に与える可能性のある影響について説明します。 これらの設定には、パスワード更新、syslog サーバー、SNMP 監視、認証、およびアクセス制御リストが含まれます。 

Azure Stack ソリューションのデプロイ時に、OEM (相手先ブランド供給) によって、TOR と BMC の両方のスイッチ構成が作成され、適用されます。 OEM は、Azure Stack オートメーション ツールを使用して、これらのデバイスで必要な構成が正しく設定されていることを検証します。 構成は、Azure Stack [デプロイ ワークシート](azure-stack-deployment-worksheet.md)の情報に基づきます。 OEM が構成を作成した後は、OEM または Microsoft Azure Stack エンジニアリングチームからの同意なしに構成を**変更しない**でください。 ネットワーク デバイスの構成を変更すると、Azure Stack インスタンスのネットワークの問題の操作やトラブルシューティングに大きな影響を与える可能性があります。

しかし、ネットワーク スイッチの構成で追加、削除、または変更できる値がいくつかあります。

>[!Warning]  
> OEM または Microsoft Azure Stack エンジニアリング チームからの同意なしに構成を**変更しない**でください。 ネットワーク デバイスの構成を変更すると、Azure Stack インスタンスのネットワークの問題の操作やトラブルシューティングに大きな影響を与える可能性があります。
>
> ネットワーク デバイスのこれらの機能の詳細、これらの変更を行う方法については、OEM ハードウェアプロバイダーまたは Microsoft サポートにお問い合わせください。 OEM には、Azure Stack デプロイ ワークシートに基づいた、オートメーション ツールによって作成された構成ファイルがあります。 

## <a name="password-update"></a>パスワードの更新

オペレーターは、ネットワークスイッチ上の任意のユーザーのパスワードをいつでも更新できます。 Azure Stack システムの情報を変更したり、[Azure Stack でシークレットをローテーションする](azure-stack-rotate-secrets.md)手順を使用したりする必要はありません。

## <a name="syslog-server"></a>Syslog サーバー

オペレーターは、データセンターの syslog サーバーにスイッチ ログをリダイレクトできます。 この構成を使用することで、特定の時点のログをトラブルシューティングに使用できるようにします。 既定で、ログはスイッチに格納されます。ログを格納するためのそれらの容量は限られています。 スイッチ管理アクセスのアクセス許可を構成する方法の概要については、「[アクセス制御リストの更新](#access-control-list-updates)」セクションを参照してください。

## <a name="snmp-monitoring"></a>SNMP の監視

オペレーターは、ネットワーク デバイスを監視し、データセンターのネットワーク監視アプリケーションにトラップを送信するように簡易ネットワーク管理プロトコル (SNMP) v2 または v3 を構成できます。 セキュリティ上の理由から、v2 より安全であるため、SNMPv3 を使用してください。 必要な MIB と構成については、OEM ハードウェア プロバイダーに問い合わせてください。 スイッチ管理アクセスのアクセス許可を構成する方法の概要については、「[アクセス制御リストの更新](#access-control-list-updates)」セクションを参照してください。

## <a name="authentication"></a>Authentication

オペレーターは、RADIUS または TACACS を構成して、ネットワークデバイスの認証を管理できます。 サポートされている方法と必要な構成については、OEM ハードウェア プロバイダーに問い合わせてください。  スイッチ管理アクセスのアクセス許可を構成する方法の概要については、「[アクセス制御リストの更新](#access-control-list-updates)」セクションを参照してください。

## <a name="access-control-list-updates"></a>アクセス制御リストの更新

オペレーターは、いくつかのアクセス制御リスト (ACL) を変更して、信頼されたデータセンター ネットワーク範囲から、ネットワークデバイス管理インターフェイスおよびハードウェア ライフサイクル ホスト (HLH) にアクセスできるようにすることができます。 オペレーターは、到達可能にするコンポーネントおよびどこから到達可能にするかを選択できます。 アクセス制御リストによって、オペレーターは、特定のネットワーク範囲内の管理ジャンプボックス VM に、スイッチ管理インターフェイス、HLH OS、HLH BMC へのアクセスを許可することができます。

## <a name="next-steps"></a>次の手順

[Azure Stack とデータセンターの統合 - DNS](azure-stack-integrate-dns.md)