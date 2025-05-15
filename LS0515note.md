# 第48章：動物中的電信號 (Electrical Signals in Animals)

## 48.1 神經元結構與組織反映信息傳遞功能 (Neuron Structure and Organization Reflect Function in Information Transfer)

### 信息處理簡介 (Introduction to Information Processing)
- 神經系統的信息處理分三個階段：
  - **感覺輸入** (sensory input)：透過感覺神經元傳遞關於外部刺激和內部狀況的信息
  - **整合** (integration)：由中樞神經系統內的中間神經元進行分析與解釋
  - **運動輸出** (motor output)：通過運動神經元將信號傳導至肌肉或腺體
- 所有神經元以相同方式傳遞電信號，但**不同神經連接**產生不同信息傳遞

### 神經元結構與功能 (Neuron Structure and Function)
- **細胞體** (cell body)：包含大部分細胞器官和細胞核
- **樹突** (dendrites)：高度分支的延伸部分，與細胞體一起接收來自其他神經元的信號
- **軸突** (axon)：傳遞信號至其他細胞的單一延伸部分
  - **軸丘** (axon hillock)：軸突的錐形基部，通常是生成信號的地方
  - 軸突末端分支形成**突觸** (synapse)，通過**突觸終端** (synaptic terminal)與其他細胞接觸
  - 大多數突觸通過稱為**神經遞質** (neurotransmitters)的化學信使傳遞信息

### 神經系統組織 (Neural Organization)
- **中樞神經系統** (central nervous system, CNS)：包括腦或較簡單的神經節，進行信息排序、處理和整合
- **周圍神經系統** (peripheral nervous system, PNS)：負責傳導信息進出CNS的神經元
- **神經膠質細胞** (glial cells/glia)：支持細胞，為神經元提供營養和絕緣等功能

## 48.2 離子泵和離子通道建立神經元的靜息電位 (Ion Pumps and Ion Channels Establish the Resting Potential of a Neuron)

### 靜息電位的形成 (Formation of the Resting Potential)
- **膜電位** (membrane potential)：細胞膜兩側的電荷差異或電壓
- **靜息電位** (resting potential)：靜息神經元的膜電位，通常為-60至-80毫伏(mV)
- 關鍵離子濃度梯度：
  - K⁺（鉀離子）：細胞內濃度高，細胞外濃度低
  - Na⁺（鈉離子）：細胞內濃度低，細胞外濃度高
  - 這些梯度由**鈉鉀泵** (sodium-potassium pump)維持，每泵出3個Na⁺就泵入2個K⁺

### 靜息電位的模型 (Modeling the Resting Potential)
- **離子通道** (ion channels)：允許特定離子通過的膜蛋白
  - 靜息神經元有許多開放的鉀通道但很少開放的鈉通道
  - K⁺外流形成負電荷在細胞內部累積，產生膜電位
- **平衡電位** (equilibrium potential)：當離子的化學梯度與電梯度平衡時的膜電壓
  - 通過**能斯特方程** (Nernst equation)計算：$E_{ion} = 62 mV \times \log\frac{[ion]_{outside}}{[ion]_{inside}}$
  - K⁺的平衡電位(EK)約為-90 mV
  - Na⁺的平衡電位(ENa)約為+62 mV
- 靜息電位(-60至-80 mV)接近EK但不相等，因為少量Na⁺通道開放使電位略微偏移

### 細胞內記錄 (Intracellular Recording)
- 使用微電極技術測量細胞內外之間的電位差
- 微電極為充滿導電鹽溶液的玻璃毛細管，尖端直徑小於1μm
- 通過顯微鏡引導將電極插入細胞，記錄膜電位變化

## 48.3 動作電位是由軸突傳導的信號 (Action Potentials are the Signals Conducted by Axons)

### 超極化與去極化 (Hyperpolarization and Depolarization)
- **門控離子通道** (gated ion channels)：響應刺激開閉的通道
  - **電壓門控離子通道** (voltage-gated ion channel)：對膜電位變化作出反應
- **超極化** (hyperpolarization)：膜電位絕對值增加，內部變得更負
  - 當K⁺外流增加或負離子內流增加時發生
- **去極化** (depolarization)：膜電位絕對值減少，內部變得較不負
  - 通常涉及Na⁺內流

### 分級電位與動作電位 (Graded Potentials and Action Potentials)
- **分級電位** (graded potential)：
  - 強度與刺激強度成比例的局部膜電位變化
  - 隨距離和時間衰減
- **動作電位** (action potential)：
  - 當去極化達到**閾值** (threshold，約-55 mV)時產生
  - 全或無反應，強度不隨刺激強度變化
  - 可沿軸突再生並傳導至遠處

### 動作電位的產生：深入探討 (Generation of Action Potentials: A Closer Look)
- 動作電位階段：
  1. **靜息狀態**：大多數電壓門控鈉通道關閉，一些鉀通道開放
  2. **去極化**：當刺激使膜電位達到閾值，鈉通道開放，Na⁺內流
  3. **上升相** (rising phase)：正反饋過程，更多鈉通道開放，膜電位接近ENa
  4. **下降相** (falling phase)：
     - 鈉通道去活化（通道阻斷）：即使通道處於「開放」狀態，無法傳導Na⁺
     - 電壓門控鉀通道開放，K⁺外流增加
  5. **超射** (undershoot)：K⁺通透性高於靜息狀態，膜電位更接近EK
- **不應期** (refractory period)：鈉通道去活化期間，無法被再次刺激產生新的動作電位

### 動作電位的傳導 (Conduction of Action Potentials)
- 動作電位從軸丘開始，沿軸突**單向傳播**至突觸終端
- 傳導速度影響因素：
  - **軸突直徑**：較粗的軸突提供較低的電阻，傳導更快
  - **髓鞘** (myelin sheath)：由神經膠質細胞形成的絕緣層
    - 中樞神經系統中由**少突神經膠質細胞** (oligodendrocytes)形成
    - 周圍神經系統中由**施旺細胞** (Schwann cells)形成
    - 髓鞘間隙稱為**郎飛氏結** (nodes of Ranvier)
- **跳躍式傳導** (saltatory conduction)：動作電位從一個郎飛氏結「跳躍」至下一個
  - 顯著提高傳導速度，節省空間（相較於無脊椎動物的巨軸突）

## 48.4 神經元在突觸處與其他細胞通訊 (Neurons Communicate with Other Cells at Synapses)

### 突觸類型 (Types of Synapses)
- **電突觸** (electrical synapse)：
  - 包含**間隙連接** (gap junctions)
  - 電流直接從一個神經元流向另一個
  - 通常與需要快速、一致反應的行為相關
- **化學突觸** (chemical synapse)：
  - 依靠神經遞質傳遞信息
  - 過程：動作電位→Ca²⁺內流→突觸小泡與終端膜融合→神經遞質釋放

### 突觸後電位的產生 (Generation of Postsynaptic Potentials)
- **配體門控離子通道** (ligand-gated ion channel)/**離子型受體** (ionotropic receptor)：
  - 神經遞質結合引起通道開放，允許特定離子通過
  - 產生**突觸後電位** (postsynaptic potential)
- **興奮性突觸後電位** (excitatory postsynaptic potential, EPSP)：
  - 通道對K⁺和Na⁺均有通透性
  - 使膜電位去極化，接近閾值
- **抑制性突觸後電位** (inhibitory postsynaptic potential, IPSP)：
  - 通道通常僅對K⁺或Cl⁻通透
  - 使膜電位超極化，遠離閾值

### 突觸後電位的總和 (Summation of Postsynaptic Potentials)
- 單一突觸的輸入通常不足以觸發反應
- **時間總和** (temporal summation)：
  - 同一突觸上快速連續的多個電位相加
- **空間總和** (spatial summation)：
  - 來自不同突觸的電位同時相加
- 軸丘是**整合中心**，膜電位反映所有EPSP和IPSP的總和效果
- 當總和使軸丘的膜電位達到閾值時，產生動作電位

### 神經遞質信號的終止 (Termination of Neurotransmitter Signaling)
- 清除突觸間隙中的神經遞質分子：
  - **酶水解** (enzymatic hydrolysis)
  - **再攝取** (reuptake)進入突觸前神經元
  - 轉移到神經膠質細胞進行代謝或回收

### 突觸處的調控信號 (Modulated Signaling at Synapses)
- **代謝型受體** (metabotropic receptor)/**G蛋白偶聯受體** (G protein-coupled receptor)：
  - 啟動信號轉導途徑，涉及第二信使
  - 與離子型受體相比，效應啟動較慢但持續時間更長
  - 例如：去甲腎上腺素受體→G蛋白→腺苷酸環化酶→cAMP→蛋白激酶A→離子通道磷酸化

### 神經遞質 (Neurotransmitters)
1. **乙酰膽鹼** (Acetylcholine)：
   - 與肌肉刺激、記憶形成和學習相關
   - 離子型受體：在神經肌肉接頭產生EPSP
   - 代謝型受體：在心臟中產生抑制效應
   - 乙酰膽鹼酯酶(acetylcholinesterase)通過水解終止其活性

2. **氨基酸類** (Amino Acids)：
   - **穀氨酸** (Glutamate)：脊椎動物CNS中最常見的興奮性神經遞質
   - **GABA** (gamma-aminobutyric acid)：大腦中主要的抑制性神經遞質
   - **甘氨酸** (Glycine)：在大腦外CNS部分的抑制性突觸作用

3. **生物胺** (Biogenic Amines)：
   - **去甲腎上腺素** (Norepinephrine)：自主神經系統中的興奮性神經遞質
   - **多巴胺** (Dopamine)和**血清素** (Serotonin)：影響睡眠、情緒、注意力和學習

4. **神經肽** (Neuropeptides)：
   - **P物質** (Substance P)：調節疼痛感知的關鍵興奮性神經遞質
   - **內啡肽** (Endorphins)：天然鎮痛劑，減少疼痛感知

5. **氣體** (Gases)：
   - **一氧化氮** (NO)：在需要時合成，不儲存在囊泡中
   - **一氧化碳** (CO)：小量作為神經遞質產生

# 神經調控 (Neural Regulation)

## 神經系統的基本結構 (Nervous System Structure)

### 神經系統的演化 (Evolution of Nervous Systems)
- 感知和反應能力起源於數十億年前的原核生物，提高了生物在變化環境中的生存與繁殖成功率
- 腔腸動物（如水螅、水母）擁有最簡單的神經系統——擴散型神經網絡（diffuse nerve net）
- 更複雜動物的神經軸突經常束聚在一起形成神經（nerves），引導特定路徑的信息流動
- 雙側對稱動物展現頭化（cephalization）——感覺神經元和中間神經元集中在身體前端

### 神經系統的組織 (Organization of Nervous Systems)
- **無脊椎動物神經系統複雜性變化**：
  - 水螅（水母）：擴散型神經網絡
  - 海星：輻射神經連接至中央神經環
  - 扁形蟲：小腦和縱行神經索構成簡單的中樞神經系統
  - 線蟲（如秀麗隱桿線蟲）：成蟲有精確的302個神經元
  - 環節動物（如水蛭）：分節排列的神經節控制行為
  - 節肢動物（如昆蟲）：腹神經索含有神經節
  - 軟體動物：從簡單（如蛤蜊）到複雜（如章魚，有數百萬神經元）

- **脊椎動物神經系統**：
  - 中樞神經系統（CNS）：腦和脊髓，負責整合信息
  - 周圍神經系統（PNS）：神經和神經節，傳遞感覺和運動信號

### 脊椎動物神經系統詳解 (Vertebrate Nervous System in Detail)
- **脊髓和腦的發育**：
  - 由胚胎期的中空背神經管發育而成
  - 神經管的腔形成脊髓的中央管道和腦室
  - 腦室和中央管道充滿腦脊髓液（cerebrospinal fluid）

- **灰質與白質**：
  - 灰質（gray matter）：主要由神經元細胞體組成
  - 白質（white matter）：由束狀軸突組成
  - 脊髓：白質在外層，反映其連接中樞神經系統與感覺和運動神經元的作用
  - 腦：白質主要在內部，參與學習、情緒處理、感覺信息處理和指令生成

- **反射**：
  - 為身體提供快速、非自願的反應
  - 不需經過大腦處理，直接在脊髓層面完成
  - 例如：燙手反射、膝跳反射

### 周圍神經系統 (Peripheral Nervous System)
- **兩種類型的神經元**：
  - 傳入神經元（afferent neurons）：將感覺信息傳入中樞神經系統
  - 傳出神經元（efferent neurons）：將指令從中樞神經系統傳出

- **傳出神經元的兩個分支**：
  1. **運動系統**：控制骨骼肌
     - 可自願控制（如舉手）
     - 也可非自願（如膝跳反射）
  
  2. **自主神經系統**：控制平滑肌和心肌，通常為非自願
     - **交感神經系統**（sympathetic division）
       - 引起興奮和能量產生（"戰鬥或逃跑"反應）
       - 增加心率，抑制消化，轉化肝糖原為葡萄糖，增加腎上腺髓質分泌腎上腺素
     
     - **副交感神經系統**（parasympathetic division）
       - 引起平靜和自我維護功能（"休息與消化"）
       - 降低心率，促進消化，增加糖原生成
       - 在生殖活動調控中與交感神經系統協同而非拮抗
     
     - **腸神經系統**（enteric nervous system）
       - 對消化道、胰腺和膽囊進行直接且部分獨立的控制

- **自主神經系統的神經元路徑**：
  - 通常包含節前神經元（preganglionic neurons）和節後神經元（postganglionic neurons）
  - 節前神經元：細胞體在中樞神經系統，釋放乙醯膽鹼（acetylcholine）
  - 節後神經元：
    - 副交感神經系統：釋放乙醯膽鹼
    - 交感神經系統：主要釋放去甲腎上腺素（norepinephrine）

### 神經膠質細胞 (Glial Cells)
- 神經膠質細胞（glial cells/glia）在神經系統中支持、營養和調節神經元功能
- **主要類型**：
  - **室管膜細胞**（ependymal cells）：鋪砌腦室，有纖毛促進腦脊髓液循環
  - **少突膠質細胞**（oligodendrocytes）：在中樞神經系統形成髓鞘，顯著增加動作電位傳導速度
  - **小膠質細胞**（microglia）：中樞神經系統內的免疫細胞，保護against病原體
  - **許旺細胞**（Schwann cells）：在周圍神經系統形成髓鞘
  - **星形膠質細胞**（astrocytes）：
    - 促進信息傳遞
    - 調節細胞外離子濃度
    - 促進血液流向神經元
    - 幫助形成血腦屏障（blood-brain barrier）
    - 作為干細胞更新某些神經元

- **胚胎發育中的膠質細胞**：
  - 放射狀膠質細胞（radial glia）：形成軌道，使新形成的神經元從神經管遷移
  - 星形膠質細胞：參與血腦屏障形成，也可作為干細胞

## 脊椎動物腦的區域專門化 (Regional Specialization of the Vertebrate Brain)

### 腦的主要區域 (Major Brain Regions)
- **前腦**（forebrain）：
  - 包含嗅球（olfactory bulb）和大腦（cerebrum）
  - 功能：處理嗅覺輸入，調節睡眠，學習和複雜處理

- **中腦**（midbrain）：
  - 位於腦的中央部分
  - 功能：協調感覺輸入的路由

- **後腦**（hindbrain）：
  - 部分形成小腦（cerebellum）
  - 功能：控制非自願活動（如血液循環），協調運動活動（如運動）

### 腦結構的演化對比 (Evolutionary Comparison of Brain Structures)
- 腦區的相對大小在不同脊椎動物之間有所不同，反映特定腦功能的重要性
- 例如：游泳的硬骨魚類（如鮪魚）有較大的小腦，而不活躍游泳的七鰓鰻小腦相對較小
- 鳥類和哺乳動物的前腦占腦部比例比兩棲動物、魚類等其他脊椎動物更大
- 鳥類和哺乳動物的腦對體重比例是進化祖先的十倍，反映更高認知和推理能力

### 人類腦的組織 (Organization of the Human Brain)
- **胚胎發育**：
  - 神經管形成三個前部膨大：前腦、中腦和後腦
  - 中腦和部分後腦形成腦幹（brainstem）
  - 後腦其餘部分形成小腦
  - 前腦發育成間腦（diencephalon）和端腦（telencephalon，發展為大腦）

- **主要結構**：
  1. **大腦**（cerebrum）：
     - 控制骨骼肌收縮，是學習、情緒、記憶和感知的中心
     - 分為左右兩半球（cerebral hemispheres）
     - 外層是大腦皮層（cerebral cortex），對感知、自願運動和學習至關重要
     - 胼胝體（corpus callosum）：連接左右大腦皮層的一束軸突
     - 基底核（basal nuclei）：計劃和學習運動序列的中心

  2. **小腦**（cerebellum）：
     - 協調運動和平衡，幫助學習和記憶運動技能
     - 接收關於關節位置和肌肉長度的感覺信息，以及聽覺和視覺系統的輸入
     - 監控大腦發出的運動指令
     - 手眼協調是小腦控制的例子

  3. **腦幹**（brainstem）：
     - 包括中腦、腦橋（pons）和延髓（medulla oblongata）
     - 中腦接收並整合多種感覺信息
     - 腦橋和延髓在中樞神經系統和周圍神經系統之間傳遞信息
     - 協調大規模身體運動如奔跑和攀爬
     - 延髓控制多種自動的內環境穩定功能：呼吸、心血管活動、吞嚥等
     - 腦橋也參與一些這些活動，如調節延髓的呼吸中心

  4. **間腦**（diencephalon）：
     - 形成丘腦（thalamus）、下丘腦（hypothalamus）和上丘腦（epithalamus）
     - 丘腦是感覺信息進入大腦的主要輸入中心
     - 下丘腦是控制中心，包括體溫調節器和中央生物鐘
     - 上丘腦包括松果體（pineal gland），是褪黑素的來源

### 喚醒和睡眠 (Arousal and Sleep)
- **喚醒**（arousal）：對外部世界的意識狀態
- **睡眠**（sleep）：接收外部刺激但無意識感知的狀態
- 睡眠是大腦的活躍狀態，可通過腦電圖（EEG）記錄腦波頻率
- 瓶鼻海豚在睡眠時繼續游泳，每次只有一側大腦半球睡眠
- 睡眠的功能可能涉及鞏固學習和記憶
- 網狀結構（reticular formation）：控制睡眠時期的時間，特別是具有快速眼動（REM）和生動夢境的睡眠

### 生物鐘調節 (Biological Clock Regulation)
- 睡眠和清醒的週期是晝夜節律（circadian rhythm）的例子
- 生物鐘（biological clock）是控制基因表達和細胞活動的週期性分子機制
- 在哺乳動物中，下丘腦中的視交叉上核（suprachiasmatic nucleus，SCN）協調全身細胞的生物鐘

### 情緒 (Emotions)
- 情緒的產生和體驗依賴多種腦結構：杏仁核（amygdala）、海馬體（hippocampus）和丘腦的部分
- 這些結構位於哺乳動物腦幹邊緣，稱為邊緣系統（limbic system）
- 邊緣系統通過將情緒體驗存儲為記憶來貢獻情緒
- 杏仁核對情緒記憶的存儲和回憶特別重要

### 腦的功能成像 (Functional Imaging of the Brain)
- **正電子發射斷層掃描**（positron-emission tomography，PET）：注射放射性葡萄糖顯示代謝活動
- **功能性磁共振成像**（functional magnetic resonance imaging，fMRI）：通過檢測富氧血液流入特定區域來檢測腦活動
- 功能成像可用於研究特定腦功能，如音樂對情緒的影響

## 大腦皮層對自願運動和認知功能的控制 (Cerebral Cortex Control)

### 信息處理 (Information Processing)
- 人腦皮層從兩個來源接收感覺信息：
  1. 位於手、頭皮等處的個體感受器（體感感受器）
  2. 專門感覺器官中的感受器群（如眼睛和鼻子）

- 通過丘腦進入皮層的大部分感覺信息被引導至大腦葉中的初級感覺區
- 接收到的信息傳遞到附近的聯合區進行處理
- 處理後的感覺信息傳遞到前額葉皮層，幫助計劃動作和運動
- 運動指令由額葉後部的運動皮層（motor cortex）產生

### 軀體感覺皮層和運動皮層 (Somatosensory and Motor Cortex)
- 在這兩個皮層區域，神經元按照產生感覺輸入或接收運動指令的身體部位排列
- 皮層表面積與每個身體部位不成比例，而是與神經控制程度（運動皮層）或感覺神經元數量（軀體感覺皮層）相關
- 例如，負責面部的運動皮層區域較大，反映面部肌肉在溝通中的廣泛參與

### 語言和言語 (Language and Speech)
- **布羅卡區**（Broca's area）：位於左額葉，與言語產生相關
- **韋尼克區**（Wernicke's area）：位於左顳葉後部，與言語理解相關
- PET研究證實說話時布羅卡區活躍，聽話時韋尼克區活躍

### 皮層功能的側化 (Lateralization of Cortical Function)
- 左半球更擅長語言、數學和邏輯操作
- 右半球更擅長臉部和模式識別、空間關係和非語言思維
- 這種左右半球功能差異稱為側化（lateralization）
- 胼胝體通常允許兩個半球交換信息
- 切斷胼胝體導致"分裂腦"效應，兩個半球獨立運作

### 前額葉功能 (Frontal Lobe Function)
- 前額葉皮層在性格和決策中起重要作用
- 前額葉腫瘤或前額葉切除術患者表現出相似症狀：智力和記憶似乎完好，但決策有缺陷，情緒反應減弱

### 脊椎動物認知的演化 (Evolution of Cognition in Vertebrates)
- 哺乳動物和鳥類都具有高級認知能力，儘管腦部組織不同
- 以前認為高階推理需要有褶皺的大腦皮層（如在人類、靈長類和鯨類中）
- 然而，鳥類表現出高智力能力：
  - 西部灌木松鴉能記住它們首先藏起哪些食物
  - 新喀里多尼亞烏鴉精於製作和使用工具
  - 非洲灰鸚鵡能理解數字和抽象概念

- **兩種不同的外腦組織**：
  1. 鳥類的蒼白（pallium）：神經元呈核狀（聚集）排列
  2. 人類大腦皮層：六層平行神經元排列

## 突觸連接的變化是記憶和學習的基礎 (Synaptic Connections in Memory and Learning)

### 神經系統的形成 (Formation of the Nervous System)
- 神經系統的形成分步驟進行：
  1. 基因表達和信號轉導決定神經元在發育中胚胎中的形成位置
  2. 神經元競爭生存，未能到達適當位置的神經元無法接收生長支持因子，導致程序性細胞死亡
  3. 突觸消除發生，許多不必要的連接被淘汰
- 在胎兒發育過程中，超過一半的神經元被淘汰
- 突觸修剪（synaptic pruning）在出生後和整個童年期繼續進行

### 神經元可塑性 (Neuronal Plasticity)
- 神經元可塑性（neuronal plasticity）：神經系統重塑的能力，特別是回應自身活動
- 當突觸活動與其他突觸活動一致時，該突觸連接可能增強
- 當突觸活動與其他突觸活動不一致時，該突觸連接可能減弱
- 自閉症譜系障礙（autism spectrum disorder）可能涉及突觸活動依賴性重塑的破壞

### 記憶和學習 (Memory and Learning)
- **短期記憶**（short-term memory）：
  - 通過海馬體中形成的臨時連接訪問在大腦皮層中存儲的信息
  - 如果信息變得無關緊要，則釋放它

- **長期記憶**（long-term memory）：
  - 海馬體中的連接被大腦皮層本身的連接所取代
  - 部分記憶鞏固（consolidation）可能在睡眠期間發生

- 海馬體對獲取新的長期記憶至關重要，但對維持它們並不必要
- 海馬體損傷可能導致無法形成新的持久記憶，但能夠自由回憶受傷前的事件
- 阿爾茨海默病早期階段常見海馬體損傷和記憶喪失

- **運動技能學習**：
  - 通過重複學習（如系鞋帶或書寫）
  - 涉及類似於大腦生長和發育的細胞機制
  - 神經元實際上形成新的連接

- **記憶事實**：
  - 可能非常快速，可能只需要一次接觸相關項目
  - 主要依靠改變現有神經元連接的強度

### 長期增強 (Long-Term Potentiation)
- 長期增強（long-term potentiation，LTP）：突觸傳遞強度的持久增加
- 在海馬體組織切片中首次表徵，涉及釋放興奮性神經遞質谷氨酸（glutamate）的突觸前神經元
- 涉及兩種谷氨酸受體：NMDA和AMPA
- 當兩個條件滿足時，突觸後膜上存在的受體組會發生變化：
  1. 突觸前神經元的一系列快速動作電位
  2. 突觸後細胞其他部位的去極化刺激
- 結果是穩定增加與另一輸入活動一致的突觸的突觸後電位

## 許多神經系統疾病現在可以用分子術語解釋 (Molecular Explanation of Nervous System Disorders)

### 精神分裂症 (Schizophrenia)
- 約1%的世界人口患有精神分裂症，一種嚴重的精神障礙
- 特徵：精神病發作，患者對現實有扭曲感知
- 症狀包括幻覺（如只有他們能聽到的"聲音"）和妄想（如他人密謀傷害他們的想法）
- 家族研究表明有很強的遺傳成分，但也受環境影響
- 使用多巴胺（dopamine）作為神經遞質的神經元通路可能在精神分裂症中被破壞
- 支持證據：緩解精神分裂症症狀的藥物阻斷多巴胺受體
- 苯丙胺（amphetamine，"speed"）能刺激多巴胺釋放，可產生與精神分裂症相同的症狀

### 抑鬱症 (Depression)
- **兩種形式**：
  1. **重性抑鬱障礙**（major depressive disorder）：
     - 持續數月的抑鬱情緒
     - 曾經愉快的活動不再提供快樂或引起興趣
     - 影響約七分之一的成年人，女性是男性的兩倍
  
  2. **雙相情感障礙**（bipolar disorder，躁鬱症）：
     - 情緒極端波動，影響約1%的世界人口
     - 躁狂期：自尊心高，精力充沛，想法湧現，過度談話，增加冒險
     - 抑鬱期：動機、自我價值感和感受快樂的能力降低，睡眠障礙
     - 症狀嚴重可導致自殺嘗試

- 治療：許多用於治療抑鬱症的藥物（如氟西汀/百憂解Prozac）增加大腦中生物胺（biogenic amines）的活性

### 大腦獎勵系統與藥物成癮 (Brain's Reward System and Drug Addiction)
- 獎勵系統為增強生存和繁殖的活動提供動機
- 腹側被蓋區（ventral tegmental area，VTA）接收輸入，位於中腦內
- 激活時，VTA神經元從其突觸末端釋放神經遞質多巴胺
- 多巴胺信號的目標包括伏隔核（nucleus accumbens）和前額葉皮層
- 藥物成癮（drug addiction）特徵為強迫性消費藥物和失去控制攝入量的能力
- 成癮藥物增強多巴胺通路的活性
- 成癮發展時，獎勵迴路發生持久變化
- 結果是對藥物的渴望獨立於與消費相關的任何快樂

### 阿爾茨海默病 (Alzheimer's Disease)
- 阿爾茨海默病是一種精神退化或痴呆，特徵為混亂和記憶喪失
- 其發生率與年齡相關，65歲時約10%，85歲時約35%
- 占痴呆症病例的約三分之二
- 是美國成人第六大常見死亡原因
- 阿爾茨海默病是漸進性的，患者逐漸變得無法自理
- 死於阿爾茨海默病的個體腦部呈現兩個特徵：
  1. **澱粉樣斑塊**（amyloid plaques）：
     - β-澱粉樣蛋白（β-amyloid）的聚集物
     - 由分泌酶（secretases）從神經元膜蛋白的細胞外部分切割而成
     - 這些斑塊似乎觸發周圍神經元的死亡
  
  2. **神經纖維纏結**（neurofibrillary tangles）：
     - 主要由tau蛋白組成
     - 正常情況下，tau蛋白幫助組裝和維護運輸營養物質的微管
     - 在阿爾茨海默病中，tau發生變化導致自身結合，形成神經纖維纏結

- 慢性創傷性腦病（chronic traumatic encephalopathy，CTE）也以tau蛋白累積為特徵
  - 在運動員、軍人和有腦震盪或重複性腦外傷史的人中發現

### 帕金森病 (Parkinson's Disease)
- 帕金森病是一種運動障礙
- 症狀包括肌肉震顫、平衡不良、屈曲姿勢和蹣跚步態
- 面部肌肉變得僵硬，限制患者變換表情的能力
- 可能發展認知缺陷
- 與阿爾茨海默病一樣是漸進性的腦部疾病，隨年齡增長更常見
- 涉及中腦中正常釋放多巴胺的神經元死亡
- 蛋白質聚集物累積
- 大多數病例沒有可識別的原因，但早發型帕金森病有明確的遺傳基礎
- 與早發型帕金森病相關的突變顯示出線粒體功能所需基因的破壞
- 目前可以治療症狀但無法治癒
- 治療方法包括腦外科手術、深部腦刺激和L-多巴（L-dopa）藥物
- L-多巴可通過血腦屏障，在腦內被多巴脫羧酶（dopa decarboxylase）轉化為多巴胺

### 未來腦研究方向 (Future Directions in Brain Research)
- 2014年，美國政府啟動了12年的BRAIN（腦研究通過推進創新神經技術）計劃
- 目標是開發創新技術並推動科學進步
- 具體目標：繪製腦回路圖，測量這些回路內的活動，並發現該活動如何轉化為思想和行為