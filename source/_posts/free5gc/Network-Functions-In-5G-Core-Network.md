---
title: Network Functions In 5G Core Network
date: 2021-10-11 19:07:49
author: 昌維 Chang Wei<changwei1006@gmail.com>
category: telecommunication network
tag: [Free5GC,Code Tracing,5G,5GC,5G核心网,5G核心網,云核心网, Core Network, Network Function]
---

# 5G 核心網路中的網路元件

Network Function 即 網路元件，指的是 5G 核心網路中負責執行各種功能的軟體程式

這些程式將透過 SBI(Service-Based Interface) 接口進行通訊

## 3GPP Release-15

3GPP Release-15 的 Specification 文檔對應編號為 `3GPP TR 21.915 V15.0.0 (2019-09) `     

其全名為 `Release 15 Description;  Summary of Rel-15 Work Items  (Release 15)`

在 3GPP Release-15 的 Specification 文檔中，`5.5.2 The 5G Core Network` 部分描述了 5G 核心網路中的各類 NF 對應的 specification 規格文檔，以及其主要功能

## Interface

在 5G 核心網路中，各類 NF 為應用程式軟體，而非傳統的硬體網路元件，因此這些應用程式軟體之間並沒有實際的物理接口進行互聯，而是透過網路的方式互相通訊

由於有可能存在一台物理伺服器上安裝多個 NF 並且運行，因此有可能多個 NF 共用同一張網路卡在同一個網段內進行通訊，因此稱 5G 核心網路的 NF 為總線型通訊，意味著多個 NF 的資料有可能在同一條網路線上進行傳輸

為了解釋方便起見，雖然 NF 都共用同一個網路線進行傳輸資料，但是我們仍然為 NF 之間定義一些邏輯接口 (logical interface) ，例如 

- Namf 為 AMF 這個 NF 對外的通訊接口
- N1 為 AMF 這個 NF 和 UE 之間的通訊接口

等等

## 協定

在  3GPP Release-15 中定義了其通訊協定為 HTTP2，並且 3GPP 使用 OpenAPI 3.0 對 NF 的標準 API 格式進行了定義，這些格式包括 DTO(Data Transport Object 數據傳輸對象)，以及 request / respone 應該具有的內容

其在 R15 原文中有如下定義

> The services provided by 5G NFs are designed as a set of APIs based on the following protocol stack:
>
> \-   the transport layer protocol is TCP as specified in IETF RFC 793;
>
> \-   transport layer security protection is supported with TLS;
>
> \-   the application layer protocol is HTTP/2 as specified in IETF RFC 7540;
>
> \-   the serialization protocol is JSON as specified in IETF RFC 8259;
>
> \-   the OpenAPI 3.0.0 is adopted as the Interface Definition Language.

## 網元與其協定

### TS 24.501      Non-Access-Stratum (NAS) protocol for 5G System (5GS); Stage 3

描述 NAS 協定

### TS 24.502      Access to the 5G Core Network (5GCN) via non-3GPP access networks; Stage 3

描述非 3GPP 如何接入 5G 核心網路

### TS 24.526      5G System -Phase 1, UE policy; CT WG1 Aspects

TODO，描述一些 UE 策略

### TS 23.527      5G System; Restoration Procedures; Stage 2

TODO

### TS 29.500      5G System; Technical Realization of Service Based Architecture; Stage 3

TODO

### TS 29.501      5G System; Principles and Guidelines for Services Definition; Stage 3

TODO

### TS 29.502      5G System; Session Management Services; Stage 3

描述 SMF 的規格

SMF 主要處理 UE 到 DN(Data Network) 的資料傳輸，以及為 UE 分配 IP 等操作

### TS 29.503      5G System; Unified Data Management Services; Stage 3

描述 UDM 的規格

UDM 更適合作為無狀態的資料獲取服務，例如緩存

### TS 29.504      5G System; Unified Data Repository Services; Stage 3

描述 UDR 的規格

UDR 更適合做有狀態的資料獲取服務，例如用於持久化存儲資料的資料庫

### TS 29.505      5G System; Usage of the Unified Data Repository services for Subscription Data; Stage 3

資料用量

### TS 29.507      5G System; Access and Mobility Policy Control Service; Stage 3

TODO

### TS 29.508      5G System; Session Management Event Exposure Service; Stage 3

做事件通知

### TS 29.509      5G System; Authentication Server Services; Stage 3

描述 AUSF 的規格

用於驗證 UE 是否被允許接入網路

### TS 29.510      5G System; Network Function Repository Services; Stage 3

描述 NRF 的規格

NRF 主要維護當前正在運行的 NF，主要做服務發現功能

### TS 29.511      5G System; Equipment Identity Register Services; Stage 3

UE 的標識註冊功能

### TS 29.512      5G System; Session Management Policy Control Service; Stage 3

描述 PCF 的規格

### TS 29.513      5G System; Policy and Charging Control signalling flows and QoS parameter mapping; Stage 3

TODO

### TS 29.514      5G System; Policy Authorization Service; Stage 3

TODO，驗證策略相關服務

### TS 29.518      5G System; Access and Mobility Management Services; Stage 3

描述 AMF 的規格

AMF 主要負責將 UE 連接至 5G 核心網路，他將透過 N1 接口連接至基地台 gNB，再由 gNB 透過空口連接至 UE

### TS 29.519      5G System; Usage of the Unified Data Repository Service for Policy Data, Application Data and Structured Data for Exposure; Stage 3

TODO

### TS 29.520      5G System; Network Data Analytics Services; Stage 3

TODO，網路資料分析服務

### TS 29.521      5G System; Binding Support Management Service; Stage 3

TODO

### TS 29.522      5G System; Network Exposure Function Northbound APIs; Stage 3

網路能力宣告，北向接口

### TS 29.531      5G System; Network Slice Selection Services; Stage 3

描述 NSSF 的規格

主要處理網路切片相關功能

### TS 29.540      5G System; SMS Services; Stage 3

描述 SMSF 的規格

主要處理 “簡訊” 相關服務，“簡訊” 在中國大陸地區稱作 “短信”

### TS 29.551      5G System; Packet Flow Description Management Service; Stage 3

TODO

### TS 29.554      5G System; Background Data Transfer Policy Control Service; Stage 3

TODO

### TS 29.561      5G System; Interworking between 5G Network and external Data Networks; Stage 3

TODO

### TS 29.571      5G System; Common Data Types for Service Based Interfaces; Stage 3

TODO

### TS 29.572      5G System; Location Management Services; Stage 3

TODO

### TS 29.594      5G System; Spending Limit Control Service; Stage 3

TODO

 

The corresponding Security aspects of 5G System are defined by SA3 (UID: 750016). The work also serves as the basis for related charging and management, i.e., Data Charging in 5G System Architecture Phase 1 (UID: 780035), Service Based Interface for 5G Charging (UID: 780034), Management and orchestration of 5G networks and network slicing (UID: 760066).



# Reference

- https://www.3gpp.org/release-15
- https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=3389
- https://www.3gpp.org/ftp/Specs/archive/21_series/21.915