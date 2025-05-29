# Astropy 時間序列分析 Cheat Sheet

## 1. 基礎設定與匯入

```python
# 基本匯入
import numpy as np
import pandas as pd
from astropy.time import Time, TimeDelta
from astropy.timeseries import TimeSeries, BinnedTimeSeries
from astropy.timeseries import aggregate_downsample
from astropy import units as u
from astropy.stats import sigma_clip, median_absolute_deviation
from astropy.timeseries import LombScargle
from astropy.table import Table

# 機器學習相關
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
import matplotlib.pyplot as plt
```

## 2. 時間處理基礎

### 時間物件建立與轉換
```python
# 建立時間物件
t1 = Time('2024-01-01T00:00:00', format='isot', scale='utc')
t2 = Time(['2024-01-01', '2024-01-02', '2024-01-03'], format='iso')

# 從 MJD (Modified Julian Date) 建立
t_mjd = Time(59945.0, format='mjd')

# 從 Unix timestamp 建立
t_unix = Time(1704067200, format='unix')

# 時間格式轉換
t1.iso  # ISO 格式
t1.mjd  # MJD 格式
t1.datetime  # Python datetime
t1.unix  # Unix timestamp

# 時間差計算
dt = t2[1] - t2[0]  # TimeDelta 物件
dt.to(u.hour)  # 轉換為小時
dt.to(u.day)   # 轉換為天
```

### 時間序列建立
```python
# 建立等間隔時間序列
start_time = Time('2024-01-01T00:00:00', scale='utc')
dt = TimeDelta(1*u.hour)
times = start_time + dt * np.arange(100)

# 建立不規則時間序列
random_times = start_time + TimeDelta(np.sort(np.random.uniform(0, 10, 50))*u.day)
```

## 3. TimeSeries 物件操作

### 建立 TimeSeries
```python
# 從頭建立
ts = TimeSeries(time=times)
ts['flux'] = np.random.normal(100, 10, len(times))
ts['flux_err'] = np.ones(len(times)) * 0.5

# 從 pandas DataFrame 建立
df = pd.DataFrame({
    'time': pd.date_range('2024-01-01', periods=100, freq='h'),
    'value': np.random.randn(100)
})
ts_from_df = TimeSeries.from_pandas(df, time_column='time')

# 從檔案讀取
ts = TimeSeries.read('data.fits', format='fits')
```

### 資料存取與操作
```python
# 存取欄位
flux_values = ts['flux']
time_values = ts.time

# 新增欄位
ts['normalized_flux'] = ts['flux'] / np.median(ts['flux'])

# 篩選資料
mask = ts['flux'] > 100
ts_filtered = ts[mask]

# 時間範圍篩選
t_start = Time('2024-01-01T12:00:00')
t_end = Time('2024-01-02T12:00:00')
ts_subset = ts[(ts.time >= t_start) & (ts.time <= t_end)]
```

## 4. 時間序列分析功能

### 週期性分析 (Lomb-Scargle)
```python
# 準備資料
t = np.random.uniform(0, 10, 100)
y = np.sin(2 * np.pi * t) + 0.1 * np.random.randn(100)
dy = 0.1 * np.ones_like(y)

# 建立 Lomb-Scargle 週期圖
frequency, power = LombScargle(t, y, dy).autopower()

# 找出最佳週期
best_frequency = frequency[np.argmax(power)]
best_period = 1 / best_frequency

# 計算特定頻率的功率
ls = LombScargle(t, y, dy)
freq_grid = np.linspace(0.1, 10, 1000)
power_grid = ls.power(freq_grid)

# False Alarm Probability
fap = ls.false_alarm_probability(power.max())
```

### 資料合併與重採樣
```python
# 下採樣
ts_downsampled = aggregate_downsample(ts, time_bin_size=1*u.hour)

# 建立 BinnedTimeSeries
bts = BinnedTimeSeries(
    time_bin_start=times[:-1],
    time_bin_end=times[1:],
    flux=np.random.normal(100, 10, len(times)-1)
)

# 合併多個 TimeSeries
ts1 = TimeSeries(time=times[:50], flux=np.random.randn(50))
ts2 = TimeSeries(time=times[40:], flux=np.random.randn(60))
ts_merged = ts1.join(ts2, join_type='outer')
```

### 統計分析
```python
# Sigma clipping (移除離群值)
filtered_data = sigma_clip(ts['flux'], sigma=3, maxiters=5)
ts_clean = ts[~filtered_data.mask]

# 計算 MAD (Median Absolute Deviation)
mad = median_absolute_deviation(ts['flux'])

# 移動統計
window_size = 10
ts['rolling_mean'] = pd.Series(ts['flux']).rolling(window_size).mean()
ts['rolling_std'] = pd.Series(ts['flux']).rolling(window_size).std()
```

## 5. 機器學習整合

### 特徵工程
```python
def extract_features(ts):
    """從時間序列提取特徵"""
    features = {}
    
    # 基本統計特徵
    features['mean'] = np.mean(ts['flux'])
    features['std'] = np.std(ts['flux'])
    features['skew'] = pd.Series(ts['flux']).skew()
    features['kurtosis'] = pd.Series(ts['flux']).kurtosis()
    
    # 時間特徵
    features['duration'] = (ts.time[-1] - ts.time[0]).to(u.day).value
    features['n_points'] = len(ts)
    
    # 週期性特徵 (使用 Lomb-Scargle)
    t = (ts.time - ts.time[0]).to(u.day).value
    frequency, power = LombScargle(t, ts['flux']).autopower()
    features['best_period'] = 1 / frequency[np.argmax(power)]
    features['max_power'] = np.max(power)
    
    return features

# 對多個時間序列提取特徵
feature_list = []
for i, ts in enumerate(timeseries_list):
    features = extract_features(ts)
    features['id'] = i
    feature_list.append(features)

features_df = pd.DataFrame(feature_list)
```

### 時間序列分類範例
```python
# 準備資料
X = features_df.drop(['id'], axis=1)
y = labels  # 假設有標籤

# 標準化
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# 分割資料
X_train, X_test, y_train, y_test = train_test_split(
    X_scaled, y, test_size=0.2, random_state=42
)

# 訓練模型 (以 RandomForest 為例)
from sklearn.ensemble import RandomForestClassifier
clf = RandomForestClassifier(n_estimators=100, random_state=42)
clf.fit(X_train, y_train)
```

### 時間序列預測準備
```python
def prepare_ml_data(ts, window_size=10, horizon=1):
    """準備監督式學習資料"""
    flux = ts['flux']
    X, y = [], []
    
    for i in range(len(flux) - window_size - horizon + 1):
        X.append(flux[i:i+window_size])
        y.append(flux[i+window_size+horizon-1])
    
    return np.array(X), np.array(y)

# 使用範例
X, y = prepare_ml_data(ts, window_size=24, horizon=6)

# 加入時間特徵
def add_time_features(ts, X):
    """加入時間相關特徵"""
    time_features = []
    for i in range(len(X)):
        t = ts.time[i]
        features = [
            t.datetime.hour,
            t.datetime.dayofweek,
            t.datetime.month,
            np.sin(2*np.pi*t.datetime.hour/24),  # 週期性編碼
            np.cos(2*np.pi*t.datetime.hour/24)
        ]
        time_features.append(features)
    
    return np.hstack([X, np.array(time_features)])
```

## 6. 實用函數集

### 資料品質檢查
```python
def check_data_quality(ts):
    """檢查時間序列資料品質"""
    report = {}
    
    # 缺失值
    report['missing_values'] = np.sum(np.isnan(ts['flux']))
    report['missing_ratio'] = report['missing_values'] / len(ts)
    
    # 時間間隔
    dt = np.diff(ts.time.mjd)
    report['median_dt'] = np.median(dt)
    report['irregular_sampling'] = np.std(dt) > 0.01
    
    # 離群值
    filtered = sigma_clip(ts['flux'], sigma=5)
    report['n_outliers'] = np.sum(filtered.mask)
    
    return report
```

### 時間序列視覺化
```python
def plot_timeseries(ts, feature='flux', errors=None):
    """繪製時間序列"""
    fig, ax = plt.subplots(figsize=(12, 6))
    
    # 主要資料
    ax.plot(ts.time.datetime, ts[feature], 'b-', alpha=0.7, label=feature)
    
    # 誤差條
    if errors and errors in ts.colnames:
        ax.errorbar(ts.time.datetime, ts[feature], 
                   yerr=ts[errors], fmt='none', alpha=0.3)
    
    # 格式設定
    ax.set_xlabel('Time')
    ax.set_ylabel(feature)
    ax.legend()
    plt.xticks(rotation=45)
    plt.tight_layout()
    return fig, ax
```

### 批次處理
```python
def batch_process_timeseries(file_list, process_func):
    """批次處理多個時間序列檔案"""
    results = []
    
    for file in file_list:
        try:
            ts = TimeSeries.read(file)
            result = process_func(ts)
            results.append({
                'file': file,
                'result': result,
                'status': 'success'
            })
        except Exception as e:
            results.append({
                'file': file,
                'error': str(e),
                'status': 'failed'
            })
    
    return pd.DataFrame(results)
```

## 7. 常見應用場景

### 天文光變曲線分析
```python
# 讀取光變曲線
lc = TimeSeries.read('lightcurve.fits')

# 去趨勢
from scipy.signal import savgol_filter
lc['detrended'] = lc['flux'] - savgol_filter(lc['flux'], 101, 3)

# 尋找週期
ls = LombScargle(lc.time.mjd, lc['detrended'])
frequency, power = ls.autopower(maximum_frequency=10)
best_period = 1/frequency[np.argmax(power)]

# 相位摺疊
phase = ((lc.time.mjd - lc.time.mjd[0]) % best_period) / best_period
```

### 異常檢測
```python
from sklearn.ensemble import IsolationForest

# 準備特徵
features = np.column_stack([
    ts['flux'],
    pd.Series(ts['flux']).rolling(5).mean(),
    pd.Series(ts['flux']).rolling(5).std()
])

# 訓練異常檢測模型
iso_forest = IsolationForest(contamination=0.1, random_state=42)
anomalies = iso_forest.fit_predict(features)

# 標記異常點
ts['is_anomaly'] = anomalies == -1
```

## 8. 效能優化建議

1. **大型資料集處理**
   - 使用 `memmap=True` 讀取大檔案
   - 考慮使用 `BinnedTimeSeries` 減少資料點
   - 利用 `chunk` 參數分批處理

2. **平行處理**
   ```python
   from multiprocessing import Pool
   
   def process_single_ts(filename):
       ts = TimeSeries.read(filename)
       return extract_features(ts)
   
   with Pool(processes=4) as pool:
       results = pool.map(process_single_ts, file_list)
   ```

3. **記憶體管理**
   - 使用 `del` 刪除不需要的變數
   - 考慮使用 generator 處理大量檔案
   - 適時使用 `gc.collect()`

## 9. 常用資源連結

- [Astropy TimeSeries 文件](https://docs.astropy.org/en/stable/timeseries/)
- [Lomb-Scargle 週期圖教學](https://docs.astropy.org/en/stable/timeseries/lombscargle.html)
- [Astropy 統計工具](https://docs.astropy.org/en/stable/stats/)