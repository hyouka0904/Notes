# Python ARCH 套件 Cheat Sheet
## 機器學習結合時間序列分析 & 單純時間序列分析

### 🔧 安裝與導入

```python
# 安裝
pip install arch

# 核心導入
import arch
from arch import arch_model
from arch.unitroot import ADF, KPSS, VarianceRatio
from arch.bootstrap import IIDBootstrap, StationaryBootstrap
from arch.data import sp500
import numpy as np
import pandas as pd
```

### 📊 資料準備與前處理

```python
# 載入範例數據
data = arch.data.sp500.load()
returns = 100 * data['Adj Close'].pct_change().dropna()

# 計算對數報酬率
log_returns = np.log(data['Adj Close'] / data['Adj Close'].shift(1)).dropna()

# 檢查數據統計特性
print(returns.describe())
print(f"偏度: {returns.skew():.4f}")
print(f"峰度: {returns.kurtosis():.4f}")

# 繪製時間序列圖
returns.plot(title='Returns', figsize=(12, 6))
```

### 🧪 單位根檢定 (Stationarity Tests)

```python
# ADF 檢定 (Augmented Dickey-Fuller)
adf = ADF(returns)
print(f"ADF 統計量: {adf.stat:.4f}")
print(f"p-value: {adf.pvalue:.4f}")
print(f"臨界值: {adf.critical_values}")

# KPSS 檢定
kpss = KPSS(returns)
print(f"KPSS 統計量: {kpss.stat:.4f}")
print(f"p-value: {kpss.pvalue:.4f}")

# Variance Ratio 檢定
vr = VarianceRatio(returns, lags=2)
print(f"Variance Ratio: {vr.stat:.4f}")
```

### 📈 ARCH/GARCH 模型建立

#### 基本 GARCH 模型

```python
# GARCH(1,1) 模型
model = arch_model(returns, vol='Garch', p=1, q=1)
fitted_model = model.fit()
print(fitted_model.summary())

# 預測波動率
forecasts = fitted_model.forecast(horizon=5)
print(forecasts.variance[-1:])  # 未來5期的方差預測
```

#### 不同類型的波動率模型

```python
# ARCH 模型
arch_model = arch_model(returns, vol='ARCH', p=5)

# EGARCH 模型 (指數GARCH)
egarch_model = arch_model(returns, vol='EGARCH', p=1, q=1)

# GJR-GARCH 模型
gjr_model = arch_model(returns, vol='GARCH', p=1, o=1, q=1)

# 適配模型
fitted_arch = arch_model.fit()
fitted_egarch = egarch_model.fit()
fitted_gjr = gjr_model.fit()
```

#### 不同分佈假設

```python
# 常態分佈 (預設)
normal_model = arch_model(returns, dist='normal')

# t 分佈
t_model = arch_model(returns, dist='t')

# 偏 t 分佈
skew_t_model = arch_model(returns, dist='skewt')

# GED 分佈
ged_model = arch_model(returns, dist='ged')
```

### 🔮 波動率預測

```python
# 單步預測
forecast_1 = fitted_model.forecast(horizon=1)
next_variance = forecast_1.variance.iloc[-1, 0]

# 多步預測
forecast_5 = fitted_model.forecast(horizon=5)
variance_forecasts = forecast_5.variance.iloc[-1, :]

# 條件波動率 (fitted values)
conditional_volatility = fitted_model.conditional_volatility

# 繪製波動率圖
import matplotlib.pyplot as plt
plt.figure(figsize=(12, 6))
plt.plot(conditional_volatility.index, conditional_volatility, label='Conditional Volatility')
plt.title('GARCH Conditional Volatility')
plt.legend()
plt.show()
```

### 🤖 機器學習結合時間序列

#### 特徵工程

```python
def create_features(returns, lags=5):
    """建立滯後特徵和技術指標"""
    df = pd.DataFrame(index=returns.index)
    
    # 滯後報酬率
    for i in range(1, lags+1):
        df[f'lag_{i}'] = returns.shift(i)
    
    # 移動平均
    df['ma_5'] = returns.rolling(5).mean()
    df['ma_20'] = returns.rolling(20).mean()
    
    # 波動率特徵
    df['volatility_5'] = returns.rolling(5).std()
    df['volatility_20'] = returns.rolling(20).std()
    
    # GARCH 波動率作為特徵
    garch_model = arch_model(returns, vol='Garch', p=1, q=1)
    garch_fitted = garch_model.fit(disp='off')
    df['garch_vol'] = garch_fitted.conditional_volatility
    
    return df.dropna()

features = create_features(returns)
```

#### 使用 GARCH 殘差作為 ML 特徵

```python
# 適配 GARCH 模型並提取標準化殘差
garch_model = arch_model(returns, vol='Garch', p=1, q=1)
garch_result = garch_model.fit(disp='off')

# 標準化殘差作為特徵
standardized_residuals = garch_result.std_resid

# 結合其他特徵
ml_features = pd.concat([
    features,
    standardized_residuals.rename('garch_residuals')
], axis=1).dropna()
```

#### 滾動窗口分析

```python
def rolling_garch_forecast(returns, window=252):
    """滾動窗口 GARCH 預測"""
    forecasts = []
    
    for i in range(window, len(returns)):
        # 滾動窗口數據
        window_data = returns.iloc[i-window:i]
        
        # 適配模型
        model = arch_model(window_data, vol='Garch', p=1, q=1)
        fitted = model.fit(disp='off')
        
        # 一步預測
        forecast = fitted.forecast(horizon=1)
        forecasts.append(forecast.variance.iloc[0, 0])
    
    return pd.Series(forecasts, index=returns.index[window:])

rolling_forecasts = rolling_garch_forecast(returns)
```

### 📊 模型診斷與評估

```python
# 殘差檢定
residuals = fitted_model.resid
standardized_residuals = fitted_model.std_resid

# Ljung-Box 檢定 (殘差自相關)
from statsmodels.stats.diagnostic import acorr_ljungbox
lb_stat = acorr_ljungbox(residuals, lags=10, return_df=True)
print(lb_stat)

# ARCH 效應檢定 (殘差平方的自相關)
lb_squared = acorr_ljungbox(residuals**2, lags=10, return_df=True)
print(lb_squared)

# Jarque-Bera 常態性檢定
from scipy.stats import jarque_bera
jb_stat, jb_pvalue = jarque_bera(standardized_residuals)
print(f"JB 統計量: {jb_stat:.4f}, p-value: {jb_pvalue:.4f}")
```

### 🎯 模型比較與選擇

```python
# 資訊準則比較
models = {
    'GARCH(1,1)': arch_model(returns, vol='Garch', p=1, q=1),
    'GARCH(2,1)': arch_model(returns, vol='Garch', p=2, q=1),
    'EGARCH(1,1)': arch_model(returns, vol='EGARCH', p=1, q=1),
    'GJR-GARCH(1,1,1)': arch_model(returns, vol='GARCH', p=1, o=1, q=1)
}

results = {}
for name, model in models.items():
    fitted = model.fit(disp='off')
    results[name] = {
        'AIC': fitted.aic,
        'BIC': fitted.bic,
        'Log-likelihood': fitted.loglikelihood
    }

comparison_df = pd.DataFrame(results).T
print(comparison_df.sort_values('AIC'))
```

### 🔄 Bootstrap 方法

```python
# IID Bootstrap
iid_bootstrap = IIDBootstrap(returns)
bootstrap_samples = iid_bootstrap.bootstrap(1000)

# Stationary Bootstrap (適用於時間序列)
stationary_bootstrap = StationaryBootstrap(12, returns)  # 平均區塊長度為12
stationary_samples = stationary_bootstrap.bootstrap(1000)

# Bootstrap 信賴區間
def bootstrap_garch_params(returns, n_bootstrap=1000):
    """Bootstrap GARCH 參數信賴區間"""
    bootstrap = StationaryBootstrap(12, returns)
    params = []
    
    for data in bootstrap.bootstrap(n_bootstrap):
        try:
            model = arch_model(data[0][0], vol='Garch', p=1, q=1)
            result = model.fit(disp='off')
            params.append(result.params.values)
        except:
            continue
    
    params_df = pd.DataFrame(params, columns=['mu', 'omega', 'alpha[1]', 'beta[1]'])
    return params_df.describe(percentiles=[0.025, 0.975])

bootstrap_ci = bootstrap_garch_params(returns)
```

### 📈 進階應用案例

#### 條件 VaR 計算

```python
def calculate_conditional_var(fitted_model, confidence_level=0.05):
    """計算條件風險值 (VaR)"""
    forecasts = fitted_model.forecast(horizon=1)
    volatility_forecast = np.sqrt(forecasts.variance.iloc[-1, 0])
    
    # 假設 t 分佈
    from scipy.stats import t
    df = fitted_model.params.iloc[-1]  # 自由度參數
    var = -volatility_forecast * t.ppf(confidence_level, df)
    
    return var

conditional_var = calculate_conditional_var(fitted_model)
print(f"1% VaR: {conditional_var:.4f}")
```

#### 多變量 GARCH (DCC-GARCH)

```python
# 準備多個時間序列
from arch.data import sp500, nasdaq
sp500_ret = 100 * sp500.load()['Adj Close'].pct_change().dropna()
nasdaq_ret = 100 * nasdaq.load()['Adj Close'].pct_change().dropna()

# 對齊數據
aligned_data = pd.concat([sp500_ret, nasdaq_ret], axis=1).dropna()
aligned_data.columns = ['SP500', 'NASDAQ']

# 單變量 GARCH 模型 (DCC 的第一步)
univariate_models = {}
for col in aligned_data.columns:
    model = arch_model(aligned_data[col], vol='Garch', p=1, q=1)
    univariate_models[col] = model.fit(disp='off')
```

### 🛠️ 實用工具函數

```python
# 模型摘要輸出
def model_summary(fitted_model):
    """簡潔的模型摘要"""
    summary = {
        'AIC': fitted_model.aic,
        'BIC': fitted_model.bic,
        'Log-likelihood': fitted_model.loglikelihood,
        'Parameters': dict(fitted_model.params),
        'Volatility_mean': fitted_model.conditional_volatility.mean(),
        'Volatility_std': fitted_model.conditional_volatility.std()
    }
    return summary

# 批次預測函數
def batch_forecast(returns, models_config, horizon=5):
    """批次運行多個模型並比較預測"""
    forecasts = {}
    
    for name, config in models_config.items():
        model = arch_model(returns, **config)
        fitted = model.fit(disp='off')
        forecast = fitted.forecast(horizon=horizon)
        forecasts[name] = forecast.variance.iloc[-1, :]
    
    return pd.DataFrame(forecasts)

# 使用範例
configs = {
    'GARCH(1,1)': {'vol': 'Garch', 'p': 1, 'q': 1},
    'EGARCH(1,1)': {'vol': 'EGARCH', 'p': 1, 'q': 1},
    'GJR-GARCH': {'vol': 'GARCH', 'p': 1, 'o': 1, 'q': 1}
}

batch_results = batch_forecast(returns, configs)
```

### 📚 常用參數速查

| 參數      | 說明             | 常用值                        |
| --------- | ---------------- | ----------------------------- |
| `vol`     | 波動率模型類型   | 'Garch', 'EGARCH', 'ARCH'     |
| `p`       | ARCH 項數        | 1, 2                          |
| `q`       | GARCH 項數       | 1, 2                          |
| `o`       | 非對稱項數 (GJR) | 1                             |
| `dist`    | 分佈假設         | 'normal', 't', 'skewt', 'ged' |
| `mean`    | 均值模型         | 'Constant', 'Zero', 'AR'      |
| `horizon` | 預測期數         | 1, 5, 10                      |

### ⚡ 效能優化技巧

```python
# 1. 關閉輸出以加速運算
model.fit(disp='off')

# 2. 設定起始值以加速收斂
starting_values = np.array([0.01, 0.01, 0.05, 0.90])
model.fit(starting_values=starting_values)

# 3. 使用較寬鬆的收斂條件
model.fit(options={'ftol': 1e-6})

# 4. 並行處理多個模型
from multiprocessing import Pool

def fit_model(config):
    model = arch_model(returns, **config)
    return model.fit(disp='off')

with Pool() as pool:
    results = pool.map(fit_model, list(configs.values()))
```

### 🎯 最佳實踐建議

1. **資料前處理**: 確保數據無缺失值且為穩態
2. **模型選擇**: 使用資訊準則 (AIC/BIC) 進行比較
3. **殘差診斷**: 檢查殘差是否無自相關和 ARCH 效應
4. **預測評估**: 使用滾動窗口進行樣本外預測評估
5. **穩健性檢驗**: 使用 Bootstrap 方法檢驗參數穩定性
6. **計算效率**: 對於大數據集，考慮使用簡化模型或並行處理