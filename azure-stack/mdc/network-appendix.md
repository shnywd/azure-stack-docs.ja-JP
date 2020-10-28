---
title: Modular Data Center (MDC) ネットワークの付録
description: MDC ネットワークに関する付録。
author: BryanLa
ms.author: bryanla
ms.service: azure-stack
ms.topic: conceptual
ms.date: 12/30/2019
ms.lastreviewed: 12/30/2019
ms.openlocfilehash: 10de658f7e5f8621d44baaa16f2181f58f47811e
ms.sourcegitcommit: e4e2cc6a68f02c3e856f58ca5ee51b3313c7ff8f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183455"
---
# <a name="mdc-network-appendix"></a>MDC ネットワークの付録

この付録には、MDC ハードウェアのデバイス パラメーターと ID に関する情報が記載されています。

## <a name="technical-device-parameters"></a>技術的なデバイス パラメーター

### <a name="cisco-93360yc-fx2--cisco-9348gc-fxp"></a>Cisco 93360YC-FX2 & Cisco 9348GC-FXP

| **機能** | **Cisco Nexus 93360YC-FX2** | **Cisco Nexus 9348GC-FXP** |
|---|---|---|
| Port | 96 x 1/10/25-Gbps および 12 x 40/100-Gbps QSFP28 ポート | 48 x 1-GBASE-T ポート、4 x 1/10/25-Gbps SFP28 ポート、および 2 x 40/100 QSFP28 ポート |
| サポートされる速度 | ダウンリンクでは 1/10/25-Gbps、アップリンクでは 40/100-Gbps、ブレークアウト対応ポート、97-108:4x10/25G | 100-Mbps と 1-Gbps の速度 |
| CPU | 4 コア | 4 コア |
| システム メモリ | 最大 24 GB | 24 GB |
| SSD ドライブ | 128 GB | 128 GB |
| システム バッファー | 40 MB | 40 MB |
| 管理ポート | 2 つのポート:1 つの RJ-45 と 1 つの SFP+ | 2 つのポート:1 つの RJ-45 と 1 つの SFP+ |
| USB ポート | 1 つ | 1 つ |
| RS-232 シリアル ポート | 1 つ | 1 つ |
| 電源 (最大 2 つ) | 1200 W 交互電流 (AC)、1200 W HVAC/HVDC | 350 W AC、440 W DC |
| 標準的な電源 (AC) | 404 W | 178 W |
| 最大電力 (AC) | 900 W | 287 W |
| 入力電圧 (AC) | 100 V から 240 V | 100 V から 240 V |
| 入力電圧 (高電圧 AC [HVAC]) | 100 V から 277 V | 90 V から 305 V |
| 入力電圧 (DC) | -40 V から -72 V | \-36 V から -72 V |
| 入力電圧 (高電圧 DC [HVDC]) | -240 V から -380 V | 192 V から 400 V |
| 周波数 (AC) | 50 Hz ～ 60 Hz | 50 Hz ～ 60 Hz |
| ファン | 3 つのファン トレイ | 3 |
| エアフロー | ポート側の吸気と排気 | ポート側の吸気と排気 |
| 物理的寸法 | 3.38 x 17.41 x 24.14 インチ (8.59 x 44.23 x 61.31 cm) | 1.72 x 17.3 x 19.7 インチ |
| (H x W x D) | | (4.4 x 43.9 x 49.9 cm) |
| 音響 | ファン速度 40% で 76.7 dBA、ファン速度 70% で 88.7 dBA、ファン速度 100% で 97.4 dBA | ファン速度 50% で 67.5 dBA、ファン速度 70% で 73.2 dBA、ファン速度 100% で 81.6 dBA |
| RoHS コンプライアンス | はい | はい |
| MTBF | 320,040 時間 | 257,860 時間 |
| 最小 ACI イメージ | ACI-N9KDK9-14.1.2 | ACI-N9KDK9-13.0 |
| 最小 NX-OS イメージ | NXOS-9.3(1) | NXOS-703I7.1 |

### <a name="juniper-mx204"></a>Juniper MX204

|  |  |  |
|--|--|--|
| Layout | システム容量 | 3 Tbps |
|        | スロットの向き | NA |
|        | 取り付け | 前面または中央 |
| 物理仕様 | 寸法 (W x H x D) | 17.45 インチ x 8.71 インチ x 24.5 インチ (44.3 x 22.1 x 62.2 cm) |
|                        | フル装備の重量 | 130 ポンド/59 kg |
|                        | 未装備の重量 | 52 ポンド/23.6 kg |
| ルーティング エンジン | 既定のメモリ | 2x16 MB NOR フラッシュ ストレージ、64 GB DDR4 RAM、2x50 GB SSD |
|                | コア数 | 6 コア |
| 冗長性 | コンポーネント | 電源、RE、ファン |
| 環境 | エア フロー | サイド ツー サイド |
|               | 操作温度 | 海抜ゼロ地点で 32° から 115° F (0° から 46° C) |
|               | 操作湿度 | 5% から 90% |
|               | 操作高度 | 10,000 フィート (3048 m) |
| 認定 | NEBS | • GR-1089-Core EMC and Electrical Safety |
|                | | • Common Bonding Network (CBN) |
|                | | • National Electrical Code (NEC) |
|                | | • GR-63-Core Physical Protection |

### <a name="dell-emc-s4148f-on"></a>Dell EMC S4148F-ON

機能:

- 48 10 ギガビット イーサネット SFP+ ポート
- 6 40 ギガビット イーサネット QSFP+ ポート
- 1 つの RJ45 コンソール/管理ポートと RS232 信号 
- 1 つの RJ45 マイクロ USB-B コンソール ポート 
- 1 つの RJ45 10/100/1000Base-T 管理イーサネット ポート 

仕様:

- サイズ: 
  - 1 つの RU、1.75 インチ (h) x 17 インチ (w) x 18 (d) インチ (4.4 cm (h) x 43.1 cm (w) x 45.7 cm (d)) 
  - S4112:1.7 インチ (h) x 8.28 インチ (w) x 18 インチ (d) (4.125 cm (h) x 20.9 cm (w) x 45 cm (d) 
- 電源:100-240 VAC 50/60 Hz
- S4412 に適用される電源 (DC): 定格 -40 VDC から -72 VDC
- システムあたりの最大消費電流:100/120V AC で 6A/5A、200/240V AC で 3A/2.5A
- S4112:100/120V AC で 2A/1.7A、200/240V AC で 1A/0.8A。 
- S4112 (DC): -40V/5A、-48V/4.2A、-72V/2.8A 

最大操作仕様:
- 操作温度:41° から 104° F (5° から 40° C)
- 操作湿度:5% から 85% (RH)、結露なきこと 

最大非操作仕様:
- ストレージの温度: -40° から 149°F (-40° C から 65° C) 
- ストレージの湿度:5% から 95% (RH)、結露なきこと


## <a name="identity"></a>ID

すべてのネットワーク デバイスは、ターミナル アクセス コントローラー アクセス制御システム (TACACS) を使用して動作するように設計されています。 TACACS によって、一元化された ID エクスペリエンスが提供されます。 TACACS サーバーは、初期構成の一部として指定する必要があります。

TACACS サーバーが使用できなくなると、デバイスは以下に示すローカル ID にフォールバックされることに注意してください。 通常の動作中は、これらのアカウントは無効になります。


次のユーザー アカウントによって、デバイスごとに終了が実行されます。

| Device | ユーザー名 | 既定のパスワード |
|---------|--------------|----------------------|
| Edge 1 | root | お客様による割り当て |
| Edge 2 | Root | お客様による割り当て |
| Tor 1 | Root | お客様による割り当て |
| Tor 2 | Root | お客様による割り当て |
| BMC | Root | お客様による割り当て |
| PDU 1 | root | お客様による割り当て |
| PDU 2 | root | お客様による割り当て |
| シリアル | Root | お客様による割り当て |
| Avocent | Root & Admin | お客様による割り当て |
