# Python ADTK (Anomaly Detection Toolkit) Cheat Sheet

## 安裝與導入

```python
# 安裝
pip install adtk

# 基本導入
import pandas as pd
import numpy as np
from datetime import datetime
import matplotlib.pyplot as plt

# ADTK 核心模組
from adtk.data import validate_series
from adtk.visualization import plot
from adtk.detector import *
from adtk.transformer import *
from adtk.aggregator import *
from adtk.pipe import Pipeline
```

## 資料準備

```python
# 建立時間序列資料
dates = pd.date_range('2023-01-01', periods=1000, freq='H')
values = np.random.randn(1000).cumsum() + 50
ts = pd.Series(values, index=dates)

# 驗證時間序列格式
ts = validate_series(ts)

# 處理缺失值
ts = ts.fillna(method='ffill')  # 前向填充
ts = ts.interpolate(method='linear')  # 線性插值
```

---

## 單純時間序列分析

### 1. 統計型異常檢測器

#### ThresholdAD - 閾值檢測
```python
# 固定閾值
threshold_ad = ThresholdAD(high=100, low=0)
anomalies = threshold_ad.detect(ts)

# 百分位數閾值
from adtk.detector import QuantileAD
quantile_ad = QuantileAD(high=0.99, low=0.01)
anomalies = quantile_ad.detect(ts)
```

#### InterQuartileRangeAD - 四分位距檢測
```python
iqr_ad = InterQuartileRangeAD(c=1.5)
anomalies = iqr_ad.detect(ts)
plot(ts, anomaly=anomalies, ts_linewidth=1, ts_markersize=3)
```

#### GeneralizedESDTestAD - 廣義ESD檢驗
```python
esd_ad = GeneralizedESDTestAD(alpha=0.05)
anomalies = esd_ad.detect(ts)
```

### 2. 時間序列特徵檢測

#### PersistAD - 持續性異常
```python
# 檢測值長時間不變的異常
persist_ad = PersistAD(window=20, c=3.0)
anomalies = persist_ad.detect(ts)
```

#### LevelShiftAD - 水平位移檢測
```python
# 檢測時間序列中的突然水平變化
level_shift_ad = LevelShiftAD(window=30, c=6.0)
anomalies = level_shift_ad.detect(ts)
```

#### VolatilityShiftAD - 波動性變化檢測
```python
# 檢測波動性的突然變化
volatility_shift_ad = VolatilityShiftAD(window=30, c=6.0)
anomalies = volatility_shift_ad.detect(ts)
```

#### SeasonalAD - 季節性異常檢測
```python
# 檢測偏離季節性模式的異常
seasonal_ad = SeasonalAD(freq=24)  # 每日季節性（小時數據）
anomalies = seasonal_ad.detect(ts)
```

### 3. 自定義轉換器

```python
# 移動平均
rolling_mean = RollingAggregate(agg='mean', window=10)
ts_smooth = rolling_mean.transform(ts)

# 差分
diff = Difference(n=1)
ts_diff = diff.transform(ts)

# 雙重指數平滑
double_rolling = DoubleRollingAggregate(
    agg='mean',
    window=10,
    diff='diff'
)
ts_transformed = double_rolling.transform(ts)
```

---

## 機器學習結合時間序列分析

### 1. 基於機器學習的檢測器

#### MinClusterDetector - 最小聚類檢測
```python
from adtk.detector import MinClusterDetector

# 需要訓練集和測試集
train = ts[:700]
test = ts[700:]

# 建立檢測器
min_cluster = MinClusterDetector(n_clusters=3)
min_cluster.fit(train)
anomalies = min_cluster.detect(test)
```

#### OutlierDetector - 離群值檢測（使用 sklearn 模型）
```python
from sklearn.neighbors import LocalOutlierFactor
from adtk.detector import OutlierDetector

# 使用 LOF 演算法
outlier_detector = OutlierDetector(
    LocalOutlierFactor(contamination=0.05)
)
anomalies = outlier_detector.fit_detect(ts)
```

#### RegressionAD - 迴歸模型異常檢測
```python
from sklearn.linear_model import LinearRegression
from adtk.detector import RegressionAD

# 使用線性迴歸預測並檢測異常
regression_ad = RegressionAD(
    regressor=LinearRegression(),
    target="value",
    c=3.0
)

# 準備特徵（例如：時間特徵）
df = pd.DataFrame({
    'value': ts.values,
    'hour': ts.index.hour,
    'dayofweek': ts.index.dayofweek
}, index=ts.index)

anomalies = regression_ad.fit_detect(df)
```

### 2. 自動編碼器異常檢測

```python
from adtk.detector import AutoregressionAD

# 自回歸模型
autoregression_ad = AutoregressionAD(
    n_steps=10,  # 使用過去10個時間點
    step_size=1,
    c=3.0
)
anomalies = autoregression_ad.fit_detect(ts)
```

### 3. 管道（Pipeline）組合

```python
from adtk.pipe import Pipeline

# 建立複雜的異常檢測管道
steps = [
    # 步驟1: 移動平均平滑
    ("rolling_mean", RollingAggregate(agg='mean', window=5)),
    
    # 步驟2: 計算殘差
    ("residual", ResidualTransformer()),
    
    # 步驟3: 標準化
    ("standardize", StandardScaler()),
    
    # 步驟4: 異常檢測
    ("detector", ThresholdAD(high=3, low=-3))
]

pipeline = Pipeline(steps)
anomalies = pipeline.fit_detect(ts)
```

### 4. 特徵工程與機器學習結合

```python
# 創建多維特徵
def create_features(ts):
    df = pd.DataFrame(index=ts.index)
    df['value'] = ts.values
    
    # 時間特徵
    df['hour'] = ts.index.hour
    df['dayofweek'] = ts.index.dayofweek
    df['month'] = ts.index.month
    
    # 滾動統計特徵
    df['rolling_mean'] = ts.rolling(window=24).mean()
    df['rolling_std'] = ts.rolling(window=24).std()
    df['rolling_min'] = ts.rolling(window=24).min()
    df['rolling_max'] = ts.rolling(window=24).max()
    
    # 滯後特徵
    for i in [1, 24, 48]:
        df[f'lag_{i}'] = ts.shift(i)
    
    return df.dropna()

# 使用隨機森林進行異常檢測
from sklearn.ensemble import IsolationForest

features_df = create_features(ts)
iso_forest = OutlierDetector(
    IsolationForest(contamination=0.05, random_state=42)
)
anomalies = iso_forest.fit_detect(features_df)
```

---

## 視覺化

```python
# 基本視覺化
plot(ts, anomaly=anomalies, ts_linewidth=1, ts_markersize=3)

# 多個異常檢測結果比較
fig, axes = plt.subplots(3, 1, figsize=(15, 10))

# 原始數據
plot(ts, ax=axes[0], title="Original Time Series")

# 檢測結果1
plot(ts, anomaly=anomalies1, ax=axes[1], title="Method 1")

# 檢測結果2
plot(ts, anomaly=anomalies2, ax=axes[2], title="Method 2")

plt.tight_layout()
plt.show()
```

## 聚合器（Aggregator）

```python
# OR 聚合器 - 任一檢測器發現異常即為異常
or_aggregator = OrAggregator()
combined_anomalies = or_aggregator.aggregate([anomalies1, anomalies2])

# AND 聚合器 - 所有檢測器都發現異常才是異常
and_aggregator = AndAggregator()
combined_anomalies = and_aggregator.aggregate([anomalies1, anomalies2])
```

## 實用技巧

### 1. 處理多變量時間序列
```python
# 多變量時間序列
multivariate_ts = pd.DataFrame({
    'var1': ts1,
    'var2': ts2,
    'var3': ts3
}, index=dates)

# 對每個變量分別檢測
anomalies = {}
detector = ThresholdAD(high=3, low=-3)

for col in multivariate_ts.columns:
    anomalies[col] = detector.detect(multivariate_ts[col])
```

### 2. 自訂異常檢測器
```python
from adtk.detector import Detector

class CustomDetector(Detector):
    def __init__(self, threshold=3):
        self.threshold = threshold
        
    def _fit(self, ts):
        self.mean_ = ts.mean()
        self.std_ = ts.std()
        
    def _predict(self, ts):
        z_scores = np.abs((ts - self.mean_) / self.std_)
        return z_scores > self.threshold
```

### 3. 保存和載入模型
```python
import pickle

# 保存模型
with open('detector_model.pkl', 'wb') as f:
    pickle.dump(detector, f)

# 載入模型
with open('detector_model.pkl', 'rb') as f:
    loaded_detector = pickle.load(f)
```

## 常用參數說明

- **window**: 滑動窗口大小
- **c**: 控制敏感度的係數（越大越不敏感）
- **freq**: 季節性頻率
- **contamination**: 預期異常比例
- **alpha**: 顯著性水平
- **n_steps**: 自回歸步數
- **agg**: 聚合函數（'mean', 'median', 'sum', 'std', 'var', 'max', 'min'）