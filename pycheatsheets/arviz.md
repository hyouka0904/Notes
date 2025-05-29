# ArviZ 時間序列分析 Cheat Sheet

## 基本設置與導入

```python
import arviz as az
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 設置 ArviZ 樣式
az.style.use("arviz-darkgrid")
az.rcParams["plot.max_subplots"] = 40

# 設置隨機種子
np.random.seed(42)
```

## 1. 數據準備與轉換

### 從時間序列創建 InferenceData
```python
# 從 PyMC 模型創建
idata = az.from_pymc(trace, model=model)

# 從 Stan 結果創建
idata = az.from_cmdstanpy(fit)

# 從字典創建（適用於自定義時間序列數據）
idata = az.from_dict({
    'posterior': {
        'beta': beta_samples,  # (chain, draw, feature)
        'sigma': sigma_samples,  # (chain, draw)
        'trend': trend_samples,  # (chain, draw, time)
    },
    'observed_data': {
        'y': y_observed,  # 觀測時間序列
    },
    'posterior_predictive': {
        'y_pred': y_pred_samples,  # 後驗預測
    }
})
```

### 添加時間坐標
```python
# 為時間序列添加時間索引
time_coords = pd.date_range('2020-01-01', periods=len(y), freq='D')
idata.posterior = idata.posterior.assign_coords(time=time_coords)
idata.observed_data = idata.observed_data.assign_coords(time=time_coords)
```

## 2. 模型診斷

### MCMC 診斷
```python
# 基本診斷統計
print(az.summary(idata, var_names=['beta', 'sigma']))

# R-hat 診斷（應該接近 1.0）
az.rhat(idata, var_names=['beta', 'sigma'])

# 有效樣本數
az.ess(idata, var_names=['beta', 'sigma'])

# 能量診斷（僅適用於 HMC/NUTS）
az.bfmi(idata)  # 貝葉斯分數匹配指數
```

### 可視化診斷
```python
# Trace plots - 檢查收斂性
az.plot_trace(idata, var_names=['beta', 'sigma'], compact=True)

# 自相關圖
az.plot_autocorr(idata, var_names=['beta'], max_lag=50)

# Rank plots - 檢查鏈間混合
az.plot_rank(idata, var_names=['beta', 'sigma'])

# 能量圖（HMC/NUTS）
az.plot_energy(idata)
```

## 3. 參數可視化

### 後驗分佈
```python
# 森林圖 - 適合比較多個參數
az.plot_forest(idata, var_names=['beta'], 
               coords={'feature': ['trend', 'seasonality', 'holiday']})

# 密度圖
az.plot_density(idata, var_names=['sigma'], hdi_prob=0.95)

# 小提琴圖
az.plot_violin(idata, var_names=['beta'])

# 配對圖 - 檢查參數相關性
az.plot_pair(idata, var_names=['beta'], 
             coords={'feature': ['trend', 'seasonality']})
```

### 時間序列特定可視化
```python
# 趨勢分量可視化
az.plot_posterior(idata, var_names=['trend'], 
                  coords={'time': slice(0, 50)})

# 季節性分量
az.plot_trace(idata, var_names=['seasonal'], 
              coords={'season': [0, 1, 2, 3]})
```

## 4. 模型比較與選擇

### 信息準則比較
```python
# WAIC 比較
waic_results = az.compare({
    'ARIMA': idata_arima,
    'Prophet': idata_prophet,
    'LSTM_Bayes': idata_lstm
}, ic='waic')

# LOO-CV 比較
loo_results = az.compare({
    'Model_1': idata1,
    'Model_2': idata2
}, ic='loo')

# 可視化比較
az.plot_compare(waic_results)
```

### 模型權重
```python
# 計算模型權重（基於 WAIC）
model_weights = az.weight_predictions([idata1, idata2, idata3])
print(f"模型權重: {model_weights}")
```

## 5. 預測分析

### 後驗預測檢查
```python
# 後驗預測檢查
az.plot_ppc(idata, num_pp_samples=100, 
            observed_rug=True, mean=False)

# 時間序列特定 PPC
az.plot_ppc(idata, num_pp_samples=50, 
            group='posterior_predictive',
            data_pairs={'y': 'y_pred'})
```

### 預測區間可視化
```python
# HDI 預測區間
az.plot_hdi(y_time, idata.posterior_predictive['y_pred'], 
            hdi_prob=0.95, color='blue', fill_kwargs={'alpha':0.3})

# 預測 vs 實際對比
fig, ax = plt.subplots(figsize=(12, 6))
az.plot_hdi(time_coords, idata.posterior_predictive['y_pred'], 
            ax=ax, hdi_prob=[0.5, 0.95])
ax.plot(time_coords, y_observed, 'k-', label='實際觀測')
ax.legend()
```

## 6. 機器學習 + 時間序列特殊功能

### 特徵重要性分析
```python
# 特徵係數的後驗分佈
feature_names = ['trend', 'seasonality', 'holiday', 'weather', 'promotion']
az.plot_forest(idata, var_names=['beta'], 
               coords={'feature': feature_names},
               combined=True, hdi_prob=0.95)

# 特徵選擇概率
inclusion_prob = (idata.posterior['beta'].abs() > threshold).mean(dim=['chain', 'draw'])
print(f"特徵包含概率: {inclusion_prob}")
```

### 時變參數可視化
```python
# 時變係數
az.plot_trace(idata, var_names=['beta_time'], 
              coords={'time': slice(0, 100, 10)})

# 時變參數的 HDI 區間
time_varying_beta = idata.posterior['beta_time']
az.plot_hdi(time_coords, time_varying_beta, 
            hdi_prob=0.9, smooth=True)
```

### 異常檢測
```python
# 基於預測殘差的異常檢測
residuals = y_observed - idata.posterior_predictive['y_pred'].mean(dim=['chain', 'draw'])
anomaly_threshold = np.percentile(np.abs(residuals), 95)
anomalies = np.abs(residuals) > anomaly_threshold

# 可視化異常點
fig, ax = plt.subplots(figsize=(12, 6))
ax.plot(time_coords, y_observed, 'b-', label='觀測值')
ax.scatter(time_coords[anomalies], y_observed[anomalies], 
           c='red', s=50, label='異常點')
ax.legend()
```

## 7. 模型性能評估

### 預測準確性指標
```python
# 計算預測指標
y_pred_mean = idata.posterior_predictive['y_pred'].mean(dim=['chain', 'draw'])

def calculate_metrics(y_true, y_pred):
    mae = np.mean(np.abs(y_true - y_pred))
    rmse = np.sqrt(np.mean((y_true - y_pred)**2))
    mape = np.mean(np.abs((y_true - y_pred) / y_true)) * 100
    return {'MAE': mae, 'RMSE': rmse, 'MAPE': mape}

metrics = calculate_metrics(y_test, y_pred_mean)
print(f"預測指標: {metrics}")
```

### 殘差分析
```python
# 殘差分佈
residuals = y_observed - y_pred_mean
az.plot_density({'residuals': residuals[None, None, :]})

# QQ 圖檢查殘差正態性
from scipy import stats
stats.probplot(residuals, dist="norm", plot=plt)
plt.title("殘差 Q-Q 圖")
```

## 8. 進階可視化技巧

### 自定義主題和樣式
```python
# 設置自定義樣式
custom_style = {
    'axes.facecolor': 'white',
    'figure.facecolor': 'white',
    'axes.edgecolor': 'black',
    'axes.linewidth': 0.8,
    'axes.spines.left': True,
    'axes.spines.bottom': True,
    'axes.spines.top': False,
    'axes.spines.right': False,
}

with az.rc_context(custom_style):
    az.plot_trace(idata, var_names=['beta'])
```

### 多子圖布局
```python
# 創建自定義多子圖布局
fig, axes = plt.subplots(2, 2, figsize=(15, 10))

# 第一個子圖：原始時間序列
axes[0,0].plot(time_coords, y_observed)
axes[0,0].set_title('原始時間序列')

# 第二個子圖：預測結果
az.plot_hdi(time_coords, idata.posterior_predictive['y_pred'], 
            ax=axes[0,1], hdi_prob=0.95)
axes[0,1].set_title('預測結果')

# 第三個子圖：參數後驗
az.plot_density(idata, var_names=['sigma'], ax=axes[1,0])
axes[1,0].set_title('誤差項後驗分佈')

# 第四個子圖：殘差
axes[1,1].scatter(y_pred_mean, residuals)
axes[1,1].set_title('殘差圖')
axes[1,1].set_xlabel('預測值')
axes[1,1].set_ylabel('殘差')

plt.tight_layout()
```

## 9. 實用工具函數

### 快速診斷函數
```python
def quick_diagnostics(idata, var_names=None):
    """快速模型診斷"""
    print("=== 模型診斷摘要 ===")
    
    # 基本統計
    summary = az.summary(idata, var_names=var_names)
    print("參數摘要:")
    print(summary)
    
    # R-hat 檢查
    rhat_max = az.rhat(idata, var_names=var_names).max()
    print(f"\n最大 R-hat: {rhat_max:.3f}")
    if rhat_max > 1.1:
        print("⚠️  警告: R-hat > 1.1，可能未收斂")
    
    # 有效樣本數檢查
    ess_min = az.ess(idata, var_names=var_names).min()
    print(f"最小有效樣本數: {ess_min:.0f}")
    if ess_min < 100:
        print("⚠️  警告: 有效樣本數 < 100")

# 使用示例
quick_diagnostics(idata, var_names=['beta', 'sigma'])
```

### 批量可視化函數
```python
def plot_timeseries_results(idata, y_observed, time_coords, 
                           figsize=(15, 12)):
    """批量生成時間序列分析結果圖"""
    
    fig, axes = plt.subplots(3, 2, figsize=figsize)
    
    # Trace plot
    az.plot_trace(idata, var_names=['beta'], ax=axes[0,:])
    
    # 預測結果
    az.plot_hdi(time_coords, idata.posterior_predictive['y_pred'], 
                ax=axes[1,0], hdi_prob=0.95)
    axes[1,0].plot(time_coords, y_observed, 'k-', alpha=0.7)
    axes[1,0].set_title('預測 vs 實際')
    
    # 後驗預測檢查
    az.plot_ppc(idata, num_pp_samples=50, ax=axes[1,1])
    
    # 參數後驗分佈
    az.plot_forest(idata, var_names=['beta'], ax=axes[2,0])
    
    # 自相關圖
    az.plot_autocorr(idata, var_names=['beta'], ax=axes[2,1])
    
    plt.tight_layout()
    return fig

# 使用示例
fig = plot_timeseries_results(idata, y_observed, time_coords)
plt.show()
```

## 10. 常見問題解決

### 內存優化
```python
# 減少內存使用
idata_thin = idata.sel(draw=slice(None, None, 2))  # 每隔一個樣本

# 只保留需要的變量
idata_subset = idata[['posterior', 'observed_data']]

# 轉換為更節省內存的格式
idata.to_netcdf('model_results.nc')  # 保存到磁盤
idata_loaded = az.load_arviz_data('model_results.nc')  # 從磁盤加載
```

### 處理大型時間序列
```python
# 分批處理大型時間序列
def process_large_timeseries(idata, batch_size=1000):
    n_times = idata.posterior.dims['time']
    results = []
    
    for i in range(0, n_times, batch_size):
        batch = idata.sel(time=slice(i, i+batch_size))
        batch_result = az.summary(batch)
        results.append(batch_result)
    
    return pd.concat(results)

# 使用示例
summary_results = process_large_timeseries(idata)
```

---

## 參考資源

- [ArviZ 官方文檔](https://arviz-devs.github.io/arviz/)
- [PyMC 時間序列教程](https://www.pymc.io/projects/examples/en/latest/time_series/index.html)
- [貝葉斯時間序列分析最佳實踐](https://betanalpha.github.io/assets/case_studies/time_series.html)