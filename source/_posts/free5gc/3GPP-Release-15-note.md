---
title: 3GPP Release-15 閱讀筆記
category: telecommunication network
date: 2021-09-21 05:02:14
tag: [5G,3GPP,Release-15,Release15,R15,Rel-15]
---

# 簡介

3GPP Release-15 簡稱 3GPP R15 或者 R15

官方介紹： https://www.3gpp.org/release-15

規格詳情頁： https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3389

15.0.0 版本鏈接： https://www.3gpp.org/ftp//Specs/archive/21_series/21.915/21915-f00.zip （發佈日期 2019-10-01）截至寫稿時（2021-09-21）該版本為 3GPP Release-15 最新版本

3GPP Release-15 為 5G 的第一個 3GPP 規格

R15 定義了 5G 系統的 Phase 1

R16 將定義 5G 系統的 Phase 2

# The 5GS service aspects（5G 服務場景）

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

# 5G 兼容性

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

## References

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

# NSA 非獨立組網架構 VS SA 獨立組網架構

## Non-Stand Alone(NSA) 非獨立組網架構

5G RAN 和 NRF 接口連接至 LTE 和 EPC 核心網（分別對應4G Radio 和 4G Core）

這樣可以充分利用 5G NR，但是又無需替換核心網

這種情況下只能使用 4G 服務，但是又可以享受到 NR 帶來的低延遲能力

The NSA is also known as "E-UTRA-NR Dual Connectivity (EN-DC)" or "Architecture Option 3". See also the clause on EDCE5.

In EN-DC, the 4G's eNB is the Master Node (MN) while the 5G's en-gNB is the Secondary Node (SN).

![NSA Architecture](./NSA_Architecture.png)

## Stand Alone(SA) 獨立組網架構

5G NR(New Radio) 連接至 5G CN(Core Network)

這種配置下，全部的 5G Phase 1 都將被支持

NRF 基站（邏輯節點gNB）之間使用 Xn 接口互聯

並且 AN(Access Network (called the "NG-RAN for SA architecture")) 使用 NG 接口連接至 5GC

![SA Architecture](./SA_Architecture.png)

# Core Network 核心網概述

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

## Service-Based Architecture (SBA)

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

### 特性

SBA 服務允許虛擬化部署

服務可以分佈式部署，做冗餘或者彈性伸縮

同一功能可以由多個 NF 同時提供（冗餘）並且部署在多個位置

UE 的消息將被路由至任何一個具有處理能力的 NF

因此 NF 必須盡可能保持無狀態特性，這樣才能過很方便地水平擴展或者允許請求消息被任意路由至不同的 NF

由於 NF 提供了冗餘能力，因此任何指定的 NF 都可以有計劃性的進行停機維護，當維護完畢後他們也可以自動恢復

# Access Network 接入網概述

5G AN 架構非常簡單，因為它只包含一個實體，也就是 gNB，具體可以參考 TS 38.401

- gNB 通過 NG Interface 連接至 5G CN
- gNB 通過 Xn Interface 連接至 5G's gNB
- gNB 通過 X2 Interface 連接至 4G's eNB
- gNB 通過 NR interface 連接至 UE

![AN_interfaces](./AN_interfaces.png)

AN 架構在原理上和 4G  LTE 和 eNB 組網的方式相近，可以參考 TS 36.401 和 TS 38.420

## References for 5GS Stage 2

The main Stage 2 specifications for the 5G System are:

[1]             TS 23.501, "System Architecture for the 5G System", Stage 2. It specifies the overall 5GS Stage 2: the architecture reference model, including the network functions and the description of high level functions.

[2]             TS 23.502, "Procedures for the 5G System", Stage 2. It specifies the 5GS Stage 2 for roaming and non-roaming scenarios, for the policy and charging related control framework.

[3]             TS 23.503, "Policy and Charging Control Framework for the 5G System", Stage 2. It is the companion specification to TS 23.501 and TS 23.503, and specifies the Stage 2 procedures and Network Function Services.

[4]             TR 23.799 "Study on Architecture for Next Generation System", Stage 2

[5]             TS 38.401 " NG-RAN; Architecture description"

[6]             TS 38.420 " NG-RAN; Xn general aspects and principles"

[7]             TS 36.401 "Evolved Universal Terrestrial Radio Access Network (E-UTRAN); Architecture description"

## Abbreviation applicable to this section:

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

# Radio 和 Core 之間的功能劃分

進一步了解 AN 和 CN 提供的功能

![The_5G_System_architecture](./The_5G_System_architecture.svg)

AN 和 CN 之間交換數據是通過 CN 側的 AMF，UPF 和 SMF，而 AN 側則是 gNB

![Functional_Split_between_NG-RAN_and_5GC](./Functional_Split_between_NG-RAN_and_5GC.svg)

黃色方框為 NF，白色方框為其所負責的任務

## CN 側

所有和用戶數據無關的信令都將經過 AMF ("Access and Mobility management Function") ，例如可移動性或者安全性

SMF ("Session Management Function") 負責與用戶數據有關的信令，例如會話的建立

UPF 負責處理用戶數據

## AN 側

gNB (5G Node B) 執行所有 AN 相關的主要任務，包括

* 無線電資源管理
* 無線電承載控制
* 無線電權限控制
* 移動連接控制
* 為 UE 動態分配資源

# 5G 核心網

## NF (Network Function)

### AMF (Access and Mobility management Function)

支援不同移動性管理需求的 UE

它主要處理如下工作

* Non-Access Stratum (NAS) 信令處理
* NASA 信令安全
* Access Stratum (AS) 安全控制
* CN 節點之間處理 3GPP AN 的信令
* 空閒模式下 UE 設備的可達性，包括控制和控制重傳
* 區域管理註冊
* 支援系統內外的可移動性
* 訪問認證
* 檢測漫遊權限
* 支援網路切片
* SMF 選擇

### SMF (Session Management Function)

和 AMF 共同配合，客製化移動管理模式，例如 "Mobile Initiated Connection Only" (MICO) 僅允許移動連接初始化或者 RAN enhancements RAN 增強，類似於 RRC Inactive 狀態

它主要處理如下任務

* 會話管理
* UE IP 地址分配和管理
* UPF 的選擇與控制
* 為 UPF 設定流量轉發策略，將數據包路由到正確位置
* 策略，QoS
* 下行數據通知

### UPF (User Plane Function)

主要負責的處理任務

* RAT 內外的可移動性的錨點

* 外部 PDU 會話與 Data Network 互聯
* 封包路由和轉發
* 封包偵測與用戶面的部分策略規則
* 流量報告
* 上行流量分類，將流量路由到數據網路
* 支援多宿主的 PDU 會話分支點
* 用戶面的 QoS 處理，包括封包過濾，門控，上下行速率控制
* 上行封包校驗（SDF 到 QoS flow 的映射）
* 下行封包緩衝和下行數據通知觸發

### Network Repository Function" (NRF)

提供 NF 服務管理，包括

- 服務註冊
- 服務註銷
- 服務驗證
- 服務發現

### Network Exposure Function" (NEF)

對外輸出 NF 的處理能力數據

這些“能力”包括

* 監控能力
* 供應能力
* 路由流量的應用影響能力
* 策略與計費能力

### Unified Data Management" (UDM)

5GC 資源的數據存儲架構為“計算和存儲分離”

Unified Data Repository (UDR) 是主要數據庫

Unstructured Data Storage Function (UDSF) 存儲非結構化的動態數據

## 5G 核心網路標準與相關的 Network Functions

### 託管服務與邊緣計算 Local hosting of services and Edge Computing

"Service Hosting Environment"，中文譯名“服務託管環境”，是位於運營商內部的網路，5G 使用該網路支援多種類型的服務

為了滿足

- 降低延遲
- 降低帶寬壓力

等目標，這些 Hosted Services（託管服務） 將建置在靠近用戶端的位置

Hosted Services（託管服務） 包含運營商，以及可信任的第三方服務

Hosted Services（託管服務） 支援靈活的用戶面路由策略，當用戶在通訊會話中移動位置時，其允許為了提升用戶體驗和網路負載均衡的情況下改變路由路徑

為了利用邊緣計算節點中的託管服務降低延遲，5G 核心網路將會選擇靠近 UE 的 UPF ，並且通過 N6 Interface 自行 UPF 到本地網路的路由策略

一部分邊緣計算節點將具有如下特性：

- 本地與遠端服務可以同時訪問數據網路，這是構成低延遲網路的方案
- 應用可以主動影響路由策略
- 支援 URLLC (Ultra Reliable Low-Latency) 超低延遲服務
- 支援本地區域數據網路

### 網路切片 Network slicing

網路切片是為提供某種特定類型服務的網路而命名的一種元素

例如，可以分別為 IoT 設備，經典 UE 手機設備，V2X 等設備分別提供網路切片

通常情況下，不同的服務功能具有不同的網路要求

包括優先級要求和策略控制要求

網路能效要求，包括延遲，移動性，傳輸速率

或者這些服務僅僅針對特定的用戶類型，例如 MPS 用戶群，公共安全用戶，企業客戶，漫遊客戶，託管的 MVNO

不同的網路切片可以同時使用

端到端的網絡切片是 5G 系統的一大特點。在與其他 PLMN 互操作所需的範圍內，每個已經部署的 PLMN 都支持它

例如運營商 A 的物聯網切片可以直接與運營商 B 的物聯網切片互聯，運營商可以根據業務場景決定部署多少個網絡切片以及跨多個切片共享哪些功能/特性

每個切片的特性根據 QoS、比特率、延遲等進行定義

對於給定的切片，這些特性要么在 3GPP 標準中預先定義，要么由運營商定義

預定義切片分為三種類型：

- 類型 1 專用於支援 eMBB
- 類型 2 用於 URLLC
- 類型 3 用於 MIoT 支持

這些預定義的網路切片允許在 PLMN 間互相操作，同時減少運營商之間的協調工作

對於運營商定義的切片，它們可以在網絡切片之間實現更多差異化

為切片處理引入了專用 NF：“網絡切片選擇功能”（"Network Slice Selection Function" (NSSF)），它可以選擇適當的切片

UE 可以同時使用多個網絡切片。

UE 中的網絡切片選擇策略將應用程序鏈接到網絡切片

網絡切片還支持漫遊場景

最後，網絡切片可以與 EPS 互通，無論器是否支援 4G's Dedicated Core Networks Selection Mechanism (e)DECOR

### 統一接入控制 Unified access control

當在 5G 系統中發生網路擁塞，會使用不同的標準決定是否應該阻止或允許訪問

標準取決於運營商策略，部署方案，訂閱客戶資料以及可用服務

5G 系統提供單一的統一訪問控制，運營商根據這些與所謂的“訪問身份和訪問類別”相關聯的標準來控制接入與否

5G 系統也提供

- 基於移動性管理擁塞控制
- 基於 DNN 的擁塞控制
- 基於切片的擁塞控制

### 支援 3GPP 與 非 3GPP 的接入

5G 系統支援 3GPP 技術雞兒，包括一種或多種 5G NR 以及 4G's E-UTRA

他也支援非 3GPP 技術接入，甚至包括不可信設備

為了優化和資源效率，5G 系統能夠為一個服務選擇最適當的 3GPP 或者 non-3GPP 技術進行接入

潛在地允許多種接入技術同時用於 UE 上活動的一項或多項服務

還支援不同接入之間的無縫移動

名為 "Authentication Server Function "(AUSF) 的 NF 允許一個統一框架支援 3GPP 和 non-3GPP 接入

當同時透過 3GPP 和non-3GPP 接入時，5G Globally Unique Temporary Identifier (5G-GUTI) 將唯一標識一個 UE

### 策略框架與 QoS 支援

策略框架用於支援

- 會話
- 接入
- 移動性控制
- QoS
- 計費

和 UE 中分發的策略類似

UE 使用兩種機制連接到 QoS 和策略

- UE 路由選擇策略，政策一個應用是否可以被關聯到已經存在的 PDU 會話，能夠卸載到 PDU 會話外部的 non-3GPP 訪問，能夠觸發簡歷一個新的 PDU 會話
- 接入網路發現與策略選擇，用於選擇非 3GPP 訪問

在網絡中，使用名為“網絡數據分析功能”（NWDAF）的 NF 來提供數據分析支持，即提供每個網絡切片的負載

對於QoS，系統定義了一個基於流的QoS框架，有兩種基本模式：有或沒有QoS專用信令

對於沒有任何特定 QoS 信令流的選項將會應用標準化的數據包標記，用於通知應該提供何種 QoS 

帶有 QoS 專用協商的選項提供了更大的靈活性和更細粒度的 QoS 支持。

此外，還引入了一種新的 QoS 類型：“反射 QoS”，其中 UE 請求上行鏈路流量與其接收的下行鏈路流量相同的 QoS 規則。 在這種模式下，通過最少的控制平面信令支援下行鏈路和上行鏈路上的對稱 QoS 差異

### 網路容量公開

運營商公開網路容量（例如 QoS 策略）給第三方 ISP ICP 的機制包括

- Service Exposure and Enablement Support (SEES)
- (enhanced) Flexible Mobile Service Steering ((e)FMSS)

在 5G 網路中，新的網路容量將被公開給第三方，例如允許第三方針對不同使用案例客製化一個特定的網路切片

為提升用戶體驗，允許第三方在可行的託管服務環境中管理第三方應用，有效利用回源流程和用戶資源

關於網路容量公開的更多部分，請參考 Northbound APIs

### 其他特殊服務

5GS 中還支援其他一些特殊服務，包括

#### Short Message Service (SMS)

支援 SMS over NAS，包括 SMS over non-3GPP 接入

#### IP-Multimedia Subsystem (IMS)

大部分 5G 部署初級階段並不可用，如果連接在沒有 IMS 的 5GS 的 UE 設備執行了 IMS 業務，將會觸發網路切換到關聯的 EPS 或者適當的 RAT

這也適用於 IMS 緊急服務

#### Multi-Operator Core Network (MOCN)

一個 RAN 被多個 Core Network 共享使用

#### Public Warning System (PWS)

在 Cell Broadcast Centre (CBC) Function (CBCF) 和 AMF 之間透過服務化接口互聯進行支援

#### Multimedia Priority Services (MPS)

透過 MPS-specific exemptions 支援 5GS 移動性管理與會話管理

#### Mission Critical Services (MCS)

透過訂閱 5G QoS 配置和必要的策略進行支援

一些標準的 QoS 指標在未來將為 MCS 定義

#### PS Data Off

向後兼容

提供：

- Control Plane Load Control

- Congestion and Overload control‘’

包括

- AMF Load-rebalancing
- TNL (Transport Network Layer between 5GC and 5G-AN) Load (re-)balancing

和

- AMF Overload Control

- SMF Overload Control

類似

### 其他 5G 規格

當漫遊在 VPLMN 中，UE 在 VPLMN 中的漫遊策略允許 HPLMN 為 UE 提供和更新一個首選 PLMN 或者接入技術的組合。這些行為配置透過預先在 USIM 卡中寫入或者透過 NAS 信令的方式提供

## 核心網路協定

摘要基於中國移動、諾基亞、愛立信、華為在 SP-180595 中提供的內容

5G NF 提供的服務被設計為一組基於以下協議棧的 API：

- 傳輸層協議是 IETF RFC 793 中規定的 TCP
- 使用 TLS 支持傳輸層安全保護
- 應用層協議是 IETF RFC 7540 中規定的 HTTP/2
- 序列化協議是 IETF RFC 8259 中指定的 JSON
- 採用 OpenAPI 3.0.0 作為接口定義語言

### 協議棧

- Application
- HTTP/2
- TLS
- TCP
- IP
- L2

為了減少客戶端和服務器之間的耦合，API設計採用RESTful框架如下：

- the REST-style service operations implement the Level 2 of the Richardson maturity model
- Level 3 (HATEOAS) of the Richardson maturity model is optional

OAuth2（在 IETF RFC 6749 中指定）用於授權 NF 訪問其他服務，NRF 充當授權服務器

基於服務的接口還支援過載控制和優先級消息

在 N4 Interface 上使用 PFCP（Packet Forwarding Control Protocol），用於在 5GC 中分離 Control Plane 和 User Plane。這與 EPC 中 CUPS 支持的協議相同，但有一些擴展以支持所有 5GC 要求（例如以太網流量、QoS 流）

GTPv2 通過 N26 Interface 用於 EPC 和 5GC 之間的移動性。這與 EPC 中通過 S10 支持的協議相同，只需進行最少的擴展即可支持 5GS 要求（例如 5GS TAI、gNB ID）

對於與外部 DN（即 N6 Interface）的 5G 網絡互通，TS 29.061 中規定的那些協議（IP、non-IP、DHCP、RADIUS 和 Diameter 協議）仍然適用於 SMF/UPF 和外部 DN 之間的資料傳輸，並盡可能適配他們。此外，SMF/UPF 還支持以太網流量與外部 DN 互通。

## Reference

**References**

The main protocols of the 5G Core Network are specified in:

TS 24.501      Non-Access-Stratum (NAS) protocol for 5G System (5GS); Stage 3

TS 24.502      Access to the 5G Core Network (5GCN) via non-3GPP access networks; Stage 3

TS 24.526      5G System -Phase 1, UE policy; CT WG1 Aspects

TS 23.527      5G System; Restoration Procedures; Stage 2

TS 29.500      5G System; Technical Realization of Service Based Architecture; Stage 3

TS 29.501      5G System; Principles and Guidelines for Services Definition; Stage 3

TS 29.502      5G System; Session Management Services; Stage 3

TS 29.503      5G System; Unified Data Management Services; Stage 3

TS 29.504      5G System; Unified Data Repository Services; Stage 3

TS 29.505      5G System; Usage of the Unified Data Repository services for Subscription Data; Stage 3

TS 29.507      5G System; Access and Mobility Policy Control Service; Stage 3

TS 29.508      5G System; Session Management Event Exposure Service; Stage 3

TS 29.509      5G System; Authentication Server Services; Stage 3

TS 29.510      5G System; Network Function Repository Services; Stage 3

TS 29.511      5G System; Equipment Identity Register Services; Stage 3

TS 29.512      5G System; Session Management Policy Control Service; Stage 3

TS 29.513      5G System; Policy and Charging Control signalling flows and QoS parameter mapping; Stage 3

TS 29.514      5G System; Policy Authorization Service; Stage 3

TS 29.518      5G System; Access and Mobility Management Services; Stage 3

TS 29.519      5G System; Usage of the Unified Data Repository Service for Policy Data, Application Data and Structured Data for Exposure; Stage 3

TS 29.520      5G System; Network Data Analytics Services; Stage 3

TS 29.521      5G System; Binding Support Management Service; Stage 3

TS 29.522      5G System; Network Exposure Function Northbound APIs; Stage 3

TS 29.531      5G System; Network Slice Selection Services; Stage 3

TS 29.540      5G System; SMS Services; Stage 3

TS 29.551      5G System; Packet Flow Description Management Service; Stage 3

TS 29.554      5G System; Background Data Transfer Policy Control Service; Stage 3

TS 29.561      5G System; Interworking between 5G Network and external Data Networks; Stage 3

TS 29.571      5G System; Common Data Types for Service Based Interfaces; Stage 3

TS 29.572      5G System; Location Management Services; Stage 3

TS 29.594      5G System; Spending Limit Control Service; Stage 3

 

The corresponding Security aspects of 5G System are defined by SA3 (UID: 750016). The work also serves as the basis for related charging and management, i.e., Data Charging in 5G System Architecture Phase 1 (UID: 780035), Service Based Interface for 5G Charging (UID: 780034), Management and orchestration of 5G networks and network slicing (UID: 760066).
