# 時間序列分析詞彙詳解

## 引言 (Introduction)

### 五個重要的現實問題 (Five Important Real-world Problems)

**時間序列 (Time Series)**：按時間順序排列的數據點序列，如股價、氣溫、銷售量等隨時間變化的數據。

**預測 (Forecasting)**：基於歷史數據和統計模型來預測未來值的過程，是時間序列分析的主要目標之一。

**趨勢 (Trend)**：數據在長期內的上升或下降方向，反映了系統的整體發展方向。

**季節性 (Seasonality)**：數據在固定時間間隔內重複出現的規律性變化，如季節、月份或週期性波動。

**噪音 (Noise)**：數據中的隨機波動部分，無法用確定性模型解釋的不規則變化。

### 隨機性和確定性的動態數學模型 (Stochastic and Deterministic Dynamic Mathematical Models)

**隨機過程 (Stochastic Process)**：隨時間變化的隨機變量序列，每個時點的值都具有不確定性。

**確定性模型 (Deterministic Model)**：給定初始條件和輸入，輸出完全可預測的數學模型。

**動態系統 (Dynamic System)**：系統的當前狀態會影響未來狀態的系統，強調時間依賴性。

**狀態空間 (State Space)**：描述系統所有可能狀態的數學框架，常用於複雜系統建模。

### 建模的基本思想 (Basic Ideas of Modeling)

**模型選擇 (Model Selection)**：從多個候選模型中選擇最適合數據的模型的過程。

**參數估計 (Parameter Estimation)**：利用觀測數據估計模型中未知參數值的統計方法。

**模型診斷 (Model Diagnostics)**：檢驗模型是否適合數據的各種統計測試方法。

**殘差分析 (Residual Analysis)**：分析實際值與模型預測值之間差異的方法，用於評估模型質量。

## 平穩過程的自相關函數和譜 (Autocorrelation Function and Spectrum of Stationary Processes)

### 平穩模型的自相關性質 (Autocorrelation Properties of Stationary Models)

**平穩性 (Stationarity)**：時間序列的統計性質（均值、方差）不隨時間變化的特性。

**弱平穩 (Weak Stationarity)**：要求序列的均值和方差恆定，且協方差僅依賴於時間間隔。

**自相關函數 (Autocorrelation Function, ACF)**：衡量序列與其滯後版本之間線性關係強度的函數。

**偏自相關函數 (Partial Autocorrelation Function, PACF)**：在控制中間變量影響下，衡量兩個時點直接相關程度的函數。

**自協方差 (Autocovariance)**：序列與其滯後版本之間的協方差，反映內部相關性。

**滯後 (Lag)**：時間序列中兩個觀測值之間的時間間隔。

### 平穩模型的頻譜特性 (Spectral Properties of Stationary Models)

**頻譜密度 (Spectral Density)**：描述時間序列在不同頻率上功率分布的函數。

**功率譜 (Power Spectrum)**：信號功率在頻域上的分布，用於分析週期性成分。

**週期圖 (Periodogram)**：估計頻譜密度的基本方法，基於傅立葉變換。

**頻域分析 (Frequency Domain Analysis)**：在頻率領域而非時間領域分析信號特性的方法。

### 樣本譜和自相關函數估計之間的聯繫 (Relationship between Sample Spectrum and Autocorrelation Function Estimation)

**樣本自相關函數 (Sample Autocorrelation Function)**：基於有限樣本估計的自相關函數。

**譜估計 (Spectral Estimation)**：從有限數據估計真實頻譜密度的方法。

**快速傅立葉變換 (Fast Fourier Transform, FFT)**：高效計算離散傅立葉變換的算法。

## 線性平穩模型 (Linear Stationary Models)

### 一般線性過程 (General Linear Process)

**線性濾波器 (Linear Filter)**：輸出是輸入加權線性組合的系統。

**白噪音 (White Noise)**：均值為零、方差恆定且不相關的隨機序列。

**線性組合 (Linear Combination)**：多個變量的加權求和。

### 自回歸過程 (Autoregressive Process)

**自回歸模型 (Autoregressive Model, AR)**：當前值依賴於過去若干期值的線性模型。

**AR(p)模型 (AR(p) Model)**：依賴於過去p期值的自回歸模型。

**特徵方程 (Characteristic Equation)**：用於判斷AR模型穩定性的代數方程。

**平穩條件 (Stationarity Condition)**：AR模型保持平穩所需滿足的數學條件。

### 移動平均過程 (Moving Average Process)

**移動平均模型 (Moving Average Model, MA)**：當前值依賴於當前和過去若干期隨機擾動的模型。

**MA(q)模型 (MA(q) Model)**：依賴於當前和過去q期隨機擾動的移動平均模型。

**可逆性 (Invertibility)**：MA模型能夠表示為無限階AR模型的特性。

### 自回歸移動平均混合過程 (Autoregressive Moving Average Mixed Process)

**ARMA模型 (ARMA Model)**：結合自回歸和移動平均成分的模型。

**ARMA(p,q)模型 (ARMA(p,q) Model)**：包含p階自回歸和q階移動平均的混合模型。

**因式分解 (Factorization)**：將多項式分解為更簡單因子的數學技術。

### 附錄 (Appendix)

**Durbin-Levinson算法 (Durbin-Levinson Algorithm)**：遞回計算自回歸參數的高效算法。

**Yule-Walker方程 (Yule-Walker Equations)**：連接自相關函數和AR參數的線性方程組。

## 線性非平穩模型 (Linear Non-stationary Models)

### 求和自回歸移動平均過程 (Autoregressive Integrated Moving Average Process)

**ARIMA模型 (ARIMA Model)**：通過差分使非平穩序列變平穩的ARMA模型擴展。

**單位根 (Unit Root)**：使時間序列非平穩的特徵根，其絕對值等於1。

**差分 (Differencing)**：通過計算相鄰觀測值之差來消除趨勢的方法。

**積分階數 (Order of Integration)**：序列需要差分多少次才能變為平穩。

### 求和自回歸移動平均模型的三種表現形式 (Three Forms of ARIMA Models)

**差分形式 (Difference Form)**：使用差分運算子表示的ARIMA模型形式。

**後移運算子 (Backshift Operator)**：將序列向後移動一期的數學運算子。

**逆轉運算子 (Inverse Operator)**：後移運算子的逆運算。

### 求和移動平均過程 (Integrated Moving Average Process)

**IMA模型 (IMA Model)**：積分移動平均模型，MA模型的非平穩版本。

**隨機遊走 (Random Walk)**：每步變化為隨機的非平穩過程。

**趨勢平穩 (Trend Stationary)**：去除確定性趨勢後變為平穩的序列。

### 線性差分方程 (Linear Difference Equations)

**齊次方程 (Homogeneous Equation)**：不含常數項的差分方程。

**非齊次方程 (Non-homogeneous Equation)**：含有外生項的差分方程。

**特解 (Particular Solution)**：非齊次方程的一個特定解。

### 具有確定性偏差的IMA(0,1,1)過程 (IMA(0,1,1) Process with Deterministic Drift)

**漂移項 (Drift Term)**：序列的確定性線性趨勢成分。

**確定性成分 (Deterministic Component)**：可以用確定函數描述的序列成分。

### 帶有附加噪音的ARIMA過程 (ARIMA Process with Additional Noise)

**測量誤差 (Measurement Error)**：觀測過程中產生的隨機誤差。

**觀測噪音 (Observation Noise)**：疊加在真實信號上的隨機擾動。

## 預測 (Forecasting)

### 最小均方差預測及其性質 (Minimum Mean Square Error Prediction and Its Properties)

**均方誤差 (Mean Square Error, MSE)**：預測誤差平方的期望值，衡量預測精度。

**最優預測 (Optimal Prediction)**：在某種準則下最好的預測方法。

**預測誤差 (Prediction Error)**：實際值與預測值之間的差異。

**預測區間 (Prediction Interval)**：包含未來真實值的概率區間。

### 預測的計算和修正 (Calculation and Updating of Forecasts)

**遞推預測 (Recursive Forecasting)**：利用新信息逐步更新預測的方法。

**預測更新 (Forecast Updating)**：當獲得新觀測值時修正預測的過程。

**Kalman濾波 (Kalman Filter)**：動態系統的最優遞推估計算法。

### 預測函數和預測權 (Prediction Function and Prediction Weights)

**預測權重 (Prediction Weights)**：線性預測中各歷史觀測值的權重係數。

**線性預測器 (Linear Predictor)**：歷史觀測值線性組合形式的預測器。

### 狀態空間模型公式用於精確預測 (State Space Model Formulation for Exact Prediction)

**狀態向量 (State Vector)**：包含系統所有相關信息的向量。

**觀測方程 (Observation Equation)**：連接觀測值和狀態的方程。

**狀態方程 (State Equation)**：描述狀態演化的動態方程。

## 模型識別 (Model Identification)

### 識別的目的 (Purpose of Identification)

**模型階數 (Model Order)**：模型中包含的滯後項數量。

**參數識別 (Parameter Identification)**：確定模型中參數值的過程。

### 識別技巧 (Identification Techniques)

**Box-Jenkins方法 (Box-Jenkins Method)**：識別ARIMA模型的經典三步驟方法。

**信息準則 (Information Criteria)**：平衡模型擬合度和複雜度的選擇準則。

**AIC準則 (Akaike Information Criterion)**：赤池信息準則，常用的模型選擇標準。

**BIC準則 (Bayesian Information Criterion)**：貝葉斯信息準則，對複雜模型懲罰更重。

### 參數的初估計 (Initial Parameter Estimation)

**矩估計 (Method of Moments)**：基於樣本矩等於總體矩的估計方法。

**最小二乘法 (Least Squares Method)**：最小化殘差平方和的參數估計方法。

### 模型的多重性 (Model Multiplicity)

**模型選擇 (Model Selection)**：在多個模型中選擇最佳模型的過程。

**過度參數化 (Over-parameterization)**：模型包含過多不必要參數的問題。

## 模型的估計 (Model Estimation)

### 似然函數和平方和函數的研究 (Study of Likelihood Function and Sum of Squares Function)

**最大似然估計 (Maximum Likelihood Estimation, MLE)**：最大化似然函數的參數估計方法。

**對數似然函數 (Log-likelihood Function)**：似然函數的對數形式，便於計算和優化。

**殘差平方和 (Residual Sum of Squares)**：所有殘差平方的總和。

### 非線性估計 (Nonlinear Estimation)

**牛頓-拉夫遜方法 (Newton-Raphson Method)**：求解非線性方程的迭代算法。

**梯度下降法 (Gradient Descent)**：通過梯度方向尋找最優解的算法。

**Gauss-Newton方法 (Gauss-Newton Method)**：解決非線性最小二乘問題的算法。

### ARIMA模型中的單位根 (Unit Roots in ARIMA Models)

**單位根檢驗 (Unit Root Test)**：檢驗序列是否存在單位根的統計測試。

**ADF檢驗 (Augmented Dickey-Fuller Test)**：增廣迪基-富勒檢驗，常用的單位根檢驗。

**協整 (Cointegration)**：多個非平穩序列之間存在長期穩定關係。

### 使用Bayes原理的估計 (Estimation Using Bayesian Principles)

**貝葉斯估計 (Bayesian Estimation)**：結合先驗信息和樣本信息的估計方法。

**先驗分布 (Prior Distribution)**：參數的先驗概率分布。

**後驗分布 (Posterior Distribution)**：結合數據後的參數概率分布。

### 自回歸模型估計量的漸進分布 (Asymptotic Distribution of AR Model Estimators)

**漸近正態性 (Asymptotic Normality)**：估計量在大樣本下趨於正態分布。

**一致性 (Consistency)**：估計量隨樣本增大收斂到真值的性質。

## 模型的診斷檢驗 (Model Diagnostic Testing)

### 隨機模型的檢驗 (Testing of Stochastic Models)

**殘差檢驗 (Residual Testing)**：通過分析殘差來檢驗模型適合度。

**白噪音檢驗 (White Noise Test)**：檢驗殘差是否為白噪音的測試。

**Ljung-Box檢驗 (Ljung-Box Test)**：檢驗殘差自相關的統計測試。

### 應用於殘差的診斷檢驗 (Diagnostic Tests Applied to Residuals)

**正態性檢驗 (Normality Test)**：檢驗殘差是否服從正態分布。

**異方差檢驗 (Heteroscedasticity Test)**：檢驗殘差方差是否恆定。

**自相關檢驗 (Autocorrelation Test)**：檢驗殘差是否存在自相關。

### 利用殘差修正模型 (Model Modification Using Residuals)

**模型改進 (Model Improvement)**：基於診斷結果改進模型的過程。

**過度擬合 (Overfitting)**：模型過於複雜導致泛化能力差的問題。

## 季節模型 (Seasonal Models)

### 季節時間序列的簡約模型 (Parsimonious Models for Seasonal Time Series)

**季節性ARIMA (Seasonal ARIMA, SARIMA)**：處理季節性數據的ARIMA模型擴展。

**季節差分 (Seasonal Differencing)**：對季節間隔的觀測值進行差分。

**季節週期 (Seasonal Period)**：季節性模式重複的時間間隔。

### 更一般的季節模型ARIMA的某些方面 (Some Aspects of More General Seasonal ARIMA Models)

**乘積季節模型 (Multiplicative Seasonal Model)**：季節和非季節成分相互作用的模型。

**季節單位根 (Seasonal Unit Root)**：在季節頻率上的單位根。

### 結構分量模型和確定性季節分量 (Structural Component Models and Deterministic Seasonal Components)

**分解模型 (Decomposition Model)**：將序列分解為不同成分的模型。

**趨勢分量 (Trend Component)**：序列的長期趨勢部分。

**季節分量 (Seasonal Component)**：規律性季節變化部分。

**不規則分量 (Irregular Component)**：隨機波動部分。

### 帶有時間序列誤差項的回歸模型 (Regression Models with Time Series Error Terms)

**回歸模型 (Regression Model)**：解釋變量與響應變量關係的統計模型。

**時間序列回歸 (Time Series Regression)**：誤差項具有時間相關性的回歸模型。

**虛假回歸 (Spurious Regression)**：非平穩變量間的偽相關關係。

## 非線性和長記憶模型 (Nonlinear and Long Memory Models)

### 自回歸條件異方差(ARCH)模型 (Autoregressive Conditional Heteroscedasticity (ARCH) Models)

**ARCH模型 (ARCH Model)**：條件方差依賴於過去殘差平方的模型。

**GARCH模型 (GARCH Model)**：廣義ARCH模型，條件方差也依賴於過去條件方差。

**條件方差 (Conditional Variance)**：給定過去信息的條件下的方差。

**波動性聚集 (Volatility Clustering)**：高波動期和低波動期聚集出現的現象。

### 非線性時間序列模型 (Nonlinear Time Series Models)

**閾值模型 (Threshold Model)**：在不同區間採用不同線性模型的分段模型。

**平滑轉換模型 (Smooth Transition Model)**：模式間平滑轉換的非線性模型。

**神經網絡模型 (Neural Network Model)**：基於人工神經網絡的非線性時間序列模型。

### 長記憶時間序列過程 (Long Memory Time Series Processes)

**長記憶 (Long Memory)**：自相關函數緩慢衰減的時間序列特性。

**分數差分 (Fractional Differencing)**：使用非整數階差分的方法。

**Hurst指數 (Hurst Exponent)**：衡量長記憶強度的參數。

**ARFIMA模型 (ARFIMA Model)**：自回歸分數積分移動平均模型。

## 傳遞函數模型 (Transfer Function Models)

### 線性傳遞函數模型 (Linear Transfer Function Models)

**傳遞函數 (Transfer Function)**：描述輸入如何影響輸出的數學函數。

**輸入序列 (Input Series)**：影響系統的外生變量序列。

**輸出序列 (Output Series)**：系統響應的被解釋變量序列。

**脈衝響應函數 (Impulse Response Function)**：單位脈衝輸入引起的輸出響應。

### 用差分方程表示的離散動態模型 (Discrete Dynamic Models Represented by Difference Equations)

**差分方程 (Difference Equations)**：描述離散時間動態系統的方程。

**動態回歸 (Dynamic Regression)**：包含滯後變量的回歸模型。

### 離散模型和連續模型的關係 (Relationship between Discrete and Continuous Models)

**連續時間模型 (Continuous Time Model)**：時間連續的動態模型。

**離散化 (Discretization)**：將連續時間模型轉換為離散時間模型。

### 具有脈衝式輸入的連續模型 (Continuous Models with Impulse Inputs)

**脈衝響應 (Impulse Response)**：系統對脈衝輸入的響應。

**階躍響應 (Step Response)**：系統對階躍輸入的響應。

### 非線性傳遞函數與線性化 (Nonlinear Transfer Functions and Linearization)

**線性化 (Linearization)**：將非線性系統近似為線性系統的方法。

**泰勒展開 (Taylor Expansion)**：函數的多項式近似展開。

## 傳遞函數模型的識別、擬合及檢驗 (Identification, Fitting and Testing of Transfer Function Models)

### 互相關函數 (Cross-correlation Function)

**互相關函數 (Cross-correlation Function, CCF)**：衡量兩個序列間相關關係的函數。

**互協方差 (Cross-covariance)**：兩個序列間的協方差函數。

**領先指標 (Leading Indicator)**：提前反映其他變量變化的指標。

**滯後指標 (Lagging Indicator)**：延後反映其他變量變化的指標。

### 傳遞函數模型的識別 (Identification of Transfer Function Models)

**前白化 (Prewhitening)**：消除輸入序列自相關的預處理步驟。

**互相關分析 (Cross-correlation Analysis)**：通過互相關函數識別傳遞函數結構。

### 互譜分析用於傳遞函數模型的識別 (Cross-spectral Analysis for Transfer Function Model Identification)

**互譜 (Cross-spectrum)**：兩個序列在頻域的相關關係。

**相干性 (Coherence)**：衡量兩序列在特定頻率相關程度的指標。

**相位譜 (Phase Spectrum)**：描述兩序列間相位關係的頻域函數。

## 干預分析模型和異常值檢測 (Intervention Analysis Models and Outlier Detection)

### 干預分析方法 (Intervention Analysis Methods)

**干預分析 (Intervention Analysis)**：分析外部事件對時間序列影響的方法。

**結構變化 (Structural Change)**：系統參數在某時點發生的顯著變化。

**虛擬變量 (Dummy Variable)**：表示定性因素的二元變量。

**政策評估 (Policy Evaluation)**：評估政策實施效果的統計方法。

### 時間序列的異常值分析 (Outlier Analysis in Time Series)

**異常值 (Outlier)**：與正常模式顯著偏離的觀測值。

**創新異常值 (Innovation Outlier)**：影響當期及後續所有時期的異常值。

**附加異常值 (Additive Outlier)**：僅影響特定時期的異常值。

**異常值檢測 (Outlier Detection)**：識別和處理異常觀測值的方法。

### 對存在缺失值ARMA模型的估計 (Estimation of ARMA Models with Missing Values)

**缺失值 (Missing Values)**：數據集中缺失的觀測值。

**EM算法 (EM Algorithm)**：處理缺失數據的期望最大化算法。

**插值 (Interpolation)**：估計缺失值的數值方法。

## 多元時間序列分析 (Multivariate Time Series Analysis)

### 平穩多元時間序列模型 (Stationary Multivariate Time Series Models)

**向量時間序列 (Vector Time Series)**：多個相關時間序列組成的向量。

**向量自回歸模型 (Vector Autoregressive Model, VAR)**：多元時間序列的自回歸模型。

**向量移動平均模型 (Vector Moving Average Model, VMA)**：多元時間序列的移動平均模型。

### 平穩多元過程的線性模型表達式 (Linear Model Representations of Stationary Multivariate Processes)

**向量ARMA模型 (Vector ARMA Model, VARMA)**：多元ARMA模型。

**Wold分解 (Wold Decomposition)**：將平穩過程分解為確定性和隨機成分。

### 非平穩向量自回歸移動平均模型 (Non-stationary Vector Autoregressive Moving Average Models)

**向量誤差修正模型 (Vector Error Correction Model, VECM)**：包含協整關係的VAR模型。

**協整向量 (Cointegrating Vector)**：使非平穩變量線性組合變平穩的係數向量。

**Granger因果關係 (Granger Causality)**：一個變量是否有助於預測另一個變量。

### 對向量自回歸移動平均過程的預測 (Forecasting Vector ARMA Processes)

**多元預測 (Multivariate Forecasting)**：同時預測多個相關序列。

**預測誤差方差矩陣 (Forecast Error Variance Matrix)**：多元預測誤差的協方差矩陣。

### 向量ARMA模型的統計分析 (Statistical Analysis of Vector ARMA Models)

**似然比檢驗 (Likelihood Ratio Test)**：比較嵌套模型的統計檢驗。

**Wald檢驗 (Wald Test)**：檢驗參數約束的統計方法。

## 過程控制的各個方面 (Various Aspects of Process Control)

### 過程監測和過程調整 (Process Monitoring and Process Adjustment)

**統計過程控制 (Statistical Process Control, SPC)**：使用統計方法監控過程穩定性。

**控制圖 (Control Chart)**：監測過程變異的統計圖表工具。

**過程能力 (Process Capability)**：過程滿足規格要求的能力。

### 使用反饋控制的過程調整 (Process Adjustment Using Feedback Control)

**反饋控制 (Feedback Control)**：基於輸出偏差調整輸入的控制方法。

**控制器設計 (Controller Design)**：設計控制系統參數的工程方法。

**PID控制器 (PID Controller)**：比例-積分-微分控制器。

### MMSE控制有時所需的過度調整 (Over-adjustment Sometimes Required for MMSE Control)

**最小均方誤差控制 (MMSE Control)**：最小化均方誤差的控制策略。

**過度調整 (Over-adjustment)**：為達到最優控制而採用的激進調整。

### 對於具有固定調整和監測代價的最小代價控制 (Minimum Cost Control with Fixed Adjustment and Monitoring Costs)

**成本函數 (Cost Function)**：量化控制成本的目標函數。

**最優控制 (Optimal Control)**：最小化成本函數的控制策略。

### 前饋控制 (Feedforward Control)

**前饋控制 (Feedforward Control)**：基於干擾預測進行預防性調整的控制方法。

**預測控制 (Predictive Control)**：基於模型預測進行控制的方法。

### 調整方案有約束的反饋控制 (Feedback Control with Constrained Adjustment Schemes)

**約束優化 (Constrained Optimization)**：在約束條件下的最優化問題。

**線性規劃 (Linear Programming)**：目標函數和約束都是線性的優化問題。

### 抽樣間隔的選擇 (Selection of Sampling Intervals)

**抽樣頻率 (Sampling Frequency)**：單位時間內的採樣次數。

**Nyquist頻率 (Nyquist Frequency)**：避免混疊所需的最小採樣頻率。

**混疊 (Aliasing)**：由於採樣頻率不足導致的頻率混淆現象。