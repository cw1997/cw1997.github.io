---
title: Tracing Source Code of AMF of Free5GC
category: telecommunication network
date: 2021/8/29 11:00:01
tag: [Free5GC,Code Tracing,5G,5GC,5G核心网,5G核心網,云核心网,AMF]
---

# AMF v3.0.5

## Call Flow

* main.go
    * 基於urfave/cli作為CLI程式的參數處理工具
    * 基於sirupsen/logrus為日誌系統
    * 設定app名稱，參數使用方法usage的顯示，參數flag的獲取，參數動作的routing
    * 判斷app運行後是否出錯，若有錯誤直接退出並且打日誌
    * 將action掛載到app cli程式上，action是程式的主體邏輯與動作
        * 執行AMF.Initialize，若有錯誤則打日誌
            * 執行factory.InitConfigFactory(amfcfg)讀取amfcfg文件中的配置信息（根據是否傳入amfcfg這個flag決定從何處讀取配置文件）
            * 根據配置文件中的log level，對logrus進行正確的log level設定
            * 檢測配置版本是否為AMF_EXPECTED_CONFIG_VERSION（在v3.0.5中該值為1.0.2），若不匹配則throw err
        * 執行AMF.Start開始運行
            * 使用logger_util.NewGinWithLogrus創建了一個基於Gin和Logrus的REST HTTP API Server Middleware（背景知識：Gin為golang語言中的高性能HTTP REST API Server框架，Logrus為golang語言中的日誌框架，Middleware將會攔截每個請求，並在請求處理的之前（pre）和之後（post）執行相關動作）
                * 這裡實際將如下訊息打印到日誌中
                  * path
                  * query body
                  * client ip
                  * request method
                  * response status
                  * error message
            * 使用router.Use(cors.New(cors.Config{}))創建middleware，用於支援CORS請求（Cross-origin resource sharing），CORS是在傳統瀏覽器環境下需要為“同源策略”放行的規則，為了相容類似於Web Console等需要使用Web Browser訪問某些free5gc中的HTTP API的場景，因此需要設定該CORS規則。
            * 將httpcallback的router註冊到URL為`/namf-callback/v1`的分組上（部分細節待確認）
            * 將oam的router註冊到URL為`/namf-oam/v1`的分組上（部分細節待確認）
                * TODO and future work
                    * 此處有coding style問題，例如文件名未統一為route或者routes
                    * 此處有專案目錄結構問題，例如未把service層和controller層規劃在一個專門的package下，導致專案package結構不夠清晰明確（通常在Java Web等專案中都會有統一的controller，service，modle等分層架構）
            * 從AMF配置文件的AmfConfig.Configuration.ServiceNameList部分讀取服務名稱列表，並且將對應的router註冊到對應的URL
                * 其Configuration Temple File中(/config/amfcfg.yaml)包含如下配置
            ```
                    serviceNameList: # the SBI services provided by this AMF, refer to TS 29.518
                        - namf-comm # Namf_Communication service
                        - namf-evts # Namf_EventExposure service
                        - namf-mt   # Namf_MT service
                        - namf-loc  # Namf_Location service
                        - namf-oam  # OAM service
            ```
            * 其程式碼中只包含如下服務的註冊流程
                * ServiceName_NAMF_COMM
                * ServiceName_NAMF_EVTS
                * ServiceName_NAMF_MT
                * ServiceName_NAMF_LOC
                
            * `self := context.AMF_Self()` 將 /amf/context/context.go@struct context賦值給self並且調用 /amf/util/init_context@InitAmfContext
                * 該流程中主要是讀取配置文件並且將配置信息注入struct context中，對不合法的配置項設定缺省值（default value）
                * context 包含 NFs/amf/context/context.go@struct AMFContext和一些methods
                
            * `addr := fmt.Sprintf("%s:%d", self.BindingIPv4, self.SBIPort)` 組裝IPv4和port為inet addr格式的字符串
            
            * 在NgapIpList配置和38412端口運行ngap服務
                * 該服務包含的handler
                    * ngap.Dispatch
                    * ngap.HandleSCTPNotification
                
            * 將本服務即AMF註冊至NRF (Register to NRF)
                * 調用 /consumer/BuildNFInstance() 構建 AMF 需要在 NRF 中註冊的資料結構(一個 type為 NfProfile 的 model 對象)
                * 註冊 NF 後獲取 nfId 並且寫入當前 context 中
                
            * 偵聽 os signal，當signal 為 interrupt 時執行 amf.Terminate() 並且退出進程，返回 0 號錯誤碼給 OS
            
                * amf.Terminate()
            
                    * ```
                        TODO: forward registered UE contexts to target AMF in the same AMF set if there is one
                        ```
            
                        根據此項 TODO 看，未來工作中將要引入自動 AMF 遷移機制，當一個 AMF 停機後，需要遷移連接至該 AMF 的所有 UE 到其他 AMF 上，實現自動故障轉移和負載均衡
            
                    * 執行 SendDeregisterNFInstance 流程，從 NF 中移除當前實例
            
                    * 執行 BuildUnavailableGUAMIList 流程(暫不清楚該流程的作用和內容)
            
                    * 執行 ngap_message.SendAMFStatusIndication(ran, unavailableGuamiList) 動作
            
                    * 停止 ngap_service 服務
            
                    *  向其他subscription 宣告自己將要停止服務
    
    
    
* factory
    * config.go
        * 配置struct的聲明和default value的getter（當某些配置不存在時會自動返回默認值）
    * factory.go
        * InitConfigFactory讀取文件，並使用yaml.Unmarshal將配置信息解析到AmfConfig變數中
        * TODO：需要支援從API中更新配置（我個人猜測可能是未來工作中需要做一個配置中心，因為微服務環境下通常都有配置中心集中管理配置一些需要統一設定的配置，而非每個微服務自己攜帶一些重複性的配置文件，至於非重複性的配置，根據我的工作經驗，通常使用.env環境變量的方式進行管理）