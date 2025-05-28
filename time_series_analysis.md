# 時間序列分析相關詞彙

## 引言 (Introduction)

### 五個重要的現實問題 (Five Important Real-world Problems)

**時間序列 (Time Series)**：按時間順序排列的數據點序列，如股價、氣溫、銷售量等隨時間變化的數據。
$$X_t, t = 1, 2, 3, ..., n$$
可用python套件: pandas, numpy, matplotlib, seaborn
```python
# pandas, numpy, matplotlib
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# 建立時間序列
dates = pd.date_range('2023-01-01', periods=100, freq='D')
ts = pd.Series(np.random.randn(100), index=dates)

# 繪製時間序列
ts.plot()
plt.show()
```



**預測 (Forecasting)**：基於歷史數據和統計模型來預測未來值的過程，是時間序列分析的主要目標之一。
$$\hat{X}_{t+h} = E[X_{t+h}|X_t, X_{t-1}, ..., X_1]$$
可用python套件: statsmodels, prophet, pmdarima, sktime
```python
from statsmodels.tsa.arima.model import ARIMA
from prophet import Prophet

# ARIMA預測
model = ARIMA(ts, order=(1,1,1))
fitted = model.fit()
forecast = fitted.forecast(steps=10)

# Prophet預測
df = pd.DataFrame({'ds': dates, 'y': ts.values})
m = Prophet()
m.fit(df)
future = m.make_future_dataframe(periods=10)
forecast = m.predict(future)
```

**趨勢 (Trend)**：數據在長期內的上升或下降方向，反映了系統的整體發展方向。
$$T_t = \alpha + \beta t + \epsilon_t$$
可用python套件: statsmodels, scipy, pytrends
```python
from statsmodels.tsa.seasonal import seasonal_decompose
import scipy.signal

# 分解趨勢
decomposition = seasonal_decompose(ts, model='additive', period=7)
trend = decomposition.trend

# 去趨勢
detrended = scipy.signal.detrend(ts)
```

**季節性 (Seasonality)**：數據在固定時間間隔內重複出現的規律性變化，如季節、月份或週期性波動。
$$S_t = S_{t-s}$$
可用python套件: seasonal, statsmodels, x13arima-select
```python
from statsmodels.tsa.seasonal import STL

# STL分解
stl = STL(ts, seasonal=13)
res = stl.fit()
seasonal = res.seasonal
```

**噪音 (Noise)**：數據中的隨機波動部分，無法用確定性模型解釋的不規則變化。
$$\epsilon_t \sim WN(0, \sigma^2)$$
可用python套件: scipy, numpy
```python
# 生成白噪音
white_noise = np.random.normal(0, 1, 100)

# 從信號中提取噪音
noise = ts - trend - seasonal  # 假設已有trend和seasonal
```

### 隨機性和確定性的動態數學模型 (Stochastic and Deterministic Dynamic Mathematical Models)

**隨機過程 (Stochastic Process)**：隨時間變化的隨機變量序列，每個時點的值都具有不確定性。
$$\{X_t, t \in T\}$$
可用python套件: stochastic, simpy
```python
import stochastic

# 建立布朗運動
brownian = stochastic.processes.continuous.BrownianMotion(t=1, rng=None)
s = brownian.sample(10000)

# 建立泊松過程
poisson = stochastic.processes.continuous.PoissonProcess(rate=1)
s = poisson.sample(10000)
```

**確定性模型 (Deterministic Model)**：給定初始條件和輸入，輸出完全可預測的數學模型。
$$X_t = f(X_{t-1}, X_{t-2}, ..., u_t)$$
可用python套件: scipy, sympy

**動態系統 (Dynamic System)**：系統的當前狀態會影響未來狀態的系統，強調時間依賴性。
$$X_t = F(X_{t-1}) + G(u_t)$$
可用python套件: dynaconf, control

**狀態空間 (State Space)**：描述系統所有可能狀態的數學框架，常用於複雜系統建模。
$$\begin{cases}
X_t = A X_{t-1} + B u_t + w_t \\
Y_t = C X_t + v_t
\end{cases}$$
可用python套件: statsmodels, filterpy, pykalman
```python
from statsmodels.tsa.statespace.sarimax import SARIMAX
from pykalman import KalmanFilter

# 狀態空間模型
model = SARIMAX(ts, order=(1,0,1))
results = model.fit()

# Kalman濾波器
kf = KalmanFilter(initial_state_mean=0, n_dim_obs=1)
filtered_state_means, filtered_state_covariances = kf.filter(ts.values)
```

### 建模的基本思想 (Basic Ideas of Modeling)

**模型選擇 (Model Selection)**：從多個候選模型中選擇最適合數據的模型的過程。
可用python套件: scikit-learn, statsmodels
```python
from pmdarima import auto_arima
from sklearn.model_selection import TimeSeriesSplit

# 自動ARIMA選擇
auto_model = auto_arima(ts, seasonal=True, m=12, 
                       stepwise=True, suppress_warnings=True)

# 時間序列交叉驗證
tscv = TimeSeriesSplit(n_splits=5)
for train_idx, test_idx in tscv.split(ts):
    train, test = ts[train_idx], ts[test_idx]
    
```

**參數估計 (Parameter Estimation)**：利用觀測數據估計模型中未知參數值的統計方法。
$$\hat{\theta} = \arg\max_{\theta} L(\theta|X_1, ..., X_n)$$
可用python套件: scipy, lmfit
```python
from scipy.optimize import minimize
from lmfit import Model, Parameters

# 使用scipy優化
def objective(params):
    return np.sum((ts - model_function(params))**2)
    
result = minimize(objective, initial_params)

# 使用lmfit
def model_func(x, a, b):
    return a * x + b
    
model = Model(model_func)
result = model.fit(ts.values, x=np.arange(len(ts)), a=1, b=0)
```

**模型診斷 (Model Diagnostics)**：檢驗模型是否適合數據的各種統計測試方法。
可用python套件: statsmodels, arch

**殘差分析 (Residual Analysis)**：分析實際值與模型預測值之間差異的方法，用於評估模型質量。
$$e_t = X_t - \hat{X}_t$$
可用python套件: statsmodels, scipy
```python
from statsmodels.stats.diagnostic import acorr_ljungbox

# 計算殘差
residuals = fitted.resid

# Ljung-Box檢驗
lb_test = acorr_ljungbox(residuals, lags=10)

# 殘差圖
fig, axes = plt.subplots(2, 1)
residuals.plot(ax=axes[0])
residuals.plot(kind='kde', ax=axes[1])
```

## 平穩過程的自相關函數和譜 (Autocorrelation Function and Spectrum of Stationary Processes)

### 平穩模型的自相關性質 (Autocorrelation Properties of Stationary Models)

**平穩性 (Stationarity)**：時間序列的統計性質（均值、方差）不隨時間變化的特性。
$$E[X_t] = \mu, \quad Var(X_t) = \sigma^2, \quad \forall t$$
可用python套件: statsmodels, arch
```python
from statsmodels.tsa.stattools import adfuller, kpss

# ADF檢驗
adf_result = adfuller(ts)
print(f'ADF統計量: {adf_result[0]}')
print(f'p-value: {adf_result[1]}')

# KPSS檢驗
kpss_result = kpss(ts)
print(f'KPSS統計量: {kpss_result[0]}')
print(f'p-value: {kpss_result[1]}')
```

**弱平穩 (Weak Stationarity)**：要求序列的均值和方差恆定，且協方差僅依賴於時間間隔。
$$Cov(X_t, X_{t+h}) = \gamma(h)$$

**自相關函數 (Autocorrelation Function, ACF)**：衡量序列與其滯後版本之間線性關係強度的函數。
$$\rho(h) = \frac{\gamma(h)}{\gamma(0)} = \frac{Cov(X_t, X_{t+h})}{Var(X_t)}$$
可用python套件: statsmodels, pandas

**偏自相關函數 (Partial Autocorrelation Function, PACF)**：在控制中間變量影響下，衡量兩個時點直接相關程度的函數。
$$\phi_{hh} = Corr(X_t, X_{t+h}|X_{t+1}, ..., X_{t+h-1})$$
可用python套件: statsmodels, pandas
```python
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf
from statsmodels.tsa.stattools import acf, pacf

# 計算ACF和PACF
acf_values = acf(ts, nlags=20)
pacf_values = pacf(ts, nlags=20)

# 繪製ACF和PACF圖
fig, axes = plt.subplots(2, 1)
plot_acf(ts, lags=20, ax=axes[0])
plot_pacf(ts, lags=20, ax=axes[1])
```

**自協方差 (Autocovariance)**：序列與其滯後版本之間的協方差，反映內部相關性。
$$\gamma(h) = Cov(X_t, X_{t+h}) = E[(X_t - \mu)(X_{t+h} - \mu)]$$

**滯後 (Lag)**：時間序列中兩個觀測值之間的時間間隔。

### 平穩模型的頻譜特性 (Spectral Properties of Stationary Models)

**頻譜密度 (Spectral Density)**：描述時間序列在不同頻率上功率分布的函數。
$$f(\omega) = \frac{1}{2\pi} \sum_{h=-\infty}^{\infty} \gamma(h) e^{-ih\omega}$$
可用python套件: scipy, spectrum
```python
from scipy import signal
import spectrum

# 使用scipy計算功率譜密度
frequencies, psd = signal.periodogram(ts)

# 使用spectrum套件
p = spectrum.Periodogram(ts.values)
p.run()
p.plot()
```

**功率譜 (Power Spectrum)**：信號功率在頻域上的分布，用於分析週期性成分。
可用python套件: scipy, matplotlib

**週期圖 (Periodogram)**：估計頻譜密度的基本方法，基於傅立葉變換。
$$I(\omega) = \frac{1}{2\pi n} \left|\sum_{t=1}^n X_t e^{-it\omega}\right|^2$$
可用python套件: scipy, astropy

**頻域分析 (Frequency Domain Analysis)**：在頻率領域而非時間領域分析信號特性的方法。

### 樣本譜和自相關函數估計之間的聯繫 (Relationship between Sample Spectrum and Autocorrelation Function Estimation)

**樣本自相關函數 (Sample Autocorrelation Function)**：基於有限樣本估計的自相關函數。
$$\hat{\rho}(h) = \frac{\sum_{t=1}^{n-h}(X_t - \bar{X})(X_{t+h} - \bar{X})}{\sum_{t=1}^n(X_t - \bar{X})^2}$$

**譜估計 (Spectral Estimation)**：從有限數據估計真實頻譜密度的方法。

**快速傅立葉變換 (Fast Fourier Transform, FFT)**：高效計算離散傅立葉變換的算法。
可用python套件: numpy, scipy
```python
# FFT分析
fft_values = np.fft.fft(ts.values)
fft_freq = np.fft.fftfreq(len(ts), d=1)

# 功率譜
power_spectrum = np.abs(fft_values)**2
```

## 線性平穩模型 (Linear Stationary Models)

### 一般線性過程 (General Linear Process)

**線性濾波器 (Linear Filter)**：輸出是輸入加權線性組合的系統。
$$X_t = \sum_{j=0}^{\infty} \psi_j \epsilon_{t-j}$$
可用python套件: scipy, numpy

**白噪音 (White Noise)**：均值為零、方差恆定且不相關的隨機序列。
$$\epsilon_t \sim WN(0, \sigma^2), \quad E[\epsilon_t \epsilon_s] = 0, \quad t \neq s$$
可用python套件: numpy, statsmodels

**線性組合 (Linear Combination)**：多個變量的加權求和。
$$Y = \sum_{i=1}^n a_i X_i$$

### 自回歸過程 (Autoregressive Process)

**自回歸模型 (Autoregressive Model, AR)**：當前值依賴於過去若干期值的線性模型。
```python
from statsmodels.tsa.ar_model import AutoReg

# 建立AR模型
ar_model = AutoReg(ts, lags=2)
ar_result = ar_model.fit()

# 預測
predictions = ar_result.predict(start=len(ts), end=len(ts)+10)

# 取得參數
print(ar_result.params)
```

**AR(p)模型 (AR(p) Model)**：依賴於過去p期值的自回歸模型。
$$X_t = c + \phi_1 X_{t-1} + \phi_2 X_{t-2} + ... + \phi_p X_{t-p} + \epsilon_t$$
可用python套件: statsmodels, pmdarima

**特徵方程 (Characteristic Equation)**：用於判斷AR模型穩定性的代數方程。
$$1 - \phi_1 z - \phi_2 z^2 - ... - \phi_p z^p = 0$$

**平穩條件 (Stationarity Condition)**：AR模型保持平穩所需滿足的數學條件。
所有特徵根的絕對值必須小於1

### 移動平均過程 (Moving Average Process)

**移動平均模型 (Moving Average Model, MA)**：當前值依賴於當前和過去若干期隨機擾動的模型。
```python
from statsmodels.tsa.arima.model import ARIMA

# 建立MA模型 (ARIMA(0,0,q))
ma_model = ARIMA(ts, order=(0,0,2))
ma_result = ma_model.fit()

# 取得MA係數
print(ma_result.maparams)
```

**MA(q)模型 (MA(q) Model)**：依賴於當前和過去q期隨機擾動的移動平均模型。
$$X_t = \mu + \epsilon_t + \theta_1 \epsilon_{t-1} + \theta_2 \epsilon_{t-2} + ... + \theta_q \epsilon_{t-q}$$
可用python套件: statsmodels, pmdarima

**可逆性 (Invertibility)**：MA模型能夠表示為無限階AR模型的特性。
$$1 + \theta_1 z + \theta_2 z^2 + ... + \theta_q z^q = 0$$
所有根的絕對值必須大於1

### 自回歸移動平均混合過程 (Autoregressive Moving Average Mixed Process)

**ARMA模型 (ARMA Model)**：結合自回歸和移動平均成分的模型。
可用python套件: statsmodels, pmdarima

**ARMA(p,q)模型 (ARMA(p,q) Model)**：包含p階自回歸和q階移動平均的混合模型。
$$X_t = c + \sum_{i=1}^p \phi_i X_{t-i} + \sum_{j=1}^q \theta_j \epsilon_{t-j} + \epsilon_t$$
```python
# 建立ARMA模型
arma_model = ARIMA(ts, order=(2,0,2))
arma_result = arma_model.fit()

# 模型摘要
print(arma_result.summary())

# 預測與信賴區間
forecast = arma_result.forecast(steps=10)
forecast_df = arma_result.get_forecast(steps=10).summary_frame()
```

**因式分解 (Factorization)**：將多項式分解為更簡單因子的數學技術。

### 附錄 (Appendix)

**Durbin-Levinson算法 (Durbin-Levinson Algorithm)**：遞回計算自回歸參數的高效算法。

**Yule-Walker方程 (Yule-Walker Equations)**：連接自相關函數和AR參數的線性方程組。
$$\rho(k) = \phi_1 \rho(k-1) + \phi_2 \rho(k-2) + ... + \phi_p \rho(k-p)$$
可用python套件: statsmodels, scipy
```python
from statsmodels.regression.linear_model import yule_walker

# 解Yule-Walker方程
ar_coeffs, sigma = yule_walker(ts, order=2)
print(f'AR係數: {ar_coeffs}')
print(f'雜訊方差: {sigma}')
```

## 線性非平穩模型 (Linear Non-stationary Models)

### 求和自回歸移動平均過程 (Autoregressive Integrated Moving Average Process)

**ARIMA模型 (ARIMA Model)**：通過差分使非平穩序列變平穩的ARMA模型擴展。
可用python套件: statsmodels, pmdarima, prophet
```python
from pmdarima.arima import ndiffs

# 檢驗需要差分的階數
d = ndiffs(ts, test='adf')
print(f'建議差分階數: {d}')

# 建立ARIMA模型
arima_model = ARIMA(ts, order=(1,1,1))
arima_result = arima_model.fit()

# 差分運算
diff_1 = ts.diff().dropna()
diff_2 = ts.diff().diff().dropna()
```

**單位根 (Unit Root)**：使時間序列非平穩的特徵根，其絕對值等於1。
可用python套件: statsmodels, arch
```python
from arch.unitroot import ADF, KPSS, PhillipsPerron

# 各種單位根檢驗
adf = ADF(ts)
print(adf.summary())

kpss = KPSS(ts)
print(kpss.summary())

pp = PhillipsPerron(ts)
print(pp.summary())
```

**差分 (Differencing)**：通過計算相鄰觀測值之差來消除趨勢的方法。
$$\nabla X_t = X_t - X_{t-1}$$
$$\nabla^d X_t = (1-B)^d X_t$$
可用python套件: pandas, numpy

**積分階數 (Order of Integration)**：序列需要差分多少次才能變為平穩。

### 求和自回歸移動平均模型的三種表現形式 (Three Forms of ARIMA Models)

**差分形式 (Difference Form)**：使用差分運算子表示的ARIMA模型形式。

**後移運算子 (Backshift Operator)**：將序列向後移動一期的數學運算子。
$$B X_t = X_{t-1}$$

**逆轉運算子 (Inverse Operator)**：後移運算子的逆運算。

### 求和移動平均過程 (Integrated Moving Average Process)

**IMA模型 (IMA Model)**：積分移動平均模型，MA模型的非平穩版本。

**隨機遊走 (Random Walk)**：每步變化為隨機的非平穩過程。
$$X_t = X_{t-1} + \epsilon_t$$
可用python套件: numpy, pandas

**趨勢平穩 (Trend Stationary)**：去除確定性趨勢後變為平穩的序列。

### 線性差分方程 (Linear Difference Equations)

**齊次方程 (Homogeneous Equation)**：不含常數項的差分方程。

**非齊次方程 (Non-homogeneous Equation)**：含有外生項的差分方程。

**特解 (Particular Solution)**：非齊次方程的一個特定解。

### 具有確定性偏差的IMA(0,1,1)過程 (IMA(0,1,1) Process with Deterministic Drift)

**漂移項 (Drift Term)**：序列的確定性線性趨勢成分。
$$X_t = \mu + X_{t-1} + \epsilon_t + \theta \epsilon_{t-1}$$

**確定性成分 (Deterministic Component)**：可以用確定函數描述的序列成分。

### 帶有附加噪音的ARIMA過程 (ARIMA Process with Additional Noise)

**測量誤差 (Measurement Error)**：觀測過程中產生的隨機誤差。

**觀測噪音 (Observation Noise)**：疊加在真實信號上的隨機擾動。

## 預測 (Forecasting)

### 最小均方差預測及其性質 (Minimum Mean Square Error Prediction and Its Properties)

**均方誤差 (Mean Square Error, MSE)**：預測誤差平方的期望值，衡量預測精度。
$$MSE = E[(X_{t+h} - \hat{X}_{t+h})^2]$$
```python
# 計算預測誤差
from sklearn.metrics import mean_squared_error

# 一步預測
forecast_1 = arima_result.forecast(steps=1)

# 多步預測
forecast_multi = arima_result.forecast(steps=10)

# 預測區間
forecast_df = arima_result.get_forecast(steps=10).summary_frame(alpha=0.05)
lower_bound = forecast_df['mean_ci_lower']
upper_bound = forecast_df['mean_ci_upper']
```

**最優預測 (Optimal Prediction)**：在某種準則下最好的預測方法。

**預測誤差 (Prediction Error)**：實際值與預測值之間的差異。
$$e_{t+h} = X_{t+h} - \hat{X}_{t+h}$$

**預測區間 (Prediction Interval)**：包含未來真實值的概率區間。
$$\hat{X}_{t+h} \pm z_{\alpha/2} \sqrt{Var(e_{t+h})}$$
可用python套件: statsmodels, scikit-learn

### 預測的計算和修正 (Calculation and Updating of Forecasts)

**遞推預測 (Recursive Forecasting)**：利用新信息逐步更新預測的方法。

**預測更新 (Forecast Updating)**：當獲得新觀測值時修正預測的過程。

**Kalman濾波 (Kalman Filter)**：動態系統的最優遞推估計算法。
$$\begin{cases}
\hat{x}_{t|t-1} = A \hat{x}_{t-1|t-1} \\
\hat{x}_{t|t} = \hat{x}_{t|t-1} + K_t (y_t - C \hat{x}_{t|t-1})
\end{cases}$$
可用python套件: pykalman, filterpy
```python
from pykalman import KalmanFilter
from filterpy.kalman import KalmanFilter as KF

# pykalman
kf = KalmanFilter(
    transition_matrices=[[1]],
    observation_matrices=[[1]],
    initial_state_mean=0,
    initial_state_covariance=1,
    observation_covariance=1,
    transition_covariance=0.1
)
state_means, state_covariances = kf.filter(ts.values)
```

### 預測函數和預測權 (Prediction Function and Prediction Weights)

**預測權重 (Prediction Weights)**：線性預測中各歷史觀測值的權重係數。
$$\hat{X}_{t+h} = \sum_{j=1}^n w_j X_{t+1-j}$$

**線性預測器 (Linear Predictor)**：歷史觀測值線性組合形式的預測器。

### 狀態空間模型公式用於精確預測 (State Space Model Formulation for Exact Prediction)
可用python套件: statsmodels, simdkalman

**狀態向量 (State Vector)**：包含系統所有相關信息的向量。

**觀測方程 (Observation Equation)**：連接觀測值和狀態的方程。
$$Y_t = C X_t + v_t$$

**狀態方程 (State Equation)**：描述狀態演化的動態方程。
$$X_t = A X_{t-1} + B u_t + w_t$$

## 模型識別 (Model Identification)

### 識別的目的 (Purpose of Identification)

**模型階數 (Model Order)**：模型中包含的滯後項數量。

**參數識別 (Parameter Identification)**：確定模型中參數值的過程。

### 識別技巧 (Identification Techniques)

**Box-Jenkins方法 (Box-Jenkins Method)**：識別ARIMA模型的經典三步驟方法。
可用python套件: statsmodels, pmdarima
```python
from pmdarima import auto_arima

# 自動識別最佳ARIMA階數
model = auto_arima(
    ts,
    start_p=0, start_q=0,
    max_p=5, max_q=5,
    seasonal=False,
    stepwise=True,
    suppress_warnings=True,
    information_criterion='aic',
    trace=True
)
```

**信息準則 (Information Criteria)**：平衡模型擬合度和複雜度的選擇準則。
```python
# 比較不同模型的AIC/BIC
models = []
for p in range(3):
    for q in range(3):
        try:
            model = ARIMA(ts, order=(p,1,q))
            fitted = model.fit()
            models.append({
                'order': (p,1,q),
                'aic': fitted.aic,
                'bic': fitted.bic
            })
        except:
            continue

# 找出最佳模型
best_aic = min(models, key=lambda x: x['aic'])
best_bic = min(models, key=lambda x: x['bic'])
```

**AIC準則 (Akaike Information Criterion)**：赤池信息準則，常用的模型選擇標準。
$$AIC = -2\ln(L) + 2k$$
可用python套件: statsmodels, scikit-learn

**BIC準則 (Bayesian Information Criterion)**：貝葉斯信息準則，對複雜模型懲罰更重。
$$BIC = -2\ln(L) + k\ln(n)$$
可用python套件: statsmodels, scikit-learn

### 參數的初估計 (Initial Parameter Estimation)

**矩估計 (Method of Moments)**：基於樣本矩等於總體矩的估計方法。
可用python套件: scipy, statsmodels

**最小二乘法 (Least Squares Method)**：最小化殘差平方和的參數估計方法。
$$\hat{\theta} = \arg\min_{\theta} \sum_{t=1}^n (X_t - f(X_{t-1}, ..., \theta))^2$$
可用python套件: numpy, scipy

### 模型的多重性 (Model Multiplicity)

**模型選擇 (Model Selection)**：在多個模型中選擇最佳模型的過程。

**過度參數化 (Over-parameterization)**：模型包含過多不必要參數的問題。

## 模型的估計 (Model Estimation)

### 似然函數和平方和函數的研究 (Study of Likelihood Function and Sum of Squares Function)

**最大似然估計 (Maximum Likelihood Estimation, MLE)**：最大化似然函數的參數估計方法。
$$\hat{\theta}_{MLE} = \arg\max_{\theta} L(\theta|X_1, ..., X_n)$$
可用python套件: statsmodels, scipy

**對數似然函數 (Log-likelihood Function)**：似然函數的對數形式，便於計算和優化。
$$\ell(\theta) = \ln L(\theta)$$

**殘差平方和 (Residual Sum of Squares)**：所有殘差平方的總和。
$$RSS = \sum_{t=1}^n e_t^2$$

### 非線性估計 (Nonlinear Estimation)

**牛頓-拉夫遜方法 (Newton-Raphson Method)**：求解非線性方程的迭代算法。
$$\theta_{k+1} = \theta_k - H^{-1}(\theta_k) g(\theta_k)$$
可用python套件: scipy, numpy

**梯度下降法 (Gradient Descent)**：通過梯度方向尋找最優解的算法。
$$\theta_{k+1} = \theta_k - \alpha \nabla f(\theta_k)$$
可用python套件: tensorflow, torch

**Gauss-Newton方法 (Gauss-Newton Method)**：解決非線性最小二乘問題的算法。

### ARIMA模型中的單位根 (Unit Roots in ARIMA Models)

**單位根檢驗 (Unit Root Test)**：檢驗序列是否存在單位根的統計測試。

**ADF檢驗 (Augmented Dickey-Fuller Test)**：增廣迪基-富勒檢驗，常用的單位根檢驗。
$$\Delta X_t = \alpha + \beta t + \gamma X_{t-1} + \sum_{i=1}^p \delta_i \Delta X_{t-i} + \epsilon_t$$
可用python套件: statsmodels, arch

**協整 (Cointegration)**：多個非平穩序列之間存在長期穩定關係。
可用python套件: statsmodels, coint

### 使用Bayes原理的估計 (Estimation Using Bayesian Principles)

**貝葉斯估計 (Bayesian Estimation)**：結合先驗信息和樣本信息的估計方法。
$$p(\theta|X) = \frac{p(X|\theta)p(\theta)}{p(X)}$$
可用python套件: pystan, pymc3, arviz

**先驗分布 (Prior Distribution)**：參數的先驗概率分布。
$$p(\theta)$$

**後驗分布 (Posterior Distribution)**：結合數據後的參數概率分布。
$$p(\theta|X)$$

### 自回歸模型估計量的漸進分布 (Asymptotic Distribution of AR Model Estimators)

**漸近正態性 (Asymptotic Normality)**：估計量在大樣本下趨於正態分布。
$$\sqrt{n}(\hat{\theta} - \theta) \xrightarrow{d} N(0, \Sigma)$$

**一致性 (Consistency)**：估計量隨樣本增大收斂到真值的性質。
$$\hat{\theta}_n \xrightarrow{p} \theta$$

## 模型的診斷檢驗 (Model Diagnostic Testing)

### 隨機模型的檢驗 (Testing of Stochastic Models)

**殘差檢驗 (Residual Testing)**：通過分析殘差來檢驗模型適合度。

**白噪音檢驗 (White Noise Test)**：檢驗殘差是否為白噪音的測試。

**Ljung-Box檢驗 (Ljung-Box Test)**：檢驗殘差自相關的統計測試。
$$Q_{LB} = n(n+2)\sum_{k=1}^h \frac{\hat{\rho}_k^2}{n-k}$$
可用python套件: scipy, statsmodels
```python
from statsmodels.stats.diagnostic import acorr_ljungbox

# 對殘差進行Ljung-Box檢驗
residuals = arima_result.resid
lb_result = acorr_ljungbox(residuals, lags=[10, 20], return_df=True)
print(lb_result)
```

### 應用於殘差的診斷檢驗 (Diagnostic Tests Applied to Residuals)

**正態性檢驗 (Normality Test)**：檢驗殘差是否服從正態分布。
可用python套件: scipy, statsmodels
```python
from scipy import stats

# Shapiro-Wilk檢驗
shapiro_stat, shapiro_pvalue = stats.shapiro(residuals)

# Jarque-Bera檢驗
jb_stat, jb_pvalue = stats.jarque_bera(residuals)

# Q-Q圖
stats.probplot(residuals, dist="norm", plot=plt)
```

**異方差檢驗 (Heteroscedasticity Test)**：檢驗殘差方差是否恆定。
可用python套件: statsmodels, arch
```python
from statsmodels.stats.diagnostic import het_breuschpagan
from arch.unitroot import VarianceRatio

# Breusch-Pagan檢驗
bp_stat, bp_pvalue, _, _ = het_breuschpagan(residuals, sm.add_constant(np.arange(len(residuals))))

# ARCH效應檢驗
from statsmodels.stats.diagnostic import het_arch
arch_stat, arch_pvalue, _, _ = het_arch(residuals)
```

**自相關檢驗 (Autocorrelation Test)**：檢驗殘差是否存在自相關。

### 利用殘差修正模型 (Model Modification Using Residuals)

**模型改進 (Model Improvement)**：基於診斷結果改進模型的過程。

**過度擬合 (Overfitting)**：模型過於複雜導致泛化能力差的問題。

## 季節模型 (Seasonal Models)

### 季節時間序列的簡約模型 (Parsimonious Models for Seasonal Time Series)

**季節性ARIMA (Seasonal ARIMA, SARIMA)**：處理季節性數據的ARIMA模型擴展。
$$ARIMA(p,d,q) \times (P,D,Q)_s$$
可用python套件: statsmodels, pmdarima
```python
from statsmodels.tsa.statespace.sarimax import SARIMAX

# 建立SARIMA模型
sarima_model = SARIMAX(
    ts,
    order=(1,1,1),
    seasonal_order=(1,1,1,12),
    enforce_stationarity=False,
    enforce_invertibility=False
)
sarima_result = sarima_model.fit()

# 季節差分
seasonal_diff = ts.diff(12).dropna()
```

**季節差分 (Seasonal Differencing)**：對季節間隔的觀測值進行差分。
$$\nabla_s X_t = X_t - X_{t-s}$$

**季節週期 (Seasonal Period)**：季節性模式重複的時間間隔。
**季節分解 (Seasonal Decomposition)**
```python
from statsmodels.tsa.seasonal import STL, seasonal_decompose

# STL分解
stl = STL(ts, seasonal=13, trend=None)
stl_result = stl.fit()

# 傳統分解
decomposition = seasonal_decompose(ts, model='multiplicative', period=12)

# 取得各成分
trend = decomposition.trend
seasonal = decomposition.seasonal
residual = decomposition.resid
```

### 更一般的季節模型ARIMA的某些方面 (Some Aspects of More General Seasonal ARIMA Models)

**乘積季節模型 (Multiplicative Seasonal Model)**：季節和非季節成分相互作用的模型。
$$\phi(B)\Phi(B^s)(1-B)^d(1-B^s)^D X_t = \theta(B)\Theta(B^s)\epsilon_t$$

**季節單位根 (Seasonal Unit Root)**：在季節頻率上的單位根。

### 結構分量模型和確定性季節分量 (Structural Component Models and Deterministic Seasonal Components)

**分解模型 (Decomposition Model)**：將序列分解為不同成分的模型。
$$X_t = T_t + S_t + I_t$$
可用python套件: statsmodels, seasonal

**趨勢分量 (Trend Component)**：序列的長期趨勢部分。

**季節分量 (Seasonal Component)**：規律性季節變化部分。

**不規則分量 (Irregular Component)**：隨機波動部分。

### 帶有時間序列誤差項的回歸模型 (Regression Models with Time Series Error Terms)

**回歸模型 (Regression Model)**：解釋變量與響應變量關係的統計模型。
$$Y_t = \beta_0 + \beta_1 X_t + \epsilon_t$$

**時間序列回歸 (Time Series Regression)**：誤差項具有時間相關性的回歸模型。
可用python套件: statsmodels, linearmodels

**虛假回歸 (Spurious Regression)**：非平穩變量間的偽相關關係。

## 非線性和長記憶模型 (Nonlinear and Long Memory Models)

### 自回歸條件異方差(ARCH)模型 (Autoregressive Conditional Heteroscedasticity (ARCH) Models)

**ARCH模型 (ARCH Model)**：條件方差依賴於過去殘差平方的模型。
$$\sigma_t^2 = \alpha_0 + \sum_{i=1}^q \alpha_i \epsilon_{t-i}^2$$
可用python套件: arch, statsmodels

**GARCH模型 (GARCH Model)**：廣義ARCH模型，條件方差也依賴於過去條件方差。
$$\sigma_t^2 = \alpha_0 + \sum_{i=1}^q \alpha_i \epsilon_{t-i}^2 + \sum_{j=1}^p \beta_j \sigma_{t-j}^2$$
可用python套件: arch, statsmodels
```python
from arch import arch_model

# 建立GARCH模型
garch = arch_model(ts, vol='Garch', p=1, q=1)
garch_result = garch.fit()

# 預測波動性
volatility_forecast = garch_result.forecast(horizon=5)
print(volatility_forecast.variance.iloc[-1])

# EGARCH模型
egarch = arch_model(ts, vol='EGARCH', p=1, q=1)
egarch_result = egarch.fit()
```

**條件方差 (Conditional Variance)**：給定過去信息的條件下的方差。
$$Var(\epsilon_t|\mathcal{F}_{t-1}) = \sigma_t^2$$

**波動性聚集 (Volatility Clustering)**：高波動期和低波動期聚集出現的現象。

### 非線性時間序列模型 (Nonlinear Time Series Models)

**閾值模型 (Threshold Model)**：在不同區間採用不同線性模型的分段模型。
$$X_t = \begin{cases}
\phi_1 X_{t-1} + \epsilon_t & \text{if } X_{t-d} \leq r \\
\phi_2 X_{t-1} + \epsilon_t & \text{if } X_{t-d} > r
\end{cases}$$
可用python套件: statsmodels, tsfresh

**平滑轉換模型 (Smooth Transition Model)**：模式間平滑轉換的非線性模型。

**神經網絡模型 (Neural Network Model)**：基於人工神經網絡的非線性時間序列模型。
可用python套件: tensorflow, keras, torch

### 長記憶時間序列過程 (Long Memory Time Series Processes)

**長記憶 (Long Memory)**：自相關函數緩慢衰減的時間序列特性。
$$\sum_{h=1}^{\infty} |\rho(h)| = \infty$$
可用python套件: arch, statsmodels
```python
from arch.covariance.kernel import Bartlett
import hurst

# 計算Hurst指數
H = hurst.compute_Hc(ts.values)[0]
print(f'Hurst指數: {H}')

# 檢測長記憶性
from statsmodels.tsa.stattools import adfuller
# 分數差分
from fracdiff import Fracdiff
fd = Fracdiff(0.5)
ts_fracdiff = fd.fit_transform(ts.values.reshape(-1, 1))
```
```python
import tensorflow as tf
from tensorflow import keras

# 建立LSTM模型預測時間序列
model = keras.Sequential([
    keras.layers.LSTM(50, activation='relu', input_shape=(10, 1)),
    keras.layers.Dense(1)
])
model.compile(optimizer='adam', loss='mse')

# 準備數據（需要轉換為序列格式）
# X_train, y_train = prepare_sequences(ts, n_steps=10)
# model.fit(X_train, y_train, epochs=200, verbose=0)
```

**分數差分 (Fractional Differencing)**：使用非整數階差分的方法。
$$(1-B)^d X_t$$，其中 $d$ 可以是分數

**Hurst指數 (Hurst Exponent)**：衡量長記憶強度的參數。
$$H = d + 0.5$$
可用python套件: hurst, nolds

**ARFIMA模型 (ARFIMA Model)**：自回歸分數積分移動平均模型。
$$\phi(B)(1-B)^d X_t = \theta(B)\epsilon_t$$

## 傳遞函數模型 (Transfer Function Models)

### 線性傳遞函數模型 (Linear Transfer Function Models)

**傳遞函數 (Transfer Function)**：描述輸入如何影響輸出的數學函數。
$$Y_t = \frac{\omega(B)}{\delta(B)} B^b X_t + \frac{\theta(B)}{\phi(B)} \epsilon_t$$
可用python套件: control, scipy

**輸入序列 (Input Series)**：影響系統的外生變量序列。

**輸出序列 (Output Series)**：系統響應的被解釋變量序列。

**脈衝響應函數 (Impulse Response Function)**：單位脈衝輸入引起的輸出響應。
$$v_j = \frac{\partial Y_{t+j}}{\partial \epsilon_t}$$
可用python套件: statsmodels, control

### 用差分方程表示的離散動態模型 (Discrete Dynamic Models Represented by Difference Equations)

**差分方程 (Difference Equations)**：描述離散時間動態系統的方程。

**動態回歸 (Dynamic Regression)**：包含滯後變量的回歸模型。
$$Y_t = \sum_{i=0}^{\infty} \beta_i X_{t-i} + \epsilon_t$$

### 離散模型和連續模型的關係 (Relationship between Discrete and Continuous Models)

**連續時間模型 (Continuous Time Model)**：時間連續的動態模型。

**離散化 (Discretization)**：將連續時間模型轉換為離散時間模型。

### 具有脈衝式輸入的連續模型 (Continuous Models with Impulse Inputs)

**脈衝響應 (Impulse Response)**：系統對脈衝輸入的響應。

**階躍響應 (Step Response)**：系統對階躍輸入的響應。

### 非線性傳遞函數與線性化 (Nonlinear Transfer Functions and Linearization)

**線性化 (Linearization)**：將非線性系統近似為線性系統的方法。

**泰勒展開 (Taylor Expansion)**：函數的多項式近似展開。
$$f(x) = f(a) + f'(a)(x-a) + \frac{f''(a)}{2!}(x-a)^2 + ...$$

## 傳遞函數模型的識別、擬合及檢驗 (Identification, Fitting and Testing of Transfer Function Models)

### 互相關函數 (Cross-correlation Function)

**互相關函數 (Cross-correlation Function, CCF)**：衡量兩個序列間相關關係的函數。
$$\rho_{xy}(h) = \frac{\gamma_{xy}(h)}{\sqrt{\gamma_x(0)\gamma_y(0)}}$$
可用python套件: statsmodels, scipy

**互協方差 (Cross-covariance)**：兩個序列間的協方差函數。
$$\gamma_{xy}(h) = E[(X_t - \mu_x)(Y_{t+h} - \mu_y)]$$

**領先指標 (Leading Indicator)**：提前反映其他變量變化的指標。

**滯後指標 (Lagging Indicator)**：延後反映其他變量變化的指標。

### 傳遞函數模型的識別 (Identification of Transfer Function Models)

**前白化 (Prewhitening)**：消除輸入序列自相關的預處理步驟。

**互相關分析 (Cross-correlation Analysis)**：通過互相關函數識別傳遞函數結構。

### 互譜分析用於傳遞函數模型的識別 (Cross-spectral Analysis for Transfer Function Model Identification)

**互譜 (Cross-spectrum)**：兩個序列在頻域的相關關係。
$$f_{xy}(\omega) = c_{xy}(\omega) - iq_{xy}(\omega)$$
可用python套件: scipy, spectrum

**相干性 (Coherence)**：衡量兩序列在特定頻率相關程度的指標。
$$C_{xy}(\omega) = \frac{|f_{xy}(\omega)|^2}{f_x(\omega)f_y(\omega)}$$
可用python套件: scipy, mne

**相位譜 (Phase Spectrum)**：描述兩序列間相位關係的頻域函數。
$$\phi_{xy}(\omega) = \arctan\left(\frac{q_{xy}(\omega)}{c_{xy}(\omega)}\right)$$

## 干預分析模型和異常值檢測 (Intervention Analysis Models and Outlier Detection)

### 干預分析方法 (Intervention Analysis Methods)

**干預分析 (Intervention Analysis)**：分析外部事件對時間序列影響的方法。
$$X_t = \frac{\omega(B)}{\delta(B)} I_t + N_t$$
可用python套件: statsmodels, causalimipact
```python
from causalimpact import CausalImpact

# 準備數據
data = pd.DataFrame({
    'y': ts,  # 受影響的序列
    'x1': control_series1,  # 控制序列1
    'x2': control_series2   # 控制序列2
})

# 定義前期和後期
pre_period = [0, 70]
post_period = [71, 100]

# 進行因果影響分析
ci = CausalImpact(data, pre_period, post_period)
print(ci.summary())
ci.plot()
```

**結構變化 (Structural Change)**：系統參數在某時點發生的顯著變化。

**虛擬變量 (Dummy Variable)**：表示定性因素的二元變量。
$$D_t = \begin{cases}
1 & \text{if intervention occurs} \\
0 & \text{otherwise}
\end{cases}$$

**政策評估 (Policy Evaluation)**：評估政策實施效果的統計方法。

### 時間序列的異常值分析 (Outlier Analysis in Time Series)

**異常值 (Outlier)**：與正常模式顯著偏離的觀測值。

**創新異常值 (Innovation Outlier)**：影響當期及後續所有時期的異常值。

**附加異常值 (Additive Outlier)**：僅影響特定時期的異常值。

**異常值檢測 (Outlier Detection)**：識別和處理異常觀測值的方法。
可用python套件: pyod, adtk, prophet
```python
from pyod.models.iforest import IForest
from adtk.detector import ThresholdAD

# 使用PyOD
clf = IForest()
clf.fit(ts.values.reshape(-1, 1))
outliers = clf.predict(ts.values.reshape(-1, 1))

# 使用ADTK
threshold_ad = ThresholdAD(high=ts.quantile(0.95), low=ts.quantile(0.05))
anomalies = threshold_ad.detect(ts)
```

### 對存在缺失值ARMA模型的估計 (Estimation of ARMA Models with Missing Values)

**缺失值 (Missing Values)**：數據集中缺失的觀測值。

**EM算法 (EM Algorithm)**：處理缺失數據的期望最大化算法。
可用python套件: sklearn, statsmodels

**插值 (Interpolation)**：估計缺失值的數值方法。
可用python套件: pandas, scipy

## 多元時間序列分析 (Multivariate Time Series Analysis)

### 平穩多元時間序列模型 (Stationary Multivariate Time Series Models)

**向量時間序列 (Vector Time Series)**：多個相關時間序列組成的向量。
$$\mathbf{X}_t = (X_{1t}, X_{2t}, ..., X_{kt})'$$

**向量自回歸模型 (Vector Autoregressive Model, VAR)**：多元時間序列的自回歸模型。
$$\mathbf{X}_t = \boldsymbol{\Phi}_1 \mathbf{X}_{t-1} + ... + \boldsymbol{\Phi}_p \mathbf{X}_{t-p} + \boldsymbol{\epsilon}_t$$
可用python套件: statsmodels, vars
```python
from statsmodels.tsa.vector_ar.var_model import VAR
from statsmodels.tsa.api import VAR

# 準備多元時間序列數據
data = pd.DataFrame({
    'series1': ts1,
    'series2': ts2,
    'series3': ts3
})

# 建立VAR模型
var_model = VAR(data)
var_result = var_model.fit(maxlags=5, ic='aic')

# 預測
forecast = var_result.forecast(data.values[-var_result.k_ar:], steps=5)

# 脈衝響應分析
irf = var_result.irf(10)
irf.plot()
```

**向量移動平均模型 (Vector Moving Average Model, VMA)**：多元時間序列的移動平均模型。
$$\mathbf{X}_t = \boldsymbol{\epsilon}_t + \boldsymbol{\Theta}_1 \boldsymbol{\epsilon}_{t-1} + ... + \boldsymbol{\Theta}_q \boldsymbol{\epsilon}_{t-q}$$

### 平穩多元過程的線性模型表達式 (Linear Model Representations of Stationary Multivariate Processes)

**向量ARMA模型 (Vector ARMA Model, VARMA)**：多元ARMA模型。
$$\boldsymbol{\Phi}(B)\mathbf{X}_t = \boldsymbol{\Theta}(B)\boldsymbol{\epsilon}_t$$

**Wold分解 (Wold Decomposition)**：將平穩過程分解為確定性和隨機成分。

### 非平穩向量自回歸移動平均模型 (Non-stationary Vector Autoregressive Moving Average Models)

**向量誤差修正模型 (Vector Error Correction Model, VECM)**：包含協整關係的VAR模型。
$$\Delta \mathbf{X}_t = \boldsymbol{\alpha} \boldsymbol{\beta}' \mathbf{X}_{t-1} + \sum_{i=1}^{p-1} \boldsymbol{\Gamma}_i \Delta \mathbf{X}_{t-i} + \boldsymbol{\epsilon}_t$$
可用python套件: statsmodels, arch

**協整向量 (Cointegrating Vector)**：使非平穩變量線性組合變平穩的係數向量。

**Granger因果關係 (Granger Causality)**：一個變量是否有助於預測另一個變量。
可用python套件: statsmodels, grangercausality
```python
from statsmodels.tsa.stattools import grangercausalitytests

# Granger因果檢驗
max_lag = 4
granger_result = grangercausalitytests(
    data[['series1', 'series2']], 
    maxlag=max_lag
)
```
**協整檢驗**
```python
from statsmodels.tsa.vector_ar.vecm import coint_johansen

# Johansen協整檢驗
johansen_result = coint_johansen(data, det_order=0, k_ar_diff=1)
print(f'協整向量數: {johansen_result.r_coint}')

# Engle-Granger協整檢驗
from statsmodels.tsa.stattools import coint
score, pvalue, _ = coint(ts1, ts2)
```

### 對向量自回歸移動平均過程的預測 (Forecasting Vector ARMA Processes)

**多元預測 (Multivariate Forecasting)**：同時預測多個相關序列。

**預測誤差方差矩陣 (Forecast Error Variance Matrix)**：多元預測誤差的協方差矩陣。
$$\boldsymbol{\Sigma}_h = E[(\mathbf{X}_{t+h} - \hat{\mathbf{X}}_{t+h})(\mathbf{X}_{t+h} - \hat{\mathbf{X}}_{t+h})']$$

### 向量ARMA模型的統計分析 (Statistical Analysis of Vector ARMA Models)

**似然比檢驗 (Likelihood Ratio Test)**：比較嵌套模型的統計檢驗。
$$LR = -2(\ell_0 - \ell_1)$$
可用python套件: statsmodels, scipy

**Wald檢驗 (Wald Test)**：檢驗參數約束的統計方法。
$$W = (\mathbf{R}\hat{\boldsymbol{\theta}} - \mathbf{r})' [\mathbf{R} \hat{\mathbf{V}} \mathbf{R}']^{-1} (\mathbf{R}\hat{\boldsymbol{\theta}} - \mathbf{r})$$

## 過程控制的各個方面 (Various Aspects of Process Control)

### 過程監測和過程調整 (Process Monitoring and Process Adjustment)

**統計過程控制 (Statistical Process Control, SPC)**：使用統計方法監控過程穩定性。
可用python套件: pyspc, controlchart

**控制圖 (Control Chart)**：監測過程變異的統計圖表工具。
$$UCL = \mu + 3\sigma, \quad LCL = \mu - 3\sigma$$
可用python套件: matplotlib, pysqc
```python
import matplotlib.pyplot as plt
import numpy as np

# 建立控制圖
def control_chart(data, cl=None, ucl=None, lcl=None):
    if cl is None:
        cl = np.mean(data)
    if ucl is None:
        ucl = cl + 3 * np.std(data)
    if lcl is None:
        lcl = cl - 3 * np.std(data)
    
    plt.figure(figsize=(10, 6))
    plt.plot(data, 'b-', marker='o')
    plt.axhline(cl, color='g', linestyle='-', label='CL')
    plt.axhline(ucl, color='r', linestyle='--', label='UCL')
    plt.axhline(lcl, color='r', linestyle='--', label='LCL')
    plt.legend()
    plt.show()

control_chart(ts)
```

**過程能力 (Process Capabilities)**：過程滿足規格要求的能力。
$$C_p = \frac{USL - LSL}{6\sigma}$$

### 使用反饋控制的過程調整 (Process Adjustment Using Feedback Control)

**反饋控制 (Feedback Control)**：基於輸出偏差調整輸入的控制方法。
$$u_t = K(r_t - y_t)$$

**控制器設計 (Controller Design)**：設計控制系統參數的工程方法。

**PID控制器 (PID Controller)**：比例-積分-微分控制器。
$$u(t) = K_p e(t) + K_i \int_0^t e(\tau) d\tau + K_d \frac{de(t)}{dt}$$
可用python套件: simple-pid, control
```python
from simple_pid import PID

# 建立PID控制器
pid = PID(1, 0.1, 0.05, setpoint=1)
pid.output_limits = (0, 10)  # 設定輸出限制

# 控制迴圈
controlled_values = []
for current_value in ts:
    control = pid(current_value)
    controlled_values.append(control)
```

### MMSE控制有時所需的過度調整 (Over-adjustment Sometimes Required for MMSE Control)

**最小均方誤差控制 (MMSE Control)**：最小化均方誤差的控制策略。

**過度調整 (Over-adjustment)**：為達到最優控制而採用的激進調整。

### 對於具有固定調整和監測代價的最小代價控制 (Minimum Cost Control with Fixed Adjustment and Monitoring Costs)

**成本函數 (Cost Function)**：量化控制成本的目標函數。
$$J = E[\sum_{t=0}^{\infty} \beta^t (q e_t^2 + c u_t^2)]$$

**最優控制 (Optimal Control)**：最小化成本函數的控制策略。
可用python套件: cvxpy, control
```python
import cvxpy as cp

# 定義優化問題
n = len(ts)
x = cp.Variable(n)
objective = cp.Minimize(cp.sum_squares(x - ts.values))
constraints = [-1 <= x, x <= 1]  # 約束條件

# 求解
problem = cp.Problem(objective, constraints)
problem.solve()

optimal_values = x.value
```

### 前饋控制 (Feedforward Control)

**前饋控制 (Feedforward Control)**：基於干擾預測進行預防性調整的控制方法。

**預測控制 (Predictive Control)**：基於模型預測進行控制的方法。

### 調整方案有約束的反饋控制 (Feedback Control with Constrained Adjustment Schemes)

**約束優化 (Constrained Optimization)**：在約束條件下的最優化問題。

**線性規劃 (Linear Programming)**：目標函數和約束都是線性的優化問題。
可用python套件: pulp, scipy, cvxpy

### 抽樣間隔的選擇 (Selection of Sampling Intervals)
可用python套件: scipy, numpy

**抽樣頻率 (Sampling Frequency)**：單位時間內的採樣次數。

**Nyquist頻率 (Nyquist Frequency)**：避免混疊所需的最小採樣頻率。
$$f_{Nyquist} = \frac{f_s}{2}$$

**混疊 (Aliasing)**：由於採樣頻率不足導致的頻率混淆現象。


# 時間序列分析相關python套件整理
## 基礎數據處理與視覺化
- `pip install numpy` - 數值計算基礎套件
- `pip install pandas` - 數據分析與處理
- `pip install matplotlib` - 基礎繪圖
- `pip install seaborn` - 統計圖形視覺化

## 統計分析與時間序列核心套件
- `pip install statsmodels` - 統計建模（ARIMA, VAR, 檢定等）
- `pip install scipy` - 科學計算（優化、信號處理、統計）
- `pip install scikit-learn` - 機器學習（模型選擇、預測）
- `pip install arch` - ARCH/GARCH模型、單位根檢定

## 時間序列專門套件
- `pip install pmdarima` - 自動ARIMA模型選擇
- `pip install prophet` - Facebook時間序列預測
- `pip install sktime` - 時間序列機器學習
- `pip install seasonal` - 季節性分解
- `pip install x13arima-select` - X-13ARIMA-SEATS季節調整
- `pip install tsfresh` - 時間序列特徵提取

## 頻譜分析
- `pip install spectrum` - 頻譜分析工具
- `pip install astropy` - 天文學套件（含週期圖功能）

## 狀態空間與濾波
- `pip install pykalman` - 卡爾曼濾波
- `pip install filterpy` - 各種濾波器實現
- `pip install simdkalman` - 簡化的卡爾曼濾波

## 隨機過程與模擬
- `pip install stochastic` - 隨機過程生成
- `pip install simpy` - 離散事件模擬

## 貝葉斯統計
- `pip install pystan` - Stan概率編程語言接口
- `pip install pymc3` - 貝葉斯統計建模
- `pip install arviz` - 貝葉斯模型診斷與視覺化

## 深度學習框架
- `pip install tensorflow` - Google深度學習框架
- `pip install keras` - 高階神經網絡API
- `pip install torch` - PyTorch深度學習框架

## 異常檢測
- `pip install pyod` - 異常值檢測演算法庫
- `pip install adtk` - 時間序列異常檢測

## 因果分析
- `pip install causalimpact` - 因果影響分析
- `pip install grangercausality` - Granger因果檢定

## 控制系統
- `pip install control` - 控制系統分析與設計
- `pip install simple-pid` - PID控制器實現
- `pip install pyspc` - 統計過程控制
- `pip install controlchart` - 控制圖繪製

## 優化與數學
- `pip install cvxpy` - 凸優化
- `pip install pulp` - 線性規劃
- `pip install lmfit` - 非線性最小二乘擬合
- `pip install sympy` - 符號數學

## 其他專門工具
- `pip install hurst` - Hurst指數計算
- `pip install nolds` - 非線性動力學分析
- `pip install mne` - 腦電圖/腦磁圖分析（含相干性分析）
- `pip install pytrends` - Google趨勢API
- `pip install linearmodels` - 線性模型擴展
- `pip install coint` - 協整檢定
- `pip install vars` - 向量自回歸分析
- `pip install dynaconf` - 動態配置管理
- `pip install PyWavelets` - 小波變換，可捕捉時頻特徵
- `pip install EMD-signal` - 經驗模態分解，分離不同頻率成分
