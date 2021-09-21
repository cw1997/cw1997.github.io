---
title: 3GPP Release-15 閱讀筆記
category: telecommunication network
date: 2021-09-21 05:02:14
tag: [5G,3GPP,Release-15,Release15,R15,Rel-15]
---

# 3GPP Release-15 閱讀筆記

3GPP Release-15 簡稱 3GPP R15 或者 R15

官方介紹： https://www.3gpp.org/release-15

規格詳情頁： https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3389

15.0.0 版本鏈接： https://www.3gpp.org/ftp//Specs/archive/21_series/21.915/21915-f00.zip （發佈日期 2019-10-01）截至寫稿時（2021-09-21）該版本為 3GPP Release-15 最新版本

## 簡介

3GPP Release-15 為 5G 的第一個 3GPP 規格

R15 定義了 5G 系統的 Phase 1

R16 將定義 5G 系統的 Phase 2

## The 5GS service aspects（5G 服務場景）

在 SMARTER 工作組下，SA1 定義了 5G 的需求和新服務

TS 22.261 描述了不同需求類型對應的 5G 使用場景

主要包括：

* **Enhanced Mobile Broadband (eMBB)**
* **Critical Communications (CC) and Ultra Reliable and Low Latency Communications (URLLC)**
* **Massive Internet of Things (mIoT)**
* **Flexible network operations**

一些 5G 能夠應用的垂直行業領域包括：

- Automotive and other transport (trains, maritime communications)
- Transport, logistics, IoT
- Discrete automation
- Electricity distribution
- Public Safety
- Health and wellness
- Smart cities
- Media and entertainment

其中還有一些垂直行業在 TS 22.261 中有提及

還有一些方面，例如 鐵路，eV2X 和相關聯的需求在 TS 22.186 中有提及

## 5G 兼容性

5G 除了支持新的 5G 標準服務以外，還支持所有在

- TS 22.278

- TSs 22.011
- 22.101
- 22.185
- 22.071
- 22.115
- 22.153
- 22.173

中定義的 EPS(4G) 功能

### References

[1]             TS 22.261, "Service requirements for the 5G system".

[2]             TS 22.278, "Service requirements for the Evolved Packet System (EPS)".

[3]             TS 22.011, "Service accessibility".

[4]             TS 22.101, "Service aspects; Service principles".

[5]             TS 22.185, "Service requirements for V2X services".

[6]             TS 22.071, "Location Services (LCS); Service description".

[7]             TS 22.115, "Service aspects; Charging and billing".

[8]             TS 22.153, "Multimedia priority service".

[9]             TS 22.173, "IP Multimedia Core Network Subsystem (IMS) Multimedia Telephony Service and supplementary services".

[10]            TS 22.186, "Service requirements for enhanced V2X scenarios".

## NSA 非獨立組網架構 VS SA 獨立組網架構

### Non-Stand Alone(NSA) 非獨立組網架構

5G RAN 和 NRF 接口連接至 LTE 和 EPC 核心網（分別對應4G Radio 和 4G Core）

這樣可以充分利用 5G NR，但是又無需替換核心網

這種情況下只能使用 4G 服務，但是又可以享受到 NR 帶來的低延遲能力

The NSA is also known as "E-UTRA-NR Dual Connectivity (EN-DC)" or "Architecture Option 3". See also the clause on EDCE5.

In EN-DC, the 4G's eNB is the Master Node (MN) while the 5G's en-gNB is the Secondary Node (SN).

![NSA Architecture](./NSA_Architecture.png)

### Stand Alone(SA) 獨立組網架構

5G NR(New Radio) 連接至 5G CN(Core Network)

這種配置下，全部的 5G Phase 1 都將被支持

NRF 基站（邏輯節點gNB）之間使用 Xn 接口互聯

並且 AN(Access Network (called the "NG-RAN for SA architecture")) 使用 NG 接口連接至 5GC

![SA Architecture](./SA_Architecture.png)

## Core Network 核心網概述

5G 獨立組網系統中，5G System（5GS）由

- User Equipment
- Access Network（New Radio，NRF）
- Core Network

組成

之前提到的服務需求將作為架構定義的基礎

TR 23.799 開始定義了架構規格（stage 2），也稱為 NextGen TR

之後在

- TS 23.501
- TS 23.502
- TS 23.503

中完全定義

### Service-Based Architecture (SBA)

5G 架構依賴於 SBA 框架

一系列的 Network Function 替代了 傳統 Network Entities 組成架構內的元素

通過公共框架的一些接口，任何給定的 NF （服務提供者）都可以提供服務給被授權的其他 NFs 或者服務消費者。

![5G_System_architecture](./5G_System_architecture.png)

在當前 stage，只有如下的 NF（Network Function）和元素將被重點提到

- The User Equipment (UE);
- The (Radio) Access Network [(R)AN];
- The User Plane Function (UPF), handling the user data;
- The (external) Data Network (DN);
- Some remarkable Network Functions (NFs): 
  - The Application Function (AF), handling the application(s);
  - The Access and Mobility management Function (AMF), that accesses the UE and the (R)AN;
  - The Session Management Function (SMF), that accesses the UPF.

其他 NF 之後會提到

#### 特性

SBA 服務允許虛擬化部署

服務可以分佈式部署，做冗餘或者彈性伸縮

同一功能可以由多個 NF 同時提供（冗餘）並且部署在多個位置

UE 的消息將被路由至任何一個具有處理能力的 NF

因此 NF 必須盡可能保持無狀態特性，這樣才能過很方便地水平擴展或者允許請求消息被任意路由至不同的 NF

由於 NF 提供了冗餘能力，因此任何指定的 NF 都可以有計劃性的進行停機維護，當維護完畢後他們也可以自動恢復

## Access Network 接入網概述

5G AN 架構非常簡單，因為它只包含一個實體，也就是 gNB

- gNB 通過 NG Interface 連接至 5G CN
- gNB 通過 Xn Interface 連接至 5G's gNB
- gNB 通過 X2 Interface 連接至 4G's eNB

![AN_interfaces](./AN_interfaces.png)

### References for 5GS Stage 2

The main Stage 2 specifications for the 5G System are:

[1]             TS 23.501, "System Architecture for the 5G System", Stage 2. It specifies the overall 5GS Stage 2: the architecture reference model, including the network functions and the description of high level functions.

[2]             TS 23.502, "Procedures for the 5G System", Stage 2. It specifies the 5GS Stage 2 for roaming and non-roaming scenarios, for the policy and charging related control framework.

[3]             TS 23.503, "Policy and Charging Control Framework for the 5G System", Stage 2. It is the companion specification to TS 23.501 and TS 23.503, and specifies the Stage 2 procedures and Network Function Services.

[4]             TR 23.799 "Study on Architecture for Next Generation System", Stage 2

[5]             TS 38.401 " NG-RAN; Architecture description"

[6]             TS 38.420 " NG-RAN; Xn general aspects and principles"

[7]             TS 36.401 "Evolved Universal Terrestrial Radio Access Network (E-UTRAN); Architecture description"

### Abbreviation applicable to this section:

NR             New Radio (5G Radio)

NSA            Non Stand-Alone

PBCH          Physical Broadcast Channel

PDCCH         Physical Downlink Control Channel

PDSCH         Physical Downlink Shared Channel

PRACH         Physical Random Access Channel

PSS            Primary Synchronisation Signal

PUCCH         Physical Uplink Control Channel

PUSCH         Physical Uplink Shared Channel

SA             Stand-Alone

SSS            Secondary Synchronisation Signal

