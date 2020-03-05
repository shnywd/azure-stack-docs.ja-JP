---
title: 物理デバイスの監査を Azure Stack Hub データセンターと統合する
description: 物理デバイスのアクセス監査を、ご利用の Azure Stack Hub データセンターと統合する方法を学習します。
author: IngridAtMicrosoft
ms.topic: article
ms.date: 06/10/2019
ms.author: inhenkel
ms.reviewer: thoroet
ms.lastreviewed: 06/10/2019
ms.openlocfilehash: fe76a0cd0763f7a5d8b1eddcba61402806a15c11
ms.sourcegitcommit: 4ac711ec37c6653c71b126d09c1f93ec4215a489
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77699407"
---
# <a name="integrate-physical-device-auditing-with-your-azure-stack-hub-datacenter"></a>物理デバイスの監査を Azure Stack Hub データセンターと統合する

ベースボード管理コントローラー (BMC) やネットワーク スイッチなど、Azure Stack Hub 内のすべての物理デバイスで監査ログを出力します。 監査ログは、全体的な監査ソリューションに統合できまます。 デバイスは Azure Stack Hub の OEM ハードウェア ベンダーによって異なるため、監査の統合に関するドキュメントについては、ご利用のベンダーにお問い合わせください。 以下のセクションでは、Azure Stack Hub での物理デバイスの監査に関する一般的な情報が提供されます。  

## <a name="physical-device-access-auditing"></a>物理デバイスのアクセスの監査

Azure Stack Hub 内のすべての物理デバイスで、TACACS または RADIUS の使用がサポートされます。 サポート対象には、ベースボード管理コント ローラー (BMC) およびネットワーク スイッチへのアクセスが含まれます。

Azure Stack Hub ソリューションには、RADIUS と TACACS のどちらも組み込まれていません。 ただしこのソリューションは、市場で入手可能な既存の RADIUS または TACACS ソリューションの使用をサポートすることが検証済みです。

RADIUS に対してのみ、MSCHAPv2 が検証されました。 これは、RADIUS を使用する最も安全な実装です。 Azure Stack Hub ソリューションに含まれているデバイスで TACACS または RADIUS を有効にする場合は、OEM ハードウェア ベンダーにお問い合わせください。

## <a name="syslog-forwarding-for-network-devices"></a>ネットワーク デバイスの Syslog 転送

Azure Stack Hub 内のすべての物理ネットワーク デバイスで、syslog メッセージがサポートされます。 Azure Stack Hub ソリューションには、syslog サーバーは含まれていません。 ただしデバイスは、市場で入手可能な既存の Syslog ソリューションへのメッセージ送信をサポートすることが検証済みです。

Syslog の送信先アドレスは、デプロイ時に収集される省略可能なパラメーターですが、デプロイ後に追加することもできます。 お使いのネットワーク デバイスで Syslog 転送を構成するにあたっては、ご利用の OEM ハードウェア ベンダーに相談してください。

## <a name="next-steps"></a>次のステップ

[サービス ポリシー](azure-stack-servicing-policy.md)
