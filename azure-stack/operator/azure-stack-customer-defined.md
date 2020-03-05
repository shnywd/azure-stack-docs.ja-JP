---
title: Azure Stack Hub スイッチ構成の特定の設定を変更する
description: Azure Stack Hub スイッチ構成でカスタマイズできるものについて説明します。 相手先ブランド供給 (OEM) によって構成が作成されたら、OEM または Microsoft Azure Stack Hub エンジニアリング チームからの同意なしに変更しないでください。
author: IngridAtMicrosoft
ms.topic: article
ms.date: 11/11/2019
ms.author: inhenkel
ms.reviewer: wamota
ms.lastreviewed: 11/11/2019
ms.openlocfilehash: c4d0335f03e0e6012423cfe7d7a7eb684872db3e
ms.sourcegitcommit: 4ac711ec37c6653c71b126d09c1f93ec4215a489
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77700308"
---
#  <a name="modify-specific-settings-on-your-azure-stack-hub-switch-configuration"></a>Azure Stack Hub スイッチ構成の特定の設定を変更する

Azure Stack Hub スイッチ構成について、いくつかの環境設定を変更できます。 相手先ブランド供給 (OEM) によって作成されたテンプレートで、どの設定を変更できるかを特定できます。 この記事では、それらのカスタマイズ可能な設定のそれぞれと、変更がご利用の Azure Stack Hub に与える可能性のある影響について説明します。 これらの設定には、パスワード更新、syslog サーバー、SNMP 監視、認証、およびアクセス制御リストが含まれます。 

Azure Stack Hub ソリューションのデプロイ時に、OEM (相手先ブランド供給) によって、TOR と BMC の両方のスイッチ構成が作成され、適用されます。 OEM は、Azure Stack Hub オートメーション ツールを使用して、これらのデバイスで必要な構成が正しく設定されていることを検証します。 この構成は、ご利用の Azure Stack Hub [デプロイ ワークシート](azure-stack-deployment-worksheet.md)の情報に基づきます。 OEM が構成を作成した後は、OEM または Microsoft Azure Stack Hub エンジニアリングチームからの同意なしに構成を**変更しない**でください。 ネットワーク デバイスの構成を変更すると、ご利用の Azure Stack Hub インスタンスのネットワークの問題の操作やトラブルシューティングに大きな影響を与える可能性があります。

しかし、ネットワーク スイッチの構成で追加、削除、または変更できる値がいくつかあります。

>[!Warning]  
> OEM または Microsoft Azure Stack Hub エンジニアリング チームからの同意なしに構成を**変更しない**でください。 ネットワーク デバイスの構成を変更すると、ご利用の Azure Stack Hub インスタンスのネットワークの問題の操作やトラブルシューティングに大きな影響を与える可能性があります。
>
> ネットワーク デバイスのこれらの機能の詳細、これらの変更を行う方法については、OEM ハードウェアプロバイダーまたは Microsoft サポートにお問い合わせください。 ご利用の OEM には、お客様の Azure Stack Hub デプロイ ワークシートに基づいて、オートメーション ツールによって作成された構成ファイルがあります。 

## <a name="password-update"></a>パスワードの更新

オペレーターは、ネットワークスイッチ上の任意のユーザーのパスワードをいつでも更新できます。 Azure Stack Hub システムの情報を変更したり、[Azure Stack Hub でシークレットをローテーションする](azure-stack-rotate-secrets.md)手順を使用したりする必要はありません。

## <a name="syslog-server"></a>Syslog サーバー

オペレーターは、データセンターの syslog サーバーにスイッチ ログをリダイレクトできます。 この構成を使用することで、特定の時点のログをトラブルシューティングに使用できるようにします。 既定で、ログはスイッチに格納されます。ログを格納するためのそれらの容量は限られています。 スイッチ管理アクセスのアクセス許可を構成する方法の概要については、「[アクセス制御リストの更新](#access-control-list-updates)」セクションを参照してください。

## <a name="snmp-monitoring"></a>SNMP の監視

オペレーターは、ネットワーク デバイスを監視し、データセンターのネットワーク監視アプリケーションにトラップを送信するように簡易ネットワーク管理プロトコル (SNMP) v2 または v3 を構成できます。 セキュリティ上の理由から、v2 より安全であるため、SNMPv3 を使用してください。 必要な MIB と構成については、OEM ハードウェア プロバイダーに問い合わせてください。 スイッチ管理アクセスのアクセス許可を構成する方法の概要については、「[アクセス制御リストの更新](#access-control-list-updates)」セクションを参照してください。

## <a name="authentication"></a>認証

オペレーターは、RADIUS または TACACS を構成して、ネットワークデバイスの認証を管理できます。 サポートされている方法と必要な構成については、OEM ハードウェア プロバイダーに問い合わせてください。  スイッチ管理アクセスのアクセス許可を構成する方法の概要については、「[アクセス制御リストの更新](#access-control-list-updates)」セクションを参照してください。

## <a name="access-control-list-updates"></a>アクセス制御リストの更新

> [!NOTE]
> 1910 以降、デプロイ ワークシートに、**許可されるネットワーク**用の新しいフィールドが追加されます。信頼されたデータセンター ネットワーク範囲からネットワーク デバイス管理インターフェイスとハードウェア ライフサイクル ホスト (HLH) へのアクセスを許可するために必要な主導の手順の代わりに、これが使用されます。 この新機能の詳細については、[Azure Stack Hub のネットワーク統合計画](azure-stack-network.md#permitted-networks)をご確認ください。

オペレーターは、いくつかのアクセス制御リスト (ACL) を変更して、信頼されたデータセンター ネットワーク範囲から、ネットワークデバイス管理インターフェイスおよびハードウェア ライフサイクル ホスト (HLH) にアクセスできるようにすることができます。 アクセス制御リストによって、オペレーターは、特定のネットワーク範囲内の管理ジャンプボックス VM に、スイッチ管理インターフェイス、HLH OS、および HLH BMC へのアクセスを許可することができます。

## <a name="next-steps"></a>次のステップ

[Azure Stack Hub とデータセンターの統合 - DNS](azure-stack-integrate-dns.md)
