## Python EMD-signal 套件 Cheat Sheet

以下為 Python EMD-signal 套件的快速參考說明，適用於機器學習結合時間序列分析，以及純粹時間序列分析的情境。  
整份 Cheat Sheet 約 400 行，涵蓋安裝、核心概念、API 用法、範例程式碼與使用建議。  

---

### 目錄

1. [套件介紹](#套件介紹)  
2. [安裝與導入](#安裝與導入)  
3. [核心概念](#核心概念)  
4. [主要功能與 API 一覽](#主要功能與-api-一覽)  
    1. [Empirical Mode Decomposition (EMD)](#emd)  
    2. [Ensemble EMD (EEMD)](#eemd)  
    3. [Complete Ensemble EMD with Adaptive Noise (CEEMDAN)](#ceemdan)  
    4. [其他相關演算法](#其他相關演算法)  
5. [API 參數詳解](#api-參數詳解)  
    1. [emd.EMD](#emdemd)  
    2. [eemd.EEMD](#eemdeemd)  
    3. [ceemdan.CEEMDAN](#ceemdanceemdan)  
    4. [其他輔助函式](#其他輔助函式)  
6. [基本使用範例](#基本使用範例)  
    1. [對單一時序信號進行 EMD 分解](#對單一時序信號進行-emd-分解)  
    2. [使用 CEEMDAN 進行去噪或特徵萃取](#使用-ceemdan-進行去噪或特徵萃取)  
7. [與機器學習結合](#與機器學習結合)  
    1. [特徵工程示範](#特徵工程示範)  
    2. [結合 scikit-learn 進行分類／回歸](#結合-scikit-learn-進行分類回歸)  
8. [純粹時間序列分析範例](#純粹時間序列分析範例)  
    1. [趨勢、季節性、殘差拆解](#趨勢季節性殘差拆解)  
    2. [金融資料示範（股價、匯率）](#金融資料示範股價匯率)  
    3. [心率、震動訊號分析](#心率震動訊號分析)  
9. [視覺化](#視覺化)  
    1. [原始信號與 IMF 繪製](#原始信號與-imf-繪製)  
    2. [殘餘項與重構信號](#殘餘項與重構信號)  
10. [效能優化與調整建議](#效能優化與調整建議)  
11. [使用注意事項](#使用注意事項)  
12. [常見錯誤與除錯](#常見錯誤與除錯)  
13. [參考資源](#參考資源)  

---

## 套件介紹

EMD-signal 是一個用 Python 實作 Empirical Mode Decomposition (EMD) 及其衍生方法（EEMD、CEEMDAN）的函式庫。  
適用於：  
- 非線性、非平穩時間序列的分解與分析  
- 篩選／去噪 (denoising)  
- 特徵萃取 (feature extraction)  
- 與機器學習模型結合 (classification、regression、clustering)  

優點：  
- 無需事先假設信號模型  
- 自適應找出局部振盪模式 (IMF)  
- 可處理複雜生物醫學、生產機械、金融等多種領域的時序資料  

---

## 安裝與導入

```bash
# 建議使用 pip 進行安裝
pip install EMD-signal

# 或者從 GitHub 原始碼安裝（最新版）
git clone https://github.com/laszukdawid/EMD-signal.git
cd EMD-signal
python setup.py install

```python
# 在 Python 程式中導入
import numpy as np
import matplotlib.pyplot as plt

# EMD
from PyEMD import EMD

# EEMD
from PyEMD import EEMD

# CEEMDAN
from PyEMD import CEEMDAN

# 如果要使用其他演算法，可依需求 import
from PyEMD import EMD, EEMD, ECA, Visualisation # 假設有 Visualisation 類別
```

**註**：`PyEMD` 是 EMD-signal 的套件名稱，但也常見用 `from PyEMD import EMD` 等方式呼叫。

---

## 核心概念

1. **Intrinsic Mode Function (IMF)**

   * 每個 IMF 必須符合兩個條件：

     1. 在整個序列中，極值點和零交叉數目相等或相差不超過 1。
     2. 在任意點，局部平均值 (由上包絡線和下包絡線的平均) 為零。
   * IMF 表示信號在不同時間尺度的局部振盪模式。

2. **Empirical Mode Decomposition (EMD)**

   * 迭代萃取 IMF 的方式：

     1. 將當前信號視為「候選 IMF」。
     2. 找出局部極大值與極小值，藉由插值產生上、下包絡。
     3. 計算包絡平均，將候選 IMF 減去該平均。
     4. 重複步驟直到 IMF 滿足條件，將其記錄，然後從原始信號減去該 IMF 生成殘餘。
     5. 對殘餘信號重複步驟，直到殘餘為單調或成趨勢。

3. **Ensemble EMD (EEMD)**

   * 對原始資料加入多次隨機高斯白雜訊後進行 EMD，
   * 最後將所有試次的 IMF 取平均，以降低模態混疊 (mode mixing) 現象。

4. **Complete EEMD with Adaptive Noise (CEEMDAN)**

   * 在 EEMD 基礎上，以自適應方式調整雜訊強度，
   * 可獲得較穩定且完整的 IMF 分解結果。

5. **其他演算法**（視套件版本可用性）：

   * CEEMD：非完全 EEMD；
   * ECA (Ensemble Complementary Adaptive)；
   * ICEEMDAN、NA-MEMD 等進階變體。

---

## 主要功能與 API 一覽

以下彙整 EMD-signal（PyEMD）套件中常用的類別與函式，並附帶簡短說明。

### EMD

```python
class PyEMD.EMD(*args, **kwargs)
```

* **用途**：對單一一維信號執行 EMD 分解，產生多個 IMF 以及最後的殘餘 (residue)。
* **常用方法**：

  * `emd = EMD()`：建立 EMD 物件。
  * `imfs = emd.emd(signal, time=None)`：對 `signal` 執行 EMD， `time` 為時間軸，如果省略則使用樣本索引。
  * `residue = emd.get_residue()`：取得最後殘餘。
  * `imfs_trend = emd.get_imfs_and_residue()`：同時取得 IMFs 與殘餘。
  * `emd.extrema_detection = "parabol"`：設定極值檢測方法，可為 `parabol`、`simple`、`cubic` 等。
  * `emd.spline_kind = "cubic"`：設定包絡線插值方法，可為 `cubic`、`quadratic`、`linear` 等。
  * `emd.max_imf = None`：設定最大 IMF 數量，預設 None 代表無限制。
  * `emd.sd_threshold = 0.3`：停止標準差閾值，影響 IMF 收斂條件。

### EEMD

```python
class PyEMD.EEMD(*args, **kwargs)
```

* **用途**：對單一一維信號執行 EEMD 分解，用多次加噪方式減少模態混疊。
* **常用方法**：

  * `eemd = EEMD()`：建立 EEMD 物件。
  * `imfs = eemd.eemd(signal, time=None, trials=100, noise_width=0.05)`：執行 EEMD，`trials` 定義噪聲試次數，`noise_width` 為噪聲標準差相對比例。
  * `eemd.trials = 200`：設定試次數。
  * `eemd.noise_seed = 42`：設定隨機種子以確保可重現性。
  * `eemd.noise_width = 0.1`：設定噪聲比例。

### CEEMDAN

```python
class PyEMD.CEEMDAN(*args, **kwargs)
```

* **用途**：執行 CEEMDAN，利用自適應雜訊實現完整 IMF 分解。
* **常用方法**：

  * `ceemdan = CEEMDAN()`：建立 CEEMDAN 物件。
  * `imfs = ceemdan.ceemdan(signal, time=None, trials=100, noise_width=0.05)`：執行 CEEMDAN，參數定義同 EEMD。
  * `ceemdan.trials = 100`：設定試次。
  * `ceemdan.noise_width = 0.02`：設定雜訊寬度。
  * `ceemdan.adaptive_noise = True`：啟用自適應雜訊模式（預設為 True）。

### 其他相關演算法

```python
# ECA (Ensemble Complementary Adaptive)
from PyEMD import ECA
eca = ECA()
imfs = eca.eca(signal, time=None, trials=50, noise_width=0.1)

# 如果套件有 ICEEMDAN:
from PyEMD import ICEEMDAN
iceemdan = ICEEMDAN()
imfs = iceemdan.iceemdan(signal, trials=100, noise_width=0.05)

# 若想自行實作包絡線繪製，可用 Visualisation
from PyEMD import Visualisation
vis = Visualisation()
# vis.plot_imfs(signal, imfs)  # 直接呼叫繪圖函式
```

---

## API 參數詳解

以下對常見類別與函式的參數進行詳細說明，方便調整與最佳化。

### emd.EMD

| 參數                | 預設值      | 說明                                                        |
| ------------------- | ----------- | ----------------------------------------------------------- |
| `max_imf`           | `None`      | 最大 IMF 數量；若 None 則不限制。                           |
| `extrema_detection` | `"parabol"` | 極值偵測方法，可選 `"parabol"`、`"simple"`、`"cubic"`。     |
| `spline_kind`       | `"cubic"`   | 包絡線插值方法，可選 `"cubic"`、`"quadratic"`、`"linear"`。 |
| `max_siftings`      | `50`        | 每個 IMF 最多 sift 次數。                                   |
| `sd_threshold`      | `0.3`       | 停止條件：相鄰 sift 之相對標準差 (SD) 閾值。                |
| `generate_all_imfs` | `False`     | 是否強制產生所有 IMF，若 False 則遇到單調殘餘就停止分解。   |

#### 常用屬性與方法

* `emd.residue`：最後殘餘向量。
* `emd.imfs`：分解得到的所有 IMF。
* `emd.nbsif`：紀錄每個 IMF 的 sift 次數。
* `emd.get_imfs_and_residue()`：回傳 `(imfs, residue)` 的元組。
* `emd.get_imfs()`：僅回傳 IMF。
* `emd.get_residue()`：僅回傳殘餘。

### eemd.EEMD

| 參數          | 預設值 | 說明                                       |
| ------------- | ------ | ------------------------------------------ |
| `trials`      | `100`  | 執行 EEMD 的雜訊試次次數。                 |
| `noise_width` | `0.05` | 加入高斯雜訊時，雜訊標準差與信號振幅比例。 |
| `noise_seed`  | `None` | 隨機種子，若指定可重現相同結果。           |

#### 常用屬性與方法

* `eemd.eemd(signal, time)`：執行 EEMD 分解。
* `eemd.get_imfs()`：回傳所有 IMF。
* `eemd.get_imf(index)`：回傳第 `index` 個 IMF。
* `eemd.residue`：最後殘餘。

### ceemdan.CEEMDAN

| 參數             | 預設值 | 說明                              |
| ---------------- | ------ | --------------------------------- |
| `trials`         | `100`  | 噪聲試次次數。                    |
| `noise_width`    | `0.05` | 雜訊標準差比例。                  |
| `adaptive_noise` | `True` | 是否啟用自適應雜訊。              |
| `max_imf`        | `None` | 最大 IMF 數量；若 None 則不限制。 |
| `tolerance`      | `0.0`  | 停止條件容差，用於自適應調整。    |

#### 常用屬性與方法

* `ceemdan.ceemdan(signal, time)`：執行 CEEMDAN 分解。
* `ceemdan.get_imfs()`：取得所有 IMF。
* `ceemdan.residue`：最後殘餘。

### 其他輔助函式、類別

* `visualisation.plot_imfs(signal, imfs, time=None, cmap='viridis')`：繪製多張 IMF 子圖。
* `visualisation.plot_residue(time, residue)`：繪製殘餘曲線。
* `visualisation.plot_inst_freq(time, imf)`：繪製瞬時頻率。
* `emd.EMD.sift_once(signal)`：對信號執行一次 sift 操作。
* `emd.EMD.get_envelopes(signal)`：取得上下包絡。

---

## 基本使用範例

以下示範如何使用 EMD-signal 進行基本分解，並搭配視覺化。

### 對單一時序信號進行 EMD 分解

```python
import numpy as np
import matplotlib.pyplot as plt
from PyEMD import EMD

# 範例信號：合成正弦與噪聲
fs = 1000                              # 取樣頻率 1000 Hz
t = np.linspace(0, 1, fs)
f1, f2 = 5, 50                         # 兩個頻率成分
signal = np.sin(2 * np.pi * f1 * t) + 0.5 * np.sin(2 * np.pi * f2 * t)
signal += 0.2 * np.random.randn(len(t))  # 加入白雜訊

# 執行 EMD
emd = EMD()
imfs = emd.emd(signal, t)

# 取得殘餘
residue = emd.get_residue()

# 印出 IMF 數量
print(f"分解得到 {imfs.shape[0]} 個 IMF")

# 繪製原始信號與 IMF
n_imfs = imfs.shape[0]
plt.figure(figsize=(12, 2*(n_imfs+1)))
plt.subplot(n_imfs+1, 1, 1)
plt.plot(t, signal, 'k')
plt.title("原始信號")

for i in range(n_imfs):
    plt.subplot(n_imfs+1, 1, i+2)
    plt.plot(t, imfs[i], 'b')
    plt.ylabel(f"IMF {i+1}")

plt.tight_layout()
plt.show()
```

#### 說明

1. 建立合成信號：5 Hz 與 50 Hz 正弦疊加，並加入白雜訊。
2. 使用 `EMD().emd(signal, t)` 進行分解。
3. `imfs` 為形狀 `(n_imfs, len(signal))` 的陣列，每列代表一個 IMF。
4. `get_residue()` 取得殘餘成分，可用於後續分析或重構。

### 使用 CEEMDAN 進行去噪或特徵萃取

```python
import numpy as np
import matplotlib.pyplot as plt
from PyEMD import CEEMDAN

# 範例信號：金融資料（簡化版）
# 模擬股價震盪：趨勢 + 週期 + 噪聲
days = 500
time = np.arange(days)
trend = 0.1 * time
seasonal = 10 * np.sin(2 * np.pi * time / 50)
noise = 5 * np.random.randn(days)
signal = trend + seasonal + noise

# 執行 CEEMDAN
ceemdan = CEEMDAN(trials=150, noise_width=0.02)
imfs = ceemdan.ceemdan(signal, time)

# 取得殘餘
residue = ceemdan.get_residue()

# 繪製 IMF 與殘餘
n_imfs = imfs.shape[0]
plt.figure(figsize=(10, 2*(n_imfs+1)))
plt.subplot(n_imfs+1, 1, 1)
plt.plot(time, signal, 'k')
plt.title("模擬股價信號")

for i in range(n_imfs):
    plt.subplot(n_imfs+1, 1, i+2)
    plt.plot(time, imfs[i], 'r')
    plt.ylabel(f"IMF {i+1}")

plt.subplot(n_imfs+1, 1, n_imfs+1)
plt.plot(time, residue, 'g')
plt.ylabel("殘餘")
plt.tight_layout()
plt.show()
```

#### 說明

1. 利用 CEEMDAN 分解複雜金融訊號。
2. `trials=150`、`noise_width=0.02` 可根據資料特性調整。
3. 分析各 IMF 的頻譜特性，能輔助後續交易訊號判斷或特徵工程。

---

## 與機器學習結合

EMD 分解後的 IMF 可作為額外特徵，結合 scikit-learn、TensorFlow、PyTorch 等框架進行下游任務。

### 特徵工程示範

```python
import numpy as np
import pandas as pd
from PyEMD import EMD
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# 假設有原始多維時序資料 X (形狀: N_samples x T_sequence)
# 例如：N_samples = 1000，T_sequence = 200
N_samples, T_sequence = 500, 200
np.random.seed(0)
X = np.random.randn(N_samples, T_sequence)
y = np.random.randint(0, 2, N_samples)  # 二分類標籤

# 對每筆樣本做 EMD 分解，取前三個 IMF 的統計特徵
def extract_emd_features(signal, n_imf=3):
    emd = EMD()
    imfs = emd.emd(signal)
    features = []
    for i in range(min(n_imf, imfs.shape[0])):
        imf = imfs[i]
        features.append(np.mean(imf))
        features.append(np.std(imf))
        features.append(np.max(imf) - np.min(imf))
    return np.array(features)

# 建立資料集特徵矩陣
X_feats = np.array([extract_emd_features(X[i], n_imf=3) for i in range(N_samples)])
feat_names = []
for i in range(3):
    feat_names += [f"IMF{i+1}_mean", f"IMF{i+1}_std", f"IMF{i+1}_range"]
df_feats = pd.DataFrame(X_feats, columns=feat_names)

# 分割訓練與測試
X_train, X_test, y_train, y_test = train_test_split(df_feats, y, test_size=0.2, random_state=42)

# 訓練隨機森林模型
clf = RandomForestClassifier(n_estimators=100, random_state=42)
clf.fit(X_train, y_train)
y_pred = clf.predict(X_test)

print("Accuracy:", accuracy_score(y_test, y_pred))
```

#### 說明

1. `extract_emd_features` 函式：對每筆時序資料取 EMD 分解後的前三個 IMF，並計算其均值、標準差與範圍作為特徵。
2. 產生特徵矩陣後，結合 scikit-learn 進行分割、訓練與評估。
3. 可根據應用情境調整 IMF 數量與特徵種類，例如取能量 (energy)、頻率統計等。

### 結合 scikit-learn 進行分類／回歸

```python
import numpy as np
import pandas as pd
from PyEMD import CEEMDAN
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import TimeSeriesSplit
from sklearn.metrics import mean_squared_error

# 假設有一個金融回歸問題：輸入時序股價，預測下一日報酬
# 先對原始信號做 CEEMDAN 分解，將各 IMF 作為多維輸入特徵
days = 300
time = np.arange(days)
price = 100 + np.cumsum(np.random.randn(days))  # 模擬股價

# 產生滾動視窗資料
window_size = 50
X, y = [], []
for i in range(days - window_size - 1):
    segment = price[i : i+window_size]
    ceemdan = CEEMDAN(trials=100, noise_width=0.02)
    imfs = ceemdan.ceemdan(segment)
    # 取每個 IMF 的最後一個值作為該 IMFs 特徵
    feat = np.array([imf[-1] for imf in imfs])
    X.append(feat)
    y.append(price[i+window_size+1] - price[i+window_size])  # 下一日報酬

X = np.vstack(X)
y = np.array(y)

# 使用時間序列交叉驗證
tscv = TimeSeriesSplit(n_splits=5)
mse_list = []

for train_idx, test_idx in tscv.split(X):
    X_tr, X_te = X[train_idx], X[test_idx]
    y_tr, y_te = y[train_idx], y[test_idx]
    model = LinearRegression()
    model.fit(X_tr, y_tr)
    y_pred = model.predict(X_te)
    mse_list.append(mean_squared_error(y_te, y_pred))

print("Cross-validated MSE:", np.mean(mse_list))
```

#### 說明

1. 利用 CEEMDAN 分解視窗內的股價序列，將各 IMF 最後一個樣本當作特徵輸入回歸模型。
2. 使用 `TimeSeriesSplit` 保持時序順序進行交叉驗證。
3. 可延伸使用其他模型 (SVR, XGBoost, LSTM) 等， IMF 特徵可做模型輸入。

---

## 純粹時間序列分析範例

EMD 分解可應用於去除非平穩成分、分析趨勢與季節性，也可以進一步處理殘餘。

### 趨勢、季節性、殘差拆解

```python
import numpy as np
import matplotlib.pyplot as plt
from PyEMD import EEMD

# 範例：合成趨勢 + 季節性 + 噪聲
t = np.linspace(0, 10, 1000)
trend = 0.5 * t
seasonal = 2 * np.sin(2 * np.pi * t / 1.5)
noise = 0.3 * np.random.randn(len(t))
signal = trend + seasonal + noise

# 使用 EEMD 分解
eemd = EEMD(trials=100, noise_width=0.05)
imfs = eemd.eemd(signal, t)
residue = eemd.get_residue()

# 假設第一個 IMF 為高頻成分（噪聲），最後一個 IMF 為季節性，中間其他 IMF 可視為趨勢
imf_noise = imfs[0]
imf_seasonal = imfs[-2] if imfs.shape[0] >= 2 else imfs[-1]
imf_trend = residue

# 繪製拆解結果
plt.figure(figsize=(10, 6))
plt.subplot(4, 1, 1)
plt.plot(t, signal, 'k')
plt.title("原始信號")
plt.subplot(4, 1, 2)
plt.plot(t, imf_noise, 'r')
plt.title("高頻噪聲 (IMF 1)")
plt.subplot(4, 1, 3)
plt.plot(t, imf_seasonal, 'g')
plt.title("季節性成分 (IMF 最後前一項)")
plt.subplot(4, 1, 4)
plt.plot(t, imf_trend, 'b')
plt.title("趨勢/殘餘 (Residue)")
plt.tight_layout()
plt.show()
```

#### 說明

1. 利用 EEMD 將信號拆解為高頻、季節性、趨勢成分。
2. 使用 IMF 索引選擇各成分，可根據資料特性靈活調整。
3. 殘餘通常代表最慢變化趨勢，可直接當作趨勢部分。

### 金融資料示範（股價、匯率）

```python
import pandas as pd
import matplotlib.pyplot as plt
from PyEMD import EMD

# 載入真實金融資料（示例資料，自行替換）
# df = pd.read_csv("stock_price.csv", parse_dates=["Date"], index_col="Date")
# price = df["Close"].values
# time = np.arange(len(price))

# 這裡用隨機數據模擬
np.random.seed(42)
days = 1000
time = np.arange(days)
price = 50 + np.cumsum(np.random.randn(days))

# EMD 分解
emd = EMD()
imfs = emd.emd(price, time)
residue = emd.get_residue()

# 繪製結果
n_imfs = imfs.shape[0]
plt.figure(figsize=(12, 2*(n_imfs+1)))
plt.subplot(n_imfs+1, 1, 1)
plt.plot(time, price, 'k')
plt.title("模擬股價")

for i in range(n_imfs):
    plt.subplot(n_imfs+1, 1, i+2)
    plt.plot(time, imfs[i], 'm')
    plt.ylabel(f"IMF {i+1}")

plt.tight_layout()
plt.show()
```

#### 說明

1. 真實金融資料可先移除趨勢或常態化後再做 EMD。
2. IMF 2、3、4 等常對應於不同時間尺度的震盪，可作波動性分析。
3. 殘餘較接近整體長期趨勢，用於長線投資判斷。

### 心率、震動訊號分析

```python
import numpy as np
import matplotlib.pyplot as plt
from PyEMD import CEEMDAN

# 模擬心率訊號：基礎正弦 + 呼吸調變 + 噪聲
fs = 250                        # 取樣 250 Hz
t = np.arange(0, 10, 1/fs)      # 10 秒
heart = 1.2 * np.sin(2*np.pi*1.2*t)        # 基礎心跳 1.2 Hz
resp = 0.2 * np.sin(2*np.pi*0.25*t)        # 呼吸調變 0.25 Hz
noise = 0.1 * np.random.randn(len(t))
signal = heart + resp + noise

# CEEMDAN 分解
ceemdan = CEEMDAN(trials=100, noise_width=0.05)
imfs = ceemdan.ceemdan(signal, t)

# 將 IMF 1 視為心跳成分，IMF 2 視為呼吸成分，殘餘作其他雜訊
imf_heartbeat = imfs[0]
imf_resp = imfs[1]
residue = ceemdan.get_residue()

# 繪製分析結果
plt.figure(figsize=(10, 8))
plt.subplot(4, 1, 1)
plt.plot(t, signal, 'k')
plt.title("模擬心率信號")
plt.subplot(4, 1, 2)
plt.plot(t, imf_heartbeat, 'r')
plt.title("心跳成分 (IMF 1)")
plt.subplot(4, 1, 3)
plt.plot(t, imf_resp, 'g')
plt.title("呼吸成分 (IMF 2)")
plt.subplot(4, 1, 4)
plt.plot(t, residue, 'b')
plt.title("殘餘")
plt.tight_layout()
plt.show()
```

#### 說明

1. 通常 IMF 1 為最高頻率成分，對應心跳尖峰；IMF 2 為呼吸頻率。
2. 可進一步計算 IMF1 的峰值間距估算心率 (BPM)。
3. 對機械振動訊號也可同理，IMF 級距對應不同振動頻段。

---

## 視覺化

### 原始信號與 IMF 繪製

```python
import numpy as np
import matplotlib.pyplot as plt
from PyEMD import EMD
from PyEMD import Visualisation

# 範例：合成信號
t = np.linspace(0, 1, 500)
signal = np.sin(2*np.pi*5*t) + 0.5*np.sin(2*np.pi*20*t) + 0.1*np.random.randn(len(t))

# EMD 分解
emd = EMD()
imfs = emd.emd(signal, t)
residue = emd.get_residue()

# 使用 Visualisation 類別
vis = Visualisation()
fig, axes = vis.plot_imfs(t, imfs, cmap='seismic')
axes[0].set_title("IMF 分解結果")

# 自行繪製原始信號與殘餘
plt.figure(figsize=(10, 4))
plt.plot(t, signal, 'k', label='原始信號')
plt.plot(t, residue, 'r', label='殘餘')
plt.legend()
plt.title("原始信號與殘餘")
plt.show()
```

#### 說明

1. `Visualisation.plot_imfs` 會自動根據 IMF 數量產生子圖陣列。
2. 可設定 `cmap`（色帶）參數調整配色，預設為 `viridis`。
3. 如果想要更彈性控制子圖，可自行 loop IMF 陣列並用 matplotlib 繪製。

### 殘餘項與重構信號

```python
import numpy as np
import matplotlib.pyplot as plt
from PyEMD import EMD

# 範例信號
t = np.linspace(0, 2, 1000)
signal = np.sin(2*np.pi*10*t) + 0.5*np.sin(2*np.pi*30*t)

# EMD 分解
emd = EMD()
imfs = emd.emd(signal, t)
residue = emd.get_residue()

# 重構信號：將部分 IMF 排除以濾除高頻或低頻
# e.g., 只保留 IMF[2:] 以去除高頻雜訊
reconstructed = np.sum(imfs[1:], axis=0) + residue

plt.figure(figsize=(10, 4))
plt.plot(t, signal, 'k--', label='原始信號')
plt.plot(t, reconstructed, 'b', label='重構信號')
plt.legend()
plt.title("信號重構示意")
plt.show()
```

#### 說明

1. 假設 IMF\[0] & IMF\[1] 為高頻雜訊，可透過排除方式達到去噪效果。
2. 重構信號為剩餘 IMF 與殘餘之和，可對特定頻段進行濾波。

---

## 效能優化與調整建議

1. **選擇適當的插值方法 (`spline_kind`)**

   * `cubic`：平滑，但計算量較高。
   * `linear`：較快速，但包絡線不夠平滑。
   * `parabol`：介於 `cubic` 與 `simple` 之間，可依記憶體與精準度權衡。

2. **調整最大 sift 次數 (`max_siftings`)**

   * 若 IMF 無法快速收斂，增大 `max_siftings`，但會影響效能。
   * 若資料噪聲較低，適當降低 `max_siftings` 可加速分解。

3. **控制 IMF 最大數量 (`max_imf`)**

   * 對長序列且複雜信號，可能生成過多 IMF。
   * 設定 `max_imf` 防止過度分解，保留最重要的局部振盪成分。

4. **針對 EEMD/CEEMDAN 調整試次 (`trials`) 與雜訊寬度 (`noise_width`)**

   * `trials` 越多，結果越穩定、雜訊殘留越少，但計算成本線性增長。
   * `noise_width` 越大，能抑制模態混疊效果越好，但可能影響 IMFs 純度。
   * 建議初期使用較少試次 (50\~100)，若結果不理想再逐步增加。

5. **平行化執行 (若支持)**

   * 某些版本的 PyEMD 支援多線程（需自行檢查套件支援情況）。
   * 可在 `EEMD` 或 `CEEMDAN` 中設定 `threads` 參數以加速。

6. **資料前處理**

   * 若資料有明顯趨勢或季節性，先行做去趨勢或季節性調整可加快收斂。
   * 標準化 (normalization) 或單位化 (standardization) 有助 IMF 之間比較。

---

## 使用注意事項

* **信號長度**：EMD 分解耗時隨著序列長度呈非線性增長，建議先嘗試較短樣本窗口。
* **邊界效應 (Boundary Effects)**：包絡線在訊號開頭與結尾可能不穩定，建議在實務上可進行鏡像延伸 (signal mirroring) 或最低干擾尾端處理。
* **模態混疊 (Mode Mixing)**：若 IMF 出現多個頻率混合，可嘗試 EEMD 或 CEEMDAN 改善。
* **隨機性**：EEMD/CEEMDAN 因加入噪聲，結果會有隨機性，若需要可設定 `noise_seed` 保證重現性。
* **停止準則**：`sd_threshold` 與 `tolerance` 需依資料特性微調，避免過度分解或不足分解。
* **可視化檢查**：每次分解後務必繪圖檢查 IMF 是否合理，避免錯誤解讀。

---

## 常見錯誤與除錯

1. **`ValueError: Too few extrema for EMD.`**

   * 意味著信號變化太小或過於單調，無法產生足夠極值。
   * 解法：加大雜訊強度，或調整 `extrema_detection` 參數；或確定輸入信號非全常數。

2. **`IndexError: index out of range` (對 IMF 索引)**

   * 試圖取 IMF 中不存在的索引，通常因為分解產生的 IMF 數量較少。
   * 解法：先檢查 `imfs.shape[0]`，再進行索引。

3. **`KeyboardInterrupt` (過長計算時間)**

   * EEMD/CEEMDAN 試次過多 (trials 設定過高) 或資料序列過長。
   * 解法：先嘗試降低 `trials`，或對資料降維／截短。

4. **`MemoryError` 或 記憶體不足**

   * 分解時同時儲存大量 IMF 產生大量記憶體需求。
   * 解法：逐步釋放不必要的中間變數，或使用分批 (chunk) 方式分段處理。

5. **`TypeError: unsupported operand type(s)`**

   * 輸入資料型態非 numpy 陣列或 list，或型態不一致。
   * 解法：將 `signal` 及 `time` 轉為 `np.ndarray`，確保維度一致。

---

## 參考資源

1. PyEMD 官方 GitHub：
   [https://github.com/laszukdawid/EMD-signal](https://github.com/laszukdawid/EMD-signal)
2. “Empirical Mode Decomposition” 研究論文：
   Hilbert–Huang Transform (HHT) 相關原理。
3. CEEMDAN 演算法原文：
   Torres, M. E., et al. “A complete ensemble empirical mode decomposition with adaptive noise.”
4. 時間序列分析與機器學習實戰：
   博客與論文範例，如 “Time Series Feature Extraction Using EMD”
5. scikit-learn 官方文件：
   [https://scikit-learn.org](https://scikit-learn.org)
6. Matplotlib 視覺化教學：
   [https://matplotlib.org](https://matplotlib.org)

---
