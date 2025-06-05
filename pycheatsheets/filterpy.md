````markdown
<!-- 安裝與導入 -->
## 安裝與導入
在使用 FilterPy 之前，請先安裝套件：
```bash
pip install filterpy
````

在程式中導入 FilterPy 的常用模組：

```python
# Import core filter classes
from filterpy.kalman import KalmanFilter, ExtendedKalmanFilter, UnscentedKalmanFilter
from filterpy.common import Q_discrete_white_noise
from filterpy.monte_carlo import systematic_resample, stratified_resample
from filterpy.monte_carlo import residual_resample
from filterpy.kalman import JosephFormKalmanFilter

# For array and matrix operations
import numpy as np

# For plotting
import matplotlib.pyplot as plt
```

---

<!-- 濾波基本概念 -->

## 濾波基本概念

* 卡爾曼濾波器（Kalman Filter）：適用於線性高斯模型，估計隱藏狀態。
* 擴展卡爾曼濾波器（EKF）：線性化非線性系統，用雅可比矩陣近似。
* 無跡卡爾曼濾波器（UKF）：透過採樣點（sigma points）傳播非線性，而非線性化。
* 粒子濾波（Particle Filter）：以蒙地卡羅方法近似後驗分佈，適用於高度非線性或非高斯情況。

在機器學習結合時間序列分析時，經常會用到以下概念：

1. **狀態轉移方程**：𝑥ₖ = 𝐹ₖ·𝑥ₖ₋₁ + 𝑢ₖ
2. **觀測方程**：𝑧ₖ = 𝐻ₖ·𝑥ₖ + 𝑣ₖ

   * 𝑥ₖ：隱藏狀態向量
   * 𝑧ₖ：觀測值向量
   * 𝐹ₖ：狀態轉移矩陣
   * 𝐻ₖ：觀測矩陣
   * 𝑢ₖ：過程噪聲（通常假設為零均值、高斯）
   * 𝑣ₖ：觀測噪聲（通常假設為零均值、高斯）

---

<!-- 一維卡爾曼濾波器 -->

## 一維卡爾曼濾波器（1D Kalman Filter）

### 初始化

```python
from filterpy.kalman import KalmanFilter
import numpy as np

# 建立一維 Kalman Filter（假設狀態包含位置與速度，可擴展為 2 維）
kf = KalmanFilter(dim_x=2, dim_z=1)

# 狀態轉移矩陣 F
dt = 1.0  # 時間間隔
kf.F = np.array([[1, dt],
                 [0,  1]])

# 觀測矩陣 H（只觀測位置）
kf.H = np.array([[1, 0]])

# 初始狀態估計 x
kf.x = np.array([[0.],   # 初始位置
                 [1.]])  # 初始速度

# 狀態協方差矩陣 P（初始化不確定度較大）
kf.P = np.array([[1000., 0.],
                 [0., 1000.]])

# 觀測噪聲協方差 R（假設觀測誤差為 10）
kf.R = np.array([[10.]])

# 過程噪聲協方差 Q（設為小幅度白噪聲）
q = 0.01
kf.Q = Q_discrete_white_noise(dim=2, dt=dt, var=q)

# 控制輸入矩陣 B（若無則設為零矩陣）
kf.B = None
```

### 更新與預測

```python
# 假設有觀測值 z_list
z_list = [1., 2., 3., 2., 1.5]

# 儲存估計結果
estimates = []

for z in z_list:
    # 預測步驟
    kf.predict()
    # 更新步驟（傳入觀測值）
    kf.update(np.array([z]))
    # 儲存此時間點估計的狀態
    estimates.append(kf.x.copy())

# 將 estimates 轉為 numpy array，方便後續繪圖
estimates = np.array(estimates).squeeze()
```

### 繪圖

```python
time_steps = np.arange(len(z_list))

plt.figure(figsize=(10, 6))
plt.plot(time_steps, [z for z in z_list], 'ro', label='觀測值')
plt.plot(time_steps, estimates[:, 0], 'b-', label='估計位置')
plt.xlabel('時間步')
plt.ylabel('位置')
plt.legend()
plt.title('一維卡爾曼濾波示例')
plt.show()
```

---

<!-- 多維卡爾曼濾波器 -->

## 多維卡爾曼濾波器（Multidimensional Kalman Filter）

### 2D 位置與速度範例

假設同時追蹤 x 與 y 方向的運動，狀態向量為 \[x, x\_dot, y, y\_dot]。

```python
from filterpy.kalman import KalmanFilter
import numpy as np

# 狀態維度為 4，觀測維度為 2（同時觀測 x, y 位置）
kf = KalmanFilter(dim_x=4, dim_z=2)

dt = 1.0
kf.F = np.array([[1, dt, 0,  0],
                 [0,  1, 0,  0],
                 [0,  0, 1, dt],
                 [0,  0, 0,  1]])

# 觀測矩陣 H（只觀測 x, y 位置）
kf.H = np.array([[1, 0, 0, 0],
                 [0, 0, 1, 0]])

# 初始狀態
kf.x = np.array([[0.],
                 [1.],
                 [0.],
                 [1.]])  # 假設初始位置 (0,0)，速度 (1,1)

# 狀態協方差 P
kf.P = np.eye(4) * 500.

# 觀測噪聲 R（x, y 方向均假設為誤差 5）
kf.R = np.array([[5., 0.],
                 [0., 5.]])

# 過程噪聲 Q
q = 0.1
q_matrix = Q_discrete_white_noise(dim=2, dt=dt, var=q)
kf.Q = np.block([[q_matrix, np.zeros((2, 2))],
                 [np.zeros((2, 2)), q_matrix]])

# 控制矩陣若無可省略
kf.B = None
```

### 更新與預測

```python
# 假設有觀測值 z_list，其中每個元素為 [x_obs, y_obs]
z_list = [
    [1., 1.], [2., 2.], [3., 3.], [4., 3.5], [5., 4.5]
]

estimates = []

for z in z_list:
    kf.predict()
    kf.update(np.array(z))
    estimates.append(kf.x.copy())

estimates = np.array(estimates).squeeze()
```

### 繪圖

```python
time_steps = np.arange(len(z_list))

plt.figure(figsize=(10, 6))
plt.plot([z[0] for z in z_list], [z[1] for z in z_list],
         'ro', label='觀測值')
plt.plot(estimates[:, 0], estimates[:, 2], 'b-', label='估計軌跡')
plt.xlabel('X 位置')
plt.ylabel('Y 位置')
plt.legend()
plt.title('二維卡爾曼濾波示例')
plt.show()
```

---

<!-- 擴展卡爾曼濾波器 -->

## 擴展卡爾曼濾波器（Extended Kalman Filter，EKF）

### 系統模型

* 非線性狀態轉移 𝑥ₖ = 𝑓(𝑥ₖ₋₁, 𝑢ₖ) + 𝑢ₖ\_noise
* 非線性觀測 𝑧ₖ = 𝑕(𝑥ₖ) + 𝑣ₖ\_noise

需要定義：

1. `f(x, u)`：狀態轉移函數
2. `F_jacobian(x, u)`：狀態轉移函數的雅可比矩陣
3. `h(x)`：觀測函數
4. `H_jacobian(x)`：觀測函數的雅可比矩陣

### 範例：極座標轉換

假設狀態為 \[x, y, x\_dot, y\_dot]，觀測為 \[距離, 夾角]。

```python
from filterpy.kalman import ExtendedKalmanFilter
import numpy as np

def fx(x, dt):
    """State transition function for constant velocity model."""
    F = np.array([[1, 0, dt, 0],
                  [0, 1, 0,  dt],
                  [0, 0, 1,  0],
                  [0, 0, 0,  1]])
    return F @ x

def F_jacobian(x, dt):
    """Jacobian of the state transition function."""
    return np.array([[1, 0, dt, 0],
                     [0, 1, 0,  dt],
                     [0, 0, 1,  0],
                     [0, 0, 0,  1]])

def hx(x):
    """Measurement function converting Cartesian to polar."""
    px, py, vx, vy = x.flatten()
    rho = np.sqrt(px**2 + py**2)
    phi = np.arctan2(py, px)
    return np.array([rho, phi])

def H_jacobian(x):
    """Jacobian of the measurement function."""
    px, py, vx, vy = x.flatten()
    rho2 = px**2 + py**2
    rho = np.sqrt(rho2)
    if rho2 == 0:
        return np.zeros((2, 4))
    return np.array([
        [px/rho, py/rho, 0,      0],
        [-py/rho2, px/rho2, 0, 0]
    ])

# 初始化 EKF
ekf = ExtendedKalmanFilter(dim_x=4, dim_z=2)
ekf.x = np.array([[1.], [1.], [1.], [0.]])  # 初始狀態
ekf.P = np.eye(4) * 500.
ekf.R = np.diag([5., 0.1])  # 觀測噪聲
ekf.Q = Q_discrete_white_noise(dim=4, dt=1.0, var=0.1)  # 簡化版本

# 更新與預測範例
measurements = [
    np.array([1.414, 0.785]),  # 假設極座標 (距離, 夾角)
    np.array([2.236, 0.785]),
    np.array([3.162, 0.785])
]

estimates = []
dt = 1.0

for z in measurements:
    # 傳入雅可比矩陣函數
    ekf.F = F_jacobian(ekf.x, dt)
    ekf.predict_update(z, HJacobian=H_jacobian, Hx=hx, args=(), hx_args=())
    estimates.append(ekf.x.copy())

estimates = np.array(estimates).squeeze()
```

---

<!-- 無跡卡爾曼濾波器 -->

## 無跡卡爾曼濾波器（Unscented Kalman Filter，UKF）

### 系統模型

同 EKF，但 UKF 不需定義雅可比矩陣，而是透過 sigma points 傳播非線性。

需要定義：

1. `fx(x, dt)`：狀態轉移函數
2. `hx(x)`：觀測函數

### UKF 初始化範例

```python
from filterpy.kalman import MerweScaledSigmaPoints, UnscentedKalmanFilter
import numpy as np

# 狀態維度與觀測維度
dim_x = 4
dim_z = 2

# 傳統常數速度模型（同 EKF 示例）
def fx(x, dt):
    F = np.array([[1, 0, dt, 0],
                  [0, 1, 0,  dt],
                  [0, 0, 1,  0],
                  [0, 0, 0,  1]])
    return F @ x

def hx(x):
    px, py, vx, vy = x.flatten()
    rho = np.sqrt(px**2 + py**2)
    phi = np.arctan2(py, px)
    return np.array([rho, phi])

# 產生 sigma points
points = MerweScaledSigmaPoints(n=dim_x, alpha=0.1, beta=2., kappa=1.)

# 初始化 UKF
ukf = UnscentedKalmanFilter(dim_x=dim_x, dim_z=dim_z, fx=fx, hx=hx,
                            dt=1.0, points=points)

ukf.x = np.array([1., 1., 1., 0.])  # 初始狀態
ukf.P = np.eye(dim_x) * 500.
ukf.R = np.diag([5., 0.1])  # 觀測噪聲
ukf.Q = np.eye(dim_x) * 0.1  # 過程噪聲

# 更新與預測範例
measurements = [
    np.array([1.414, 0.785]),
    np.array([2.236, 0.785]),
    np.array([3.162, 0.785])
]

estimates = []

for z in measurements:
    ukf.predict()
    ukf.update(z)
    estimates.append(ukf.x.copy())

estimates = np.array(estimates).squeeze()
```

---

<!-- 粒子濾波（Particle Filter） -->

## 粒子濾波（Particle Filter）

粒子濾波以一組粒子（samples）代表後驗分佈，逐步進行以下步驟：

1. **初始化粒子**：根據先驗分佈隨機取 N 個粒子。
2. **預測步驟**：對每個粒子施加狀態轉移與隨機噪聲。
3. **更新權重**：根據觀測 likelihood 更新每個粒子的權重。
4. **重採樣**：若有效粒子數（Effective N）太低，則按權重重採樣。

### 範例：一維隨機行走

```python
import numpy as np
from filterpy.monte_carlo import systematic_resample

# 狀態維度 1，一維隨機行走：x_k = x_{k-1} + noise
def predict(particles, dt=1.0, process_var=1.0):
    # 對每個粒子加入隨機過程噪聲
    return particles + np.random.normal(0, np.sqrt(process_var), size=particles.shape)

def update(particles, weights, z, meas_var=2.0):
    # 計算每個粒子的觀測 likelihood（假設觀測 z = true x + 考慮高斯噪聲）
    weights *= np.exp(-0.5 * (particles - z)**2 / meas_var)
    weights += 1.e-300  # 防止權重為 0
    weights /= np.sum(weights)
    return weights

def effective_n(weights):
    return 1.0 / np.sum(weights**2)

# 初始化粒子與權重
N = 1000
particles = np.random.normal(0, 10, size=N)
weights = np.ones(N) / N

# 模擬觀測值（假設真實值逐步增加）
true_states = np.linspace(0, 10, 10)
measurements = true_states + np.random.normal(0, 2, size=true_states.shape)

estimates = []

for z in measurements:
    # 預測
    particles = predict(particles, process_var=1.0)
    # 更新權重
    weights = update(particles, weights, z, meas_var=2.0)
    # 計算有效粒子數
    if effective_n(weights) < N/2:
        indices = systematic_resample(weights)
        particles = particles[indices]
        weights = np.ones(N) / N
    # 粒子平均作為估計
    estimates.append(np.average(particles, weights=weights))

estimates = np.array(estimates)
```

### 粒子濾波繪圖

```python
plt.figure(figsize=(10, 6))
plt.plot(true_states, 'g-', label='真實值')
plt.plot(measurements, 'ro', label='觀測值')
plt.plot(estimates, 'b-', label='粒子濾波估計')
plt.xlabel('時間步')
plt.ylabel('狀態值')
plt.legend()
plt.title('一維粒子濾波示例')
plt.show()
```

---

<!-- Joseph 格式卡爾曼濾波器 -->

## Joseph 格式卡爾曼濾波器（Joseph Form KF）

Joseph 格式可以在數值不穩定時提供更好的協方差更新。

```python
from filterpy.kalman import JosephFormKalmanFilter
import numpy as np

# 初始化
jf_kf = JosephFormKalmanFilter(dim_x=2, dim_z=1)
jf_kf.F = np.array([[1, dt],
                   [0,  1]])
jf_kf.H = np.array([[1, 0]])
jf_kf.x = np.array([[0.], [1.]])
jf_kf.P = np.eye(2) * 1000.
jf_kf.R = np.array([[10.]])
jf_kf.Q = Q_discrete_white_noise(dim=2, dt=dt, var=0.01)

# 更新與預測流程同 KalmanFilter
for z in z_list:
    jf_kf.predict()
    jf_kf.update(np.array([z]))
```

---

<!-- 常用函數與工具 -->

## 常用函數與工具

### `Q_discrete_white_noise`

* 用途：產生離散白噪聲的過程噪聲協方差矩陣。
* 參數：

  * `dim`：狀態維度（例如 1、2…）。
  * `dt`：時間間隔。
  * `var`：噪聲變異數。

```python
from filterpy.common import Q_discrete_white_noise

Q = Q_discrete_white_noise(dim=2, dt=1.0, var=0.1)
```

### 重採樣函數

* `systematic_resample(weights)`：Systematic 重採樣。
* `stratified_resample(weights)`：分層重採樣。
* `residual_resample(weights)`：殘差重採樣。

```python
from filterpy.monte_carlo import systematic_resample

indices = systematic_resample(weights)
particles = particles[indices]
```

---

<!-- 在機器學習中的應用 -->

## 在機器學習與時間序列分析中的應用

1. **噪聲數據去除**：將物理量或金融時間序列中的測量誤差最小化，得到平滑曲線。
2. **特徵工程**：利用卡爾曼濾波生成平滑特徵、殘差誤差作為特徵。
3. **參數估計**：在隱狀態空間模型（State Space Model）下估計系統參數（如 ARIMA）。
4. **異常檢測**：監控估計誤差殘差，若超過閾值則判定為異常。
5. **加速度計與陀螺儀融合**：在機器人與自駕車中，將多種感測器資料融合，得到穩定姿態估計。

---

<!-- 細節與提示 -->

## 細節與提示

* **初始參數選擇**：協方差矩陣 P、過程噪聲 Q、觀測噪聲 R 對結果影響顯著，需根據實際資料調參。
* **數值穩定**：若發現 P 矩陣變成奇異，可嘗試 Joseph 格式或增加小量對角項（如 `P += 1e-6 * np.eye(dim)`）。
* **時間序列平滑**：若資料頻率不固定，可動態調整 dt。
* **多模型融合**：可將 EKF 與 UKF 結合於同一框架中，根據非線性程度選擇適合的濾波器。
* **向量化運算**：若有多序列要同時計算，可考慮批次運行，減少 Python 迴圈開銷。

---

<!-- 常見錯誤排解 -->

## 常見錯誤排解

1. **維度不匹配**：

   * `dim_x` 與 `dim_z` 應嚴格對應狀態與觀測方程維度。
   * `F`、`H`、`Q`、`R` 大小需要匹配 `dim_x`、`dim_z`。

2. **權重為零（Particle Filter）**：

   * 更新後若所有權重都接近 0，可嘗試增加測量噪聲變異數或使用其他重採樣方法。

3. **協方差變為負**：

   * 推斷模型假設錯誤，或數值誤差導致，建議檢查 `Q`、`R` 設定並使用 JosephFormKalmanFilter。

4. **觀測函數非線性錯誤（EKF/UKF）**：

   * 若 EKF Jacobian 計算錯誤，會導致濾波不收斂，建議多做單步測試。
   * 若無跡卡爾曼濾波器發散，可調整 sigma points 參數（`alpha`、`beta`、`kappa`）。

---

<!-- 範例整合：機器學習與時間序列分析 -->

## 範例整合：機器學習與時間序列分析

### 具體示例：匯率預測與噪聲濾除

```python
import numpy as np
import pandas as pd
from filterpy.kalman import KalmanFilter, Q_discrete_white_noise
import matplotlib.pyplot as plt

# 範例假設：每日匯率時間序列（USD/TWD），並具有雜訊
# Create synthetic time series data
np.random.seed(42)
days = 100
true_rate = np.linspace(30, 32, days)  # 線性上升趨勢
noise = np.random.normal(0, 0.2, size=days)
measured_rate = true_rate + noise

# 初始化 Kalman Filter，狀態為 [匯率, 匯率變化率]
kf = KalmanFilter(dim_x=2, dim_z=1)
dt = 1.0
kf.F = np.array([[1, dt],
                 [0,  1]])
kf.H = np.array([[1, 0]])
kf.x = np.array([[measured_rate[0]],
                 [0.]])
kf.P = np.eye(2) * 50.
kf.R = np.array([[0.04]])  # 觀測噪聲小一點
kf.Q = Q_discrete_white_noise(dim=2, dt=dt, var=0.01)

estimates = []
for z in measured_rate:
    kf.predict()
    kf.update(np.array([z]))
    estimates.append(kf.x[0, 0])

# 繪圖
plt.figure(figsize=(12, 6))
plt.plot(range(days), measured_rate, 'ro', markersize=4, label='觀測匯率')
plt.plot(range(days), true_rate, 'k--', label='真實匯率')
plt.plot(range(days), estimates, 'b-', label='卡爾曼濾波估計')
plt.xlabel('天數')
plt.ylabel('匯率 (USD/TWD)')
plt.legend()
plt.title('卡爾曼濾波在匯率時間序列分析中的應用')
plt.show()
```

---

<!-- 專案結構與檔案示例 -->

## 專案結構與檔案示例

```
my_filterpy_project/
├── data/
│   └── exchange_rates.csv   # 原始時間序列資料
├── notebooks/
│   └── kalman_filter.ipynb  # Jupyter 筆記本示例
├── src/
│   ├── kf_models.py         # 定義各種 Kalman Filter 模型與函數
│   ├── ekf_models.py        # 擴展卡爾曼濾波模型
│   ├── ukf_models.py        # 無跡卡爾曼濾波模型
│   └── particle_filter.py   # 粒子濾波實現
├── requirements.txt         # FilterPy 與其他套件依賴
└── README.md                # 專案說明
```

### `src/kf_models.py` 範例

```python
import numpy as np
from filterpy.kalman import KalmanFilter
from filterpy.common import Q_discrete_white_noise

def create_1d_kf(dt=1.0, process_var=0.01, meas_var=10.0):
    """
    Create 1D Kalman Filter for position & velocity.
    State: [position, velocity]
    """
    kf = KalmanFilter(dim_x=2, dim_z=1)
    kf.F = np.array([[1, dt],
                     [0,  1]])
    kf.H = np.array([[1, 0]])
    kf.x = np.array([[0.], [0.]])
    kf.P = np.eye(2) * 1000.
    kf.R = np.array([[meas_var]])
    kf.Q = Q_discrete_white_noise(dim=2, dt=dt, var=process_var)
    return kf

def run_1d_kf(kf, measurements):
    """
    Run the filter over a sequence of measurements.
    Returns list of estimated positions.
    """
    estimates = []
    for z in measurements:
        kf.predict()
        kf.update(np.array([z]))
        estimates.append(kf.x[0, 0])
    return np.array(estimates)
```

---
