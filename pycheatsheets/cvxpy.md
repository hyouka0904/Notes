# CVXPY 時間序列分析 Cheat Sheet

## 基礎設定與導入

```python
import cvxpy as cp
import numpy as np
import pandas as pd
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split

# 常用設定
cp.settings.USE_INDIRECT = False  # 使用直接求解器
```

## 核心概念

### 1. 變數定義
```python
# 一維變數
x = cp.Variable(n)                    # n維向量
theta = cp.Variable(n, nonneg=True)   # 非負變數

# 多維變數
W = cp.Variable((m, n))               # m×n矩陣
beta = cp.Variable((p, 1))            # p×1向量

# 時間序列專用
states = cp.Variable((T, d))          # T個時間點，d維狀態
```

### 2. 約束條件
```python
# 基本約束
constraints = [
    x >= 0,                           # 非負約束
    cp.sum(x) == 1,                   # 總和約束
    cp.norm(x, 2) <= alpha,           # L2範數約束
    A @ x == b,                       # 線性等式約束
    x[:-1] <= x[1:]                   # 單調遞增約束
]
```

## 單純時間序列分析

### 1. 趨勢過濾 (Trend Filtering)
```python
def trend_filtering(y, lambda_param=1.0):
    """L1趨勢過濾，用於提取時間序列的趨勢"""
    n = len(y)
    x = cp.Variable(n)
    
    # 目標函數：擬合誤差 + 二階差分懲罰
    objective = cp.Minimize(
        cp.sum_squares(y - x) + 
        lambda_param * cp.norm(cp.diff(x, k=2), 1)
    )
    
    problem = cp.Problem(objective)
    problem.solve()
    return x.value
```

### 2. 異常檢測 (Anomaly Detection)
```python
def anomaly_detection(y, lambda_sparse=0.1):
    """稀疏加法模型：y = trend + seasonal + sparse"""
    n = len(y)
    trend = cp.Variable(n)
    sparse = cp.Variable(n)
    
    objective = cp.Minimize(
        cp.sum_squares(y - trend - sparse) +
        lambda_sparse * cp.norm(sparse, 1)
    )
    
    # 趨勢平滑約束
    constraints = [cp.norm(cp.diff(trend, k=2), 1) <= 1.0]
    
    problem = cp.Problem(objective, constraints)
    problem.solve()
    
    return trend.value, sparse.value
```

### 3. 時間序列分解
```python
def ts_decomposition(y, period=12, lambda_trend=1.0, lambda_season=10.0):
    """將時間序列分解為趨勢、季節性和殘差"""
    n = len(y)
    trend = cp.Variable(n)
    seasonal = cp.Variable(n)
    
    objective = cp.Minimize(
        cp.sum_squares(y - trend - seasonal) +
        lambda_trend * cp.norm(cp.diff(trend, k=2), 1) +
        lambda_season * cp.sum_squares(cp.diff(seasonal, k=period))
    )
    
    # 季節性約束：週期性
    constraints = []
    for i in range(period):
        if i + period < n:
            constraints.append(seasonal[i] == seasonal[i + period])
    
    problem = cp.Problem(objective, constraints)
    problem.solve()
    
    residual = y - trend.value - seasonal.value
    return trend.value, seasonal.value, residual
```

### 4. 平滑技術
```python
def hodrick_prescott_filter(y, lambda_param=1600):
    """Hodrick-Prescott濾波器"""
    n = len(y)
    trend = cp.Variable(n)
    
    objective = cp.Minimize(
        cp.sum_squares(y - trend) +
        lambda_param * cp.sum_squares(cp.diff(trend, k=2))
    )
    
    problem = cp.Problem(objective)
    problem.solve()
    return trend.value

def kalman_smoothing(y, Q=0.01, R=1.0):
    """簡化的Kalman平滑"""
    n = len(y)
    x = cp.Variable(n)
    
    # 狀態轉移平滑 + 觀測誤差
    objective = cp.Minimize(
        cp.sum_squares(cp.diff(x)) / Q +
        cp.sum_squares(y - x) / R
    )
    
    problem = cp.Problem(objective)
    problem.solve()
    return x.value
```

## 機器學習結合時間序列分析

### 1. 正則化時間序列預測
```python
def lasso_ar_model(y, p=5, lambda_param=0.1):
    """LASSO自回歸模型"""
    n = len(y)
    
    # 構建滯後矩陣
    X = np.zeros((n-p, p))
    for i in range(p):
        X[:, i] = y[p-i-1:n-i-1]
    y_target = y[p:]
    
    # LASSO回歸
    beta = cp.Variable(p)
    objective = cp.Minimize(
        cp.sum_squares(X @ beta - y_target) +
        lambda_param * cp.norm(beta, 1)
    )
    
    problem = cp.Problem(objective)
    problem.solve()
    return beta.value

def elastic_net_ts(X, y, alpha=1.0, l1_ratio=0.5):
    """Elastic Net時間序列回歸"""
    n_features = X.shape[1]
    beta = cp.Variable(n_features)
    
    l1_penalty = l1_ratio * cp.norm(beta, 1)
    l2_penalty = (1 - l1_ratio) * cp.sum_squares(beta)
    
    objective = cp.Minimize(
        cp.sum_squares(X @ beta - y) +
        alpha * (l1_penalty + l2_penalty)
    )
    
    problem = cp.Problem(objective)
    problem.solve()
    return beta.value
```

### 2. 特徵選擇與稀疏學習
```python
def sparse_feature_selection(X, y, k=10):
    """選擇k個最重要的特徵"""
    n_samples, n_features = X.shape
    beta = cp.Variable(n_features)
    z = cp.Variable(n_features, boolean=True)
    
    M = 1e6  # 大數
    
    objective = cp.Minimize(cp.sum_squares(X @ beta - y))
    constraints = [
        cp.sum(z) <= k,                    # 最多選k個特徵
        -M * z <= beta,                     # 如果z[i]=0, 則beta[i]=0
        beta <= M * z
    ]
    
    problem = cp.Problem(objective, constraints)
    problem.solve(solver=cp.GLPK_MI)
    return beta.value, z.value

def group_lasso_ts(X, y, groups, lambda_param=0.1):
    """群組LASSO用於時間序列特徵群組選擇"""
    n_features = X.shape[1]
    beta = cp.Variable(n_features)
    
    group_penalty = 0
    for group in groups:
        group_penalty += cp.norm(beta[group], 2)
    
    objective = cp.Minimize(
        cp.sum_squares(X @ beta - y) +
        lambda_param * group_penalty
    )
    
    problem = cp.Problem(objective)
    problem.solve()
    return beta.value
```

### 3. 魯棒時間序列模型
```python
def robust_regression(X, y, epsilon=0.1):
    """Huber回歸 - 對異常值魯棒"""
    n_features = X.shape[1]
    beta = cp.Variable(n_features)
    
    residual = X @ beta - y
    objective = cp.Minimize(cp.sum(cp.huber(residual, M=epsilon)))
    
    problem = cp.Problem(objective)
    problem.solve()
    return beta.value

def quantile_regression(X, y, tau=0.5):
    """分位數回歸"""
    n_features = X.shape[1]
    beta = cp.Variable(n_features)
    
    residual = y - X @ beta
    objective = cp.Minimize(
        cp.sum(cp.maximum(tau * residual, (tau - 1) * residual))
    )
    
    problem = cp.Problem(objective)
    problem.solve()
    return beta.value
```

### 4. 動態模型與狀態空間
```python
def dynamic_factor_model(Y, k=3, lambda_param=0.1):
    """動態因子模型
    Y: T×N 觀測矩陣
    k: 因子數量
    """
    T, N = Y.shape
    F = cp.Variable((T, k))  # 因子
    L = cp.Variable((N, k))  # 載荷
    
    # 因子動態約束
    factor_dynamics = cp.sum_squares(cp.diff(F, axis=0))
    
    objective = cp.Minimize(
        cp.sum_squares(Y - F @ L.T) +
        lambda_param * factor_dynamics
    )
    
    problem = cp.Problem(objective)
    problem.solve()
    return F.value, L.value

def tvp_regression(X, y, lambda_param=0.01):
    """時變參數(TVP)回歸"""
    T, p = X.shape
    beta = cp.Variable((T, p))
    
    # 擬合誤差
    fit_error = 0
    for t in range(T):
        fit_error += cp.square(y[t] - X[t] @ beta[t])
    
    # 參數平滑變化
    param_variation = cp.sum_squares(cp.diff(beta, axis=0))
    
    objective = cp.Minimize(fit_error + lambda_param * param_variation)
    
    problem = cp.Problem(objective)
    problem.solve()
    return beta.value
```

### 5. 多任務學習
```python
def multi_task_lasso(X_list, y_list, lambda_param=0.1):
    """多任務LASSO - 多個相關時間序列"""
    n_tasks = len(X_list)
    n_features = X_list[0].shape[1]
    
    W = cp.Variable((n_features, n_tasks))
    
    loss = 0
    for i in range(n_tasks):
        loss += cp.sum_squares(X_list[i] @ W[:, i] - y_list[i])
    
    # L2,1範數促進行稀疏
    regularizer = cp.sum([cp.norm(W[j, :], 2) for j in range(n_features)])
    
    objective = cp.Minimize(loss + lambda_param * regularizer)
    
    problem = cp.Problem(objective)
    problem.solve()
    return W.value
```

## 實用技巧與最佳實踐

### 1. 交叉驗證選擇參數
```python
def cv_select_lambda(X, y, lambdas, cv_folds=5):
    """使用交叉驗證選擇正則化參數"""
    from sklearn.model_selection import KFold
    
    kf = KFold(n_splits=cv_folds, shuffle=True, random_state=42)
    cv_errors = []
    
    for lambda_param in lambdas:
        fold_errors = []
        
        for train_idx, val_idx in kf.split(X):
            X_train, X_val = X[train_idx], X[val_idx]
            y_train, y_val = y[train_idx], y[val_idx]
            
            # 訓練模型
            beta = cp.Variable(X.shape[1])
            objective = cp.Minimize(
                cp.sum_squares(X_train @ beta - y_train) +
                lambda_param * cp.norm(beta, 1)
            )
            problem = cp.Problem(objective)
            problem.solve()
            
            # 驗證誤差
            val_error = np.mean((X_val @ beta.value - y_val)**2)
            fold_errors.append(val_error)
        
        cv_errors.append(np.mean(fold_errors))
    
    best_lambda = lambdas[np.argmin(cv_errors)]
    return best_lambda, cv_errors
```

### 2. 求解器選擇
```python
# 根據問題類型選擇求解器
solvers = {
    'QP': cp.OSQP,        # 二次規劃
    'LP': cp.ECOS,        # 線性規劃
    'SOCP': cp.ECOS,      # 二階錐規劃
    'SDP': cp.SCS,        # 半正定規劃
    'MIP': cp.GLPK_MI     # 混合整數規劃
}

# 設定求解器參數
problem.solve(
    solver=cp.OSQP,
    verbose=True,
    max_iter=10000,
    eps_abs=1e-6,
    eps_rel=1e-6
)
```

### 3. 診斷與除錯
```python
# 檢查問題狀態
print(f"Status: {problem.status}")
print(f"Optimal value: {problem.value}")
print(f"Solver: {problem.solver_stats.solver_name}")
print(f"Setup time: {problem.solver_stats.setup_time}")
print(f"Solve time: {problem.solver_stats.solve_time}")

# 檢查問題是否為凸
print(f"Is DCP: {problem.is_dcp()}")
print(f"Is DGP: {problem.is_dgp()}")
```

## 常見應用範例

### 1. 時間序列預測流程
```python
def ts_forecast_pipeline(data, test_size=0.2, p=10):
    """完整的時間序列預測流程"""
    # 準備數據
    train_size = int(len(data) * (1 - test_size))
    train, test = data[:train_size], data[train_size:]
    
    # 建立特徵
    X_train, y_train = create_lag_features(train, p)
    X_test, y_test = create_lag_features(test, p)
    
    # 標準化
    scaler = StandardScaler()
    X_train_scaled = scaler.fit_transform(X_train)
    X_test_scaled = scaler.transform(X_test)
    
    # 選擇最佳lambda
    lambdas = np.logspace(-3, 1, 20)
    best_lambda, _ = cv_select_lambda(X_train_scaled, y_train, lambdas)
    
    # 訓練最終模型
    beta = cp.Variable(p)
    objective = cp.Minimize(
        cp.sum_squares(X_train_scaled @ beta - y_train) +
        best_lambda * cp.norm(beta, 1)
    )
    problem = cp.Problem(objective)
    problem.solve()
    
    # 預測
    predictions = X_test_scaled @ beta.value
    mse = np.mean((predictions - y_test)**2)
    
    return beta.value, predictions, mse

def create_lag_features(data, p):
    """創建滯後特徵"""
    n = len(data)
    X = np.zeros((n-p, p))
    for i in range(p):
        X[:, i] = data[p-i-1:n-i-1]
    y = data[p:]
    return X, y
```

### 2. 異常檢測與預警系統
```python
def anomaly_alert_system(data, window_size=100, threshold=3):
    """實時異常檢測系統"""
    alerts = []
    
    for i in range(window_size, len(data)):
        window = data[i-window_size:i]
        
        # 使用魯棒統計方法
        trend, sparse = anomaly_detection(window)
        
        # 計算異常分數
        anomaly_score = np.abs(sparse[-1]) / np.std(window)
        
        if anomaly_score > threshold:
            alerts.append({
                'time': i,
                'value': data[i],
                'score': anomaly_score,
                'type': 'spike' if sparse[-1] > 0 else 'drop'
            })
    
    return alerts
```

## 性能優化建議

1. **使用暖啟動**
```python
# 對於相似問題，使用前一個解作為初始值
x.value = previous_solution
problem.solve(warm_start=True)
```

2. **問題重構**
```python
# 將大問題分解為子問題
# 使用ADMM等分散式優化方法
```

3. **向量化操作**
```python
# 避免循環，使用矩陣運算
# 錯誤：sum([cp.square(x[i]) for i in range(n)])
# 正確：cp.sum_squares(x)
```

4. **記憶體管理**
```python
# 對於大型問題，考慮稀疏矩陣
import scipy.sparse as sp
A_sparse = sp.csr_matrix(A)
```

## 參考資源

- CVXPY官方文檔: https://www.cvxpy.org/
- 時間序列分析應用: Boyd & Vandenberghe, "Convex Optimization"
- 機器學習優化: Hastie et al., "Statistical Learning with Sparsity"