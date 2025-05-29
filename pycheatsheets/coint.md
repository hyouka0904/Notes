# Python Coint 套件 Cheat Sheet
## 時間序列分析與機器學習應用

### 📚 基本概念

**共整合（Cointegration）**: 兩個或多個非平穩時間序列的線性組合可能是平穩的，這種關係稱為共整合。

### 🔧 安裝與導入

```python
# 安裝
pip install statsmodels

# 主要導入
import numpy as np
import pandas as pd
from statsmodels.tsa.stattools import coint, adfuller
from statsmodels.tsa.vector_ar.vecm import coint_johansen
import matplotlib.pyplot as plt
import seaborn as sns
```

### 📊 單純時間序列分析

#### 1. 平穩性檢定（ADF Test）

```python
# ADF 檢定 - 檢查序列是否平穩
def check_stationarity(series, name='Series'):
    result = adfuller(series)
    print(f'=== {name} ADF Test ===')
    print(f'ADF Statistic: {result[0]:.6f}')
    print(f'p-value: {result[1]:.6f}')
    print(f'Critical Values:')
    for key, value in result[4].items():
        print(f'\t{key}: {value:.3f}')
    
    if result[1] <= 0.05:
        print(f"結論: {name} 是平穩的 (拒絕單根假設)")
    else:
        print(f"結論: {name} 是非平穩的 (無法拒絕單根假設)")
    return result[1] <= 0.05

# 使用範例
is_stationary = check_stationarity(df['price'], 'Stock Price')
```

#### 2. Engle-Granger 兩步法共整合檢定

```python
# 兩個序列的共整合檢定
def engle_granger_coint(y1, y2, alpha=0.05):
    """
    執行 Engle-Granger 共整合檢定
    返回: (檢定統計量, p值, 是否共整合)
    """
    coint_stat, p_value, crit_values = coint(y1, y2)
    
    print(f'共整合檢定統計量: {coint_stat:.6f}')
    print(f'p-value: {p_value:.6f}')
    print(f'臨界值 (5%): {crit_values[1]:.6f}')
    
    is_cointegrated = p_value < alpha
    print(f'結論: {"存在" if is_cointegrated else "不存在"}共整合關係')
    
    return coint_stat, p_value, is_cointegrated

# 使用範例
stat, pval, is_coint = engle_granger_coint(series1, series2)
```

#### 3. Johansen 多變量共整合檢定

```python
def johansen_test(df, det_order=0, k_ar_diff=1):
    """
    Johansen 共整合檢定
    det_order: -1 (無常數項), 0 (有常數項), 1 (有趨勢項)
    """
    result = coint_johansen(df, det_order, k_ar_diff)
    
    # 跡檢定 (Trace Test)
    print('=== 跡檢定 (Trace Test) ===')
    trace_stats = result.lr1
    trace_crit = result.cvt
    
    for i in range(len(trace_stats)):
        print(f'r <= {i}: 統計量={trace_stats[i]:.2f}, 臨界值(5%)={trace_crit[i, 1]:.2f}')
        if trace_stats[i] > trace_crit[i, 1]:
            print(f'  -> 拒絕 H0，至少存在 {i+1} 個共整合向量')
    
    # 最大特徵值檢定
    print('\n=== 最大特徵值檢定 ===')
    eigen_stats = result.lr2
    eigen_crit = result.cvm
    
    for i in range(len(eigen_stats)):
        print(f'r = {i}: 統計量={eigen_stats[i]:.2f}, 臨界值(5%)={eigen_crit[i, 1]:.2f}')
    
    return result

# 使用範例
data = pd.DataFrame({'x1': series1, 'x2': series2, 'x3': series3})
johansen_result = johansen_test(data)
```

### 🤖 機器學習結合時間序列分析

#### 1. 特徵工程 - 共整合特徵

```python
class CointegrationFeatures:
    def __init__(self, window_size=60):
        self.window_size = window_size
        
    def compute_spread(self, y1, y2):
        """計算價差（spread）"""
        # OLS 回歸找最佳係數
        from sklearn.linear_model import LinearRegression
        model = LinearRegression()
        model.fit(y1.values.reshape(-1, 1), y2.values)
        beta = model.coef_[0]
        alpha = model.intercept_
        
        # 計算價差
        spread = y2 - beta * y1 - alpha
        return spread, beta, alpha
    
    def rolling_coint_pvalue(self, df, col1, col2):
        """滾動窗口共整合 p-value"""
        p_values = []
        
        for i in range(self.window_size, len(df)):
            window_data1 = df[col1].iloc[i-self.window_size:i]
            window_data2 = df[col2].iloc[i-self.window_size:i]
            
            try:
                _, p_value, _ = coint(window_data1, window_data2)
                p_values.append(p_value)
            except:
                p_values.append(np.nan)
        
        return pd.Series(p_values, index=df.index[self.window_size:])
    
    def create_features(self, df, pairs):
        """創建共整合相關特徵"""
        features = pd.DataFrame(index=df.index)
        
        for col1, col2 in pairs:
            # 價差特徵
            spread, beta, alpha = self.compute_spread(df[col1], df[col2])
            features[f'spread_{col1}_{col2}'] = spread
            
            # 價差統計特徵
            features[f'spread_zscore_{col1}_{col2}'] = (
                (spread - spread.rolling(self.window_size).mean()) / 
                spread.rolling(self.window_size).std()
            )
            
            # 滾動共整合 p-value
            features[f'coint_pvalue_{col1}_{col2}'] = self.rolling_coint_pvalue(
                df, col1, col2
            )
        
        return features

# 使用範例
coint_fe = CointegrationFeatures(window_size=30)
pairs = [('stock1', 'stock2'), ('stock1', 'stock3')]
features = coint_fe.create_features(df, pairs)
```

#### 2. 配對交易策略 ML 模型

```python
class PairTradingML:
    def __init__(self, lookback=60, threshold=0.05):
        self.lookback = lookback
        self.threshold = threshold
        self.model = None
        
    def prepare_data(self, df, pair):
        """準備訓練數據"""
        from sklearn.preprocessing import StandardScaler
        
        # 基礎特徵
        features = pd.DataFrame(index=df.index)
        
        # 價格比率
        features['price_ratio'] = df[pair[0]] / df[pair[1]]
        
        # 共整合檢定 p-value
        coint_pvals = []
        for i in range(self.lookback, len(df)):
            window = slice(i-self.lookback, i)
            _, pval, _ = coint(df[pair[0]].iloc[window], 
                               df[pair[1]].iloc[window])
            coint_pvals.append(pval)
        
        features['coint_pvalue'] = pd.Series(
            coint_pvals, 
            index=df.index[self.lookback:]
        )
        
        # 技術指標
        for col in pair:
            # 移動平均
            features[f'{col}_ma20'] = df[col].rolling(20).mean()
            features[f'{col}_ma60'] = df[col].rolling(60).mean()
            
            # RSI
            features[f'{col}_rsi'] = self.calculate_rsi(df[col])
            
            # 波動率
            features[f'{col}_volatility'] = df[col].rolling(20).std()
        
        # 標籤：未來收益
        features['future_return'] = (
            (df[pair[0]].shift(-5) / df[pair[0]] - 1) - 
            (df[pair[1]].shift(-5) / df[pair[1]] - 1)
        )
        
        return features.dropna()
    
    def calculate_rsi(self, series, period=14):
        """計算 RSI"""
        delta = series.diff()
        gain = (delta.where(delta > 0, 0)).rolling(window=period).mean()
        loss = (-delta.where(delta < 0, 0)).rolling(window=period).mean()
        rs = gain / loss
        rsi = 100 - (100 / (1 + rs))
        return rsi
    
    def train_model(self, features, target_col='future_return'):
        """訓練預測模型"""
        from sklearn.ensemble import RandomForestRegressor
        from sklearn.model_selection import train_test_split
        
        # 準備訓練數據
        X = features.drop(columns=[target_col])
        y = features[target_col]
        
        # 分割數據
        X_train, X_test, y_train, y_test = train_test_split(
            X, y, test_size=0.2, shuffle=False
        )
        
        # 訓練模型
        self.model = RandomForestRegressor(
            n_estimators=100,
            max_depth=10,
            random_state=42
        )
        self.model.fit(X_train, y_train)
        
        # 評估
        train_score = self.model.score(X_train, y_train)
        test_score = self.model.score(X_test, y_test)
        
        print(f'訓練 R²: {train_score:.4f}')
        print(f'測試 R²: {test_score:.4f}')
        
        # 特徵重要性
        feature_importance = pd.DataFrame({
            'feature': X.columns,
            'importance': self.model.feature_importances_
        }).sort_values('importance', ascending=False)
        
        return feature_importance
    
    def generate_signals(self, features):
        """生成交易信號"""
        if self.model is None:
            raise ValueError("模型尚未訓練")
        
        X = features.drop(columns=['future_return'])
        predictions = self.model.predict(X)
        
        # 生成信號
        signals = pd.DataFrame(index=features.index)
        signals['prediction'] = predictions
        signals['signal'] = 0
        
        # 只在共整合關係顯著時交易
        coint_mask = features['coint_pvalue'] < self.threshold
        
        # 買入信號（預測正收益且共整合）
        signals.loc[
            (predictions > 0.01) & coint_mask, 'signal'
        ] = 1
        
        # 賣出信號（預測負收益且共整合）
        signals.loc[
            (predictions < -0.01) & coint_mask, 'signal'
        ] = -1
        
        return signals

# 使用範例
pair_ml = PairTradingML(lookback=60)
features = pair_ml.prepare_data(df, ('AAPL', 'MSFT'))
importance = pair_ml.train_model(features)
signals = pair_ml.generate_signals(features)
```

#### 3. VECM (向量誤差修正模型) 預測

```python
from statsmodels.tsa.vector_ar.vecm import VECM

def vecm_forecast(data, coint_rank, steps=10):
    """
    使用 VECM 模型進行預測
    """
    # 建立 VECM 模型
    model = VECM(data, k_ar_diff=1, coint_rank=coint_rank)
    vecm_fit = model.fit()
    
    print(vecm_fit.summary())
    
    # 預測
    forecast = vecm_fit.predict(steps=steps)
    
    # 將預測結果轉換為 DataFrame
    forecast_df = pd.DataFrame(
        forecast,
        columns=data.columns,
        index=pd.date_range(
            start=data.index[-1] + pd.Timedelta(days=1),
            periods=steps,
            freq='D'
        )
    )
    
    return vecm_fit, forecast_df

# 使用範例
# 先用 Johansen 檢定確定共整合秩
johansen_result = johansen_test(data)
coint_rank = 1  # 根據檢定結果設定

vecm_model, predictions = vecm_forecast(data, coint_rank, steps=30)
```

### 📈 實戰範例：完整的共整合分析流程

```python
class CointegrationAnalysis:
    def __init__(self, data):
        self.data = data
        self.results = {}
        
    def full_analysis(self, cols):
        """執行完整的共整合分析"""
        print("=== 1. 平穩性檢定 ===")
        for col in cols:
            is_stationary = check_stationarity(self.data[col], col)
            self.results[f'{col}_stationary'] = is_stationary
        
        print("\n=== 2. 一階差分平穩性檢定 ===")
        for col in cols:
            diff_series = self.data[col].diff().dropna()
            is_stationary = check_stationarity(diff_series, f'{col}_diff')
            self.results[f'{col}_diff_stationary'] = is_stationary
        
        print("\n=== 3. 兩兩共整合檢定 ===")
        from itertools import combinations
        pairs = list(combinations(cols, 2))
        
        coint_matrix = pd.DataFrame(
            index=cols, columns=cols, dtype=float
        )
        
        for col1, col2 in pairs:
            _, pval, _ = coint(self.data[col1], self.data[col2])
            coint_matrix.loc[col1, col2] = pval
            coint_matrix.loc[col2, col1] = pval
            print(f'{col1}-{col2}: p-value = {pval:.4f}')
        
        self.results['coint_matrix'] = coint_matrix
        
        print("\n=== 4. Johansen 多變量檢定 ===")
        johansen_result = johansen_test(self.data[cols])
        self.results['johansen'] = johansen_result
        
        return self.results
    
    def plot_analysis(self, pair):
        """視覺化分析結果"""
        fig, axes = plt.subplots(3, 1, figsize=(12, 10))
        
        # 1. 原始序列
        ax1 = axes[0]
        self.data[pair[0]].plot(ax=ax1, label=pair[0])
        self.data[pair[1]].plot(ax=ax1, label=pair[1], secondary_y=True)
        ax1.set_title('原始時間序列')
        ax1.legend(loc='upper left')
        
        # 2. 標準化序列
        ax2 = axes[1]
        norm_data = (self.data[list(pair)] - self.data[list(pair)].mean()) / self.data[list(pair)].std()
        norm_data.plot(ax=ax2)
        ax2.set_title('標準化序列')
        ax2.legend()
        
        # 3. 價差
        ax3 = axes[2]
        spread, _, _ = CointegrationFeatures().compute_spread(
            self.data[pair[0]], self.data[pair[1]]
        )
        spread.plot(ax=ax3)
        ax3.axhline(y=spread.mean(), color='r', linestyle='--', label='Mean')
        ax3.axhline(y=spread.mean() + 2*spread.std(), color='g', linestyle='--', label='+2σ')
        ax3.axhline(y=spread.mean() - 2*spread.std(), color='g', linestyle='--', label='-2σ')
        ax3.set_title('價差 (Spread)')
        ax3.legend()
        
        plt.tight_layout()
        return fig

# 使用範例
analyzer = CointegrationAnalysis(df)
results = analyzer.full_analysis(['stock1', 'stock2', 'stock3'])
fig = analyzer.plot_analysis(('stock1', 'stock2'))
```

### 🎯 最佳實踐與注意事項

1. **數據預處理**
   - 確保時間序列沒有缺失值
   - 對價格數據取對數可能改善結果
   - 注意數據頻率的一致性

2. **檢定順序**
   - 先檢定平穩性（ADF test）
   - 若非平穩，檢查是否 I(1) 過程
   - 再進行共整合檢定

3. **參數選擇**
   - 滯後階數：使用 AIC/BIC 準則
   - 窗口大小：根據數據頻率調整（日頻：60-252）
   - 顯著性水平：通常使用 5%

4. **機器學習應用**
   - 共整合 p-value 作為特徵
   - 價差的統計特性作為特徵
   - 結合技術指標提升預測

5. **風險管理**
   - 監控共整合關係的穩定性
   - 設置止損和風險限制
   - 定期重新校準模型

### 🔗 相關資源

- [Statsmodels 文檔](https://www.statsmodels.org/)
- [共整合理論](https://en.wikipedia.org/wiki/Cointegration)
- [VECM 模型](https://www.statsmodels.org/stable/vector_ar.html#vector-error-correction-models-vecm)