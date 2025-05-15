# 軟體工程管理筆記

## 第二章 軟體流程（The Software Process）

### 2.1 引言（Introduction）

* **流程（process）**：專案執行的結構化活動序列，確保可重複、可傳承
* 未文件化的流程易成為「口頭知識」，導致執行不一致、難以改進
* 軟體流程是組織之智慧資產，需透過文件化與度量持續優化

### 2.2 通用流程框架（Generic Process Framework）

* **階段（Phase）**：流程的主要步驟，如需求、設計、實現、測試、維護
* **活動（Activity）**：在每個階段中執行的具體工作，例如：

  * 撰寫需求規格（Requirement specification）
  * 系統架構設計（Architectural design）
  * 程式開發（Implementation）
  * 測試計畫與執行（Testing）
  * 部署與維護（Deployment & Maintenance）
* 常見階段：

  1. **規劃與需求（Specification）**：收集、分析使用者需求，產出需求文件
  2. **設計與實現（Design & Implementation）**：從系統架構到程式碼撰寫
  3. **驗證（Validation）**：單元測試（Unit Test）、整合測試（Integration Test）、系統測試（System Test）、驗收測試（Acceptance Test）、代碼審查(Code Review)、故障模式與影響分析(FMFA)、監管合規測試
    - 單元測試: 需要達到多少的代碼覆蓋率
    - 整合測試: 需要專注在模組間的接口和數據流
    - 系統測試: 模擬各種故障場景
  4. **演進／維護（Evolution / Maintenance）**：修正缺陷、功能增強、效能優化

### 2.3 主要目標：高品質（High Quality）

* **高品質 = 準時交付（Project Timeliness）**
* **減少返工（Rework）** → 降低成本、提升客戶滿意度
* **最佳實務**：早期缺陷偵測（Shift-left Testing）、持續整合（Continuous Integration）、自動化測試（Automated Testing）

### 2.4 線性序列模型（Linear Sequential Model；瀑布模型 Waterfall Model）

* **流程圖**：
  需求分析 → 系統設計 → 程式開發 → 單元測試 → 整合測試 → 系統測試 → 運維與維護
* **適用情境**：需求明確、變更少、團隊經驗豐富
  * 需求非常明確且穩定的專案（如軍事或航太系統）
  * 技術風險低且團隊擁有豐富經驗的情況
  * 契約型專案，需要明確的里程碑與交付物
  * 法規嚴格的領域，需要全面的文件記錄
* **優點**：易於管理與監控；里程碑（milestones）清晰
* **缺點**：缺乏迭代，對中途變更反應遲緩；後期修正成本高
* **補充交付物**：需求規格書、設計文件、測試報告、使用手冊
* **歷史應用**：瀑布模型在2000年之前被大量用於軟體開發，即使在2001年敏捷宣言發布後，瀑布模型仍在許多組織中使用至上一個十年
* **瀑布模型變種**：
  * **Sashimi模型**：允許相鄰階段重疊，減少等待時間
  * **增量式瀑布**：結合增量開發特性，每個增量內採用瀑布流程

### 2.5 V 模型（V-Model）

* **核心概念**：左右對應的「驗證（Verification）」與「驗收（Validation）」，強調每個開發階段對應的驗證活動。
* **架構圖**：
![](images/2025-05-04-21-05-52.png)
* **適用**：高可靠度（safety-critical）系統，如醫療、航空、嵌入式
* **限制**：無動態迭代、風險管理不足

### 2.6 螺旋模型（Spiral Model）

* **特色**：結合 **風險管理（risk management）** 的迴圈式開發
* **四象限流程**：

  1. 目標設定（Objectives, alternatives, constraints）
     ↓
  2. 風險評估與降低（Risk analysis & mitigation）
     ↓
  3. 開發與驗證（Development & validation）
     ↓
  4. 規劃下一迴圈（Plan next iteration） ↺
* **優點**：持續風險評估，適合大型專案和核心複雜度高的專案
* **缺點**：流程複雜，對小型專案過度

### 2.7 快速應用開發（RAD Model）

* **週期**：短迭代（60–90 天）
* **四階段**：

  1. 需求規劃（Requirements Planning）
  2. 使用者設計（User Design / 原型製作）
  3. 快速建構（Rapid Construction / 多次迭代）
  4. 移轉（Cutover / 上線部署）
* **特色**：高度使用者參與，工具支援原型快速成型
* **優點**：
  * 縮短產品交付時間、減少開發資源需求
  * 可以快速預覽產品
  * 有效利用現成工具和框架
  * 持續整合有助於發現問題並獲取客戶反饋
  * 持續客戶參與降低風險

### 2.8 原型方法（Prototyping Paradigm）

* **規格性原型（Specification Prototyping）**：快速拋棄式原型，用以驗證需求
* **演化性原型（Evolutionary Prototyping）**：原型逐步演化為最終系統
* **優點**：早期回饋、降低需求不確定性
* **缺點**：若管理不當，易衍生設計混亂

### 2.9 重用導向開發（Reuse-Oriented Development）

* **流程**：

  1. 元件分析（Component Analysis）—尋找可重用元件或 COTS（Commercial Off-The-Shelf）
  2. 需求調整（Requirements Modification）—配合現有元件
  3. 重用設計（System Design with Reuse）
  4. 整合與測試（Integration & Test）
* **優點**：降低開發成本、縮短時程
* **缺點**：元件相容性、授權風險

### 2.10 漸增式開發（Incremental Development）

* **概念**：將系統拆分為多個增量（increments），每次交付部分功能
* **流程圖**：
  增量 1 → 交付 → 增量 2 → 交付 → … → 最終整合
* **優點**：早期可運作、風險分散
* **缺點**：需於初期定義整體架構

### 2.11 敏捷方法（Agile Methods）
#### 敏捷方法關鍵實踐
  
* **敏捷測試策略**：
  * **持續測試**：開發過程中持續進行，而非僅在開發完成後
  * **測試驅動開發（TDD）**：先寫測試再寫代碼
  * **自動化測試**：單元、集成、系統層級的自動化測試

* **敏捷實踐的效益量化**：
  * 提高項目成功率30-35%
  * 縮短交付時間20-50%
  * 降低缺陷率60-90%
  * 提高團隊生產力25-40%
* **核心價值（Agile Manifesto）**：

  1. 個人與互動 > 流程與工具
  2. 可運行軟體 > 完整文件
  3. 客戶協作 > 合約談判
  4. 回應變更 > 遵循計畫
* **代表流程**：eXtreme Programming（XP）、Scrum、Kanban
* **實踐**：每日站立會（Daily Stand-up）、衝刺規劃（Sprint Planning）、迭代回顧（Sprint Review）
* **XP 實踐細節**：
  * 結對編程（Pair Programming）：兩名程序員坐在一起開發代碼
  * 測試先行（Test-First）：在編寫代碼前先編寫測試
  * 持續整合（Continuous Integration）：每天多次構建
  * 集體代碼所有權（Collective Code Ownership）：任何開發人員都可以修改任何部分的代碼
  * 簡單設計（Simple Design）：保持設計簡單且必要
  * 重構（Refactoring）：不斷改進現有代碼

### 2.12 DevOps

* **CI/CD 流水線**：持續開發 → 持續整合 → 持續測試 → 持續部署 → 持續監控
  * **持續整合（CI）伺服器**：如Jenkins、GitLab CI、Circle CI
  * **版本控制系統**：Git、SVN
  * **自動化測試框架**：Selenium、Jest、JUnit
  * **基礎設施即代碼（IaC）工具**：Terraform、Ansible、Chef
  * **容器化技術**：Docker、Kubernetes
* **CALMS 模型**：Culture（文化）、Automation（自動化）、Lean（精益）、Measurement（度量）、Sharing（分享）
* **DevOps 量化效益調查**：
  | 指標                        | 改善程度 |
  | --------------------------- | -------- |
  | 增加部門間協作              | 23%      |
  | 提升部署應用程式品質        | 22%      |
  | 增加使用軟體/服務的客戶數量 | 22%      |
  | 實現原本不可能的新軟體/服務 | 21%      |
  | 減少開發和部署所需員工數    | 21%      |
  | 縮短軟體/服務上市時間       | 20%      |
  | 增加收入                    | 19%      |
  | 擴展軟體/服務至更多平台     | 19%      |
  | 降低開發和運營支出          | 18%      |
  | 增加軟體/服務部署頻率       | 17%      |

* **DevOps成熟度模型**：
  * **初始級**：基本CI實踐，有限自動化
  * **管理級**：標準化流程，全面CI實踐
  * **定義級**：CD部署流程，多環境支持
  * **量化級**：監控與度量系統，有數據驅動改進
  * **優化級**：持續實驗與創新，自動故障修復

### 2.13 成熟度與標準（Maturity & Standards）

* **CMMI（Capability Maturity Model Integration）**：五級能力成熟度
  - 第1級 - 初始級 (Initial)
    - 流程具有不可預測性，缺乏控制和反應性
    - 沒有關鍵流程領域，屬於非正式且臨時性的階段

  - 第2級 - 已管理級 (Managed)
    流程已被規劃和執行，符合既定政策，並受到監控、控制及適當審查。

    **關鍵流程領域：**
    1. **需求管理 (Requirements Management)**
      - 管理需求並識別與計劃和產品之間的不一致

    2. **專案規劃 (Project Planning)**
      - 建立和維護專案活動的計劃

    3. **專案監控 (Project Monitoring and Control)**
      - 了解專案的進展，以便在績效出現明顯偏差時採取糾正措施

    4. **供應商協議管理 (Supplier Agreement Management)**
      - 管理從供應商獲取的產品及服務

    5. **測量與分析 (Measurement and Analysis)**
      - 建立測量能力以支持管理需求

    6. **流程與產品品質保證 (Process and Product Quality Assurance)**
      - 提供員工和管理層對流程和相關工作產品的客觀洞察

    7. **配置管理 (Configuration Management)**
      - 建立和維護工作產品的完整性

  - 第3級 - 已定義級 (Defined)
    流程被清晰定義，並在整個組織中標準化，比第2級更詳細和嚴謹。

    **關鍵流程領域：**
    1. **需求開發 (Requirements Development)**
      - 識別客戶需求、產品需求和產品組件需求

    2. **技術解決方案 (Technical Solution)**
      - 設計、開發和實施解決方案

    3. **產品整合 (Product Integration)**
      - 將產品組件組裝成完整產品

    4. **驗證 (Verification)**
      - 確保產品符合指定的需求

    5. **確認 (Validation)**
      - 確保產品滿足其預期的使用需求

    6. **組織流程焦點 (Organizational Process Focus)**
      - 規劃、實施組織的流程改進

    7. **組織流程定義 (Organizational Process Definition)**
      - 建立和維護一套可用的組織流程資產

    8. **組織培訓 (Organizational Training)**
      - 發展人員的技能和知識

    9. **整合專案管理 (Integrated Project Management)**
      - 根據組織調整的流程管理專案

    10. **風險管理 (Risk Management)**
        - 識別潛在問題並採取措施降低風險

    11. **決策分析與解決 (Decision Analysis and Resolution)**
        - 分析可能的決策並選擇方案

  - 第4級 - 量化管理級 (Quantitatively Managed)
    使用統計和其他量化技術控制流程，通過建立量化目標來預測流程績效。

    **關鍵流程領域：**
    1. **組織流程績效 (Organizational Process Performance)**
      - 建立和維護對組織流程績效的量化理解

    2. **量化專案管理 (Quantitative Project Management)**
      - 量化管理專案以達到既定的品質和績效目標

    3. **風險與機會管理(Risk and Opportunity Management)**
    4. **因果分析與解決(Causal Analysis and Resolution)**

  - 第5級 - 優化級 (Optimizing)
    持續改進流程績效，使用創新方法和技術對流程進行優化。

    **關鍵流程領域：**
    1. **組織績效管理 (Organizational Performance Management)**
      - 主動管理組織績效以滿足業務目標

    2. **原因分析與解決 (Causal Analysis and Resolution)**
      - 識別問題的原因並採取措施防止其再次發生

    3. **組織創新與部署(Organizational Innovation and Deployment)**

* **PSP（Personal Software Process）／TSP（Team Software Process）**：個人／團隊流程改進
* **IEEE 1074**、**ISO/IEC 12207**：國際標準化流程

### 2.14 選擇適合模型（Select an Appropriate SDLC Model）

* 考量因素：需求穩定度（Requirements volatility）、團隊經驗與規模（Team size & experience）、使用者參與度（Customer involvement）、專案風險（Risk profile）、時程與成本限制

### 2.15 流程導入與評估（Process Implementation & Assessment）

* **指定 vs 客製化**：流程標準化後，依組織需求作適度調整
* **度量指標（Metrics）**：任務效能（Task effectiveness）、工時分布（Effort by activity）、缺陷密度（Defect density）
* **持續改進**：透過回顧（retrospectives）與 PDCA（Plan-Do-Check-Act）循環

## 第三章 Software Project Management

### 3.1 軟體專案特性

* **規模（Size）**、交付期限（Delivery deadline）、預算與成本（Budget and costs）
* **應用領域（Application domain）**、實作技術（Technology to be implemented）
* **系統限制（System constraints）**、使用者需求（User requirements）
* **可用資源（Available resources）**

---

### 3.2 為何專案失敗？（Why Projects Fail?）

* 設定不切實際的期限（Unrealistic deadline）
* 客戶需求頻繁變更（Changing customer requirements）
* 工時估算過低（Underestimate of effort）
* 可預見或不可預見的風險（Predictable/Unpredictable risks）
* 技術困難（Technical difficulties）
* 團隊溝通不良（Miscommunication among project staff）
* 專案管理失誤（Failure in project management）

---

### 3.3 專案管理定義（What is Project Management?）

* 將**知識（Knowledge）**、**技能（Skills）**、**工具（Tools）**與**技術（Techniques）** 應用於專案活動
* 目的：滿足或超越利害關係人（Stakeholders）的需求與期望
* 利害關係人：因專案結果可能獲益或受損的任何人

---

### 3.4 利害關係人（Stakeholders）

* **高階經理（Senior managers）**：定義業務目標
* **專案經理（Project managers）**：計畫（Plan）、激勵（Motivate）、組織（Organize）、控制（Control）團隊
* **實作人員（Practitioners）**：提供技術執行
* **客戶（Customers）**：提出需求
* **最終使用者（End‐users）**：實際使用系統

---

### 3.5 管理關注點（Project Management Concerns）

* 人員配置（Staffing） → 成本估算（Cost estimation） → 排程（Scheduling）
* 進度監控（Monitoring） → 資源管理（Resources） → 客戶溝通（Communication）
* 風險評估（Risk assessment） → 產品品質（Quality） → 度量指標（Measurement）

---

### 3.6 專案經理的兩難（Project Manager's Dilemma）

* 「快（Fast）」、「便宜（Cheap）」、「優質（Good）」三者只能任選兩項

---

### 3.7 範疇確定（Scope Determination）

1. **範疇敘述（Narrative）**：界定問題邊界
2. **功能分解（Decomposition）**：建立功能模組化結構

---

### 3.8 管理活動（Management Activities）

* 提案撰寫（Proposal writing）
* 專案計畫與排程（Planning & scheduling）
* 成本估算（Costing）
* 人員配置（Staffing）
* 進度監控與檢討（Monitoring & reviews）
* 人員選拔與評估（Selection & evaluation）
* 報告撰寫與簡報（Reporting & presentations）

---

### 3.9 專案人員配置（Project Staffing）

* 理想人選受限於預算（Budget）、經驗（Experience）、組織需求（Skill development）
* 必須在這些條件下妥善調度資源

---

### 3.10 專案規劃（Project Planning）

* 最耗時的管理活動，從起始到交付皆需持續更新
* 支援計畫（Support plans）輔助主計畫（Schedule & budget plan）

---

### 3.11 4P 模型（The 4 P's）

* **人（People）**、**產品（Product）**、**流程（Process）**、**專案（Project）**

---

### 3.12 團隊管理（Managing People）

* 管理個人與群組的互動
* 溝通頻率（Communication channels）影響進度與品質
* 溝通失敗常導致軟體專案失敗

---

### 3.13 溝通問題（Communication Problems）

* N 人的團隊有 N(N−1)/2 條溝通渠道
* 人越多，溝通成本越高
* 增加人手不一定能加快進度（Brooks's Law）

---

### 3.14 軟體團隊組織（Software Teams）

* **MOI 模型**：動機（Motivation）、組織（Organization）、創新（Ideas）
* **常見結構（Pressman）**：

  * 民主去中心（DD, Democratic decentralized）
  * 受控去中心（CD, Controlled decentralized）
  * 受控中心（CC, Controlled centralized）
* 選擇依據：團隊規模、模組化程度、溝通需求

---

### 3.15 經典首席程式設計師團隊（CPT）

* 架構：首席程式設計師（Chief programmer）→ 備援（Back‐up）→ 秘書（Secretary）→ 程式員（Programmers）
* 特點：功能分工、階層管理
* 不可行之處：高階程式師＋管理者短缺、秘書工作效益低

---

### 3.16 協調與溝通技術（Coordination & Communication）

* 正式無人身：文件、里程碑、追蹤工具
* 正式人身：審查會議、Walkthrough
* 非正式：團隊會議、同地辦公（Collocation）
* 電子方式：電子郵件、論壇、視訊
* 人際網絡：非正式討論、跨部門諮詢

---

### 3.17 W5HH 原則（The W5HH Principle）

* **Why**：為何開發 → 商業價值
* **What**：做什麼 → 任務集
* **When**：何時完成 → 排程與里程碑
* **Who**：由誰負責 → 角色與職責
* **Where**：組織定位 → 部門、地點
* **How**：如何執行 → 技術與管理策略
* **How much**：資源需求 → 人力、工具、資料

---

### 3.18 進度追蹤與排程（Tracking & Scheduling）

* 專案分解：階段 → 步驟 → 活動
* 任務拆分後估算時間與資源
* 並行化以提升效率
* 最小化依賴以降低等待延遲
* 預留緩衝以因應估算誤差與意外

---

### 3.19 動機管理（Motivation）

* 滿足需求層次：生理（Basic needs）、安全、社交（Social）、尊重（Esteem）、自我實現
* 人格類型：任務導向（Task‐oriented）、自我導向（Self‐oriented）、互動導向（Interaction‐oriented）
* 平衡各種動機並強化團隊文化

---

### 3.20 群組運作與凝聚力（Group Working & Cohesiveness）

* 群組互動影響績效
* 組成需兼顧不同動機類型
* 凝聚力（Cohesiveness）：共同目標、互信、互助學習
* 培養方式：社交活動、建立團隊認同、資訊透明

---

### 3.21 人員選拔與留任（Choosing & Keeping People）

* 選拔：履歷（CV）、面試、推薦、測評
* 留任：提供成長機會、管理認可、歸屬感



# 第四章 Software Project Planning and Scheduling


## 4.1 計畫與估算（Planning and Estimating）

* 規劃必須貫穿開發各階段：最早在需求完成後啟動（Initial planning），並持續更新
* **估算（Estimation）**：預測人力（person-months）、成本（cost）、工期（schedule）

  * 需仰賴經驗（Experience）、歷史資料（Historical data）、勇於量化承諾

## 4.2 估算 vs 計畫 vs 排程

* **計畫（Plan）**：識別活動（activities），未指定起訖日期
* **估算（Estimating）**：決定每項活動的規模（size）與所需工期（duration）
* **排程（Schedule）**：將活動依關係（dependencies）分配具體起迄日期與資源

## 4.3 軟體專案計畫任務集（Project Planning Task Set）

1. 確定專案範疇（Scope）
2. 可行性分析（Feasibility）
3. 風險分析（Risk analysis）
4. 定義資源需求（Resources）

   * 人力（Human resources）
   * 可重用元件（Reusable software resources）
   * 環境資源（Environmental resources）
5. 成本與工時估算（Cost & effort estimation）

   * 拆解問題（Decompose the problem）
   * 多種方法估算、再進行校正（Reconcile estimates）
6. 建立排程（Schedule）

   * 任務網路（Task network）
   * 甘特圖（Time-line chart）
   * 追蹤機制（Tracking mechanisms）

## 4.4 活動網路圖與時間表

* **活動網路圖（Task/Activity Network）**：顯示依賴性、找出關鍵路徑（Critical Path）
* **時間表（Time-line Chart）**：標示每項活動起訖、可並行或資源負載

## 4.5 資源（Resources）

* 類型：人力（People）、硬體（Hardware）、支援軟體（Support software）
* 大型專案人力使用呈 Rayleigh 曲線：
![](images/2025-05-04-21-10-20.png)
* 共用資源（People/Hardware）需跨專案協調

## 4.6 專案計畫目標（Project Planning Objectives）

* 建立框架，讓管理者能合理估算所需資源與時間
* 早期估算需在有限時間內完成，並於專案持續更新

## 4.7 規劃步驟（The Steps）

1. 範疇界定（Scoping）
2. 估算（Estimation）
3. 風險管理（Risk analysis & mitigation）
4. 排程（Scheduling）
5. 管控策略（Control strategy）

## 4.8 專案範疇（Project Scope）

* 包含：功能與特性（Functions & features）、輸入輸出資料（Data I/O）、系統效能、限制、介面、可靠度
* 以專案範疇說明書（Project Scope Statement, PSS）固定邊界，減少範疇蔓延（Scope creep）

## 4.9 範疇界定技巧（Is/Is Not Technique）

* 針對每項目標列出「是」（Is）與「非」（Is Not），幫助團隊畫出明確邊界

## 4.10 排程需求（The Need for Schedules）

* 多項目承諾需排程管理，小任務可串行、大/複雜任務or資源衝突需更精細管理

## 4.11 排程案例：Word 1.0

* 原訂365 天，實際 1,887 天（5 年）
* 主要原因：

  * 過度樂觀（Wishful thinking）
  * 開發者流動（High turnover）
  * 功能「完成」謊報（Feature short-changing）

## 4.12 專案排程（Project Scheduling）

* 決定專案拆分為哪些任務、何時執行、由誰執行、所需資源
* 估算每項活動所需日曆時間與成本

## 4.13 敏捷規劃（Agile Planning）

* 迭代增量（Increments）交付，不預先排定所有功能，而依進度與客戶優先順位決定開發內容

## 4.14 排程估算技術（Schedule Estimation Techniques）

* **Delphi 方法（Expert judgment）**：匿名專家共識
* **黃金法則（Gamma rule）**：三點估算 t = (o + 4 m + p)/6
* **第一階估算（First-Order Estimation）**：功能點數（FP）^ 指數（0.39–0.48）

  **First-Order Estimation Power Parameter**
  | Kind of Software | Best in Class | Average | Worst in Class |
  | ---------------- | ------------- | ------- | -------------- |
  | Systems          | 0.43          | 0.45    | 0.48           |
  | Business         | 0.41          | 0.43    | 0.46           |
  | Shrink-Wrap      | 0.39          | 0.42    | 0.45           |
* **承諾式排程（Commitment-based）**：由開發者自行承諾日期
* **雙倍法則（Glenn's rule）**：自身估時 × 2，再加15–20％
* **立方根法則**：排程 ≈ 3 × (man-months)^(1/3)

### 專案估算方法比較

| 估算方法 | 優點           | 缺點             | 適用情境             |
| -------- | -------------- | ---------------- | -------------------- |
| 專家判斷 | 快速、經驗導向 | 主觀性強         | 小型專案、時間緊迫   |
| 類比估算 | 基於歷史數據   | 需要類似專案經驗 | 中型專案、有歷史數據 |
| 參數模型 | 客觀、可重複   | 依賴參數精確性   | 大型專案、標準化環境 |
| 三點估算 | 考慮不確定性   | 仍有主觀因素     | 風險較高的專案       |
| 自下而上 | 詳細準確       | 耗時費力         | 關鍵業務系統         |

## 4.15 呈報估算（Presenting Estimates）

* 加減範圍（± qualifiers）
* 風險量化（Risk quantification）
* 多案例（Best/Worst/Planned/Current cases）
* 信心水準（Confidence factors）

## 4.16 規劃與控制技術（Planning & Control Techniques）

* **工作分解結構（WBS, Work Breakdown Structure）**：分層拆解至可估算粒度
  * **WBS 創建原則**：
  * **100%原則**：確保WBS包含所有必要工作，不多不少
  * **可交付物導向**：以具體成果為中心，而非活動
  * **互斥性**：每個工作項僅出現在一個分支中
  * **適度分解**：通常2-4級最為合適
  * **工作包大小**：最低層級工作包應在8-80小時範圍內

* **WBS 字典要素**：
  * 工作包識別碼與名稱
  * 責任人與相關人員
  * 詳細描述與驗收標準
  * 前置任務與相依關係
  * 估算的工時與資源需求
  * 里程碑與交付日期
* **甘特圖（Bar/Gantt chart）**：直觀顯示活動時程與責任人
* **PERT 圖（Program Evaluation and Review Technique）**：計算最早/最晚時點、總浮時（Float）

## 4.17 長期專案 WBS

* 前 3 個月：細節至兩週內可完成任務
* 3 個月後：以更粗略層次規劃，並於執行後持續細化

## 4.18 排程考量（Scheduling Considerations）

* 多人分工不一定等於工期除人數（Brooks's Law）
* 資源可用率考量：長期預設每人每週僅有 4 天可用

## 4.19 專案里程碑（Project Milestones）

* 重要可交付物完成時設定，如：需求完成、驗收測試結束
* 數量宜適中，過多失去意義、過少失去管控

## 4.20 資源計畫（Resource Plans）

* 由排程衍生：何時需何種人員、硬體、工具、外部服務
* 可用於成本預算與採購流程安排
* **資源過載處理策略**：
  * **任務分割**：將大任務分割成可並行的小任務
  * **資源平滑**：在允許的浮動時間內調整任務開始時間
  * **優先級調整**：根據關鍵程度重新排列任務順序
  * **資源替代**：尋找替代資源或外包部分工作
  * **時間調整**：在必要時延長專案時程

* **資源平衡工具功能**：
  * 自動檢測資源衝突
  * 提供平衡建議與方案比較
  * 視覺化資源使用情況
  * 模擬不同資源分配的影響
  * 追蹤實際使用與計劃的差異

# 第五章 軟體大小估算（Software Size Estimation）

## 5.1 引言（Introduction）

* 專案排程前首要任務：**大小估算（size estimation）** → 決定工期（schedule）與成本（cost）
* 不同產出物可估：需求文件、設計文件、程式碼、測試案例

## 5.2 估算原則（Estimation Principles）

* 缺乏歷史資料 → 憑直覺估算（gut instincts）、計畫失真、程式員超時工作
* 收集並運用過去任務執行時間 → 類比估算（analogy）：以相似任務平均值推算新任務

## 5.3 CMM 與估算（CMM and Estimation）

* SEI CMM Level 2 KPA: Software Project Planning

  * 目標 1：將**估算**文件化，用於專案規劃與監控
  * 能力 4：參與估算與計畫之人員需接受培訓

## 5.4 大小量測（Size Measurement）

* **大小（size）** 作為獨立變數 → 輸入至工期與成本估算
* 例：透過閱讀章節所需時間資料，估算新章節閱讀時間

## 5.5 大小度量範例（Examples of Size Measures）

* 程式行數（LOC, Lines of Code）
* 功能點（FP, Function Points）

  1. **識別功能類型**：功能點分析將應用程式的功能分為五種基本類型：
    - 內部邏輯檔案(Internal Logical Files, ILF)
    - 外部介面檔案(External Interface Files, EIF)
    - 外部輸入(External Inputs, EI)
    - 外部輸出(External Outputs, EO)
    - 外部查詢(External Inquiries, EQ)

  2. **評估複雜度**：每種功能根據其複雜度（低、中、高）被賦予不同的權重值。複雜度通常基於:
    - 資料元素類型(DET)的數量
    - 記錄元素類型(RET)或檔案類型參照(FTR)的數量

  3. **計算未調整功能點數(UFP)**：將每種功能類型的數量乘以其對應的複雜度權重，然後加總。

  4. **應用系統複雜度調整因子(VAF)**：根據14個一般系統特性(GSC)評估系統的整體複雜度，並計算值調整因子。

  5. **計算最終功能點**：將未調整功能點乘以值調整因子，得到最終的功能點數。

  ## 估算過程中使用的關鍵元素

  - **資料元素類型(DET)**：指用戶可識別、非重複的欄位。
  - **記錄元素類型(RET)**：指內部邏輯檔案或外部介面檔案中的子結構或邏輯群組。
  - **檔案類型參照(FTR)**：指被交易功能（EI、EO、EQ）處理或參照的檔案數。

* 資料流程圖氣泡數（DFD bubbles）
* 實體關係圖實體數（ERD entities）
* 結構圖方塊數（PSPEC/CSPEC boxes）
* 文件量（pages of documentation）
* 物件圖中物件、屬性與服務數
* * **COSMIC功能點**：
  * 國際標準ISO/IEC 19761
  * 基於數據移動（進入、退出、讀取、寫入）
  * 不受技術實現影響
  * 適用於實時與嵌入式系統

* **Use Case點數（UCP）**：
  * 基於用例複雜度與技術因素
  * 計算公式：UCP = UUCP × TCF × EF
    * UUCP：未調整用例點數
    * TCF：技術複雜度因子
    * EF：環境因子
  * 常用於面向對象的軟體專案

* **敏捷故事點（Story Points）**：
  * 相對大小測量，通常使用斐波那契數列（1,2,3,5,8,13...）
  * 考慮複雜性、工作量、風險和不確定性
  * 與團隊速度（Velocity）配合使用預測工期
  * 通過規劃撲克（Planning Poker）等技術估算

## 5.6 程式行數（LOC）

* 定義：非註解、非空行的程式文字行數
* NCLOC（Non-Commented LOC）＝有效行數（ELOC）
* 優點：易量化
* 缺點：對不同語言、不同程式風格敏感；不反映功能或複雜度

## 5.7 McCabe 迴圈複雜度（Cyclomatic Complexity）

* 定義：控制流圖中判斷節點數量所代表的獨立路徑數
* 計算方式：V(G)=e–n+2 （e＝邊數，n＝節點數）或「封閉區域＋1」
* 指標：V(G)>10 需加強測試，V(G)>20 需警戒
  接近警戒值或超過警戒值的測試策略應該要包含以下:
  - 詳細的單元測試，覆蓋所有獨立路徑
  - 基於路徑的測試（Path testing）
  - 代碼審查（Code review）和走查（Walkthrough）
  - 整合測試與系統測試的嚴格規劃
* 工具：Visual Studio、SonarQube、McCabe IQ …

### 代碼複雜度度量工具比較

| 工具名稱      | 支援語言                  | 主要功能               | 整合性        | 適用情境        |
| ------------- | ------------------------- | ---------------------- | ------------- | --------------- |
| SonarQube     | Java, C#, JS, Python等20+ | 代碼質量、安全、複雜度 | CI/CD, IDE    | 企業級持續集成  |
| CodeClimate   | Ruby, JS, PHP, Python等   | 質量監控、趨勢分析     | GitHub        | 開源項目        |
| Visual Studio | C#, C++, VB               | 內建複雜度分析         | .NET生態系統  | Windows開發環境 |
| PMD           | Java, JavaScript, Apex等  | 靜態代碼分析           | Maven, Gradle | Java專案        |
| Understand    | C, C++, Java, Ada等15+    | 深度依賴分析           | 獨立工具      | 安全關鍵系統    |

## 5.8 Halstead 度量（Halstead Metrics）

* 基本度量：µ₁（不同運算子數）、µ₂（不同運算元數）、N₁（運算子出現次數）、N₂（運算元出現次數）
* 衍生度量：

  * 程式詞彙量 µ=µ₁+µ₂
  * 程式長度 N=N₁+N₂
  * 容量 V=N×log₂µ
  * 難度 D=(µ₁/2)×(N₂/µ₂)
  * 工作量 E=V×D
  * 時間 T=E/β（β＝Stroud number，每秒心智判別次數）
* 優點：跨語言；量化複雜度
* 缺點：需完整程式碼；對設計階段預測價值有限

## 5.9 功能點（Function Points, FP）

* 測量系統功能而非程式長度
* 五類資訊元件：輸入（EI, External Input）、輸出（EO, External Output）、查詢（EQ, External Inquiry）、內檔（ILF, Internal Logical File）、外介面（EIF, External Interface File）
* 計算流程：

  1. 計算**未調整功能點數（UFC）**：依複雜度權重加總
  2. 應用**技術複雜度因子（TCF）**：14項一般系統特性加權
    - 每項加權0到5，0代表not relevant to the system, 5代表the component is essential
    - TCF 公式:
      $TCF = 0.65 + 0.01 \sum_{i=1}^{14} F_i$
  3. FP = UFC × TCF
* 優點：語言無關；貼近使用者價值
* 缺點：主觀性高；需專業認證
* 功能點分析法:
  總功能點/每人每月的功能點處理量
* **功能點計數標準化**：
  * IFPUG 4.3標準 - 最廣泛使用
  * NESMA標準 - 荷蘭標準，支持估算FP
  * COSMIC標準 - 適用於實時系統
  * MARK II標準 - 英國標準

* **功能點分析常見挑戰與解決方案**：
  * **挑戰**：識別邏輯文件時的主觀性
    * **解決方案**：使用數據模型與業務流程文檔交叉驗證
  * **挑戰**：現代GUI界面的計數困難
    * **解決方案**：將相關功能分組，基於業務功能計數
  * **挑戰**：敏捷環境中需求變更頻繁
    * **解決方案**：採用增量式FP計數，定期重估新功能

## 5.10 其他大小度量（Other Measures）

* 物件點（Object Point）、用例點（Use Case Point）
* 重用規模：計算重用程式碼的新增、修改與刪除比例
* 文件、GUI 元件…

## 5.11 選擇最佳度量技術（Appropriate Sizing Techniques）

* 遵循組織標準；依熟悉度、可靠度、客戶需求而定
* 較小專案可採專家判斷（Delphi）、大專案宜結合多種方法

# 第六章 軟體成本分析與估算（Software Cost Analysis and Estimation）

## 6.1 成本分析（Cost Analysis）

* 定義：針對軟體系統未來成本進行預測與量化的分析工作，既是藝術（Art）也是科學（Science）
* 目的：將需求與功能轉換為預算要求，提供可執行工作的成本基礎

## 6.2 軟體成本估算（Cost Estimation）

* 核心：預測開發所需的人力（Effort）與時間（Schedule）
* 模型：運用數學演算法，將系統規模（Size，如 LOC、FP）與複雜度（Complexity，如 Cyclomatic complexity）轉換為成本

## 6.3 軟體成本要素（Software Cost Components）

* **硬體與軟體成本（Hardware & Software）**：伺服器、授權費
* **轉換與安裝成本（Conversion & Installation）**：資料轉換、部署作業
* **旅費與訓練成本（Travel & Training）**
* **人力成本（Effort Costs）**：工程師薪資、社保與保險

  * 另須考慮辦公場地、水電、網路、共用設施等間接開銷

## 6.4 成本元素結構（Cost Element Structure, CES）

* 類似 WBS，建立分層樹狀結構，避免重複計算
* 可在不同層級彙總與解析預算
* 範例：

  ```
  1. 人力成本  
     1.1 需求分析  
     1.2 設計與實作  
     1.3 測試  
  2. 基礎設施  
     2.1 硬體  
     2.2 網路  
  3. 其它  
     3.1 培訓  
     3.2 差旅  
  ```

## 6.5 估算方法（Estimation Techniques）

* **Bottom-Up（自下而上）**：從最低層元件估算後加總；精準但易漏算整合/文件成本
* **Top-Down（自上而下）**：依整體功能分配成本；納入系統級活動但可能低估低階技術難題
* **Parkinson's Law**：工作量會填滿可用時間 → Effort = 人數 × 時間
* **專家判斷（Expert Judgment）**：多位領域專家迭代共識（如 Delphi）；成本低廉但需經驗豐富
* **類比法（Analogy）**：以過去類似專案平均成本推估；精準度取決於相似度與資料庫維護情況
* **Make/Buy 決策（Make/Buy Decision）**：自建 vs 採購 vs 修改 COTS；可結合決策樹分析
* **參數化/演算法式（Parametric / Algorithmic）**：如 COCOMO、SLIM，以統計回歸模型自動計算

## 6.6 SLIM（Putnam's Software Life-cycle Model）

* 概念：依 Rayleigh 分布模擬人力投入曲線
* 公式：
  
  $m(t) = 2 \cdot a \cdot K \cdot t \cdot e^{-a \cdot t^2}$
  
  * K：總人力（staff-years）
  * a：技術常數（technology constant）
  * td：峰值時間（time of delivery）
* 優點：強調風險控制與長期維護；適合大型專案
* 缺點：對專案規模與時程壓縮高度敏感

## 6.7 COCOMO 基本模型（COCOMO Basic）

* 三種模式：

  * **Organic**（簡單專案）
  * **Semidetached**（中等複雜度）
  * **Embedded**（高複雜度嵌入式）
* 公式：

  $\text{Effort(人月)} = a \cdot (\text{KLOC})^b$

  $\text{Duration} = c \cdot (\text{Effort})^d$

  * a, b, c, d：回歸得出常數

## 6.8 COCOMO 中介模型（COCOMO Intermediate）

* 加入 15 項「成本驅動因子（Cost Drivers）」
* 計算環境調整因子（EAF, Effort Adjustment Factor）
* Effort = a·(KLOC)^b × EAF

## 6.9 COCOMO 詳細模型（COCOMO Detailed）

* 在元件層級套用中介模型，並分階段（Requirements, Design, Code & Test, Integration）調整因子
* 更細緻地分配 Effort 與 Schedule

## 6.10 COCOMO II

* 三大子模型：

  1. **Application Composition**（應用組成）—以物件點（Object Points）估算
  2. **Early Design**（早期設計）—A·Size^B·M（7 項乘子）
  3. **Post-Architecture**（後架構）—17 項乘子＋5 項尺度因子（Scale Factors）
* **Reuse Model**：估算自動生成或重用程式碼整合 Effort

## 6.11 校準與調整（Tailoring）

* 可依組織歷史資料重新校準係數 a、b 或成本驅動因子
* 建議至少 5–10 個專案以確保可靠性

## 6.12 敏捷環境下的估算（Agile Estimation）

* 強調持續校正（Continuous Refinement）
* 早期使用類比法，後期以實際 Burn-down Data 做 Build-up
* 目的：管理不確定性、漸進交付價值

# 第七章 軟體品質保證與程式碼審查（Software Quality Assurance and Code Review）

## 7.1 引言（Introduction）

軟體品質保證是確保軟體產品滿足特定需求和標準的系統性流程。在軟體開發生命週期中，品質保證活動貫穿始終，而程式碼審查則是其中最為重要的實踐之一。良好的品質保證過程可以顯著降低缺陷率，提高產品可靠性，並降低維護成本。

## 7.2 品質定義（What Is Quality?）

軟體品質是一個多維度的概念，可以從不同角度來理解和定義。

### 7.2.1 基本定義（Basic Definition）

軟體品質是軟體產品符合明確和隱含需求的程度，包括功能性和非功能性需求的滿足度。優質軟體不僅能夠正確運行，還應具備良好的效能、安全性、可用性和可維護性等特性。

### 7.2.2 專業觀點（Professional Views）

* **Conformance to requirements**（Crosby, 1979）：
  * Requirements 必須明確定義，產品必須嚴格符合這些規定
  * 任何偏離需求的情況皆視為缺陷
  * 這種觀點強調"做正確的事"，著重於需求的準確實現

* **Fitness for use**（Juran, 1970）：
  * 強調產品滿足使用者預期需求和目的的能力
  * 注重使用者滿意度和產品的實用性
  * 這種觀點更關注"做對用戶有用的事"

### 7.2.3 傳統定義（Conventional Definitions）

* **Conformance to requirements**：
  * 著重於缺陷數量的減少，缺陷越少，品質越好
  * 可通過錯誤率、故障頻率等客觀指標來衡量
  * 技術導向，強調符合技術規格

* **Fitness for use**：
  * 強調易用性、可靠性以及滿足用戶隱含需求的能力
  * 更注重用戶體驗和實際使用價值
  * 商業導向，關注市場接受度和用戶滿意度

### 7.2.4 當代品質觀（Modern Quality Views）

* **用戶價值導向**：品質等同於用戶感知的價值，超越傳統的功能性定義
* **持續改進模式**：品質是一個動態過程，需要持續改進和調整
* **風險管理視角**：優質軟體應最小化失敗風險，提高可預測性

## 7.3 品質度量（Measuring Quality）

衡量軟體品質需要多維度的指標，從不同利益相關者角度出發。

### 7.3.1 使用者觀點（User's View）

* **Reliability**（可靠性）：
  * 衡量軟體失效的頻率和嚴重程度
  * 可通過平均故障間隔時間(MTBF)等指標量化
  * 影響用戶信任和滿意度的核心指標

* **Availability**（可用性）：
  * 系統正常運行時間與總時間的比率
  * 通常以百分比表示，如"99.9%可用性"
  * 對於關鍵業務系統尤為重要

* **Usability**（易用性）：
  * 衡量系統對用戶的友好程度
  * 包括學習曲線、操作效率、用戶滿意度等
  * 可通過用戶測試和滿意度調查來量化

* **通常可直接量化**：
  * 使用者體驗指標如任務完成時間、錯誤率
  * 用戶反饋分數、NPS(淨推薦值)等

### 7.3.2 製造者觀點（Manufacturer's View）

* **Defect counts**（缺陷數）：
  * 開發和測試階段發現的問題數量
  * 可按嚴重程度和優先級分類統計
  * 反映代碼和設計的健壯性

* **Rework costs**（返工成本）：
  * 修復缺陷所需的時間和資源
  * 包括識別、修復和再測試的成本
  * 直接影響開發效率和項目盈利能力

* **開發效率指標**：
  * 代碼複雜度、模塊耦合度
  * 單元測試覆蓋率
  * 代碼重用率和維護性指標

### 7.3.3 範例：ATM 系統

ATM系統的品質指標包括：

* **業務目標**：吸引新客戶、降低營運成本
* **Security**（安全性）：
  * 整合既有保安基礎設施
  * 確保交易安全，防止欺詐和未授權訪問
  * 符合金融行業安全標準和法規

* **學習時間**：
  * 首次使用 ≤ 5 分鐘
  * 確保新用戶能夠快速上手，減少服務台支持需求

* **回應時間**：
  * 90% 操作 ≤ 1 秒
  * 最差情況 ≤ 10 秒
  * 保證良好的用戶體驗和服務效率

* **停機時間**：
  * ≤ 1 小時／月
  * 確保系統高可用性，維持服務連續性

### 7.3.4 品質測量的挑戰

* **主觀與客觀指標的平衡**：某些品質屬性難以量化
* **測量過程本身的成本**：收集和分析數據需要資源
* **指標的準確性和相關性**：確保測量的是真正重要的方面

## 7.4 品質模型（Quality Models）

品質模型提供了系統化評估軟體品質的框架，定義了不同品質屬性及其評估方法。

### 7.4.1 ISO/IEC 9126 定義

* 定義：軟體產品的特性和特徵，這些特性和特徵能夠滿足明確或隱含的需求
* **六大特性**：
  * **Functionality**（功能性）：實現規定功能的能力
  * **Reliability**（可靠性）：維持特定性能水平的能力
  * **Usability**（易用性）：容易理解、學習和使用的程度
  * **Efficiency**（效率）：在規定條件下使用資源的水平
  * **Maintainability**（可維護性）：修改軟體的容易程度
  * **Portability**（可移植性）：從一個環境轉移到另一環境的能力

### 7.4.2 ISO/IEC 25000 SQuaRE

* **系列標準**：System and Software Quality Requirements and Evaluation
* 依 ISO 9126 擴充，強化了以下方面：
  * 更詳細的品質模型
  * 定義標準化評估流程
  * 提供測量方法和工具
  * 細分為多個分系列標準：
    * 25000-25099：Quality Management Division
    * 25100-25199：Quality Model Division
    * 25200-25299：Quality Measurement Division
    * 25300-25399：Quality Requirements Division
    * 25400-25499：Quality Evaluation Division

### 7.4.3 McCall's Triangle of Quality

McCall模型將軟體品質分為三個主要方面：

* **Product Operation**（產品操作）：
  * **Capability**（功能性）：滿足需求的能力
  * **Usability**（易用性）：學習和操作的容易程度
  * **Reliability**（可靠性）：執行指定功能而不失敗的能力
  * **Performance**（效能）：系統執行效率和資源消耗

* **Product Revision**（產品修訂）：
  * **Maintainability**（可維護性）：識別和修復缺陷的容易程度
  * **Flexibility**（靈活性）：適應變化的能力
  * **Testability**（可測試性）：驗證軟體功能的容易程度

* **Product Transition**（產品轉換）：
  * **Portability**（可移植性）：在不同環境下運行的能力
  * **Reusability**（可重用性）：組件在其他應用中重用的容易程度
  * **Interoperability**（互操作性）：與其他系統集成的能力

### 7.4.4 IBM CUPRIMDA

IBM的CUPRIMDA模型定義了八個關鍵品質屬性：

* **Capability**（能力）：軟體滿足功能需求的程度
* **Usability**（易用性）：學習和操作的容易程度
* **Performance**（效能）：響應時間和資源利用效率
* **Reliability**（可靠性）：在指定條件下正確執行的能力
* **Installability**（可安裝性）：安裝和配置的容易程度
* **Maintainability**（可維護性）：修改和更新的容易程度
* **Documentation**（文檔）：文檔的完整性和清晰度
* **Availability**（可用性）：系統正常運行時間比例

### 7.4.5 Boehm品質模型

Boehm模型強調了軟體使用者和開發者的需求平衡：

* **一般效用**：總體有用性
* **可移植性**：硬體獨立性
* **可靠性**：完成預期功能的能力
* **效率**：資源利用
* **人機工程學**：易用性
* **可測試性**：驗證容易程度
* **可理解性**：代碼清晰度
* **可修改性**：更新容易程度

## 7.5 軟體品質保證（Software Quality Assurance, SQA）

軟體品質保證是一系列計劃性和系統性的活動，旨在確保軟體開發過程和產品符合品質標準。

### 7.5.1 定義（QA Definitions）

* **正式定義**：計劃性和系統性的活動，旨在提供信心，確保產品或過程符合品質要求。
* **涵蓋範圍**：
  * 流程監控：確保開發過程符合標準和最佳實踐
  * 文件審核：驗證文檔的完整性和準確性
  * 測試監督：確保測試覆蓋和有效性
  * 審計：獨立評估開發過程和產品
  * 標準制定：建立和維護品質標準和指南

### 7.5.2 國際標準（QA Standards）

* **IEEE 730 Software Quality Assurance Plans**：
  * 定義SQA計劃的內容和結構
  * 提供SQA活動的指南和要求

* **ISO 9000系列**：
  * ISO 9001：品質管理系統要求
  * 強調過程導向和持續改進
  * 適用於各種組織，包括軟體開發

* **CMMI** (Capability Maturity Model Integration)：
  * 定義五個成熟度級別：初始、管理、定義、量化管理、優化
  * 每個級別包含多個過程領域
  * 提供組織過程改進的框架

* **其他標準**：
  * IEC 62304：醫療設備軟體
  * ANSI/IEEE 730：軟體品質保證計劃
  * EIA 12207：軟體生命週期過程

### 7.5.3 QA 部門組織範例

一個典型的品質保證部門包含以下職責：

```
Quality Assurance Department
├─ Generation of QA documentation
│  └─ 制定品質計劃、標準和指南
├─ Review of project materials
│  └─ 審核需求、設計和代碼
├─ Auditing & In-process audits
│  └─ 獨立評估開發過程和成果
├─ Monitoring project status & corrective actions
│  └─ 跟踪品質指標，識別和解決問題
├─ Inspection of delivered items
│  └─ 驗證交付成果的品質
├─ Participation in project activities
│  └─ 參與關鍵項目活動，提供品質保證視角
├─ Testing oversight
│  └─ 監督測試活動，確保充分覆蓋
```

### 7.5.4 IEEE 730 SQAP—軟體審查最低需求

IEEE 730標準定義了軟體品質保證計劃中應包含的最低審查要求：

* **4.6.2.1 Software specifications review (SSR)**
  * 審核需求規格，確保完整性、一致性和可測試性

* **4.6.2.2 Architecture design review (ADR)**
  * 評估系統架構設計，確保符合需求和技術可行性

* **4.6.2.3 Detailed design review (DDR)**
  * 檢查詳細設計，確保符合架構和需求

* **4.6.2.4 Verification & validation plan review**
  * 審核驗證和確認計劃，確保測試覆蓋所有關鍵功能和需求

* **4.6.2.5 Functional audit**
  * 確認產品功能符合規格要求

* **4.6.2.6 Physical audit**
  * 驗證產品物理組件和配置項目的完整性

* **4.6.2.7 In-process audits**
  * 開發過程中的持續審計，及早發現問題

* **4.6.2.8 Managerial reviews**
  * 管理層審查，確保資源分配和進度符合計劃

* **4.6.2.9 SCM plan review (SCMPR)**
  * 審核軟體配置管理計劃，確保版本控制和變更管理適當

* **4.6.2.10 Post-implementation review**
  * 實施後審查，評估產品在實際環境中的表現

### 7.5.5 品質保證與測試的區別

* **品質保證**：預防性活動，確保過程正確執行
* **測試**：檢測性活動，發現和修復缺陷
* **關係**：互補關係，共同確保產品品質

## 7.6 軟體審查（Software Reviews）

軟體審查是一種群體活動，旨在評估軟體工件以識別和糾正缺陷。

### 7.6.1 審查目的（Why Review?）

* **及早發現並移除缺陷**：
  * 提高缺陷移除效率(Defect Removal Efficiency)
  * 降低缺陷修復成本（越早發現，成本越低）

* **改善程式結構和一致性**：
  * 確保代碼符合團隊規範和最佳實踐
  * 提高代碼一致性和可維護性

* **知識分享和培訓**：
  * 分享領域知識和技術經驗
  * 幫助新成員理解系統和規範

* **提升開發者工作品質**：
  * 促進自我檢查和專業成長
  * 降低開發者離職風險，提高團隊穩定性

### 7.6.2 審查類型（Types of Formal Technical Review）

* **Walkthrough**：
  * 特點：無需事先準備，作者逐行示範和解釋代碼
  * 目的：分享知識，獲取反饋
  * 結構：非正式，由作者主導
  * 適用：小型更改，教學場景

* **Peer review/Buddy checking**：
  * 特點：非正式，同儕互相檢查代碼
  * 目的：快速發現明顯問題，共享知識
  * 結構：靈活，可一對一或小組
  * 適用：日常開發，持續整合

* **Inspection (Fagan review)**：
  * 特點：最正式的審查類型，遵循嚴格流程
  * 目的：系統地發現缺陷，並收集數據
  * 結構：明確的角色分配，使用檢查表
  * 適用：關鍵組件，高風險區域

* **Audit**：
  * 特點：外部獨立檢查，通常由品質保證團隊執行
  * 目的：評估符合標準和流程的程度
  * 結構：正式，基於標準和檢查表
  * 適用：合規性驗證，正式項目里程碑

### 7.6.3 審查流程（Review Process Steps）

有效的審查流程包含以下步驟：

1. **Planning**（計劃）：
   * 確定審查範圍和目標
   * 組成審查團隊，分配角色
   * 準備檢查表和審查標準

2. **Orientation**（介紹）：
   * 作者介紹背景和目的
   * 說明設計思路和實現方法
   * 角色分配和責任確認

3. **Preparation**（準備）：
   * 審查者獨立研究材料
   * 依據檢查表識別潛在問題
   * 記錄發現的缺陷和疑問

4. **Review Meeting**（審查會議）：
   * 系統性討論發現的問題
   * 對缺陷進行分類和優先級排序
   * 形成最終缺陷列表和行動項目

5. **Rework**（返工）：
   * 作者修正識別的問題
   * 記錄修改的內容和原因
   * 回報修復結果

6. **Follow-up**（跟進）：
   * 驗證修正的完整性和正確性
   * 收集和分析審查效率數據
   * 持續改進審查流程

### 7.6.4 關鍵角色（Key Roles）

成功的審查需要不同角色的參與：

* **Moderator**（主持人）：
  * 組織和引導整個審查流程
  * 保持會議專注於技術問題
  * 確保所有缺陷得到適當處理

* **Reviewer**（審查者）：
  * 識別和報告缺陷
  * 提供建設性意見
  * 確保代碼符合標準和最佳實踐

* **Reader**（閱讀者）：
  * 逐條朗讀被審查的文稿
  * 確保審查團隊關注同一部分
  * 保持審查節奏

* **Scribe**（記錄員）：
  * 詳細記錄發現的問題和決議
  * 維護缺陷列表和行動項目
  * 生成審查報告和統計數據

* **Author**（作者）：
  * 提供原始說明和背景信息
  * 回答問題和澄清疑點
  * 執行返工和修正缺陷

### 7.6.5 審查對象（What Gets Reviewed?）

軟體開發生命週期中多種工件都可以進行審查：

* **項目管理文檔**：
  * Proposal、Contract、Schedule、Budget
  * SPMP（軟體項目管理計劃）

* **品質保證文檔**：
  * SQAP（軟體品質保證計劃）
  * SCMP（軟體配置管理計劃）

* **需求和設計文檔**：
  * SRS（軟體需求規格）
  * DCD/DFD/ERD/Class/Object 模型
  * Structure chart、Nassi-Schneiderman chart

* **代碼相關**：
  * 源代碼
  * Pseudocode
  * Decision table/tree

* **測試文檔**：
  * Integration/Test/Conversion/System test plans
  * Project test plan
  * Acceptance test plan/report

* **交付和部署文檔**：
  * Software baseline
  * Acquisition/Transition/User's guide
  * Operating documentation、Installation plan
  * Maintenance plan、Operational system

### 7.6.6 檢查清單範例（Sample Review Checklist）

有效的代碼審查檢查清單應包括以下方面：

* **Control flow**（控制流）：
  * 邏輯結構是否清晰
  * 條件判斷是否正確
  * 循環是否有明確的終止條件

* **Data declarations**（數據聲明）：
  * 變量類型是否適當
  * 初始化是否正確
  * 命名是否符合規範

* **Logic errors**（邏輯錯誤）：
  * 邊界條件處理
  * 特殊情況考慮
  * 算法實現正確性

* **Naming conventions**（命名規範）：
  * 一致性
  * 描述性和可理解性
  * 符合團隊標準

* **Test coverage**（測試覆蓋）：
  * 功能覆蓋度
  * 邊界條件測試
  * 異常情況處理

* **性能考量**：
  * 算法效率
  * 資源使用
  * 可擴展性

* **安全性檢查**：
  * 輸入驗證
  * 授權和認證
  * 數據保護

### 7.6.7 次數與時機（When and How Often?）

審查的時機和頻率應根據項目特性靈活調整：

* **時機選擇**：
  * 於單元測試後進行，確保基本功能正常
  * 在基本功能驗證完成後，進行更全面的審查
  * 不宜過早（避免頻繁修改）
  * 也不可過晚（避免修改成本增加）

* **頻率考量**：
  * 關鍵組件可能需要多次審查
  * 考慮團隊規模和經驗水平
  * 平衡審查成本和收益

* **建議時間點**：
  * 關鍵設計決策後
  * 複雜功能實現後
  * 主要里程碑前
  * 版本發布前

### 7.6.8 度量指標（Metrics）

審查效果可通過以下指標衡量：

* **Defect Removal Efficiency (DRE)**：
  * 計算方式：已移除缺陷 / 預估總缺陷
  * 意義：衡量審查過程的有效性
  * 目標：接近100%

* **Defect Density**（缺陷密度）：
  * 計算方式：已移除缺陷 / 工件大小 (頁數或 SLOC)
  * 意義：反映代碼質量
  * 基準：根據行業和項目類型有所不同

* **Inspection Rate**（檢視率）：
  * 計算方式：工件大小 / 檢視時數
  * 意義：衡量審查效率
  * 最佳實踐：300-500 LOC/小時

* **其他指標**：
  * 審查準備時間
  * 缺陷修復時間
  * 缺陷分類分布

### 7.6.9 Peer Reviews 與 CMMI

在CMMI模型中，Peer Review是一個重要的過程：

* **Level 3 定義流程要求**：
  * 需有訓練有素的 Moderator 主持
  * 成果不應用於個人績效評估（避免隱藏問題）
  * 必須有明確的入口和出口標準

* **數據收集和分析**：
  * Peer review 數據納入組織流程資料庫
  * 用於項目估算、品質預測和可靠度分析
  * 支持組織級過程改進

* **持續改進**：
  * 定期評估審查效率
  * 調整審查策略和方法
  * 分享最佳實踐

## 7.7 程式碼審查最佳實踐（Code Review Best Practices）

### 7.7.1 有效的代碼審查原則

* **專注於重要問題**：優先關注安全、可靠性和功能性問題
* **保持小批量**：每次審查限制在200-400行代碼
* **設定時間限制**：審查時間不超過60分鐘
* **使用工具輔助**：靜態代碼分析、自動化檢查
* **明確標準**：事先確定審查標準和檢查點

### 7.7.2 現代代碼審查工具

* **版本控制集成**：GitHub PR、GitLab MR、Bitbucket PR
* **專用工具**：Gerrit、Review Board、Crucible
* **靜態分析工具**：SonarQube、ESLint、PMD
* **持續集成支持**：Jenkins、Travis CI、CircleCI

### 7.7.3 遠程和異步審查

* **視頻會議工具**：適用於實時遠程審查
* **註釋和線上討論**：允許非同步參與
* **截圖和錄屏共享**：幫助理解複雜問題
* **文檔和溝通工具整合**：提高協作效率

# 第八章 軟體需求規格（Software Requirements Specification）

## 8.1 參考文獻（References）

這些參考文獻代表軟體工程領域中關於需求工程的權威著作：

* Roger S. Pressman，《Software Engineering: A Practitioner's Approach》，Chapters 7–8，9th Edition，McGraw-Hill，2020
* Ian Sommerville，《Software Engineering》，Chapter 4，10th Edition，Pearson，2016
* Stephen R. Schach，《Object Oriented and Classical Software Engineering》，Chapter 11，8th Edition，McGraw-Hill，2011
* Daniel R. Windle & L. Rene Abreo，《Software Requirements Using the Unified Process》，Prentice Hall PTR，2003

## 8.2 軟體需求概念（Software Requirements Concepts）

### 8.2.1 軟體架構模型（Software Architecture Model）

* 黑箱模型（Black-box Model）用於示意需求與系統組件之間的關係
* 此模型強調系統行為而非內部實現，著重輸入、處理和輸出
* 有助於在需求階段與利害關係人溝通，不涉及技術細節

## 8.3 什麼是需求（What Is a Requirement?）

### 8.3.1 定義（Definition）

* 需求是系統必須具備的功能（feature）或必須滿足的制約（constraint），以供客戶驗收
* IEEE 標準定義：
  1. 使用者為解決問題或達成目標所需之條件或能力
  2. 系統或組件必須滿足之條件或能力，以符合法規、標準、契約或其他正式文件
  3. 上述條件或能力之書面表徵（文件化表述）

### 8.3.2 需求的範疇與抽象層級（Scope and Abstraction Level）

* **高層抽象需求**：服務或系統制約之概要敘述，適合招標文件和初期討論
* **詳細需求規格**：數學或程式層級之功能描述，適用於契約附件和技術實現
* 需求可呈現為不同粒度：史詩級(Epic)、特性(Feature)、使用者故事(User Story)、任務(Task)

### 8.3.3 黑箱模型（Black-box Model）

* 以黑箱方式組織需求主題，強調「輸入→處理→輸出」流程
* 關注系統行為而非內部實現方式
* 有助於確保需求不預設特定技術解決方案

## 8.4 需求與設計的區別（Requirements vs. Design）

明確區分需求與設計是軟體工程中的基本原則：

* **範疇（Scope）**：組織需求之界定，記錄於Vision & Scope文件中
* **需求（Requirements）**：描述軟體必須做什麼（What），即必須提供之行為和能力
* **設計（Design）**：描述如何實現需求（How），包括技術架構、模組組織和演算法選擇

需求與設計的主要區別：
1. 需求專注於問題域，設計專注於解決方案域
2. 需求由利害關係人定義，設計由開發團隊決定
3. 需求應該避免實現細節，設計必須包含實現細節

## 8.5 需求工程流程（Requirements Engineering Process）

### 8.5.1 傳統需求工程活動（Traditional Requirements Activities）

傳統需求工程包含四個核心活動：

1. **需求偵掘（Elicitation）**：從利害關係人獲取需求資訊
2. **需求分析（Analysis）**：分析、細化和模型化收集到的需求
3. **需求驗證（Validation）**：確保需求正確反映使用者期望
4. **需求管理（Management）**：控制需求變更和版本

### 8.5.2 需求工程定義（Requirements Engineering Definition）

* 定義：收集、分析、文件化、驗證並維護需求之全生命週期流程
* 目標：確保系統需求完整、一致、可追蹤，並與利害關係人期望一致

### 8.5.3 需求工程流程模型（RE Process Models）

* 傳統模型：線性順序執行偵掘→分析→驗證→管理
* 實務上為迭代且交錯執行，以螺旋式視圖顯示過程不斷循環
* 敏捷方法中，需求工程更加頻繁迭代且注重變更適應性

## 8.6 需求品質模型（Requirements Quality Models）

### 8.6.1 FURPS+ 品質模型

FURPS+是Hewlett-Packard開發的需求品質評估框架：

* **Functionality（功能性）**：系統提供的核心功能和服務
* **Usability（易用性）**：使用者操作是否直覺、學習曲線是否平緩
* **Reliability（可靠性）**：系統在各種條件下的穩定性和容錯能力
* **Performance（效能）**：處理速度、回應時間、資源利用效率
* **Supportability（可支援性）**：系統可維護性、可擴展性和技術支援

"+"代表額外考量因素：
* 設計限制
* 實現需求
* 介面需求
* 物理需求

### 8.6.2 可靠性指標範例（Reliability Metrics Examples）

* 醫院心臟監控系統故障頻率 < 1/20 年
* 銀行主機系統平均每12個月故障 ≤ 1次（辦公時間內）
* 航空控制系統可用性 > 99.999%（即每年停機時間少於5.26分鐘）

## 8.7 IEEE Std. 830 SRS 規範要素（IEEE Std. 830 Specification Issues）

IEEE 830標準定義了軟體需求規格應涵蓋的關鍵元素：

* **Functionality（功能）**：系統應提供之服務和行為
* **Design constraints（設計制約）**：實現標準、程式語言、法規政策、硬體環境要求
* **External interfaces（外部介面）**：人機介面、硬體介面、通訊協定、其他系統互動界面
* **Performance（效能）**：處理速度、可用性、回應時間、恢復時間、資源利用率
* **Attributes（屬性）**：可攜性、正確性、可維護性、安全性、可靠性等質量特性

IEEE 830標準提供了SRS文件的結構範本，包括：
1. 簡介（目的、範圍、定義、參考文獻、概述）
2. 總體描述（產品視角、功能、使用者特徵、限制條件）
3. 詳細需求（功能需求、非功能需求、外部介面）
4. 附錄（術語表、分析模型、待解決問題）

## 8.8 需求偵掘（Requirements Elicitation）

### 8.8.1 偵掘階段（Elicitation Phases）

需求偵掘通常分為四個主要階段：

1. **Discovery（發現）**：蒐集現行系統資訊與使用者需求
   * 技術：訪談、觀察、問卷、文件分析、腦力激盪
   * 目標：建立基本問題域知識並識別主要利害關係人

2. **Classification（分類）**：整理並組織收集到的需求
   * 技術：分類法、親和圖、概念建模
   * 目標：將相關需求分組並建立需求架構

3. **Prioritization & Negotiation（優先排序與協商）**：
   * 技術：MoSCoW（Must-Should-Could-Won't）、Kano模型、成本效益分析
   * 目標：解決衝突需求並確立開發順序

4. **Specification（規格化）**：
   * 技術：使用者故事、需求模板、形式化語言
   * 目標：撰寫結構化需求文件草案

### 8.8.2 需求偵掘技術（Elicitation Techniques）

#### 8.8.2.1 訪談技術（Interviewing）

* **封閉式訪談（Closed Interviews）**：預設結構化問題，適合收集特定資訊
* **開放式訪談（Open Interviews）**：無預設問題框架，適合探索性研究

訪談要點：
* 保持開放心態，避免引導性問題
* 使用原型和實例輔助溝通
* 注意非語言線索和潛在需求
* 對回答進行總結確認，確保理解無誤

#### 8.8.2.2 其他偵掘技術

* **觀察法（Observation）**：直接觀察使用者工作流程
* **問卷調查（Questionnaires）**：收集大量使用者意見
* **工作坊（Workshops）**：促進協作需求開發
* **文件分析（Document Analysis）**：研究現有系統文件
* **原型法（Prototyping）**：通過早期系統模型收集反饋

### 8.8.3 偵掘常見困難（Common Elicitation Challenges）

* 利害關係人可能不清楚自己的確切需求
* 不同群體使用不同術語，造成溝通障礙
* 組織政治和部門利益可能影響需求表達
* 需求往往隨著開發過程不斷演進和變更
* 技術約束與業務需求之間的平衡

## 8.9 需求規格（Requirements Specification）

需求規格文件化根據目標讀者可分為不同層級：

* **使用者需求（User Requirements）**：
  * 面向非技術群體，使用自然語言和圖表
  * 專注於業務目標和整體系統功能
  * 盡量避免技術術語和實現細節

* **系統需求（System Requirements）**：
  * 包含技術細節，面向開發團隊
  * 詳述系統行為、介面、資料格式、執行環境
  * 需明確、可測試且具可追蹤性

### 8.9.1 需求文件撰寫指引（Writing Guidelines）

* 建立統一用語和格式：
  * 必要需求用「shall」，建議性需求用「should」
  * 避免模糊詞彙如「通常」、「一般」、「有時」
  * 每條需求應有唯一標識符，便於追蹤

* 提高可讀性：
  * 標示關鍵字、使用一致性術語
  * 避免使用過於專業的電腦術語
  * 結構化呈現，使用表格和圖形輔助理解

* 增加理解深度：
  * 附上需求理由（rationale），解釋其重要性
  * 提供示例或場景說明需求應用情境
  * 參考相關業務規則或法規要求

## 8.10 需求類型（Types of Requirements）

### 8.10.1 功能需求（Functional Requirements）

功能需求描述系統應提供的服務和行為：
* 定義系統應執行的具體功能
* 指定輸入、處理、輸出的細節
* 明確系統在特定條件下的反應

#### 功能需求範例：

**ATM取款功能**
* F1 Withdraw Cash
  * F1.1 選擇Withdraw Cash → 系統顯示帳戶類型選擇介面
  * F1.2 選擇Account Type → 系統顯示金額輸入介面
  * F1.3 輸入Amount → 系統驗證交易、執行取款、列印回執或顯示失敗訊息

**行李處理系統**
* 系統每分鐘必須能處理不超過20件行李
* 無行李輸入時，輸送帶應自動停止以節省能源

### 8.10.2 非功能需求（Non-Functional Requirements）

非功能需求描述系統的品質特性和運行條件：
* 定義系統如何執行功能，而非執行什麼功能
* 通常跨越多個功能和組件
* 往往決定系統整體使用體驗和可接受度

#### 非功能需求範例：

**效能需求**
* 系統必須在30秒內顯示所有錯誤訊息
* 若系統無回應超過2分鐘，必須自動退回銀行卡

**可用性需求**
* 系統必須全天候24小時可用
* 計劃性維護應提前7天通知使用者

**可維護性需求**
* 只有授權維護人員可以新增或移除設備
* 系統升級不應造成超過15分鐘的停機時間

### 8.10.3 領域需求（Domain Requirements）

領域需求源自特定應用領域的特性：
* 反映產業標準、法規或最佳實踐
* 通常使用領域專業術語表達
* 實現可能需要特定領域知識

#### 領域需求範例：

**醫療系統領域需求**
* 系統必須符合HIPAA隱私保護規範
* 放射治療計劃必須經過雙重確認機制

**金融系統領域需求**
* 系統必須實現四眼原則處理超過特定金額的交易
* 必須保存所有交易記錄至少7年

## 8.11 度量非功能需求（Metrics for Non-Functional Requirements）

將非功能需求量化是確保其可測試性的關鍵：

| 屬性（Property） | 度量（Measure）                                           |
| ---------------- | --------------------------------------------------------- |
| Speed            | 交易數/秒、使用者回應時間、畫面刷新率、批次處理時間       |
| Size             | 記憶體使用量(MB)、儲存空間需求、ROM晶片數                 |
| Usability        | 培訓時間、錯誤率、完成任務時間、使用者滿意度評分          |
| Reliability      | MTBF（平均失效間隔）、故障率、資料完整性、復原點目標(RPO) |
| Availability     | 服務水平協議(SLA)、每年停機時間、計劃外中斷時間、重啟時間 |
| Portability      | 平台相依性程式碼比例、支援平台數量、移植所需工作量        |
| Security         | 安全漏洞數量、通過滲透測試百分比、身份驗證強度            |
| Scalability      | 最大併發使用者數、資料增長處理能力、垂直/水平擴展能力     |

有效的非功能需求應具備：
* 具體的可衡量值
* 明確的測量條件（環境、負載）
* 達成標準和驗收條件
* 成本與效益的平衡考量

## 8.12 SRS 建構（Building the SRS Document）

軟體需求規格書應遵循以下原則：

* 只描述「軟體應做什麼」，不包含實現細節或開發流程
* 採用一致的格式和術語，避免歧義
* 確保需求能被追蹤到來源和後續實現

常見SRS文件格式：
* **結構化規格（Structured Specifications）**：使用分層編號的文本描述
* **表格式規格（Tabular Specifications）**：以表格形式呈現需求及其屬性
* **模型導向規格（Model-Based Specifications）**：使用UML等建模語言

IEEE 830-2009標準提供了通用的SRS模板，包含以下主要章節：
1. 引言（Introduction）
2. 整體描述（Overall Description）
3. 特定需求（Specific Requirements）
4. 支援資訊（Supporting Information）

## 8.13 需求管理（Requirements Management）

需求管理是控制需求變更的系統性流程：

1. **問題分析（Problem Analysis）**：識別、討論並提出需求變更
2. **變更分析與評估（Change Analysis & Costing）**：評估變更影響與實現成本
3. **變更實施（Change Implementation）**：更新SRS及相關文件，執行必要的設計變更

需求管理是能力成熟度模型（CMMI）Level 2的關鍵流程區域（KPA），強調：
* 需求變更控制流程
* 需求追蹤機制
* 影響評估和工作量估算
* 版本管理和需求基線化

敏捷開發方法以產品待辦事項（Product Backlog）優先順序調整取代正式變更流程，更靈活地適應需求變化。

## 8.14 需求驗證（Requirements Validation）

### 8.14.1 驗證目的與技術

* **目的**：確保需求文件完整、準確地反映客戶真實需求
* **驗證技術**：
  * 需求審查（Requirements Reviews）：正式檢視需求內容
  * 原型演示（Prototyping）：通過早期實現驗證需求可行性
  * 測試案例生成（Test Case Generation）：從需求推導測試案例
  * 使用者確認（User Acceptance）：獲取最終用戶對需求的認可

### 8.14.2 需求檢查標準

高質量需求應滿足以下標準：

* **有效性（Validity）**：需求確實反映用戶期望
* **一致性（Consistency）**：需求間不存在矛盾
* **完整性（Completeness）**：包含所有必要功能和條件
* **可行性（Feasibility）**：技術上可實現且經濟合理
* **無歧義性（Unambiguity）**：只有唯一解釋
* **可驗證性（Verifiability）**：能夠通過測試證明實現
* **可追蹤性（Traceability）**：能追溯到源頭和實現元素
* **可修改性（Modifiability）**：結構允許有效進行變更

## 8.15 需求追蹤與度量（Traceability & Metrics）

### 8.15.1 需求追蹤（Requirements Traceability）

* **追蹤矩陣（Traceability Matrix）**：將需求與來源、設計元素、測試案例對應
* **雙向追蹤（Bidirectional Traceability）**：向前追蹤（需求→實現）與向後追蹤（實現→需求）
* **追蹤類型**：
  * 來源追蹤（Source Traceability）：需求與其來源（如利害關係人請求）
  * 需求間追蹤（Inter-requirement Traceability）：需求之間的依賴關係
  * 設計追蹤（Design Traceability）：需求與設計元素的對應
  * 測試追蹤（Test Traceability）：需求與驗證測試的對應

### 8.15.2 需求度量（Requirements Metrics）

常用需求度量指標：
* 需求數量及分佈（按類型、優先級）
* 需求變更頻率和範圍
* 需求不穩定性指標（變更需求百分比）
* 需求完整性驗證評分（1-5級）
* 需求缺陷密度（每需求點發現的問題數）
* 需求實現覆蓋率

這些度量有助於：
* 評估需求品質
* 預測專案風險
* 優化資源分配
* 改進需求工程流程

## 8.16 結構化／表格式規格範例（Structured & Tabular Example）

### 8.16.1 胰島素幫浦規格範例

**表格式需求規格示例：血糖變化率對照胰島素劑量**

| 血糖值（mg/dL） | 血糖變化率（mg/dL/min） | 胰島素劑量（單位/小時） |
| --------------- | ----------------------- | ----------------------- |
| < 70            | 任何值                  | 0                       |
| 70-110          | < -2                    | 0                       |
| 70-110          | -2至+2                  | 1                       |
| 70-110          | > +2                    | 2                       |
| 111-150         | < -2                    | 1                       |
| 111-150         | -2至+2                  | 2                       |
| 111-150         | > +2                    | 3                       |
| > 150           | 任何值                  | 4                       |

此表格式規格清晰定義了系統在各種條件下的精確行為，易於實現和測試。

### 8.16.2 其他結構化規格範例

**結構化功能需求範例：**

```
FR-101: 使用者登入功能
描述: 系統應提供使用者驗證機制
前置條件: 使用者已註冊帳號
基本流程:
  1. 使用者輸入用戶名和密碼
  2. 系統驗證用戶憑證
  3. 驗證成功後顯示主頁面
替代流程:
  2a. 驗證失敗:
    1. 系統顯示錯誤訊息
    2. 返回登入頁面
優先級: 高
來源: 使用者訪談 UT-23
```

# 第九章 結構化分析（Structured Analysis）

## 9.1 結構化分析概述

結構化分析是一種系統開發方法論，著重於將複雜系統分解為易於理解和管理的組件。這種方法強調系統功能、資料流程和行為的明確描述，為後續的設計和實現奠定基礎。結構化分析特別適用於需要清晰流程和資料關係的商業資訊系統。

### 參考文獻
* Roger S. Pressman，《Software Engineering: A Practitioner's Approach》，Chapter 12，8th Ed.，McGraw-Hill, 2014
* Stephen R. Schach，《Object Oriented and Classical Software Engineering》，Chapter 12，8th Ed.，McGraw-Hill, 2011
* Robert T. Futrell 等，《Quality Software Project Management》，Prentice Hall, 2002

## 9.2 分析建模基礎

### 9.2.1 分析建模起點
* 從 FAST（Facilitated Application Specification Techniques）工作文件獲取範疇說明（statement of scope）
  - FAST 是一種需求引出技術，通過團隊合作會議收集需求
  - 範疇說明定義了系統的邊界和主要功能
* 解析範疇說明以擷取三類核心資訊：
  - 資料（data）：系統需要處理的信息
  - 功能（function）：系統需要執行的操作
  - 行為（behavioral）：系統如何回應外部事件
* 分析模型（analysis model）是系統的第一個技術化表示，為後續設計模型（design model）提供基礎

### 9.2.2 規則要點
* 聚焦問題/業務領域可見之需求，保持高層次抽象
  - 避免過早考慮實現細節
  - 重點關注"做什麼"而非"怎麼做"
* 每個模型元素應增進對資訊領域、功能與行為的理解
  - 確保模型中沒有無意義或多餘的元素
* 全系統應盡量降低耦合（coupling）
  - 模組間依賴性越低，系統越易於理解和維護
* 確保模型對所有利害關係人都有價值
  - 技術人員和非技術人員都能從中獲取所需信息
* 盡量簡化模型，避免不必要複雜
  - 遵循奧卡姆剃刀原則：若有多種設計方案，選擇最簡單的

## 9.3 領域分析（Domain Analysis）

領域分析是結構化分析的前置步驟，專注於理解特定應用領域的概念和規則。

### 9.3.1 領域分析步驟
1. 定義要探討之領域（domain）
   - 明確領域範圍和特徵
2. 收集該領域具代表性的應用範例
   - 包括成功和失敗的案例
   - 相關文獻和專家知識
3. 分析每個範例
   - 識別共同特徵和變化點
   - 尋找重複出現的概念和關係
4. 發展對象（objects）之分析模型
   - 建立領域模型，捕捉關鍵概念
   - 定義概念間的關係和規則

### 9.3.2 領域分析的價值
* 提高重用機會，減少重複工作
* 確立領域共識，使專家和開發人員使用相同術語
* 幫助識別潛在需求和限制條件

## 9.4 分析模型結構（Structure of the Analysis Model）

Pressman 提出的分析模型包含多個互補的子模型，從不同角度描述系統。

### 9.4.1 核心子模型
* 資料模型：描述資料結構和關係（如ERD）
* 功能模型：展示資料如何被處理（如DFD）
* 行為模型：說明系統如何對事件做出反應（如STD）

### 9.4.2 模型間關係
* 各模型提供系統不同視角
* 需確保模型間一致性（參見9.20模型整合）
* 透過交叉引用確保完整性

## 9.5 資料建模

### 9.5.1 實體關係圖（Entity Relationship Diagram, ERD）
* 由 Peter Chen（1979）提出，最初用於關聯式資料庫設計
* 用於表示資料物件及其關係
* 主要元件：
  - 實體（entity）：現實世界中獨立存在的對象
  - 屬性（attribute）：描述實體特徵的數據項
  - 關係（relationship）：實體間的關聯
  - 型態指示（type indicators）：關係的性質和限制

#### 基本符號
* 方框：表示實體
* 線段：表示實體間關係
* 関係型態：
  - 1:1（一對一）：一個實體只能與另一實體的一個實例相關
  - 1:N（一對多）：一個實體可與另一實體的多個實例相關
  - M:N（多對多）：兩個實體的多個實例相互關聯
* Cardinality（基數）：關係中物件出現次數規格
  - 精確指定參與關係的最小/最大實例數
* Modality（必要性）：
  - 0 = 可選（非強制參與關係）
  - 1 = 強制（必須參與關係）

#### 常見物件類型
* 外部實體（External entities）：printer、user、sensor
* 事物（Things）：reports、displays、signals
* 事件（Occurrences）：interrupt、alarm
* 角色（Roles）：manager、engineer
* 組織單位（Organizational units）：division、team
* 地點（Places）：manufacturing floor
* 結構（Structures）：employee record

#### ERD建立步驟
1. Level 1：標示所有實體及其連結
   - 識別系統中的主要數據對象
2. Level 2：加入所有關係
   - 定義實體間的關聯類型
3. Level 3：新增屬性以深化模型
   - 為每個實體添加相關屬性

#### 工具支援
* Visio 2010等CASE工具支持ERD繪製
* 可通過內建資料詞典定義或查詢實體、屬性、關係

## 9.6 功能建模

### 9.6.1 資料流程圖（Data Flow Diagram, DFD）
* 圖形化描述資訊如何從輸入轉換為輸出，以及各轉換過程
* 可在任意抽象層次使用，提供功能與資訊流雙重視角
* DFD強調資料流動而非控制流程，專注於"做什麼"而非"如何做"

#### 元件符號
* 外部實體（External entity）：資料生產者/消費者
  - 表示系統外部的數據源或目標
  - 通常用方框表示
* 過程（Process）：資料轉換器
  - 表示將輸入數據轉換為輸出的功能
  - 通常用圓形或氣泡表示
* 資料流（Data flow）：箭頭
  - 表示數據在系統中的移動路徑
  - 箭頭方向表示數據流向
* 資料庫（Data store）：資料儲存
  - 表示暫時或永久存儲的數據集合
  - 通常用平行線表示

#### 建置流程
1. Level 0（Context Diagram）：
   - 單一過程表示整個系統
   - 顯示所有外部實體與資料流
   - 定義系統邊界
2. Level 1…n：分解過程
   - 每層約保持6個過程，避免圖形過於複雜
   - 保持「平衡」（balance）：上一層傳入/傳出的資料流，對應到下一層的輸入/輸出

#### 指導原則
* 所有圖示皆須以有意義名稱標註
* 由Context Level開始，並顯示所有外部實體
* 資料流箭頭皆須標示名稱
* 不得以程式流程（procedural logic）表示
  - DFD表達"什麼"而非"如何"

### 9.6.2 流程模型（Flow Model）
* 所有電腦化系統皆為「資訊轉換」（information transform）
* 將系統視作黑箱，輸入→流程→輸出
  - 輸入：收集和驗證資料
  - 轉換：處理資料產生所需結果
  - 輸出：傳送結果到目的地

### 9.6.3 資料內容圖（Data Context Diagram, DCD）
* DFD的最高層次圖（Level 0）
* 顯示系統作為單一過程及其與終端點（terminators）之互動
* 定義系統邊界和主要介面

### 9.6.4 分層化DFD指導原則
* 每層保持約6個過程，避免圖形過於複雜
* 系統層次深度指導：
  - 簡單系統：2-3層
  - 中型系統：3-6層
  - 大型系統：5-8層
* 各層間須平衡輸入/輸出，確保資料流一致性
* 過程細化時，氣泡應各司其職，功能清晰明確

### 9.6.5 流程規範（Process Specification, PSPEC）
* 圖形DFD必須加以文字補充，說明：
  - 輸入：處理的資料
  - 轉換演算法：處理邏輯
  - 輸出：結果資料
  - 限制：處理條件和規則
  - 性能特性：時間、資源要求
* PSPEC可用多種形式表達：
  - 敘述文字
  - 偽程式（PDL）
  - 方程式
  - 表格
  - 圖表

## 9.7 行為建模（Behavioral Modeling）

### 9.7.1 狀態轉換圖（State Transition Diagram, STD）
* 描繪系統的可觀察狀態、事件、動作
* 核心元素：
  - State：系統在特定時刻之可觀察情況
    - 系統在該狀態下可執行特定動作
    - 系統保持在該狀態直到事件觸發轉換
  - Event：引發狀態改變之發生
    - 內部或外部條件變化
    - 時間經過
  - Action：於轉換後執行的程序
    - 轉換過程中執行的操作
    - 可能導致新事件或數據變化

### 9.7.2 控制規範（Control Specification, CSPEC）
* 以STD或程式啟動表（PAT）描述控制行為
* 補充DFD中的控制流程
* 描述系統如何對外部事件做出反應

## 9.8 規格說明技術

### 9.8.1 程式設計語言（Program Design Language, PDL）
* 又稱結構化英文（Structured English or Pseudocode）
* 結合程式語法結構與自然語言敘述
* 比流程圖更易於修改和維護
* 包含元素：
  - 元件定義
  - 介面描述
  - 資料宣告
  - 區塊結構
  - 條件/迴圈
  - I/O操作

### 9.8.2 資料詞典（Data Dictionary）
* 用於定義資料內容、控制資料及其可能值
* 儲存「where-used/how-used」資訊
* 確保資料定義的一致性和完整性
* 符號：
  - "="：由⋯組成
  - "|"：選擇其一
  - "\[ ]"：可選
  - "{ }"：重複
  - "( )"：群組
  - "*…*"：註解
* 範例：
  ```
  telephone_no = [local_extension | outside_no | 0]
  outside_no   = 9 + [service_code | domestic_no]
  service_code = [211|411|611|911]
  ```

### 9.8.3 結構化英文（Structured English）
* 簡化英文子集，每句為：
  - 代數方程式（X = (y\*(z+2))/q+14）
  - 或命令句（verb + object），可含IF…THEN…ELSE、CASE、LOOP
* 常用動詞列表：
  - GET/READ：獲取資料
  - PUT/DISPLAY：輸出資料
  - FIND：查詢資料
  - ADD：新增資料
  - COMPUTE：計算
  - VALIDATE：驗證資料

### 9.8.4 決策表（Decision Table）
* 處理多變量且邏輯複雜時使用
* 特別適合多條件組合的情況
* 建置方法：
  1. 列所有條件（inputs）與動作（actions）
  2. 標號所有可能組合（True/False）
  3. 為每組合指定動作
* 優點：
  - 結構清晰
  - 易於驗證完整性
  - 便於維護

## 9.9 模型驅動開發

### 9.9.1 模型驅動工程（Model-Driven Engineering, MDE）
* 以模型（而非程式）為開發產出，透過轉換自動產生可執行程式
* 優點：
  - 提高抽象層次
  - 快速適配新平台
  - 減少人工編碼錯誤
* 缺點：
  - 模型未必適於實現
  - 轉譯器開發成本高
  - 對工具依賴性強

### 9.9.2 模型驅動架構（Model-Driven Architecture, MDA）
* MDE的實現標準，由OMG提出
* 採UML子集描述系統
* 關鍵概念：
  - 平台無關模型（PIM）：抽象系統功能
  - 平台特定模型（PSM）：針對特定技術平台
  - 自動轉換：PIM→PSM→代碼

### 9.9.3 敏捷方法與MDE
* MDA宣稱支援迭代開發
* 然而大多數敏捷開發者對大量先期建模仍存疑
* 潛在衝突：
  - 敏捷強調簡約文檔和快速迭代
  - MDE需要精確詳細的模型

## 9.10 模型整合（Integrating the Models）

結構化分析包含多種模型，需確保彼此一致：

* DFD ↔ 資料詞典：
  - 所有資料流/資料庫皆須在詞典中定義
  - 確保名稱和結構一致性

* DFD ↔ PSPEC：
  - 每個氣泡須對應底層DFD或PSPEC
  - PSPEC處理的資料應與DFD的資料流相符

* ERD ↔ DFD/PSPEC：
  - 資料庫須對應ERD中的實體或關係
  - Process spec.中的操作應與ERD中的實體關係一致
  - 需維護資料操作與實體間的一致性

* DFD ↔ STD：
  - 控制氣泡需有對應STD
  - 每一條件/動作皆須對應特定資料流
  - 控制流需與系統狀態轉換匹配

### 9.10.1 整合的意義
* 確保模型間一致性，避免矛盾
* 提高分析的完整性和準確性
* 便於識別潛在問題和遺漏
* 為後續設計階段提供穩固基礎

### 9.10.2 常見整合問題
* 命名不一致：同一概念在不同模型中使用不同名稱
* 範圍不一致：模型間包含的系統元素不同
* 詳細程度不一致：某些模型過於詳細，而其他模型過於抽象
* 流程與狀態不匹配：DFD中的功能與STD中的狀態動作不對應

## 9.11 結構化分析的優缺點

### 9.11.1 優點
* 提供清晰的系統視圖，便於溝通
* 分層結構使複雜系統更易於理解
* 多種互補模型提供全面視角
* 已有成熟工具支持
* 適合處理資料密集型系統

### 9.11.2 缺點
* 難以表達非功能性需求
* 對於高度交互的系統，模型可能過於複雜
* 與面向對象設計的銜接不夠自然
* 文檔工作量大
* 在敏捷環境中可能被視為過重

## 9.12 結構化分析與其他方法比較

### 9.12.1 與面向對象分析的比較
* 結構化分析：
  - 重點在功能分解
  - 先考慮數據流後考慮數據結構
  - 基於功能和數據分離
* 面向對象分析：
  - 重點在對象識別
  - 對象封裝數據和行為
  - 基於對象責任和協作

### 9.12.2 與敏捷方法的關係
* 結構化分析強調全面前期規劃
* 敏捷方法偏好增量式需求和設計
* 可結合使用：
  - 關鍵部分應用結構化分析
  - 變化較大部分採用敏捷方法

# 第十章 結構化設計（Structured Design）

## 10.1 參考文獻
* Roger S. Pressman，《Software Engineering: A Practitioner's Approach》，8th Edition，McGraw-Hill，2014
* Scott Tilley，《Systems Analysis and Design》，12th Edition，Cengage Learning，2019
* Stephen R. Schach，《Object-Oriented and Classical Software Engineering》，Chapter 12，8th Edition，McGraw-Hill，2011
* Ian Sommerville，《Software Engineering》，10th Edition，Pearson，2016
* Martin Fowler，《Patterns of Enterprise Application Architecture》，Addison-Wesley，2002

## 10.2 系統設計概覽

### 定義
系統設計是將分析模型轉換為系統設計模型的過程，它定義設計目標並將整體系統分解為可由各小組負責的子系統。這是軟體開發生命週期中連接需求分析與程式實作的關鍵橋梁。

### 價值
* **提供可評估品質的藍圖**：設計文件作為技術評審的基礎，使開發團隊能在編碼前發現潛在問題
* **精確地將使用者需求轉為最終系統**：確保系統功能與效能符合分析階段收集的需求
* **作為後續實作、測試與維護的基礎**：良好的設計文件可減少開發時間、降低測試複雜度、提高系統可維護性
* **促進團隊溝通與合作**：提供共同語言與參考點，使不同專業領域的成員能有效溝通
* **管理複雜度**：通過抽象化和模組化，幫助團隊處理大型、複雜系統

## 10.3 設計流程（The Design Process）

1. **高層次藍圖**：從分析模型的需求、資料與行為出發，描繪整體架構
   * 定義主要子系統與其相互關係
   * 規劃資料流動與儲存方式
   * 建立使用者介面的概念模型

2. **反覆精煉**：隨著設計迭代，逐步降低抽象層級，加入更多細節
   * 拆解子系統為更小的模組
   * 明確定義各模組的功能與責任
   * 設計模組間的介面與互動方式
   * 定義資料結構與演算法

3. **品質評估**：在每次迭代後，透過技術審查檢視設計品質（詳見第六章）
   * 檢視是否符合功能與非功能需求
   * 評估模組的內聚性與耦合度
   * 確認設計的可測試性與可擴展性
   * 識別潛在風險與技術限制

4. **設計特性**（McGlaughlin）：
   * **滿足所有顯性與隱性需求**：不僅考慮明確陳述的功能需求，也滿足隱含的非功能需求如效能、安全性與可用性
   * **可讀性與可理解性**：設計文件應清晰易懂，便於開發與測試團隊理解
   * **從實作角度完整呈現資料、功能與行為**：提供足夠詳細的資訊，使開發人員能無歧義地實作系統
   * **可驗證性**：設計應使測試團隊能夠驗證系統是否符合需求規格

## 10.4 設計目標（Design Goals）

### 可擴充（Extension）
* **定義**：系統能夠容易地加入新功能，無需大幅度修改現有架構
* **實現方式**：
  * 使用抽象介面與繼承機制
  * 採用插件架構與開放式擴展點
  * 實踐「開放封閉原則」(Open/Closed Principle)
* **評估指標**：新增功能所需修改的模組數量與範圍

### 可變更（Change）
* **定義**：系統能夠快速適應需求變動，降低變更成本
* **實現方式**：
  * 將易變部分與穩定部分分離
  * 使用配置文件控制系統行為
  * 隔離與第三方系統的依賴
* **評估指標**：變更傳播範圍、完成典型變更所需時間

### 簡潔（Simplicity）
* **定義**：設計清晰簡單，易於理解、實作與維護
* **實現方式**：
  * 避免不必要的複雜性
  * 遵循「KISS原則」(Keep It Simple, Stupid)
  * 使用一致的命名與設計模式
* **評估指標**：新團隊成員理解系統所需時間、模組平均規模

### 效率（Efficiency）
* **定義**：系統執行效能與資源利用率達到最佳平衡
* **實現方式**：
  * 選擇合適的演算法與資料結構
  * 識別性能瓶頸並優化
  * 實施快取與非同步處理
* **評估指標**：回應時間、吞吐量、資源占用率
* **平衡考量**：效率與可維護性間的取捨

## 10.5 設計概念（Design Concepts）

### 10.5.1 抽象（Abstraction）

#### 定義
抽象是在不同層級以問題領域或實作領域的術語描述系統的能力，隱藏不必要的細節，突顯重要特性。它是軟體工程中管理複雜度的基本手段。

#### 分層
* **需求分析階段**：環繞問題環境的高層敘述，使用業務領域術語
* **架構設計階段**：描述主要系統元件與其交互
* **詳細設計階段**：指定演算法與資料結構
* **實作階段**：逐步降低抽象，最終生成程式碼

#### 種類
* **程序抽象（Procedural Abstraction）**：
  * 命名的一段指令序列，定義操作而不揭露實作
  * 通過函數/方法/子程序實現
  * 強調「做什麼」而非「怎麼做」
  
* **資料抽象（Data Abstraction）**：
  * 描述資料物件的結構與行為，隱藏實現細節
  * 通過類別/介面/抽象資料型別實現
  * 將資料與操作封裝在一起

* **控制抽象（Control Abstraction）**：
  * 描述程序執行流程的控制結構
  * 如選擇（if-then-else）、迴圈（for, while）等

#### 範例：開門（Procedural & Data Abstraction）
```
// 程序抽象
open
 ├─ walk to door
 ├─ reach for knob
 ├─ turn knob (或 key)
 ├─ push/pull door
 └─ walk through

// 資料抽象
door = {
  manufacturer, model_number, type,
  swing_direction, inserts, lights,
  weight, opening_mechanism
}
```

#### 抽象的價值
* 減少認知負擔，使開發人員專注於當前關注點
* 促進重用與降低維護成本
* 提供清晰界面，降低系統元件間的耦合

### 10.5.2 精煉（Refinement）

#### 定義
精煉是由高層次描述逐步揭露低層次細節的過程，是從抽象到具體的系統化轉換方法。也稱為「逐步求精」或「階層式精煉」。

#### 步驟
1. 以功能宣告開始，確定「做什麼」
2. 反覆擴充細節，形成更低層結構，說明「怎麼做」
3. 直到達到可實作的程度，接近程式碼層級
4. 抽象與精煉互補：抽象避免一次顯示所有細節，精煉則在需要時揭露

#### 精煉原則
* **逐步分解**：每次只增加一層細節
* **保持一致**：確保低層級精煉符合高層級描述
* **完整性**：確保所有需求都被覆蓋
* **可追溯**：高層級與低層級元素間建立明確對應關係

#### 範例：逐步精煉
```
// 層級 1
open()

// 層級 2
open()
  ├─ walk_to_door()
  ├─ reach_knob()
  ├─ open_door()
  └─ close_door()

// 層級 3
open()
  ├─ walk_to_door()
  │   ├─ locate_door()
  │   └─ move_to_door()
  ├─ reach_knob()
  │   ├─ extend_arm()
  │   └─ grasp_knob()
  ├─ open_door()
  │   ├─ turn_knob()
  │   └─ push_door()
  └─ close_door()
      ├─ pull_door()
      └─ release_knob()
```

### 10.5.3 模組化（Modularity）

#### 定義
模組化是將大系統拆分為小且高度內聚的模組，透過定義良好的介面彼此互連的設計原則。它是軟體設計的核心原則，有助於管理複雜度與促進團隊協作。

#### 模組屬性
* **輸入／輸出**：模組接收與返回的資料
* **功能**：模組執行的任務或職責
* **內部邏輯**：實現功能的演算法與流程
* **私有資料**：模組內部使用的資料，對外界隱藏

#### 模組化優勢
* **降低複雜度**：將系統分解為可管理的部分
* **促進並行開發**：不同團隊可同時處理不同模組
* **提高可維護性**：變更影響範圍局限於少數模組
* **促進重用**：設計良好的模組可在不同系統中重用
* **簡化測試**：可獨立測試各模組功能

#### 設計取捨
* **避免副作用**：模組不應修改其參數或全域變數，降低維護成本
* **介面設計**：應明確定義輸入、輸出及錯誤處理
* **連接少且簡單**：減少模組間資料傳遞，以免模組間耦合過高
* **模組大小**：過大難以理解，過小增加系統複雜度
* **封裝程度**：決定資訊對外部的可見性

#### 模組化策略
* **資訊隱藏**：隱藏實作細節，只通過介面互動
* **功能獨立**：模組應有單一明確職責（單一責任原則）
* **重用優先**：設計時考慮模組的可重用性
* **變更預測**：隔離可能變更的部分到特定模組

## 10.6 結構化設計（Structured Design）

### 目標
結構化設計是一種系統化方法，目的是由資料流程圖（DFD）映射出程式架構（Structure Chart），並以處理規格（PSPEC）與狀態轉換圖（STD）定義各模組內容。這種方法源於1970年代，是最早的系統化軟體設計方法之一。

### 流程與步驟
1. **建立 DFD**：
   * 明確定義系統輸入、處理過程與輸出
   * 識別主要資料流與其轉換過程
   * 確定資料儲存需求

2. **轉換為結構圖**：
   * 以 DFD 氣泡對應結構圖的模組
   * 標示模組間的控制與資料耦合
   * 組織模組形成層次結構

3. **細化模組內容**：
   * 撰寫模組規範（MSPEC）
   * 定義演算法與資料結構
   * 接近最終程式碼層級但保持實作獨立性

### 結構化設計的價值
* **系統化程序**：提供明確步驟，降低設計主觀性
* **文件化**：生成標準化、易於理解的設計文件
* **一致性**：促進團隊成員間設計風格的一致
* **可追溯性**：從需求到設計到程式碼的清晰對應

### 10.6.1 結構圖（Structure Chart）

#### 構成
* **長方形**：表示程式模組或子程序
* **箭頭**：表示模組間的呼叫關係
* **符號**：表示資料傳遞、控制流程、條件或迴圈
* **層次排列**：反映模組間的控制關係

#### 控制層級
* **高階控制模組**：負責協調與呼叫下級模組
* **中間層模組**：既被上級呼叫，也呼叫下級
* **最低層模組**：執行具體功能，通常不呼叫其他模組

#### 結構圖設計原則
* **平衡深度與寬度**：避免過深或過寬的結構
* **功能聚焦**：每個模組應有明確的功能目標
* **資訊隱藏**：隱藏實作細節，減少模組間依賴
* **模組化**：促進重用與獨立測試

### 10.6.2 結構圖符號

#### 資料耦合（Data Couple）
* **實心圓○**：表示輸入資料參數
* **空心圓○**：表示輸出資料參數
* **雙向箭頭**：表示參數既是輸入也是輸出（通常應避免）

#### 控制耦合（Control Couple）
* **實心點●**：表示控制旗標或狀態參數
* **用途**：控制下級模組的行為或執行流程
* **設計建議**：盡量減少使用，因可能增加模組間依賴

#### 迴圈／條件符號
* **迴圈符號（⟳）**：表示重複呼叫模組
* **條件符號（◇）**：表示滿足條件才呼叫
* **設計建議**：條件判斷應集中在上級模組，降低複雜度

#### 其他常見符號
* **資料庫符號（㏒）**：表示資料儲存
* **註解框**：提供額外說明或限制條件
* **模組類型（I/P/T）**：輸入、處理、轉換等功能標記

### 10.6.3 資料流程分析（Data Flow Analysis, DFA）

#### 目的
資料流程分析是將資料流程圖轉換為結構圖的系統化方法，其目的是辨識系統中的高內聚區域，形成功能獨立的模組。

#### 流程
1. **找出各資料流的最高抽象點**：
   * 識別輸入轉換為輸出的關鍵節點
   * 尋找資料流的主要分支與合併點

2. **分解 DFD**：
   * 以資料流轉換點為中心劃分模組
   * 形成具有高內聚性的功能單元
   * 確定模組間的呼叫關係與資料傳遞

3. **優化設計**：
   * 若耦合過高，適度調整模組邊界
   * 平衡模組大小與複雜度
   * 確保符合設計目標與品質要求

#### 轉換策略
* **轉換中心法（Transform-Centered Approach）**：
  * 識別輸入→處理→輸出的核心轉換流程
  * 設計高層控制模組協調三個階段
  * 適用於資料處理型系統

* **交易中心法（Transaction-Centered Approach）**：
  * 識別基於不同交易類型的分支流程
  * 設計分發器模組根據交易類型呼叫對應處理模組
  * 適用於多種請求類型的系統

### 10.6.4 範例：文字計數（Word Counting）

#### DFD 第一層
* 輸入檔名 → 讀取檔案 → 計算詞數 → 輸出結果

#### 結構圖精煉
```
word_counter (主控模組)
 ├─ read_file_name()：提示並取得檔案名稱
 ├─ validate_file_name()：檢查檔案可用性
 ├─ count_words()：計算檔案中的字數
 └─ produce_output()：格式化並顯示結果
```

#### 更詳細的結構圖
```
word_counter
 ├─ read_file_name()
 │   ├─ display_prompt()
 │   └─ get_user_input()
 ├─ validate_file_name()
 │   ├─ check_file_exists()
 │   └─ check_file_readable()
 ├─ count_words()
 │   ├─ open_file()
 │   ├─ read_file_content()
 │   ├─ tokenize_content()
 │   └─ count_tokens()
 └─ produce_output()
     ├─ format_result()
     └─ display_result()
```

#### MSPEC／PDL（程序設計語言）
```
// 模組：read_file_name
PURPOSE: 提示使用者輸入檔案名稱並讀取
INPUT: 無
OUTPUT: 檔案名稱字串
ALGORITHM:
1. 顯示提示訊息「請輸入檔案名稱：」
2. 讀取使用者輸入的字串
3. 去除字串前後空白
4. 返回處理後的檔案名稱

// 模組：validate_file_name
PURPOSE: 驗證檔案名稱是否有效且可讀取
INPUT: 檔案名稱字串
OUTPUT: 布林值（是否有效）
ALGORITHM:
1. 檢查檔案名稱是否為空字串
2. 嘗試開啟檔案以確認其存在
3. 確認檔案可讀取
4. 如遇錯誤則顯示相應錯誤訊息並返回false
5. 若通過所有檢查則返回true

// 模組：count_words
PURPOSE: 計算檔案中的字數
INPUT: 有效的檔案名稱
OUTPUT: 字數計數
ALGORITHM:
1. 開啟指定檔案
2. 逐行讀取檔案內容
3. 將每行分割為單詞（以空白字元為分隔）
4. 累計單詞數量
5. 關閉檔案
6. 返回總字數

// 模組：produce_output
PURPOSE: 格式化並輸出結果
INPUT: 檔案名稱、字數計數
OUTPUT: 無（顯示到螢幕）
ALGORITHM:
1. 構造輸出字串「檔案 [檔案名稱] 包含 [字數] 個單詞」
2. 將結果顯示到螢幕
3. 詢問是否將結果寫入檔案
4. 若需要，則請求輸出檔名並寫入結果
```

## 10.7 設計品質（Design Quality）

### 10.7.1 耦合（Coupling）

#### 定義
耦合是軟體模組間相互依賴的程度，反映了一個模組依賴於其他模組的強度。良好的設計應盡量降低模組間的耦合，以提高獨立性與可維護性。

#### 種類（優良→劣質）

1. **資料耦合（Data Coupling）**
   * **特徵**：模組間僅通過簡單參數傳遞資料
   * **範例**：`result = calculate_area(length, width);`
   * **優點**：依賴最小，界面清晰，易於理解與維護

2. **資料結構耦合（Stamp Coupling）**
   * **特徵**：模組間傳遞複合資料結構
   * **範例**：`display_employee(employee_record);`
   * **問題**：模組可能只使用結構的部分欄位，增加不必要依賴

3. **控制耦合（Control Coupling）**
   * **特徵**：一個模組通過旗標、開關控制另一模組行為
   * **範例**：`process_data(data, MODE_FAST);`
   * **問題**：增加模組間邏輯依賴，降低獨立性

4. **共用耦合（Common Coupling）**
   * **特徵**：多個模組共享全域變數或資料
   * **範例**：`global_config.max_users = 100;`
   * **問題**：一個模組的變更可能影響所有使用該全域資料的模組

5. **內容耦合（Content Coupling）**
   * **特徵**：一個模組直接修改另一模組的內部資料或結構
   * **範例**：`other_module.private_data = new_value;`
   * **問題**：破壞封裝，造成高度依賴，極難維護

#### 降低耦合的策略
* **明確介面**：定義清晰的模組介面，隱藏實作細節
* **減少共享**：避免使用全域變數，使用參數傳遞資料
* **依賴注入**：通過參數傳入依賴，而非硬編碼
* **中介者模式**：使用中介者協調模組間通信
* **事件機制**：通過發佈/訂閱模式降低直接依賴

### 10.7.2 凝聚（Cohesion）

#### 定義
凝聚是模組內部元素的功能關聯強度，反映了模組執行單一、明確功能的程度。高凝聚模組專注於單一職責，易於理解、測試與重用。

#### 種類（優良→劣質）

1. **功能凝聚（Functional Cohesion）**
   * **特徵**：模組內所有元素共同完成單一相關功能
   * **範例**：`calculate_interest(principal, rate, time)`
   * **優點**：最理想的凝聚類型，易於理解、測試與維護

2. **連續凝聚（Sequential Cohesion）**
   * **特徵**：模組內元素形成處理序列，一個元素的輸出是下一個的輸入
   * **範例**：`read_data → validate_data → process_data`
   * **評價**：較好，但如果處理步驟變更則整體需修改

3. **通訊凝聚（Communicational Cohesion）**
   * **特徵**：模組內元素操作相同資料集但執行不同功能
   * **範例**：`update_customer(customer_id, name, address, credit_limit)`
   * **評價**：中等，功能間關聯較弱，但至少共享資料

4. **時序凝聚（Temporal Cohesion）**
   * **特徵**：模組內元素在相同時間執行，但功能可能不相關
   * **範例**：`system_startup()` 或 `daily_maintenance()`
   * **問題**：元素間缺乏邏輯關聯，僅因時間而組合

5. **程序凝聚（Procedural Cohesion）**
   * **特徵**：模組內元素按特定順序執行，但可能功能無關
   * **範例**：`first_step(); second_step(); final_step();`
   * **問題**：元素間僅有執行順序關聯，缺乏功能關聯

6. **邏輯凝聚（Logical Cohesion）**
   * **特徵**：模組內元素執行邏輯相似但功能不同的操作
   * **範例**：`handle_all_inputs()` 或 `process_all_errors()`
   * **問題**：模組內部實際包含多個不同功能，僅因分類相似而組合

7. **偶然凝聚（Coincidental Cohesion）**
   * **特徵**：模組內元素間沒有任何有意義的關聯
   * **範例**：`miscellaneous()` 或 `utils()`
   * **問題**：最差的凝聚類型，難以理解、測試與維護

#### 提高凝聚的策略
* **單一責任原則**：每個模組只負責一個功能
* **適當分解**：將多功能模組拆分為單功能模組
* **相關功能分組**：將高度相關的功能放在同一模組
* **定期重構**：識別並改善低凝聚模組
* **明確命名**：模組名稱應清晰反映其功能

### 10.7.3 控制層級（Control Hierarchy）

#### 深度（Depth）與寬度（Width）
* **深度**：控制層級的垂直層數，反映系統的複雜度
* **寬度**：每層的模組數量，反映系統的廣度
* **理想平衡**：
  * 適度深度（通常5-7層）避免太淺（缺乏抽象）或太深（管理複雜）
  * 適度寬度避免太窄（缺乏功能分離）或太寬（管理困難）

#### Fan-out
* **定義**：一個控制模組直接支配的下級模組數量
* **建議範圍**：3-7個（根據Miller的7±2法則）
* **過大問題**：控制模組過於複雜，難以理解與維護
* **過小問題**：系統層次過深，增加呼叫開銷與複雜度

#### Fan-in
* **定義**：一個模組被多少上級模組呼叫或使用
* **高Fan-in含義**：
  * 模組被廣泛重用，表示設計良好
  * 功能高度通用，抽象程度合適
* **最佳實踐**：
  * 工具類模組應有高Fan-in
  * 高Fan-in模組應保持高凝聚、低耦合
  * 需有良好測試覆蓋，確保穩定性

#### 控制層級的評估方法
* **模組間呼叫圖**：視覺化呈現模組間關係
* **結構度量**：計算模組的Fan-in/Fan-out比率
* **層級深度分析**：評估系統的階層複雜度
* **變更影響分析**：評估變更傳播範圍

## 10.8 設計方法（Design Approaches）

### Top-Down（自頂向下）
* **流程**：自最高層開始逐步分解為更小、更詳細的模組
* **特點**：
  * 先設計整體架構，後填充細節
  * 強調整體系統的功能與結構
  * 使用逐步精煉技術
* **優勢**：
  * 有助於理解整體系統目標
  * 提供清晰的大局觀
  * 適合有明確結構的系統
* **劣勢**：
  * 可能忽視底層實作問題
  * 初期難以驗證設計可行性
* **應用場景**：
  * 系統功能明確且穩定
  * 領域知識豐富
  * 需要嚴謹的設計階層
* **實施技巧**：
  * 使用功能分解圖（FDD）輔助分解過程
  * 在每一層級進行設計評審
  * 平衡分解深度，避免過度細化

### Bottom-Up（自底向上）
* **流程**：先構建低階模組，再逐步組合形成更高層系統
* **特點**：
  * 首先開發基礎元件
  * 通過元件組合創建子系統
  * 強調可重用性
* **優勢**：
  * 早期可驗證元件可行性
  * 有利於元件重用
  * 適合技術驅動的系統
* **劣勢**：
  * 可能偏離整體系統目標
  * 整合困難度較高
  * 難以提前規劃架構
* **應用場景**：
  * 開發底層函式庫或工具
  * 系統依賴於現有元件
  * 團隊對領域有深入理解
* **實施技巧**：
  * 定義良好的元件介面
  * 建立元件庫與版本管理
  * 定期進行整合測試

### Middle-Out（中間展開）
* **流程**：先針對主要事件或流程生成單一 DFD，再向上下分解
* **特點**：
  * 從系統核心功能開始
  * 同時向上抽象與向下精煉
  * 結合 Top-Down 與 Bottom-Up 的優點
* **優勢**：
  * 平衡系統視野與技術可行性
  * 快速聚焦於系統核心
  * 適應需求與技術的變化
* **劣勢**：
  * 需要較高的設計經驗
  * 可能導致方向不一致
  * 整合成本可能較高
* **應用場景**：
  * 開發過程中需求持續演進
  * 系統既有明確目標又有技術挑戰
  * 團隊經驗豐富
* **實施技巧**：
  * 使用原型驗證核心概念
  * 定義清晰的模組界面
  * 建立一致的設計標準

### 混合式設計（Hybrid Design）
* **概念**：根據系統不同部分的特性，彈性選用不同的設計方法
* **實施策略**：
  * 高層架構使用 Top-Down 方法確保整體一致性
  * 底層技術元件使用 Bottom-Up 方法確保可行性
  * 核心業務功能使用 Middle-Out 方法快速迭代
* **優勢**：
  * 適應不同子系統的特殊需求
  * 平衡開發速度與系統品質
  * 提高團隊協作效率
* **挑戰**：
  * 需要強大的架構協調
  * 可能增加溝通成本
  * 需要明確的設計界限

## 10.9 設計模式（Design Patterns）

### 定義與價值
設計模式是針對軟體設計中常見問題的解決方案範本，反映了專家經驗與最佳實踐。它們不是直接可用的程式碼，而是解決問題的通用模板，可適應不同情境。

* **優勢**：
  * 重用經驗證的解決方案
  * 建立共同語言，改善團隊溝通
  * 提高設計品質與一致性
  * 減少設計錯誤
  * 加速開發過程

### 設計模式分類（Gang of Four, GoF）

#### 創建型模式（Creational Patterns）
專注於物件建立機制，增加系統彈性並隱藏建立邏輯。

* **單例模式（Singleton）**：
  * 確保類別只有一個實例，提供全域存取點
  * 適用於：日誌記錄器、配置管理、資源管理
  
* **工廠方法（Factory Method）**：
  * 定義用於建立物件的介面，但由子類決定要實例化的類別
  * 適用於：物件建立過程複雜，依賴於運行時條件

* **抽象工廠（Abstract Factory）**：
  * 用於建立一系列相關或依賴物件，無需指定具體類別
  * 適用於：系統需要獨立於其產品的建立與組合

* **建造者模式（Builder）**：
  * 分離複雜物件的建構與表現，同一建構過程可創建不同表現
  * 適用於：需要建立組合物件或有多步驟建立過程

* **原型模式（Prototype）**：
  * 通過複製現有物件建立新物件，避免依賴具體類別
  * 適用於：建立複雜物件成本高，或希望減少類別數量

#### 結構型模式（Structural Patterns）
關注類別與物件的組合，形成更大結構並提供新功能。

* **適配器（Adapter）**：
  * 使不兼容介面能夠一起工作，轉換一個類別的介面為客戶期望的另一介面
  * 適用於：整合現有系統，使用遺留代碼

* **橋接模式（Bridge）**：
  * 將抽象與實現分離，使它們可以獨立變化
  * 適用於：避免抽象與實現的永久綁定，或兩者需獨立擴展

* **組合模式（Composite）**：
  * 將物件組合成樹形結構表示部分-整體層次，允許統一處理個別物件與組合物件
  * 適用於：需要表示對象的部分-整體層次結構，如圖形界面、文件系統

* **裝飾器（Decorator）**：
  * 動態地給物件添加額外職責，比繼承更靈活
  * 適用於：需要透明地擴展物件功能，特別是在不適合用繼承的情況

* **外觀模式（Facade）**：
  * 提供一個統一介面訪問子系統的一組介面，定義高層介面簡化子系統使用
  * 適用於：需要簡化複雜子系統的使用，或提供統一存取點

#### 行為型模式（Behavioral Patterns）
關注物件間的通信模式，增加彈性並降低耦合。

* **觀察者（Observer）**：
  * 定義物件間一對多依賴，一個物件狀態改變時自動通知依賴它的物件
  * 適用於：當一個對象狀態改變需通知其他對象，但不想強耦合

* **策略（Strategy）**：
  * 定義一系列演算法，使它們可以互換，讓演算法獨立於使用它的客戶端
  * 適用於：需要在運行時選擇不同演算法，或有多種實現同一功能的方式

* **命令（Command）**：
  * 將請求封裝為物件，可參數化客戶端操作、排隊請求、記錄請求、支持撤銷
  * 適用於：需要將操作封裝為對象，特別是支持撤銷、宏命令等情況

* **責任鏈（Chain of Responsibility）**：
  * 通過給多個物件處理請求的機會，避免請求發送者與接收者的耦合
  * 適用於：有多個物件可處理請求，但在運行時決定處理者

* **模板方法（Template Method）**：
  * 定義演算法骨架，將某些步驟延遲到子類實現，子類可重定義步驟但不改變結構
  * 適用於：有固定流程但具體步驟可變的情況

### 設計模式應用原則
* **選擇適當**：根據問題特性選擇合適模式，不要過度設計
* **組合使用**：多個模式常結合使用解決複雜問題
* **適度抽象**：模式提供抽象但也帶來複雜性，需平衡兩者
* **理解變化**：設計模式的核心是管理變化，明確預期變化點
* **持續學習**：模式不是固定公式，而是不斷演化的設計智慧

## 10.10 現代設計趨勢與發展

### 10.10.1 微服務架構（Microservices Architecture）
* **核心理念**：將應用程式拆分為鬆散耦合的小型服務，各自負責特定業務功能
* **設計特點**：
  * 服務獨立部署與擴展
  * 每個服務圍繞業務能力構建
  * 服務間通過輕量級協議通信
  * 去中心化資料管理
* **優勢**：
  * 提高系統彈性與可擴展性
  * 支持技術多樣性
  * 促進持續交付與部署
  * 提高團隊自主性
* **挑戰**：
  * 分散式系統複雜性
  * 服務協調與監控難度
  * 資料一致性問題
  * 跨服務調試困難

### 10.10.2 領域驅動設計（Domain-Driven Design, DDD）
* **核心理念**：將設計焦點放在核心領域概念上，建立業務與程式碼的統一語言
* **設計要素**：
  * 限界上下文（Bounded Context）
  * 領域模型（Domain Model）
  * 通用語言（Ubiquitous Language）
  * 聚合（Aggregate）與實體（Entity）
* **與結構化設計關係**：
  * 更強調領域專家與開發人員協作
  * 模型設計以領域為中心，而非以資料流為中心
  * 強調建立領域概念的清晰邊界
* **應用場景**：
  * 複雜領域問題
  * 需要長期演化的系統
  * 業務邏輯複雜的企業系統

### 10.10.3 持續集成與持續部署對設計的影響
* **快速迭代**：設計需支持頻繁變更與增量發布
* **設計要求**：
  * 模組化與鬆散耦合更加重要
  * 自動化測試成為設計考慮因素
  * 設計需考慮部署策略（如藍綠部署、金絲雀發布）
* **架構進化**：
  * 從大型單體向微服務或無服務轉變
  * API設計與版本管理更加重要
  * 配置外部化與環境隔離
* **實踐變化**：
  * 設計文件輕量化但更加精確
  * 持續重構成為常態
  * 設計決策與反饋週期加速

### 10.10.4 從結構化到面向對象到函數式設計的演進
* **結構化設計**：
  * 以流程與程序為中心
  * 強調模組化與資料流
  * 適合程序化語言環境
* **物件導向設計**：
  * 以類別與物件為中心
  * 強調繼承、封裝與多態
  * 將資料與行為結合
* **函數式設計**：
  * 以函數與不可變資料為中心
  * 強調純函數與副作用隔離
  * 使用高階函數與組合
* **混合設計**：
  * 現代系統常融合多種設計範式
  * 根據問題特性選擇適合的設計方法
  * 不同層次可採用不同設計風格

## 10.11 設計文件化與溝通

### 10.11.1 設計文件的種類與用途
* **架構概覽文件**：提供系統高層次視圖，說明主要元件與交互
* **詳細設計規格**：描述各模組的具體實現，包括演算法、資料結構與介面
* **介面規格**：定義模組間交互的介面協議與數據格式
* **資料庫設計**：描述資料庫結構、關係與存取方式
* **安全設計**：說明系統安全機制與考慮
* **部署說明**：描述系統部署架構與環境要求

### 10.11.2 統一建模語言（UML）在設計文件中的應用
* **用途**：提供標準化視覺表示法，增強設計溝通
* **常用圖表**：
  * **類別圖**：顯示系統的靜態結構，包括類別、屬性、方法與關係
  * **序列圖**：展示物件間的時序交互
  * **組件圖**：描述系統組件及其依賴關係
  * **部署圖**：說明系統物理部署方式
* **使用原則**：
  * 選擇適合設計觀點的圖表類型
  * 保持圖表簡潔，避免過度細節
  * 聚焦於關鍵設計決策
  * 保持與代碼一致性

### 10.11.3 設計評審與決策記錄
* **設計評審流程**：
  * 設計案例提出
  * 技術評審會議
  * 意見收集與問題識別
  * 設計修正與再評審
* **評審焦點**：
  * 設計是否滿足功能需求
  * 是否符合非功能性需求
  * 設計原則的遵循度
  * 技術風險評估
* **決策記錄（ADR, Architecture Decision Records）**：
  * 記錄重要設計決策及其背景
  * 包含問題描述、考慮選項、決策理由
  * 作為長期參考，有助於理解設計演化

## 10.12 結構化設計的局限與發展

### 10.12.1 結構化設計的局限性
* **靜態表示**：難以表現動態行為與系統狀態變化
* **資料隱藏不足**：傳統結構化方法對資料封裝支持較弱
* **可重用性挑戰**：以流程為中心，不利於建立可重用元件
* **大型系統複雜性**：隨系統規模增長，結構圖可能變得難以管理
* **領域知識表達**：缺乏表達業務規則與領域概念的有效機制

### 10.12.2 走向現代設計方法
* **結構化設計的遺產**：
  * 模組化思想仍是現代設計基石
  * 高內聚低耦合原則依然適用
  * 資料流分析依然有價值，尤其在特定領域如嵌入式系統
* **演進與整合**：
  * 與物件導向設計的整合
  * 吸收敏捷設計思想
  * 融入更多關注關注點分離的實踐
* **實用平衡**：
  * 在適當場景仍使用結構化技術
  * 依問題特性選擇設計方法
  * 保留結構化設計的核心價值，結合現代設計思想

## 10.13 設計案例研究

### 10.13.1 銀行交易處理系統設計
* **系統概述**：處理各類銀行交易（存款、提款、轉帳）的後台系統
* **設計挑戰**：
  * 高可靠性與一致性要求
  * 需處理高峰期大量交易
  * 安全性與審計要求嚴格
* **結構化設計應用**：
  * **DFD設計**：
    * 第0層：整體系統與外部實體
    * 第1層：身份驗證、交易處理、賬戶管理、報表生成
    * 第2層：各類交易的詳細流程
  * **結構圖設計**：
    * 交易控制模組協調各類交易
    * 特定交易處理模組實現業務邏輯
    * 資料存取模組隔離資料庫操作
  * **模組規範**：
    * 交易驗證模組確保交易合法性
    * 餘額計算模組處理資金計算
    * 日誌記錄模組確保可審計性
* **設計決策**：
  * 使用高內聚的事務處理模組
  * 將安全邏輯與業務邏輯分離
  * 實施分層設計隔離變化點

### 10.13.2 庫存管理系統設計
* **系統概述**：管理商品庫存、訂單處理與供應商互動的系統
* **設計挑戰**：
  * 需支援多位置庫存
  * 即時庫存更新與報警
  * 與多系統整合（銷售、採購、會計）
* **結構化設計應用**：
  * **DFD分析**：
    * 識別庫存移動、訂單處理、報表生成等主要流程
    * 明確外部系統界面
  * **資料字典**：
    * 定義產品、庫存、訂單等核心資料結構
    * 規範各資料項格式與約束
  * **結構圖設計**：
    * 庫存控制模組管理庫存狀態
    * 訂單處理模組處理進出庫操作
    * 報警模組監控庫存水平
* **設計特點**：
  * 採用交易中心法設計主控模組
  * 將報表生成與操作處理分離
  * 實施異步通知機制應對實時需求

### 10.13.3 討論與總結
* **結構化設計價值體現**：
  * 系統性分解複雜問題
  * 明確模組職責與界限
  * 建立清晰資料流動路徑
* **現代技術整合**：
  * 結合微服務思想劃分子系統
  * 應用DDD概念明確領域邊界
  * 引入事件驅動設計提高響應性
* **設計經驗教訓**：
  * 早期明確系統邊界至關重要
  * 平衡理論純粹性與實用性
  * 預留擴展點但避免過度設計
  * 文件應與代碼同步更新

## 10.14 結論與最佳實踐

### 10.14.1 結構化設計的核心價值
* **系統化流程**：提供從需求到設計的明確轉換路徑
* **強調模組化**：通過分解複雜系統提高可理解性與可維護性
* **品質關注**：強調內聚、耦合等影響品質的設計特性
* **可視化表示**：使用圖形化工具幫助理解與溝通設計

### 10.14.2 設計最佳實踐
* **需求驅動**：設計應始終以需求為基礎，滿足功能與非功能需求
* **簡潔為先**：追求最簡單能滿足需求的設計，避免不必要複雜性
* **變化管理**：識別變化點並隔離，提高系統適應性
* **持續評審**：定期審查設計，及早發現問題
* **技術適配**：選擇適合問題特性與團隊能力的設計方法
* **平衡權衡**：在效率、維護性、擴展性等方面做出合理取捨
* **文件化**：確保設計決策與理由得到記錄，便於理解與維護

### 10.14.3 結構化設計在現代軟體工程中的角色
* **與敏捷方法融合**：
  * 輕量級設計文件支持快速迭代
  * 增量式精煉符合敏捷開發理念
* **多範式整合**：
  * 結合面向對象、函數式設計的優點
  * 依問題特性選擇合適技術
* **專注本質**：
  * 保留模組化、高內聚、低耦合等核心原則
  * 去除過時的形式與工具，保留永恆價值
* **技術傳承**：
  * 作為軟體工程的基礎知識
  * 為新一代設計師提供重要思維工具

結構化設計雖起源於上世紀，但其核心思想與原則在當今軟體設計中仍具有重要價值。通過與現代方法整合，並適應新興技術環境，結構化設計繼續為創建高品質軟體系統提供重要指導。

# 軟體測試 (Software Testing)

## 軟體測試定義 (Software Testing Definition)

- **Dijkstra的定義**：
  - 測試可以證明錯誤的存在，但不能證明錯誤的不存在
  - 測試是對系統(組件)在預定輸入數據上進行運行，並捕獲其行為和輸出數據
  - 測試案例是一個簡單的配對：`<輸入, 預期結果>`

- **限制**：
  - 測試只能和所選測試數據一樣好
  - 測試結果需與「預期結果」進行比較
  - 程序執行的結果是複雜的實體，可能包括：
    - 程序產生的值
    - 狀態變化
    - 必須一起解釋才能使結果有效的值序列或集合

## 測試目的 (Purpose of Testing)

- **測試目標**：
  - 幫助清晰描述系統行為
  - 盡早發現需求、設計、文檔和代碼中的缺陷
  - 防止低質量軟體到達用戶/客戶手中

- **測試演進**：
  - 1960年代：**展示** - 它如何工作
  - 1970年代中期：**檢測** - 找出缺陷
  - 1990年代：**預防** - 管理質量

## 關於測試的迷思 (Myths About Testing)

- **錯誤易於移除**：許多錯誤非常難以消除
- **錯誤只在一個模塊中**：許多錯誤是由多個模塊中的多個錯誤引起的
- **大多數錯誤由編譯器捕獲**：編譯器只能捕獲某些類型的錯誤，其他類型永遠無法被編譯器捕獲
- **錯誤修復總能改善程序**：許多錯誤修復引入的問題比解決的還多

## 測試相關人員與成品 (The Workers and Artifacts Involved in Testing)

- 測試過程涉及多種角色和產物，包括：
  - 測試團隊
  - 開發人員
  - 需求分析師
  - 測試計劃
  - 測試案例
  - 測試報告

## 農藥悖論 (The Pesticide Paradox)

- 重複相同測試的軟體最終會對這些測試產生「抗藥性」
- 需要不斷更新測試方法和案例以保持有效性

## 動態與靜態測試 (Dynamic and Static Testing)

- **動態測試**：
  - 通過執行軟體並應用測試輸入來測試軟體
  - 觀察輸出並做出判斷以得出結論

- **靜態測試**：
  - 不執行代碼就檢查代碼
  - 非自動靜態測試：評審、走查、檢查

- **評審和測試**：
  - 作為驗證和確認(V&V)活動相輔相成
  - 測試是產品發布前的最後活動
  - 評審是防止返工的最有效方法
  - 返工消耗了所有項目成本的30-50%

### 缺陷發現率 (Fault Discovery Rate)

| 發現活動 | 每KLOC發現的缺陷 |
| -------- | ---------------- |
| 需求評審 | 2.5              |
| 設計評審 | 5.0              |
| 代碼檢查 | 10.0             |
| 集成測試 | 3.0              |
| 驗收測試 | 2.0              |

## 測試分類 (Classification of Test)

- **兩個分類層次**：
  1. **按粒度區分**：
     - 單元級別 (Unit level)
     - 系統級別 (System level)
     - 集成級別 (Integration level)
  
  2. **按方法區分** (主要用於單元級別)：
     - 黑箱(功能性)測試 (Black box (Functional) Testing)
     - 白箱(結構性)測試 (White box (Structural) Testing)

## 測試層級 (Testing Level)

- **V模型** (The V-Shaped Model)：
  - 左側：開發階段
  - 右側：測試階段
  - 建立各階段開發和測試之間的對應關係

## 誰執行測試 (Who Performs the Test)

- **獨立測試團隊**：
  - 避免利益衝突
  - 提高客觀性
  - 允許測試和編碼同時進行

## 測試標準 (Testing Standards)

- **IEEE Std 829-1998** 軟體測試文檔標準
- 其他軟體測試標準：
  - RTCA/DO-178B (美國空運系統與裝備保證之軟體考量標準)
  - IEC 61508 (電機、電子、可程式化的功能性安全相關系統標準)
  - FDA General Principles of Software Validation (美國食品藥品管理局軟體驗證一般原則)
  - MISRA Software Development Guidelines (車輛軟體開發指南)
  - MoD Defence Standard 00-55 (英國國防部門裝備安全性相關的軟體之要求標準)

## 測試技術分類 (Taxonomies of Testing Techniques)

- 多種測試技術和方法針對不同需求和環境

## 黑箱測試 (Black Box Testing)

- **IEEE定義**：
  - 忽略系統或組件的內部機制，僅專注於對選定輸入和執行條件產生的輸出
  - 進行測試以評估系統或組件是否符合指定的功能需求

## 功能(規格導向)測試 (Functional (Specification-Based) Testing)

- **黑箱測試** (數據驅動或輸入/輸出驅動測試)：
  - 邊界值測試 (Boundary Value testing)
  - 等價類測試 (Equivalence Class testing)
  - 決策表 (Decision Tables)
  - 特殊值測試 (Special Value testing)

- **共同特點**：
  - 都將程序視為從輸入映射到輸出的數學函數

- **比較功能測試方法時考慮**：
  - 測試工作量 (testing effort)
  - 測試效率 (testing efficiency)
  - 測試有效性 (testing effectiveness)

- **測試特點**：
  - 測試人員看不到代碼
  - 通常由測試組完成
  - 關注點在結果而非過程

## 邊界值分析 (Boundary Value Analysis)

- 邊界值分析通過考慮輸入邊界來識別測試案例
- 通過考慮程序輸入生成測試案例的技術
- 基本原理：錯誤往往發生在輸入變量的極值附近
  - 例如：循環條件可能測試 < 而非 ≤，計數器常出現「差一」錯誤
  - 美國陸軍研究發現，錯誤中有相當大比例是邊界值錯誤
  - 非強類型語言(如 .NET、Java、C# 等)的程序更適合邊界值測試

### 邊界數量 (How Many Boundaries)

- 例如：用戶必須訂購大於0且小於100的數量
  - 有兩個邊界：0和100
  - 三個區域：< 0、0-99、≥ 100

### 輸入邊界值測試 (Input Boundary Value Testing)

- 邊界值分析測試案例通過保持除一個變量外的所有變量在其標稱值，並讓該變量假設其極值來獲得
- 對變量y (a ≤ y ≤ b)的測試案例：
  - y(min)：最小值
  - y(min+)：略高於最小值
  - y(nom)：標稱值
  - y(max-)：略低於最大值
  - y(max)：最大值

## 等價類劃分 (Equivalence Class Partitioning)

- **範圍型輸入數據**：
  - 例如：1到5000之間的數字
  - 定義一個有效等價類和兩個無效等價類

- **枚舉集合型輸入**：
  - 例如：{a, b, c}
  - 定義一個有效輸入值的等價類
  - 定義另一個無效輸入值的等價類

### 等價類劃分示例

- 例如：銷售價格折扣規則
  - 銷售價格低於$15,000，不給折扣
  - 價格最高$20,000，給5%折扣
  - 低於$25,000，折扣7%
  - $25,000起，折扣8.5%
  - 可推導出四個不同的計算折扣有效等價類

## 特殊值測試 (Special Value Testing)

- 功能測試中最廣泛使用的形式
- 最直觀但最不統一
- 利用領域知識和工程判斷來識別程序的「軟弱點」，設計測試案例
- 雖然在測試案例生成上非常主觀，但在揭示程序缺陷方面通常更有效
- 其他術語：「黑客測試」、「跳出框架測試」、「臨時測試」、「遊擊測試」

## 白箱測試 (White-Box Testing)

- 目標是確保所有語句和條件至少執行一次
- 處理代碼的內部邏輯和結構
- 基於白箱測試策略編寫的測試包括對代碼、分支、路徑、語句和內部邏輯的覆蓋
- 測試人員需要處理代碼，因此需要掌握編碼和邏輯知識
- 測試人員需要查看代碼，找出哪個單元/語句出現故障
- 也稱為結構測試（或開箱測試）
- 需要程序的內部知識來識別測試案例
- 目標是至少執行所有程序語句一次
- 使用語句覆蓋和流圖工作：
  - 每個語句
  - 每個決策

## 測試計畫 (Test Plan)

測試計畫應說明其目標，包括：
- 指導測試管理
- 指導測試所需的技術工作
- 建立測試計畫和時間表
- 解釋每個測試的性質和範圍
- 解釋測試如何完全評估系統功能和性能
- 記錄測試輸入、具體測試程序和預期結果

### 測試計畫內容

1. 測試計畫識別
2. 簡介
3. 測試項目
4. 要測試的特性
5. 不測試的特性
6. 測試方法
7. 接受標準
8. 暫停和恢復標準
9. 測試交付物
10. 測試任務
11. 環境需求
12. 責任分配
13. 人員配置和培訓需求
14. 進度
15. 風險和應急計畫
16. 審批

## 測試案例 (Test Case)

- 測試案例指定如何測試用例或用例的特定場景
- 包括驗證參與者與系統之間交互的結果，用例指定的前置和後置條件得到滿足，用例指定的操作序列得到遵循
- 使用已知測試技術和策略獲得
- 有些測試案例可能類似，僅在單個輸入或結果值上有所不同

### 測試案例模板

- ID、名稱、參考、優先級、前置條件、前言、輸入、預期輸出、觀察到的輸出、判決和備註

### 測試案例定義

- 每個用例定義至少兩個測試案例：一個成功和一個失敗
- 可以為用例定義文檔中的例外和替代路徑生成更多測試案例
- 測試案例定義文檔通常包括：
  - 測試案例ID、父用例ID、測試目標、測試條件、操作員操作、輸入規範、輸出規範(預期結果)和通過或失敗的二元決策

### 日立測試案例經驗分佈

- 根據日立的經驗，100%測試案例中：
  - 60%正常情況
  - 10%邊界和限制
  - 15%錯誤情況
  - 15%不同數據庫、網絡等的互操作性
- 使用LOC(在需求階段不會知道)使用「1個測試案例/10~15 LOC」
  - 例如：500 LOC的軟體需要約34到50個測試案例

## 測試時間表 (Test Schedule)

- 應產生包括測試步驟(可能包括任務)、目標開始和結束日期以及責任的測試時間表
- 還應描述如何審查、跟踪和批准
- 可以使用項目管理工具(如Microsoft Project)格式化甘特圖來強調測試並將其分組為測試步驟

## 測試問題報告表格 (Test Problem Report Forms)

測試問題報告應包含：
- **位置**：問題在哪裡發生？
- **時間**：何時發生？
- **徵狀**：觀察到什麼？
- **結果**：後果是什麼？
- **機制**：如何發生？
- **原因**：為什麼發生？
- **嚴重性**：用戶或業務受到多大影響？
- **成本**：花費多少？

## 測試的完整性 (Testing Is Never Complete)

- 每個程序在有限數量的輸入上運行。完整的功能測試將包括使程序受制於所有可能的輸入流
- 考慮一個10字符的輸入字符串(每個字符由8位組成；每位可能是0或1)，有2^80種可能的輸入流和相應的結果
- 完整的功能測試是不切實際的！
- 不可能測試每一種數據輸入和接口組合
- 任務是找到錯誤 — 而不是找到所有錯誤或驗證程序沒有錯誤

### 何時停止測試 (When To Stop Testing)

- **可能的回答**：
  - 當不再發現錯誤時
  - 當想不出任何新的測試案例時
  - 當每個測試案例的錯誤數量開始下降時
  - 當達到覆蓋率目標時
  - 所有錯誤都被刪除時？(不可能...)

- **確保不要因以下原因停止測試**：
  - 時間限制
  - 缺乏動力

### 廣泛測試而非深入測試 (Test Broadly Not Deeply)

- 早期測試的目標是盡快發現大問題
- 隨著程序變得更穩定，會更深入地探索
- 如果設計將要改變，沒有必要對其進行徹底破壞

### 完整測試的意義 (Complete Testing)

- 完整「覆蓋」：測試了每一行/分支/基本路徑？
- 測試人員不再發現新錯誤？
- 測試計畫完成？
- 完整測試必須意味著在測試結束時，您知道沒有剩餘的未知錯誤

### 完整覆蓋 (Complete Coverage)

- 有些人嘗試通過提倡「完整覆蓋」來簡化完整測試的問題
- **覆蓋率定義**：
  - 測試程序的某些屬性或部分的範圍，如語句覆蓋、分支覆蓋或條件覆蓋
  - 與可能測試總體相比，已完成測試的範圍
- 典型定義過於簡化，例如，它們錯過了：
  - 有趣的數據值和數據組合
  - 缺失的代碼
  - 中斷和其他並行操作

### 覆蓋率 (Coverage)

- 一組測試案例的覆蓋率是衡量該組測試案例覆蓋的語句、分支或路徑比例的指標
- 語句覆蓋、分支覆蓋和路徑覆蓋是白箱測試技術，用於根據程序的邏輯結構提出測試案例

### 設計覆蓋率測試案例 (Designing Test Cases for Coverage)

1. 分析源代碼以導出流圖
2. 從流圖提出測試路徑以實現所需覆蓋
3. 評估實現每個路徑的測試條件
4. 根據條件提出輸入和輸出值

## 圖論構造 (Graph Theory Constructs)

圖論提供了分析程序結構的基礎，包括：
- 節點(代表程序語句)
- 邊(代表控制流)
- 路徑(代表執行序列)

## 語句覆蓋 (Statement Coverage)

- 選擇測試集T，使P中的每個語句至少被T中的某個t執行一次
- 如果程序中包含錯誤的部分未被執行，則無法發現錯誤
- 一個輸入數據執行許多語句
- 儘量最小化測試案例數量，同時保持所需的覆蓋率
- 100%語句覆蓋對於一組測試意味著所有語句都被測試覆蓋
- 主要思想：除非執行語句，否則我們無法知道該語句是否存在錯誤

### 語句覆蓋標準

- 觀察到語句對於一個輸入值表現正常：
  - 不能保證它對所有輸入值都表現正常

### 標準的弱點

- 例如：`if x < 0 then x := -x; end if; z := x;`
  - 它不測試x為正的情況

## 分支覆蓋 (Branch Coverage)

- 也稱為決策覆蓋、全邊覆蓋、決策-決策(D-D)路徑測試
- 報告控制結構中的布爾表達式是否被測試案例評估為真和假值
- 選擇測試集T，使每個分支至少被T中的某個d執行一次
- 節點代表程序中的決策
- 設計測試案例使不同的分支條件依次給予真和假值
- 分支測試保證語句覆蓋：比基於語句覆蓋的測試更強

### 分支覆蓋的弱點

- 例如，考慮以下代碼段：
  ```
  if x ≠ 0 then 
      y := 5; 
  else 
      z := z - x; 
  end if;
  if z > 1 then 
      z := z / x; 
  else 
      z := 0; 
  end if;
  ```
- `{<x=0, z=1>, <x=1, z=3>}`導致所有分支的執行，但未能暴露除零風險

### 分支覆蓋評論

- 考慮以下C/C++/Java代碼片段：
  ```java
  if (condition1 && (condition2 || function1())) 
      statement1; 
  else 
      statement2; 
  ```
- 這個度量可能認為控制結構已完全測試，而不需要調用function1
- 當condition1和condition2都為真時測試表達式為真，當condition1為假時測試表達式為假
- 在這種情況下，短路運算符排除了對function1的調用

## 條件覆蓋 (Condition Coverage)

- 報告每個布爾子表達式的真或假結果
- 條件覆蓋獨立於彼此測量子表達式
- 這種測量類似於決策覆蓋，但對控制流有更好的敏感性
- 設計測試案例，使復合條件中每個條件的每種可能結果至少發生一次
- 設計測試案例時：
  - 複合條件表達式的每個組成部分都給予真和假值
  - 例如：考慮條件表達式`((c1.and.c2).or.c3)`，c1、c2和c3中的每一個至少執行一次，即給予真和假值
- 短路條件覆蓋：對於具有短路運算符的語言(如C、C++和Java)，多條件覆蓋的優勢是它需要非常徹底的測試

### 條件覆蓋細節

- 條件覆蓋要求決策中的每個條件至少一次取所有可能的結果
- 條件覆蓋不要求每個決策取所有可能的結果
- 例如：`if Not (A or B) then C`
  - 設置 A = True, B = False，然後 A = False 和 B = True 滿足條件覆蓋，但語句塊C永遠不會被執行
- 條件覆蓋的擴展是決策/條件覆蓋，它是條件覆蓋和決策覆蓋的聯合

## 決策/條件覆蓋 (Decision/Condition Coverage)

- 滿足條件覆蓋，並且每個決策至少一次取所有可能的結果
- 例如對於以下代碼段：
  ```
  IF A AND B AND C THEN
      X = Y + Z
  ELSE
      X = U + V
  ENDIF
  ```
- 必須進行測試，使得：
  1. (A and B and C)至少評估為真一次：A = true, B = true, C = true，以及
  2. (A and B and C)至少評估為假一次：以下之一：
     - A = true, B = true, C = false
     - A = true, B = false, C = true
     - A = true, B = false, C = false
     - A = false, B = true, C = true
     - A = false, B = true, C = false
     - A = false, B = false, C = true
     - A = false, B = false, C = false

## 多條件覆蓋 (Multiple-Condition Coverage)

- 滿足決策覆蓋，並且每個決策的執行方式使所有可能的條件組合都被測試
- 例如對於以下代碼段：
  ```
  IF A AND B AND C THEN
      X = Y + Z
  ELSE
      X = U + V
  ENDIF
  ```
- 必須測試所有可能的A、B、C組合(8種)

### 多條件覆蓋示例

```java
if (A && B) // condition 1
    F1();
else
    F2();
if (C || D) // condition 2
    F3()
else
    F4();
```

測試案例：
- 條件1：
  - A=T, B=T
  - A=T, B=F
  - A=F, B=T
  - A=F, B=F
- 條件2：
  - C=T, D=T
  - C=T, D=F
  - C=F, D=T
  - C=F, D=F

## 修改條件/決策覆蓋 (Modified Condition/Decision Coverage)

- 由波音公司創建，美國聯邦航空管理局(FAA)根據RCTA/DO-178B要求用於航空軟體
- 通常被認為是關鍵軟體充分測試的必要條件
- 專為不短路的邏輯運算符語言(如Ada和Visual Basic.Net)設計

### MCDC覆蓋要求

設計測試案例使：
1. 程序中決策中的每個條件至少取所有可能的結果一次(條件覆蓋)
2. 決策中的每個條件都表現出獨立影響該決策的結果(通過僅改變該條件同時固定所有其他可能的條件，證明條件獨立影響決策的結果)
3. 程序中的每個決策至少取所有可能的結果一次(分支或決策覆蓋)

## 實現不同覆蓋級別的測試類型 (Types of Tests to Achieve Levels of Coverage)

考慮以下示例：
```
IF (A AND B AND C) GO TO 110
X = Y + Z
DO CASE . . .
```

不同覆蓋需要的測試組合：
- 語句覆蓋：A&B&C 為真或 A 為假或 B 為假或 C 為假
- 分支覆蓋：A&B&C 為真和 A&B&C 為假
- 條件覆蓋：A 為真和 A 為假，B 為真和 B 為假，C 為真和 C 為假
- 決策/條件覆蓋：A&B&C 為真和 A&B&C 為假，加上條件覆蓋
- 多條件覆蓋：A、B、C 的所有可能組合

## 路徑覆蓋 (Path Coverage)

- 通過評估一組提議的測試案例所覆蓋的單元執行路徑的比例來確定
- 100%路徑覆蓋是指單元中的每個路徑至少被提議的測試案例集執行一次
- 100%路徑覆蓋由理想的測試集實現
- 設計測試案例時：所有線性獨立路徑在程序中至少執行一次

## 覆蓋問題討論 (Coverage Problem Discussion)

- 可能有100%語句覆蓋但沒有100%分支覆蓋
- 可能有100%分支覆蓋但沒有100%路徑覆蓋
- 100%路徑覆蓋意味著100%分支覆蓋，100%分支覆蓋意味著100%語句覆蓋

## 函數覆蓋 (Function Coverage)

- 報告程序中的每個函數或過程是否被調用
- 與基於代碼的覆蓋不同，功能覆蓋專注於程序的功能
- 用於檢查是否測試了功能的每個方面
- 因此，功能覆蓋是設計和實現特定的，更難度量

## 呼叫覆蓋 (Call Coverage)

- 報告程序中的每個函數調用是否被執行
- 基於的假設是錯誤常發生在模塊之間的接口
- 也稱為呼叫對覆蓋(call pair coverage)

### 呼叫覆蓋示例

```java
Program A {
    ...
    F1();
    F2();
    IF (condition){
        F3();
    }
    ...
}

F1(){
    ...
    F3();
    ...
}
```

- 如果創建condition = false的測試案例 → 函數覆蓋
- 必須添加condition = true的測試案例 → 呼叫覆蓋

## 迴圈覆蓋 (Loop Coverage)

- 報告程序中的每個迴圈體是否被執行零次、恰好一次和多次(連續)
- 對於do-while迴圈，迴圈覆蓋報告迴圈體是否被執行恰好一次和多次
- 這種測量的有價值方面是確定while迴圈和for迴圈是否執行多次，這是其他測量不報告的信息

## 覆蓋標準度量 (Coverage Criteria Metrics)

- 不同覆蓋標準提供不同程度的測試嚴格性
- 從最基本的函數覆蓋到最嚴格的多條件覆蓋

## DO-178B中的軟體級別例子 (Software Levels in DO-178B)

- 不同的失敗條件需要不同的軟體條件，分為5個級別
- DO-178B定義了特定的驗證目標，包括基於需求的測試、魯棒性測試和覆蓋測試，取決於您所遵循的軟體級別

## 白箱測試：如何應用 (White-Box Testing: How to Apply)

1. 不要從設計白盒測試案例開始！
2. 從黑盒測試案例開始(等價分區、邊界值分析、因果圖、正式方法推導...)
3. 檢查白盒覆蓋(語句、分支、條件...覆蓋)
4. 使用覆蓋工具
5. 為未覆蓋的代碼設計附加的白盒測試案例

## 隨機測試：猴子和大猩猩 (Random Testing: Monkeys and Gorillas)

- 測試猴子的概念來自於這樣的想法：如果有一百萬隻猴子在一百萬台鍵盤上敲打一百萬年，統計上，它們最終可能會寫出莎士比亞的戲劇
- 當軟體發布給公眾時，將有成千上萬(或數百萬)人使用它
- 儘管您盡最大努力設計測試案例來發現錯誤，但有些錯誤會被這些用戶發現
- 只要有電和偶爾的香蕉，測試猴子就會永遠測試

## 回歸測試 (Regression Testing)

- 識別當前錯誤被糾正時可能引入的新錯誤
- 驗證新版本或發布仍然以與舊版本或發布相同的方式執行相同的功能

### 回歸測試步驟

1. 插入新代碼
2. 測試已知受新代碼影響的功能
3. 測試m的基本功能，以驗證它們仍然正常工作
4. 繼續m+1的功能測試

## 大爆炸方法 (Big Bang Approach)

- 在大爆炸集成測試中，直到所有模塊都準備好才集成各個模塊，然後運行以檢查是否運行良好
- 完成各個模塊測試後，一次性組合所有模塊並驗證功能
- 在這種類型的測試中，可能出現一些缺點，如在後期階段發現缺陷
- 很難隔離發現的缺陷，因為很難判斷缺陷是在組件中還是在接口中
- 在這種方法中，程序在沒有任何正式集成測試的情況下集成，然後運行以確保所有組件正常工作
- 在生產中可能出現一些關鍵缺陷的可能性很高

## 系統測試 (System Tests)

- 系統測試是對從客戶導出的所有系統需求的驗證和確認，以構建產品
- 系統測試通常在創建所有組件之前開始
- 一旦組件被集成，系統測試確保完整的系統符合系統的功能和非功能需求

### 系統測試類別

- 功能測試 (Functional testing)
- 性能測試 (Performance testing)
- 安全測試 (Security testing)
- 恢復測試 (Recovery testing)
- 驗收測試 (Acceptance testing)
- 安裝測試 (Installation testing)
- 規劃和執行系統測試策略的一種方法是分析 (profiling)

## 操作設定檔 (Operational Profile)

- John Musa(測試領域最著名的專家之一)描述了在軟體可靠性工程背景下使用這些配置文件
- 配置文件的使用應該在項目的需求收集階段早期開始
- 配置文件從市場/銷售開發的「客戶配置文件」延伸到用於支持黑/白盒測試的實際測試場景選擇
- 作為系統測試的範例，操作配置文件提供實際使用場景、用戶提供的精確輸入、系統的確切結果輸出或響應、處理空間的內部分區以及這些場景發生的概率

## Alpha測試 (Alpha Testing)

- Alpha測試由用戶在受控環境中執行，由開發組織觀察
- Alpha測試指的是來自客戶/客戶社區的用戶/操作員團隊來到開發人員的環境並參與測試，有時在單獨的系統上
- 用戶/操作員團隊經常使用自己的操作時間表和應用程序
- 這種方法引入了額外的測試案例、新操作員和更多系統操作時間
- 故障發生被仔細記錄並轉發給測試團隊

## Beta測試 (Beta Testing)

- Beta測試由真實用戶在自己的環境中執行實際任務，沒有干擾或密切監控
- Beta測試涉及將一個或多個軟體或系統副本交付給客戶/客戶站點
- 那些被賦予beta站點狀態的站點必須承諾向測試團隊報告故障
- 同樣，隨著更多用戶、不同操作員、更多和不同的操作時間表以及系統上更多的CPU時間，將會揭示更多故障

## 突變測試 (Mutation Testing)

- 突變測試有豐富而悠久的歷史，可以追溯到1970年代末
- 突變測試是一種專注於測量測試數據(或測試案例)充分性的技術
- 突變測試背後的最初意圖是揭示和定位測試案例中的弱點
- 因此，突變測試是衡量測試案例質量的一種方法

### 什麼是突變測試

- 突變測試是一種基於代碼的測試評估和改進技術
- 它依賴於勝任的程序員假設，這是以下假設：
  - 給定規範，程序員開發的程序要麼是正確的，要麼與正確程序的差異是簡單錯誤的組合
- 程序開發過程被視為迭代的，即初始版本的程序通過做簡單的或簡單變化的組合朝著最終版本進行優化

### 什麼是程序突變

- 假設已經針對測試集T測試了程序P，並且P在T中的任何測試案例上都沒有失敗
- 如果我們將程序P更改為P'，P'被稱為P的突變
- 在T中可能存在測試t，使得P(t)≠P'(t)。在這種情況下，我們說t區分了P'和P，或者t已經殺死了P'
- 在T中可能不存在任何測試t，使得P(t)≠P'(t)。在這種情況下，我們說T無法區分P和P'，因此P'在測試過程中被視為活的
- 如果在P的輸入域中不存在任何測試案例t可以區分P和P'，則P'被稱為等價於P
- 如果P'與P不等價，但T中沒有任何測試能夠區分它與P，則T被視為不充分

### 突變體 (Mutants)

- 術語「突變體」借用自生物學
- 原始程序P
- P的突變體P'
  - 一個類似於P的程序
  - P'與P的差異是單一突變
  - 每種突變對應於典型錯誤

### 為什麼突變測試有效

- 根據勝任的程序員假設，操作符限於簡單的單一語法更改
- 該假設指出，有經驗的程序員引入的大多數軟體錯誤是由於小的語法錯誤，或者勝任的程序員創建的程序是可編譯的，並且非常接近其規範
- 程序大部分是正確的

### 勝任的程序員假設 (The Competent Programmer Hypothesis)

- 程序員通常非常勝任，不會創建「隨機」程序
- 對於給定的問題，如果程序員有錯誤，他/她創建的程序會非常接近正確的程序
- 不正確的程序可以通過對正確程序進行一些小的更改來創建

### 早期研究：22個標準(Fortran)突變操作符

突變操作符分為多種類型，包括：
- AAR：數組引用替換數組引用
- ABS：絕對值插入
- ACR：數組引用替換常量
- AOR：算術運算符替換
- ASR：數組引用替換標量
- CAR：常量替換數組引用
- CNR：可比較數組名稱替換
- CRP：常量替換
- CSR：常量替換標量變量
- DER：Do語句結束替換
- DSA：數據語句修改
- GLR：Goto標籤替換
- LCR：邏輯連接器替換
- ROR：關係運算符替換
- RSR：返回語句替換
- SAN：語句分析
- SAR：標量替換數組
- SCR：標量替換常量
- SDL：語句刪除
- SRC：源常量替換
- SVR：標量變量替換
- UOI：一元運算符插入

### 近期研究：Java突變操作符

第一個字母是類別：
- A：訪問控制
- E：常見編程錯誤
- I：繼承
- J：Java特定特性
- O：方法重載
- P：多態性

Java突變操作符包括：
- AMC：訪問修飾符更改
- EAM：訪問器方法更改
- EMM：修飾器方法更改
- EOA：引用賦值和內容賦值替換
- EOC：引用比較和內容比較替換
- IHD：隱藏變量刪除
- IHI：隱藏變量插入
- IOD：覆蓋方法刪除
- IOP：覆蓋方法調用位置更改
- IOR：被覆蓋方法重命名
- IPC：明確調用父類構造函數刪除
- ISK：super關鍵字刪除
- JDC：Java支持的默認構造函數創建
- JID：成員變量初始化刪除
- JSC：static修飾符更改
- JTD：this關鍵字刪除
- OAO：參數順序更改
- OAN：參數數量更改
- OMD：重載方法刪除
- OMR：重載方法內容更改
- PMD：具有父類類型的實例變量刪除
- PNC：使用子類類型的新方法調用
- PPD：具有子類類型的參數變量聲明
- PRV：與其他兼容類型的引用賦值

### 一些有用的工具

- Jester (JUnit test tester)：Java中的突變測試工具(開源)
- MuClipse：Eclipse插件，提供現有MuJava突變引擎和Eclipse IDE之間的橋接
- Pester：Python中的突變測試工具(開源)
- Nester：C#中的突變測試工具(開源)
- Insure++：Parasoft, Inc提供的利用突變測試的商業測試工具


# 軟體可靠度工程 (Software Reliability Engineering)

## 可靠度與軟體需求規格 (Reliability & Software Requirement Specification)

- 軟體需求包含**功能性需求**和**非功能性需求**的可靠度要求
- 非功能性可用性和可靠度需求應該**量化**表示
- 常用的度量標準包括：
  - **失效發生概率** (POFOD, Probability of Failure on Demand)
  - **失效發生率** (ROCOF, Rate of Occurrence of Failures)
  - **平均故障間隔時間** (MTTF, Mean Time to Failure)
- 制定可靠度規格時，應考慮不同類型故障的影響後果

## 規格驗證 (Specification Validation)

- 高可靠度規格的**經驗驗證**實際上是不可能的
  - 例如：「無資料庫損壞」實際上意味著 POFOD < 1/2億
  - 如果每個交易需要1秒驗證，模擬一天的交易需要3.5天

## 可靠度度量 (Reliability Metrics)

- **平均故障時間** (MTTF, Mean Time to Failure)：系統從一次修復到下一次失效的平均時間
- **平均修復時間** (MTTR, Mean Time to Repair)：修復故障所需的平均時間
- **平均故障間隔時間** (MTBR, Mean Time Between Failure)：MTBR = MTTF + MTTR
- **可用性** (Availability)：
  - Availability = Uptime/(Uptime + Downtime)
  - Availability = MTTF/(MTTF + MTTR)

## 可用性與停機時間 (Availability and Downtime)

- 「五個九」(99.999%)可用性表示每年允許的停機時間為5.26分鐘
- 可用性等級與每年允許的停機時間對照表：
  | 可用性(%)       | 每年停機時間 |
  | --------------- | ------------ |
  | 90% (1個9)      | 36.5天       |
  | 99% (2個9)      | 3.65天       |
  | 99.9% (3個9)    | 8.76小時     |
  | 99.99% (4個9)   | 52.56分鐘    |
  | 99.999% (5個9)  | 5.26分鐘     |
  | 99.9999% (6個9) | 31.5秒       |

## MTTF工程規則 (Engineering Rules for MTTF)

- **Stephen H. Kan規則**：系統軟體中大約有一半的剩餘缺陷每年被發現（半衰期現象）
- **Capers Jones**於1991年發布的經驗數據將每KLOC的缺陷與MTTF關聯
  - 例如：如果預測每KLOC有7個缺陷，則MTTF估計約為2小時
  - 預測可靠度方程式為：$R(t) = e^{-(t/2)} \pm 50\%$

## 軟體神話 (Software Myths)

### 神話1：需求變更太頻繁，無法預測可靠度
- **事實1**：全球超過90%的組織面臨需求變更。需求變更是生活的事實。

### 神話2：程式設計師風格差異使得開發可靠度預測模型太困難
- **事實2**：對於擁有多個軟體工程師的產品，個別程式設計師能力對預測軟體缺陷和失效率相對不重要。

### 神話3：測試期間沒有失效，軟體就是可靠的
- **事實3**：測試期間可能沒有出現失效是因為：
  - 軟體可測性低
  - 測試未充分反映操作剖面 (Operational Profile)
  - 測試提供有限的代碼覆蓋率

### 神話4：遵循良好定義流程的軟體組織會產生高可靠性軟體
- **事實4**：
  - 必要但不充分條件
  - 還需要關注需求、設計和測試
  - 可靠度由軟體操作行為定義，應當量化

## 可靠軟體 (Reliable Software)

- 可靠軟體是能夠**正確且一致**運行、**缺陷少**、能適當處理異常情況且需要很少安裝工作的產品
- 開發可靠軟體需要：
  - 制定**軟體品質保證** (SQA, Software Quality Assurance) 計劃
  - 建立**軟體可靠度工程** (SRE, Software Reliability Engineering) 流程

## 軟體可靠度定義 (Software Reliability Definition)

- 依據**IEEE標準軟體工程術語詞彙**，軟體可靠度定義為：
  → 系統或組件在特定條件下，於指定時間內執行其要求功能的能力
- 軟體可靠度是影響系統可靠度的重要因素
- 與硬體可靠度不同，它反映了產品設計的完善度，而非製造的完善度

## 軟體可靠度度量 (Software Reliability Measures)

- **靜態度量** (Static Measures)：產品和過程的品質導向度量
  - 故障數量
  - 故障密度 (Fault Density)
  - 複雜度度量 (Complexity Measures)
- **動態度量** (Dynamic Measures)：特徵化失效發生的可靠度導向度量
  - 失效強度 (Failure Intensity)
  - 失效率 (Failure Rate)
  - MTTF
  - 可靠度 (Reliability)
- **使用剖面** (Usage Profile)：環境因素

## 軟體可靠度術語：錯誤、故障和失效 (Software Reliability Terminology: Error, Fault, and Failure)

- **錯誤** (Error)：導致軟體包含故障的人為行為
- **故障** (Fault/Bug)：導致程式失效或內部錯誤的原因（如不正確狀態、時間）
- **失效** (Failure)：可觀察到的結果
- 因果關係鏈：Error → Fault → Failure

## 缺陷與可靠度 (Defects and Reliability)

- 人會犯錯 (Error)
- 缺陷度量衡量的是故障 (Fault)
- 故障導致失效 (Failure)
- 識別系統所有潛在失效需要：
  - 識別系統所有功能
  - 與之相關的功能需求
- 可靠度度量衡量的是失效 (Failure)

## 軟體失效 (Software Failure)

- **故障** (Fault) 是程式的屬性，而非執行或行為的屬性（通常稱為"Bug"）
- 一個故障可能是多個失效的來源
- 故障源於程式設計師的錯誤
- 常見術語混淆：這些術語常被錯誤地互換使用

### 軟體失效定義
- 失效是程式操作外部結果與需求的偏離
- 失效是**動態**的，程式執行時才能發生失效
- 失效不同於"Bug"或故障
- 可能有不同的條件導致失效，或條件可以重複

## 故障與失效 (Faults vs. Failures)

- 如果包含故障的代碼從未在操作中執行：
  → 系統永不失效，MTBF為無限大
- 反之，如果整個系統只有1個故障，且每次都執行：
  → 系統沒有可靠度，MTBF為0
- 失效僅在操作中發生
- 故障是系統中的缺陷，可能在操作中永遠不會被發現

### 失效範例

- **電話網路**：
  - 網路不可用？掉線？連接到錯誤號碼？
- **太空梭**：
  - 爆炸？無法執行任務的某些部分？
- **亞馬遜**：
  - 網站不可用？無法接受訂單？搜索功能無法工作？個人推薦功能無法工作？

## 軟體故障分類 (Classification of Software Faults)

### Bohrbugs（玻爾蟲）
- Jim Gray (1985) 根據故障導致的失效類型提出分類
- 當執行特定操作時**始終**導致失效的錯誤
- 容易重現且基本上是永久性的設計缺陷
- 可以在測試和調試階段（或早期部署階段）輕鬆識別並消除

### Heisenbugs（海森堡蟲）
- 對於給定操作可能導致故障也可能不導致故障的錯誤
- 行為類似於硬體瞬時故障的設計缺陷
- 激活條件罕見或難以重現
- 極度依賴操作環境（其他程式、操作系統和硬體資源）
- 導致**瞬時故障**，即重啟軟體後可能不會重現的故障

## 從失效中學習 (What Can We Learn from Failures?)

- 利用失效間隔時間數據可以學習：
  - 系統可靠度
  - 系統中錯誤的大致數量
  - 移除剩餘錯誤所需的大致時間

### 範例：失效間隔時間分析
- 失效間隔時間數據：6, 4, 8, 5, 6, 9, 11, 14, 16, 19
- 失效強度 = 失效間隔時間的倒數
- MTTF = (6+4+8+5+6+9+11+14+16+19)/10 = 9.8小時
- 分析圖形得出原始失效總數約為15，意味著軟體中仍有5個錯誤
- 移除所有錯誤還需要大約62個時間單位

## 特徵化失效發生 (Characterizing Failure Occurrences)

- 特徵化時間中失效發生的四種一般方式：
  1. **失效時間** (Time to Failure)
  2. **失效間隔時間** (Time Interval Between Failures)
  3. **給定時間內的累積失效** (Cumulative Failures)
  4. **給定時間間隔內的失效** (Failures in a Given Time Interval)
- 這些量都是**隨機變數**，意味著我們無法確定地知道這些變數

## 平均值與失效強度函數 (Mean Value and Failure Intensity Function)

- 測試期間隨著故障移除，失效強度和可靠度通常會變化
- 時間單位取決於：
  - 對依賴性度量預期的準確度
  - 觀察到的失效數量
- 時間的定義方式：
  - 失效間隔時間
  - 執行時間
  - 墻上時鐘或日曆時間
  - 交易數量

## 軟體可靠度工程的定義 (What is Software Reliability Engineering?)

- **軟體可靠度工程** (SRE) 是幫助組織提高產品和流程可靠度的學科
- 美國航空航天學會 (AIAA) 定義SRE為：
  → "在系統開發和操作期間對收集的數據應用統計技術，以指定、預測、估計和評估基於軟體系統的可靠度"

## 軟體可靠度工程：已證實的標準實踐 (SRE: A Proven, Standard Practice)

- 應用SRE的開發人員用"獨特、強大、徹底、有條理、有針對性"等形容詞描述它
- 與達到SEI-CMM 4級和5級高度相關
- SRE處理整個產品，但專注於軟體部分
- 採用全生命週期、主動視角

## 與軟體流程的連結 (Link to Software Process)

- 與測試的直接連結：
  - 測試技術影響可靠度
  - SRE模型中的測試測量
  - 重複隨機抽樣
  - 錯誤植入（和模型）
- 其他過程中的測量/分析：
  - 設計和代碼測量到缺陷分析和預測建模
  - 來自其他QA活動的數據
  - 早期預防性行動

## 軟體可靠度工程：流程 (SRE: Process)

- SRE流程（對每個要測試的系統）包含5個步驟：
  1. 定義必要的可靠度 (Define Necessary Reliability)
  2. 開發操作剖面 (Develop Operational Profiles)
  3. 準備測試 (Prepare for Test)
  4. 執行測試 (Execute Test)
  5. 應用失效數據指導決策 (Apply Failure Data to Guide Decisions)

## 必要的可靠度：如何定義 (Necessary Reliability: How to)

- 為產品定義具有"失效嚴重度類別"(FSC, Failure Severity Classes)的失效
- 為所有相關系統選擇通用度量
- 為每個要測試的系統設置"失效強度目標"(FIO, Failure Intensity Objective)
- 找出已開發軟體的失效強度目標
- 設計策略以達到軟體失效強度目標

## 失效嚴重度類別 (Failure Severity Classes)

- 失效嚴重度與其複雜度不同
- 嚴重度可能隨失效時間而變化
- 需要理解失效和失效嚴重度類別（例如等價類）
- 允許優先排序修復和恢復活動
- 為定義可靠度提供更細粒度

### 範例：缺陷嚴重度（Infosys）
- **嚴重** (Critical)：完全阻止系統測試/使用，無解決方法
- **主要** (Major)：系統某些功能無法工作，但有解決方法
- **中等** (Medium)：不符合規格，但功能可用
- **次要** (Minor)：小問題，不明顯影響系統

### 定義失效嚴重度類別方法
- 需要頭腦風暴！
- 基於經驗
- 列出可能被視為項目失效嚴重度的所有因素
- 將清單縮小到最關鍵和/或可測量的因素
- 某些因素可能難以測量，例如對公司聲譽的影響等

## 失效強度 (Failure Intensity)

- 簡單直觀
- 硬體：基於墻上時鐘時間
- 軟體：基於執行時間
- 例如：每1,000個打印頁面/電話呼叫1次失效
- 由於執行時間起源，適用於分布式系統
- 系統失效強度是組件失效強度的總和（可靠度不適用此規則）

## 失效強度目標 (Failure Intensity Objectives)

- 失效強度定義為每自然單位的失效，例如：
  - 每100小時操作3次警報
  - 每1000個打印作業5次失效
- 亞馬遜範例：
  - 整個網站宕機 — 目標：每年一次
  - 某些"商店"不可用 — 目標：每週一個商店不可用
  - 交易崩潰 — 每200萬筆交易1次崩潰

### 失效強度目標特性
- 系統的失效強度是系統所有組件失效強度的總和
- 失效強度目標反映了對產品在發布時允許保留的"錯誤"的估計
- FIO是表達系統可靠度的替代方式

### 如何設置失效強度目標
- 主要基於經驗
- 取決於項目
- 取決於品質特性（開發時間和開發成本）與功能性和技術之間的權衡

#### 以可靠度設置FIO
- $R = e^{-\lambda t}$，其中λ是失效強度，R是可靠度，t是自然單位（時間等）
- 對於λ = 0.001，8個自然單位的可靠度約為0.992
- 即，如果8小時的可靠度約為0.992，則λ = 0.001或每1000小時1次失效

## 軟體可靠度評估方法 (Software Reliability Evaluation Methods)

### 從測試和/或操作期間收集的數據評估
- **帶有故障移除的方法**：
  - 可靠度增長模型
  - 度量：失效率、失效強度、累積失效數、MTTF
- **不帶故障移除的穩定可靠度模型**：
  - 度量：無失效測試持續時間（達到目標可靠度所需）、接受/拒絕軟體的概率、執行時失效概率

## 可靠度模型 (Reliability Model)

### 影響因素
- **錯誤引入**：
  - 產品特性（如程式大小）
  - 開發流程（如軟體工程工具和技術、人員經驗等）
- **錯誤移除**：
  - 失效發現（如執行範圍、操作剖面）
  - 修復活動的質量
- **環境**

## 隨機性 (Randomness)

- **隨機行為**：
  - 將缺陷引入代碼
  - 執行測試用例
- 應該定義一些隨機過程來表示隨機性
- **如何處理隨機性**：
  - 通過測試收集失效數據
  - 找到最適合收集數據的分布函數
  - 對錯誤的存在和可靠度做出假設

## 時間概念 (Time Notion)

- **日曆時間**：
  - 軟體的實際運行時間
  - 例如：系統在兩週時間內不會失效的概率
  - 以t表示
- **執行時間**：
  - 機器執行軟體所花費的時間
  - 以τ表示

## 概率分布 (Probability Distribution)

- 設M(t)為表示時間t的失效數量的隨機過程
- 平均函數m(t)表示時間t的預期失效數量：
  - m(t) = E(M(t))
- 失效強度是預期失效數量相對於時間的變化率：
  - λ(t) = dm(t)/dt
- λ(t)是每單位時間的失效數量
- λ是一個瞬時值

## Musa基本執行時間模型 (Musa Basic Execution Time Model)

- **假設**：失效強度函數的減量是恆定的
- **結果**：失效強度是給定時間點經歷的平均失效數的函數（= 失效概率）
- **公式**：
  - λ(τ)：失效強度
  - λ₀：執行開始時的初始失效強度
  - μ：給定時間點的平均總失效數
  - v₀：無限時間內的總失效數
  - $\lambda(\mu) = \lambda_0 (1 - \frac{\mu}{v_0})$

### 範例1
- 假設程式將在無限執行時間內經歷100次失效
- 在過去的t時間單位間隔內觀察到50次失效
- 初始失效強度為每CPU小時10次失效
- 計算當前（t時的）失效強度：
  - $\lambda(50) = 10 (1 - \frac{50}{100}) = 5$ 次失效/CPU小時

## Musa/Okumoto對數模型 (Musa/Okumoto Logarithmic Model)

- 每次遇到失效的減量遞減：
  - $\lambda(\mu) = \lambda_0 e^{-\phi\mu}$，其中φ是失效強度衰減參數

### 範例2
- λ₀ = 每CPU小時10次失效
- φ = 0.02/失效
- 已經經歷50次失效（μ = 50）
- 當前失效強度：
  - $\lambda(50) = 10 e^{-(0.02 \times 50)} = 10 e^{-1} = 3.68$ 次失效/CPU小時

## 模型擴展 (Model Extension)

- 平均計數經歷失效總數(μ)作為經過執行時間(τ)的函數
- 基本模型：$\mu(\tau) = v_0 (1 - e^{-\frac{\lambda_0\tau}{v_0}})$
- 對數模型：$\mu(\tau) = \frac{1}{\phi} \ln(1 + \lambda_0 \phi \tau)$

### 範例3（基本模型）
- λ₀ = 每CPU小時10次失效
- v₀ = 100（無限執行時間內的失效數）
- τ = 10 CPU小時時：μ(10) = 63次失效
- τ = 100 CPU小時時：μ(100) = 100次失效

### 範例4（對數模型）
- λ₀ = 每CPU小時10次失效
- φ = 0.02/失效
- τ = 10 CPU小時時：μ(10) = 55次失效（基本模型為63）
- τ = 100 CPU小時時：μ(100) = 152次失效（基本模型為100）

## 參數估計 (Parameter Estimation)

- 使用失效規格：
  - 失效時間
  - 失效間隔時間
  - 給定時間內的累積失效
  - 時間間隔內經歷的失效
- 獲得(τ, μ(τ))對，然後使用最大似然估計(MLE)或最小二乘估計(LSE)來找到模型參數

## 模型有效性 (Validity of the Models)

- 軟體系統在其生命週期內多次更改（更新）
- 模型適用於一個修訂期而非整個生命週期

## 操作剖面 (Operational Profile)

- **操作剖面**是一組完整的操作及其發生概率（在軟體操作使用期間）
- 操作剖面提供有關用戶將如何使用軟體產品的信息，使開發人員可以集中開發和測試資源

### 定義：操作剖面
- 操作剖面是操作集（操作名稱和頻率）及其發生概率

### 準備操作剖面
- 軟體需求規格→操作清單（場景）→歷史或估計現場使用→發生頻率→操作剖面

### 誰開發操作剖面
- 需要派生一組類似於用例的場景
- 每個場景都要解決誰、在哪裡、做什麼和為什麼
- 可以使用需求獲取技術或與最終用戶的非正式討論來派生場景

## 準備測試 (SRE: Prepare for Test)

- 準備測試活動使用操作剖面來準備測試用例和測試程序
- 根據操作剖面分配測試用例
- 通過從所有可能的操作內選擇（等概率）分配測試用例
- 測試程序是執行期間調用測試用例的控制器

### ATM測試用例分布範例
- 假設我們有資源和時間為ATM機器開發200個測試用例
- 為了在操作之間分配測試用例，首先將總數乘以每個操作的發生概率，四捨五入到最接近的整數

## AIAA/ANSI推薦實踐 (AIAA/ANSI Recommended Practice)

- AIAA軟體可靠度推薦實踐(R-013-1992)從需求階段開始到軟體生命週期的操作使用階段使用
- 描述軟體可靠度估計和預測程序的活動和品質
- 詳細說明允許風險評估和預測軟體失效率的框架
- 為軟體可靠度估計和預測推薦一組模型
- 指定強制性和推薦的數據收集要求

### AIAA/ANSI推薦實踐的價值
- 支持面臨可靠度估計和預測方法不一致、術語各異以及眾多模型和數據收集方法的軟體從業者
- 通過定義通用術語、確定模型比較標準以及確定該領域中的開放研究問題來支持研究人員

## 網站可靠度工程 (Site Reliability Engineering)

- 由Google在21世紀初期開創
- 重點：整體系統/服務可靠度和卓越運營
- 主要關注：可用性、延遲、性能、效率、變更管理、監控、緊急響應、容量規劃

## 網站可靠度工程與軟體可靠度工程 (Site Reliability Engineering vs. Software Reliability Engineering)

- 兩個學科都旨在提高軟體系統的可靠度

### 共同點
- 專注於系統/軟體可靠度
- 使用數據驅動的度量（如失效率、MTBF）
- 強調測試、故障檢測和預防

### 關鍵區別：範圍
- **網站可靠度工程**：更廣泛，涵蓋基礎設施、操作和生產系統
  - 確保軟體可靠運行
  - 確保發布後的可靠度
- **軟體可靠度工程**：更窄，專注於軟體開發生命週期和質量
  - 構建可靠軟體
  - 專注於發布前質量

### 關鍵區別：活動
- **網站可靠度工程**：
  - 管理生產系統
  - 事件響應
  - 值班輪換
  - 錯誤預算
  - 基礎設施自動化
- **軟體可靠度工程**：
  - 測試方法
  - 缺陷預測
  - 可靠度建模
  - 代碼質量分析
  - 測試覆蓋率

### 關鍵區別：團隊整合
- **網站可靠度工程**：開發和運維之間的橋樑（DevOps文化）
- **軟體可靠度工程**：整合到開發團隊或質量保證中
