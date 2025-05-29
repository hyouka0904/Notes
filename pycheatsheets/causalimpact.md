# Python CausalImpact Cheat Sheet

## 安裝與導入

```python
# 安裝
pip install pycausalimpact

# 導入必要套件
from causalimpact import CausalImpact
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from datetime import datetime, timedelta
```

## 基本概念

CausalImpact 用於評估干預（intervention）對時間序列的因果效應：
- **Pre-period**: 干預前的時期（訓練期）
- **Post-period**: 干預後的時期（評估期）
- **Counterfactual**: 如果沒有干預會發生什麼

## 資料準備

### 基本資料格式
```python
# 建立時間索引
dates = pd.date_range(start='2023-01-01', end='2023-12-31', freq='D')

# 準備資料（必須是 DataFrame，索引為時間）
data = pd.DataFrame({
    'y': np.random.randn(len(dates)) * 10 + 100,  # 目標變數
    'x1': np.random.randn(len(dates)) * 5 + 50,   # 控制變數1
    'x2': np.random.randn(len(dates)) * 3 + 30    # 控制變數2
}, index=dates)

# 定義干預時間點
intervention_date = '2023-09-01'
pre_period = ['2023-01-01', '2023-08-31']
post_period = ['2023-09-01', '2023-12-31']
```

---

## 情境一：單純時間序列分析

### 1. 基本使用
```python
# 只有目標變數（無控制變數）
simple_data = pd.DataFrame({
    'y': data['y']
}, index=data.index)

# 建立模型
ci_simple = CausalImpact(simple_data, pre_period, post_period)

# 查看結果摘要
print(ci_simple.summary())
print(ci_simple.summary(output='report'))
```

### 2. 使用控制變數
```python
# 加入控制變數以提高預測準確度
ci_with_controls = CausalImpact(data, pre_period, post_period)

# 視覺化結果
ci_with_controls.plot()
```

### 3. 自定義模型參數
```python
# 自定義 BSTS 模型參數
ci_custom = CausalImpact(
    data, 
    pre_period, 
    post_period,
    model_args={
        'niter': 2000,           # MCMC 迭代次數
        'standardize_data': True, # 標準化資料
        'prior_level_sd': 0.01,   # 先驗標準差
        'nseasons': 7,           # 季節性週期（如週數據）
        'season_duration': 1      # 季節持續時間
    }
)
```

### 4. 處理季節性
```python
# 針對有明顯季節性的資料
seasonal_data = data.copy()
seasonal_data['y'] = (
    100 + 
    10 * np.sin(2 * np.pi * np.arange(len(dates)) / 365) +  # 年度季節性
    5 * np.sin(2 * np.pi * np.arange(len(dates)) / 7) +     # 週季節性
    np.random.randn(len(dates)) * 2
)

ci_seasonal = CausalImpact(
    seasonal_data,
    pre_period,
    post_period,
    model_args={'nseasons': 52, 'season_duration': 7}  # 週季節性
)
```

---

## 情境二：機器學習結合時間序列分析

### 1. 特徵工程 + CausalImpact
```python
# 建立機器學習特徵
def create_ml_features(df):
    df_feat = df.copy()
    
    # 滯後特徵
    for lag in [1, 7, 14, 30]:
        df_feat[f'y_lag_{lag}'] = df_feat['y'].shift(lag)
    
    # 移動平均
    for window in [7, 30]:
        df_feat[f'y_ma_{window}'] = df_feat['y'].rolling(window).mean()
        df_feat[f'y_std_{window}'] = df_feat['y'].rolling(window).std()
    
    # 時間特徵
    df_feat['day_of_week'] = df_feat.index.dayofweek
    df_feat['day_of_month'] = df_feat.index.day
    df_feat['month'] = df_feat.index.month
    
    return df_feat.dropna()

# 準備特徵資料
ml_data = create_ml_features(data)
```

### 2. 使用 ML 預測作為控制變數
```python
from sklearn.ensemble import RandomForestRegressor
from sklearn.preprocessing import StandardScaler

# 分割訓練資料（只使用 pre-period）
train_mask = (ml_data.index >= pre_period[0]) & (ml_data.index <= pre_period[1])
X_train = ml_data.loc[train_mask, ml_data.columns != 'y']
y_train = ml_data.loc[train_mask, 'y']

# 訓練 ML 模型
rf_model = RandomForestRegressor(n_estimators=100, random_state=42)
rf_model.fit(X_train, y_train)

# 生成 ML 預測作為控制變數
ml_data['ml_prediction'] = rf_model.predict(ml_data[ml_data.columns != 'y'])

# 使用 ML 預測作為控制變數
ci_ml = CausalImpact(
    ml_data[['y', 'ml_prediction']], 
    pre_period, 
    post_period
)
```

### 3. 集成方法：多模型預測
```python
from sklearn.linear_model import LinearRegression
from sklearn.svm import SVR

# 訓練多個模型
models = {
    'rf': RandomForestRegressor(n_estimators=100),
    'lr': LinearRegression(),
    'svr': SVR(kernel='rbf')
}

# 生成多個預測
for name, model in models.items():
    model.fit(X_train, y_train)
    ml_data[f'pred_{name}'] = model.predict(ml_data[ml_data.columns.difference(['y'])])

# 使用所有預測作為控制變數
control_cols = [col for col in ml_data.columns if col.startswith('pred_')]
ci_ensemble = CausalImpact(
    ml_data[['y'] + control_cols],
    pre_period,
    post_period
)
```

### 4. 深度學習時間序列預測
```python
# 使用 Prophet 作為範例
from prophet import Prophet

# 準備 Prophet 資料格式
prophet_data = pd.DataFrame({
    'ds': data.index,
    'y': data['y'].values
})

# 訓練 Prophet 模型
prophet_model = Prophet(
    yearly_seasonality=True,
    weekly_seasonality=True,
    daily_seasonality=False
)
prophet_model.fit(prophet_data[prophet_data['ds'] < intervention_date])

# 預測
future = prophet_model.make_future_dataframe(periods=len(post_period))
forecast = prophet_model.predict(future)

# 將 Prophet 預測加入資料
data['prophet_pred'] = forecast.set_index('ds')['yhat']

# 結合 CausalImpact
ci_prophet = CausalImpact(
    data[['y', 'prophet_pred', 'x1', 'x2']],
    pre_period,
    post_period
)
```

---

## 進階技巧

### 1. 交叉驗證評估
```python
def rolling_causal_impact(data, intervention_date, window_days=30):
    results = []
    
    for i in range(window_days):
        # 動態調整 pre/post period
        current_intervention = intervention_date + timedelta(days=i)
        pre = [data.index[0], current_intervention - timedelta(days=1)]
        post = [current_intervention, data.index[-1]]
        
        try:
            ci = CausalImpact(data, pre, post)
            effect = ci.summary_data['average']['rel_effect']
            results.append({
                'intervention_date': current_intervention,
                'relative_effect': effect
            })
        except:
            continue
    
    return pd.DataFrame(results)
```

### 2. 多重干預分析
```python
# 分析多個干預時間點
interventions = ['2023-03-01', '2023-06-01', '2023-09-01']

for i, intervention in enumerate(interventions):
    if i == 0:
        pre = [data.index[0], pd.to_datetime(intervention) - timedelta(days=1)]
    else:
        pre = [pd.to_datetime(interventions[i-1]), 
               pd.to_datetime(intervention) - timedelta(days=1)]
    
    if i == len(interventions) - 1:
        post = [pd.to_datetime(intervention), data.index[-1]]
    else:
        post = [pd.to_datetime(intervention), 
                pd.to_datetime(interventions[i+1]) - timedelta(days=1)]
    
    ci = CausalImpact(data, pre, post)
    print(f"\n干預 {intervention}:")
    print(ci.summary())
```

### 3. 信賴區間調整
```python
# 調整信賴區間（預設 95%）
ci_90 = CausalImpact(
    data, 
    pre_period, 
    post_period,
    alpha=0.1  # 90% 信賴區間
)

# 提取詳細結果
results = ci_90.inferences
print(f"平均因果效應: {results['average']['abs_effect']}")
print(f"累積因果效應: {results['cumulative']['abs_effect']}")
```

---

## 結果解讀

### 關鍵指標
```python
# 獲取所有統計結果
summary_data = ci.summary_data

# 主要指標
print(f"實際值: {summary_data['actual']['average']}")
print(f"預測值: {summary_data['predicted']['average']}")
print(f"絕對效應: {summary_data['abs_effect']['average']}")
print(f"相對效應: {summary_data['rel_effect']['average']}%")
print(f"後驗概率 (p-value): {summary_data['p_value']}")

# 判斷顯著性
is_significant = summary_data['p_value'] < 0.05
effect_direction = "正向" if summary_data['abs_effect']['average'] > 0 else "負向"
```

### 視覺化客製化
```python
# 自定義圖表
fig = ci.plot(figsize=(15, 10))

# 加入註解
import matplotlib.patches as patches
ax = fig.gca()
rect = patches.Rectangle(
    (pd.to_datetime(post_period[0]), ax.get_ylim()[0]),
    pd.to_datetime(post_period[1]) - pd.to_datetime(post_period[0]),
    ax.get_ylim()[1] - ax.get_ylim()[0],
    alpha=0.2,
    facecolor='red',
    label='干預期間'
)
ax.add_patch(rect)
ax.legend()
plt.tight_layout()
plt.show()
```

---

## 最佳實踐

1. **資料品質**
   - 確保時間序列連續，無缺失值
   - Pre-period 至少要有 3-4 倍的 post-period 長度
   - 控制變數不應受干預影響

2. **模型選擇**
   - 簡單趨勢：使用基本 CausalImpact
   - 複雜模式：加入 ML 預測作為控制變數
   - 高維度資料：先降維再分析

3. **驗證方法**
   - 使用假干預測試（placebo test）
   - 交叉驗證不同時間窗口
   - 比較多種模型結果

4. **注意事項**
   - 避免過度擬合（特別是 ML 模型）
   - 考慮外部因素影響
   - 結果解釋要謹慎，相關不等於因果