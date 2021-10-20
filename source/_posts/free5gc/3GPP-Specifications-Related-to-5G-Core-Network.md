---
title: 3GPP Specifications Related to 5G Core Network
date: 2021-10-17 16:39:48
author: 昌維 Chang Wei<changwei1006@gmail.com>
category: telecommunication network
tag: [Free5GC,Code Tracing,5G,5GC,5G核心网,5G核心網,云核心网, Core Network, Network Function]
---

# 5G 核心網路與其相關的之 3GPP Specs 介紹

5G 行動通訊(telecommunication)系統分為 

- UE(User Equipment)，用戶的 5G 手機，5G IoT 設備等 
- AN(Access Network)，NR(New Radio) 5G 新空口，主要是基地台(基站，Base Station)部分
- CN(Core Network) ,5GC，5G 核心網路，包括很多服務化網路元件(NF, Network Function)

我的研究工作主要側重在 CN 部分，因此關注點集中在 5GC 相關部分，當然由於 5G 系統本身是一個整體，不可避免也會接觸到 AN 和 UE 部分的內容

## 3GPP

關於 3GPP 是什麼組織可以直接參考 https://www.3gpp.org/about-3gpp/about-3gpp

簡而言之就是技術標準化定義組織，通過定期開會和討論確定移動通訊技術標準，便於各類移動通訊所用的設備可以在同一個標準下互相連接

## 全部 3GPP Specs 檔案

https://www.3gpp.org/ftp/Specs/archive/

### 如果需要查看最新版可以直接在這裡搜尋

https://www.3gpp.org/ftp/Specs/latest

這裡的資料可能會根據 3GPP 相關的會議等行為而發生變化

目前 3GPP 的 5G 技術標準已經更新至 Release-16 因此可以直接參考 https://www.3gpp.org/ftp/Specs/latest/Rel-16

## R15 與 R16 對應的 OpenAPI yaml 定義文件

https://www.3gpp.org/ftp/Specs/archive/OpenAPI

## Release 系列的 Summary 部分

### Release-15

3GPP TR 21.915

https://www.3gpp.org/ftp/Specs/latest/Rel-15/21_series 

Release 15 Description;  Summary of Rel-15 Work Items  (Release 15)  

主要對 Rel-15 的工作進行總結

### Release-16

3GPP TR 21.916

https://www.3gpp.org/ftp/Specs/latest/Rel-16/21_series 

Release 16 Description;  Summary of Rel-16 Work  Items    (Release 16)  

主要對 Rel-16 的工作進行總結

## 5G 系統架構介紹

3GPP TS 23.501 System architecture for the 5G System (5GS)

https://www.3gpp.org/ftp/Specs/latest/Rel-16/23_series 

這部分是從整個宏觀角度介紹 5G 系統的架構和組成部分，是非常重要的學習資料，也是理解 5G 系統中有哪些設備，這些設備如何協同工作的重點資料，對於從事 5G 通訊研究的人是非常好的學習資料

筆者也看過很多市面上所謂的 5G 通訊培訓相關之付費學習資料或者教學錄影，幾乎都是按照這篇文章翻譯成中文後講解的，所以如果需要學習最原始的權威資料，直接看這邊的內容即可。當然內容也確實有點多，大概 500 頁左右

## 5G 業務服務介紹

5G 包含很多業務，比如

- 語音通話(IMS, IP Multimedia Subsystem)，對應 Voice over New Radio(VoNR)
- 簡訊，短信息(SMS, Short Message Service)

