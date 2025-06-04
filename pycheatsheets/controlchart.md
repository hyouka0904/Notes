# Python ControlChart 套件 Cheat Sheet

## 安裝與匯入

```python
# 安裝
pip install controlchart

# 基本匯入
import controlchart as cc
import pandas as pd
import numpy as np
from datetime import datetime, timedelta
```

## 基本概念

Control Chart（管制圖）用於監控過程的穩定性，主要包含：
- **UCL (Upper Control Limit)**: 上管制界限
- **CL (Center Line)**: 中心線
- **LCL (Lower Control Limit)**: 下管制界限

---

## 單純時間序列分析

### 1. I-MR Chart (Individual-Moving Range)
適用於單一觀測值的時序數據

```python
# 準備數據
dates = pd.date_range('2024-01-01', periods=100, freq='D')
data = np.random.normal(50, 5, 100)
df = pd.DataFrame({'date': dates, 'value': data})

# 建立 I-MR 圖
imr = cc.Imr(data=df['value'])

# 取得管制界限
print(f"UCL: {imr.ucl}")
print(f"CL: {imr.cl}")
print(f"LCL: {imr.lcl}")

# 檢測異常點
out_of_control = imr.beyond_limits
```

### 2. X-bar and R Chart
適用於子群組數據

```python
# 準備子群組數據
subgroup_size = 5
n_subgroups = 20
data = np.random.normal(100, 10, (n_subgroups, subgroup_size))

# 建立 X-bar 圖
xbar = cc.Xbar(data=data)

# 建立 R 圖
r_chart = cc.R(data=data)

# 視覺化
xbar.plot()
r_chart.plot()
```

### 3. EWMA Chart (指數加權移動平均)
適用於檢測小幅度的過程偏移

```python
# EWMA 管制圖
ewma = cc.Ewma(data=df['value'], lambda_=0.2, L=3)

# 取得 EWMA 值
ewma_values = ewma.y

# 檢測趨勢
trend_points = ewma.beyond_limits
```

### 4. CUSUM Chart (累積和)
適用於檢測過程均值的持續性偏移

```python
# CUSUM 管制圖
cusum = cc.Cusum(data=df['value'], target=50, k=0.5, h=5)

# 取得累積和
cusum_pos = cusum.cusum_pos
cusum_neg = cusum.cusum_neg

# 檢測偏移
shift_detected = cusum.beyond_limits
```

---

## 機器學習結合時間序列分析

### 1. 特徵工程與異常檢測

```python
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import IsolationForest

# 建立時序特徵
def create_time_features(df):
    df['rolling_mean'] = df['value'].rolling(window=7).mean()
    df['rolling_std'] = df['value'].rolling(window=7).std()
    df['lag_1'] = df['value'].shift(1)
    df['lag_7'] = df['value'].shift(7)
    df['diff_1'] = df['value'].diff(1)
    return df.dropna()

# 準備特徵
df_features = create_time_features(df.copy())

# 使用 Isolation Forest 檢測異常
features = ['value', 'rolling_mean', 'rolling_std', 'lag_1', 'diff_1']
X = df_features[features]

# 標準化
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# 異常檢測
iso_forest = IsolationForest(contamination=0.1, random_state=42)
anomalies = iso_forest.fit_predict(X_scaled)

# 結合管制圖
imr = cc.Imr(data=df_features['value'])
control_chart_anomalies = imr.beyond_limits

# 綜合判斷
combined_anomalies = (anomalies == -1) | control_chart_anomalies
```

### 2. 預測區間與管制界限

```python
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_squared_error

# 準備訓練數據
train_size = int(0.8 * len(df_features))
train = df_features[:train_size]
test = df_features[train_size:]

# 訓練模型
rf = RandomForestRegressor(n_estimators=100, random_state=42)
rf.fit(train[features], train['value'])

# 預測
predictions = rf.predict(test[features])

# 計算預測區間
residuals = train['value'] - rf.predict(train[features])
std_residual = np.std(residuals)

# 動態管制界限
upper_limit = predictions + 3 * std_residual
lower_limit = predictions - 3 * std_residual

# 檢測異常
anomalies = (test['value'] > upper_limit) | (test['value'] < lower_limit)
```

### 3. 多變量管制圖 (T² Chart)

```python
from scipy import stats

# 準備多變量數據
n_vars = 3
n_samples = 100
multivariate_data = np.random.multivariate_normal(
    mean=[50, 100, 75],
    cov=[[10, 5, 3], [5, 20, 7], [3, 7, 15]],
    size=n_samples
)

# 計算 T² 統計量
def calculate_t2(X, mu=None, S=None):
    if mu is None:
        mu = np.mean(X, axis=0)
    if S is None:
        S = np.cov(X.T)
    
    n = X.shape[0]
    p = X.shape[1]
    
    t2_values = []
    for i in range(n):
        diff = X[i] - mu
        t2 = diff @ np.linalg.inv(S) @ diff.T
        t2_values.append(t2)
    
    return np.array(t2_values)

# 計算管制界限
t2_values = calculate_t2(multivariate_data)
p = multivariate_data.shape[1]
n = multivariate_data.shape[0]
alpha = 0.0027  # 對應 3-sigma

# UCL for T² chart
ucl_t2 = stats.chi2.ppf(1-alpha, p)
```

### 4. 自適應管制圖

```python
# 自適應 EWMA
class AdaptiveEWMA:
    def __init__(self, initial_lambda=0.2, min_lambda=0.05, max_lambda=0.4):
        self.lambda_ = initial_lambda
        self.min_lambda = min_lambda
        self.max_lambda = max_lambda
        
    def update_lambda(self, error):
        # 根據誤差調整 lambda
        if abs(error) > 2:  # 大誤差
            self.lambda_ = min(self.lambda_ * 1.1, self.max_lambda)
        else:  # 小誤差
            self.lambda_ = max(self.lambda_ * 0.95, self.min_lambda)
    
    def fit(self, data):
        ewma_values = []
        current_ewma = data[0]
        
        for value in data:
            error = value - current_ewma
            self.update_lambda(error)
            current_ewma = self.lambda_ * value + (1 - self.lambda_) * current_ewma
            ewma_values.append(current_ewma)
            
        return np.array(ewma_values)

# 使用自適應 EWMA
adaptive_ewma = AdaptiveEWMA()
ewma_adaptive = adaptive_ewma.fit(df['value'].values)
```

---

## 實用技巧

### 1. 管制圖選擇指南

| 情境         | 推薦管制圖 | 原因           |
| ------------ | ---------- | -------------- |
| 單一觀測值   | I-MR       | 簡單有效       |
| 子群組數據   | X-bar & R  | 利用群內變異   |
| 檢測小偏移   | EWMA       | 對小變化敏感   |
| 檢測持續偏移 | CUSUM      | 累積效應       |
| 多變量監控   | T²         | 考慮變量相關性 |

### 2. 參數調整建議

```python
# EWMA lambda 選擇
# lambda = 0.05-0.1: 對小偏移敏感
# lambda = 0.2-0.3: 平衡敏感度和穩定性
# lambda = 0.4-0.5: 減少誤報

# CUSUM 參數
# k = 0.5: 標準選擇（檢測 1σ 偏移）
# h = 4-5: 平衡檢測速度和誤報率
```

### 3. 整合流程

```python
def integrated_monitoring(data, features):
    """整合機器學習和管制圖的監控流程"""
    
    # 1. 傳統管制圖
    imr = cc.Imr(data=data)
    control_anomalies = imr.beyond_limits
    
    # 2. 機器學習預測
    model = RandomForestRegressor()
    model.fit(features[:-1], data[:-1])
    prediction = model.predict(features[-1:])
    
    # 3. 預測區間
    residuals = data[:-1] - model.predict(features[:-1])
    prediction_interval = 3 * np.std(residuals)
    
    # 4. 綜合判斷
    ml_anomaly = abs(data[-1] - prediction) > prediction_interval
    
    return {
        'control_chart_anomaly': control_anomalies[-1],
        'ml_anomaly': ml_anomaly,
        'combined_alert': control_anomalies[-1] or ml_anomaly
    }
```

### 4. 視覺化最佳實踐

```python
import matplotlib.pyplot as plt

def plot_enhanced_control_chart(data, predictions=None, anomalies=None):
    fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(12, 8))
    
    # 管制圖
    imr = cc.Imr(data=data)
    ax1.plot(data, 'b-', label='觀測值')
    ax1.axhline(imr.ucl, color='r', linestyle='--', label='UCL')
    ax1.axhline(imr.cl, color='g', linestyle='-', label='CL')
    ax1.axhline(imr.lcl, color='r', linestyle='--', label='LCL')
    
    if predictions is not None:
        ax1.plot(predictions, 'orange', alpha=0.7, label='預測值')
    
    if anomalies is not None:
        ax1.scatter(np.where(anomalies)[0], data[anomalies], 
                   color='red', s=100, label='異常點')
    
    ax1.legend()
    ax1.set_title('管制圖與異常檢測')
    
    # 移動極差圖
    mr = np.abs(np.diff(data))
    ax2.plot(mr, 'g-')
    ax2.set_title('移動極差圖')
    
    plt.tight_layout()
    return fig
```

---

## 注意事項

1. **數據要求**：確保數據穩定且符合常態分布假設
2. **樣本大小**：建議至少 20-25 個數據點建立管制界限
3. **更新頻率**：定期重新計算管制界限以反映過程改變
4. **誤報處理**：結合領域知識判斷異常的真實性
5. **多重檢定**：使用多個管制圖時注意調整顯著水準