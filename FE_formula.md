# 特徵工程與特徵選擇方法的綜合整理

## 1. 數值型預測變數的轉換方法

### 1.1 單變量轉換

- **Box-Cox轉換**：適用於嚴格為正的數據
  $$x^* = \begin{cases} 
  \frac{x^\lambda - 1}{\lambda \tilde{x}^{\lambda-1}}, & \lambda \neq 0 \\
  \tilde{x}\log(x), & \lambda = 0
  \end{cases}$$
  其中$\tilde{x}$是預測變數數據的幾何平均值

- **Yeo-Johnson轉換**：類似Box-Cox但可用於任何數值數據（包括負值）
  $$x^* = \begin{cases} 
  \frac{(x+1)^\lambda - 1}{\lambda}, & \lambda \neq 0, x \geq 0 \\
  \log(x+1), & \lambda = 0, x \geq 0 \\
  -\frac{(-x+1)^{2-\lambda} - 1}{2-\lambda}, & \lambda \neq 2, x < 0 \\
  -\log(-x+1), & \lambda = 2, x < 0
  \end{cases}$$

- **Logit轉換**：適用於範圍在0和1之間的變數
  $$\text{logit}(\pi) = \log\left(\frac{\pi}{1-\pi}\right)$$

- **反正弦轉換**：用於比例數據
  $$y^* = \text{arcsine}(\sqrt{\pi})$$

- **中心化與縮放**：
  - 中心化：$x^* = x - \bar{x}$
  - 縮放：$x^* = \frac{x - \bar{x}}{s}$
  - 範圍縮放：$x^* = \frac{x - \min(x)}{\max(x) - \min(x)}$

- **平滑處理**：
  - 運行均值：$x^*_i = \frac{1}{2k+1}\sum_{j=i-k}^{i+k} x_j$
  - 運行中位數：$x^*_i = \text{median}(x_{i-k}, \ldots, x_i, \ldots, x_{i+k})$

### 1.2 一對多轉換

- **基底展開**：創建原始變數的非線性函數
  $$f(x) = \beta_1 x + \beta_2 x^2 + \beta_3 x^3$$

- **樣條函數**：
  - 多項式樣條：將預測變數空間分割成區域，每個區域使用多項式函數
  - 自然三次樣條：特殊的多項式樣條，在邊界處有額外約束
  - 鉸鏈函數（MARS模型中使用）：
    $$h(x) = x \cdot I(x > 0)$$
    其中$I$是指示函數，當$x>0$時為1，否則為0

- **離散化/分箱**：將連續變數轉換為類別變數
  - 等寬分箱：$\text{bin}_i = \lfloor \frac{x - \min(x)}{w} \rfloor + 1$，其中$w = \frac{\max(x) - \min(x)}{k}$，$k$是分箱數
  - 等頻分箱：根據分位數將數據分成相等數量的箱

### 1.3 多對多轉換

- **線性投影方法**：
  $$X^* = XA$$
  其中$A$是投影矩陣

- **主成分分析(PCA)**：找到能解釋最大方差的線性組合
  - 特徵分解：$\Sigma = V \Lambda V^T$，其中$\Sigma$是協方差矩陣，$V$是特徵向量，$\Lambda$是特徵值對角矩陣
  - 主成分：$Z = XV$，其中$X$是中心化的原始數據，$V$是特徵向量

- **核主成分分析**：使用核函數擴展PCA
  核函數示例：
  - 線性核：$k(x_1, x_2) = \langle x_1, x_2 \rangle = x_1^T x_2$
  - 多項式核：$k(x_1, x_2) = \langle x_1, x_2 \rangle^d$
  - 徑向基函數核：$k(x_1, x_2) = \exp\left(-\frac{\|x_1-x_2\|^2}{2\sigma^2}\right)$

- **獨立成分分析(ICA)**：尋找統計獨立的成分
  - 假設原始信號：$S = [s_1, s_2, \ldots, s_n]^T$
  - 觀測信號：$X = AS$，其中$A$是混合矩陣
  - 目標：找到解混矩陣$W$使得$\hat{S} = WX$近似於$S$

- **非負矩陣分解(NNMF)**：適用於非負特徵的矩陣分解
  - 分解：$X \approx WH$，其中$X, W, H$均為非負矩陣
  - $W$代表基礎模式，$H$代表這些模式的系數

- **偏最小平方法(PLS)**：找到與反應變數最佳相關的主成分
  - 目標：找到方向$w$使得$\text{cov}(Xw, y)$最大化
  - 得到潛在變量$t = Xw$
  - 將$X$和$y$分別對$t$進行回歸，得到殘差矩陣，然後迭代

- **自編碼器**：使用神經網路架構進行非線性維度降低
  - 編碼器：$h = f(Wx + b)$
  - 解碼器：$\hat{x} = g(W'h + b')$
  - 損失函數：$L(x, \hat{x}) = \|x - \hat{x}\|^2$

- **空間符號變換**：
  $$x^*_{ij} = \frac{x_{ij}}{\sqrt{\sum_{j=1}^p x_{ij}^2}}$$

## 2. 分類預測變數的編碼方法

- **虛擬變數/參考單元編碼**：將類別轉換為二進制指標變數，通常使用$C-1$個變數（$C$為類別數）
  - 對於類別$i$，除參考類別外，第$j$個二進制變數值為：
  $$x_{ij} = \begin{cases}
  1, & \text{if } i = j \\
  0, & \text{otherwise}
  \end{cases}$$

- **單熱編碼**：為每個類別創建指標變數（使用$C$個變數）
  - 對於類別$i$，第$j$個二進制變數值為：
  $$x_{ij} = \begin{cases}
  1, & \text{if } i = j \\
  0, & \text{otherwise}
  \end{cases}$$

- **多項式對比編碼**：用於有序類別，創建線性、二次等關係
  - 線性對比：$L_i = \frac{i - \frac{k+1}{2}}{\frac{k-1}{2}}$，其中$i$是級別，$k$是總級別數
  - 二次對比：$Q_i = L_i^2 - \frac{k^2-1}{12}$
  - 三次對比：$C_i = L_i^3 - L_i \frac{3(k^2-1)}{20}$

- **特徵雜湊**：將類別映射到固定數量的特徵列
  $$\text{column} = (\text{integer\_hash} \mod N) + 1$$

- **效應編碼/似然編碼**：基於類別與結果關係編碼
  - 效應編碼：類別$j$的編碼為$x_j = \frac{n_j}{n}$，其中$n_j$是類別$j$的樣本數，$n$是總樣本數
  - 似然編碼：類別$j$的編碼為$x_j = \log\frac{p_j}{1-p_j}$，其中$p_j$是類別$j$中正例的比例

- **詞嵌入/實體嵌入**：將類別映射到低維連續空間
  - 表示類別$i$為連續向量$v_i \in \mathbb{R}^d$
  - 這些向量通常通過神經網絡學習得到，目標是使得相似類別的向量在嵌入空間中彼此靠近

## 3. 交互作用檢測方法

- **交互作用的數學表示**：
  $$y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \beta_3 x_1 x_2 + \text{error}$$

- **簡單篩選**：比較包含交互作用的模型與不包含的模型
  - 無交互作用模型：$y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \text{error}$
  - 有交互作用模型：$y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \beta_3 x_1 x_2 + \text{error}$
  - 通過F檢驗或似然比檢驗比較兩個模型

- **H統計量**：比較變數聯合效應與單獨效應之間的差異
  $$H = \frac{\sum_{i=1}^n [f(x_i) - f_{-j}(x_i) - f_{-k}(x_i) + f_{-j,-k}(x_i)]^2}{\sum_{i=1}^n [f(x_i)]^2}$$
  其中，$f(x_i)$是完整模型的預測，$f_{-j}(x_i)$是移除變數$j$後的預測，$f_{-j,-k}(x_i)$是同時移除變數$j$和$k$後的預測

## 4. 缺失數據處理方法

- **數據刪除**：移除包含缺失值的預測變數或樣本

- **缺失性編碼**：將缺失值視為獨特類別（對分類變數尤其適用）

- **插補方法**：
  - 中位數/均值插補：
    $$x_{ij}^* = \begin{cases}
    x_{ij}, & \text{if } x_{ij} \text{ is not missing} \\
    \bar{x}_j \text{ or median}(x_j), & \text{if } x_{ij} \text{ is missing}
    \end{cases}$$
  
  - K-最近鄰插補：
    $$x_{ij}^* = \frac{1}{K}\sum_{l \in N_K(i)} x_{lj}$$
    其中$N_K(i)$是與樣本$i$最相似的$K$個樣本的集合
    
  - 樹基插補：使用決策樹預測缺失值
    $$\hat{x}_{ij} = f_j(x_{i,-j})$$
    其中$x_{i,-j}$表示除$j$外的所有變數
    
  - 線性方法插補：使用迴歸模型估計
    $$\hat{x}_{ij} = \beta_0 + \sum_{k \neq j} \beta_k x_{ik}$$

## 5. 特徵選擇方法

### 5.1 內在方法

- **樹型和規則型模型**：在建模過程中自然進行特徵選擇

- **正則化方法**：
  - 嶺迴歸(L2正則化)：
    $$SSE_{L2} = \sum_{i=1}^n (y_i - \hat{y}_i)^2 + \lambda_r \sum_{j=1}^P \beta_j^2$$
  - LASSO迴歸(L1正則化)：
    $$SSE_{L1} = \sum_{i=1}^n (y_i - \hat{y}_i)^2 + \lambda_{\ell} \sum_{j=1}^P |\beta_j|$$
  - 彈性網：
    $$SSE = \sum_{i=1}^n (y_i - \hat{y}_i)^2 + (1-\alpha)\lambda_r \sum_{j=1}^P \beta_j^2 + \alpha\lambda_{\ell} \sum_{j=1}^P |\beta_j|$$

### 5.2 過濾方法

- **單變量過濾器**：評估每個變數與結果的關係
  - t統計量：$t = \frac{\bar{x}_1 - \bar{x}_2}{s_p\sqrt{\frac{1}{n_1}+\frac{1}{n_2}}}$，其中$s_p$是匯總標準差
  - F統計量：$F = \frac{MS_{between}}{MS_{within}}$，其中$MS$是均方
  - 相關系數：$r = \frac{\sum_{i=1}^n (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_{i=1}^n (x_i - \bar{x})^2 \sum_{i=1}^n (y_i - \bar{y})^2}}$
  - ROC曲線下面積：$AUC = \int_0^1 ROC(t)dt$
  - 勝算比：$OR = \frac{p/(1-p)}{q/(1-q)}$，其中$p$和$q$是兩種條件下的概率
  - 卡方檢驗：$\chi^2 = \sum \frac{(O-E)^2}{E}$，其中$O$是觀測頻率，$E$是期望頻率
  - MIC值：$MIC(X,Y) = \max_{xy < B(n)} \frac{I(X,Y)}{log(min(x,y))}$，其中$I$是互信息

- **p值轉換**：將不同類型的統計量轉換為p值，便於比較
  - 用於t檢驗：$p = 2 \times P(T > |t|)$，其中$T$是自由度為$n-2$的t分布
  - 用於F檢驗：$p = P(F > F_{obs})$，其中$F$是自由度為$(k-1, n-k)$的F分布

- **假發現率(FDR)控制**：調整p值以減少假陽性
  $$p_i^* = min\left\{\frac{m \cdot p_i}{i}, 1\right\}$$
  其中$p_i$是排序後的第$i$個p值，$m$是總檢驗數量

### 5.3 包裝方法

- **貪婪搜索**：
  - 遞迴特徵消除(RFE)：基於重要性排名逐步移除特徵
  - 逐步選擇：基於模型性能逐步添加或移除特徵
    - 前向選擇：$Score(X_j | X_S) = Score(X_S \cup X_j) - Score(X_S)$
    - 後向選擇：$Score(X_j | X_S) = Score(X_S) - Score(X_S \setminus X_j)$
  - AIC指導選擇：
    $$AIC = -2\ell + 2p$$
    其中$\ell$是對數似然，$p$是參數數量

- **全局搜索**：
  - 模擬退火：接受概率
    $$Pr[accept] = \exp\left[-ic\left(\frac{old-new}{old}\right)\right]$$
  - 遺傳演算法：通過交叉和突變操作搜索最佳特徵子集
  - 多目標優化：使用期望函數
    $$D = \left(\prod_{i=1}^q d_j\right)^{1/q}$$
  
  - 期望函數（最大化數量）：
    $$d_i = \begin{cases} 
    0 & \text{if } x < A \\
    \left(\frac{x-A}{B-A}\right)^s & \text{if } A \leq x \leq B \\
    1 & \text{if } x > B
    \end{cases}$$

  - 期望函數（最小化數量）：
    $$d_i = \begin{cases}
    0 & \text{if } x > B \\
    \left(\frac{x-B}{A-B}\right)^s & \text{if } A \leq x \leq B \\
    1 & \text{if } x < A
    \end{cases}$$

## 6. 剖面數據處理方法

- **基線校正**：使用多項式擬合光譜去除背景干擾
  $$x_{ij}^* = x_{ij} - \sum_{k=0}^d \beta_k j^k$$
  其中$\beta_k$是通過多項式擬合得到的係數

- **標準正態變量(SNV)**：標準化處理使每個剖面均值為0、標準差為1
  $$x_{ij}^* = \frac{x_{ij} - \bar{x}_i}{s_i}$$
  其中$\bar{x}_i$和$s_i$分別是第$i$個剖面的均值和標準差

- **平滑處理**：減少測量噪聲
  $$x_{ij}^* = \sum_{k=-m}^m w_k x_{i,j+k}$$
  其中$w_k$是平滑係數

- **一階微分**：計算連續測量間的變化率，降低相關性
  $$x_{ij}^* = x_{i,j+1} - x_{ij}$$

## 7. 評估指標

- **回歸指標**：
  - 均方根誤差(RMSE)：
    $$RMSE = \sqrt{\frac{1}{n}\sum_{i=1}^n (y_i - \hat{y}_i)^2}$$
  
  - 決定係數($R^2$)：
    $$R^2 = 1 - \frac{\sum_{i=1}^n (y_i - \hat{y}_i)^2}{\sum_{i=1}^n (y_i - \bar{y})^2}$$
  
  - 一致相關係數：
    $$\rho_c = \frac{2\rho\sigma_y\sigma_{\hat{y}}}{\sigma_y^2 + \sigma_{\hat{y}}^2 + (\mu_y - \mu_{\hat{y}})^2}$$
    其中$\rho$是相關係數，$\sigma$是標準差，$\mu$是均值

- **分類指標**：
  - 準確率、敏感度(召回率)、特異度、精確度：
    $$\text{準確率} = \frac{TP + TN}{TP + FP + TN + FN}$$
    $$\text{敏感度} = \frac{TP}{TP + FN}$$
    $$\text{特異度} = \frac{TN}{TN + FP}$$
    $$\text{精確度} = \frac{TP}{TP + FP}$$
    其中$TP$是真陽性，$TN$是真陰性，$FP$是假陽性，$FN$是假陰性
  
  - 科恩卡帕係數：
    $$\kappa = \frac{p_o - p_e}{1 - p_e}$$
    其中$p_o$是觀測一致性，$p_e$是期望一致性
  
  - ROC曲線下面積(AUC)：
    $$AUC = \int_0^1 ROC(t)dt = P(X_1 > X_0)$$
    其中$X_1$是正例的預測機率，$X_0$是負例的預測機率
  
  - 對數似然：
    $$LL = \sum_{i=1}^n [y_i \log(\hat{p}_i) + (1-y_i) \log(1-\hat{p}_i)]$$
    其中$\hat{p}_i$是預測的正例機率
  
  - 基尼準則：
    $$Gini = 2 \times AUC - 1 = 1 - \sum_{i=1}^C p_i^2$$
    其中$p_i$是第$i$個類別的比例
  
  - 熵：
    $$Entropy = -\sum_{i=1}^C p_i \log(p_i)$$
    其中$p_i$是第$i$個類別的比例