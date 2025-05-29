# Python Control套件 Cheat Sheet
## 機器學習結合時間序列分析 & 純時間序列分析

### 🚀 基本安裝與導入
```python
# 安裝所需套件
pip install control numpy scipy matplotlib pandas scikit-learn

# 基本導入
import control as ctrl
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
from scipy import signal
```

---

## 📊 時間序列系統建模

### 傳遞函數創建
```python
# 創建傳遞函數 H(s) = K/(s+a)
num = [1]           # 分子多項式係數
den = [1, 2]        # 分母多項式係數 (s + 2)
H = ctrl.tf(num, den)

# 從零點和極點創建
zeros = []          # 零點
poles = [-2, -3]    # 極點
gain = 5            # 增益
H = ctrl.zpk(zeros, poles, gain)

# 狀態空間表示
A = [[-2, 1], [0, -3]]    # 系統矩陣
B = [[1], [0]]            # 輸入矩陣
C = [[1, 0]]              # 輸出矩陣
D = [[0]]                 # 前饋矩陣
sys = ctrl.ss(A, B, C, D)
```

### 時間響應分析
```python
# 步階響應
t, y = ctrl.step_response(H)
plt.plot(t, y)
plt.title('Step Response')

# 脈衝響應
t, y = ctrl.impulse_response(H)

# 自定義輸入響應
t = np.linspace(0, 10, 1000)
u = np.sin(t)  # 正弦輸入
t, y = ctrl.forced_response(H, t, u)
```

---

## 🤖 機器學習結合時間序列

### 系統識別與參數估計
```python
# 使用機器學習進行系統識別
from sklearn.linear_model import LinearRegression
from sklearn.preprocessing import PolynomialFeatures

def ml_system_identification(input_data, output_data, order=2):
    """
    使用機器學習識別系統參數
    """
    # 創建多項式特徵
    poly = PolynomialFeatures(degree=order)
    X = poly.fit_transform(input_data.reshape(-1, 1))
    
    # 線性回歸
    model = LinearRegression()
    model.fit(X, output_data)
    
    # 獲取係數
    coeffs = model.coef_
    return coeffs, model

# 範例使用
t = np.linspace(0, 10, 100)
u = np.random.randn(100)  # 隨機輸入
# 假設已知系統輸出 y
y = signal.lfilter([1], [1, 0.5, 0.1], u)  # 模擬系統輸出

coeffs, ml_model = ml_system_identification(u, y)
```

### 預測控制結合神經網路
```python
from sklearn.neural_network import MLPRegressor

class MLPredictiveController:
    def __init__(self, system, horizon=10):
        self.system = system
        self.horizon = horizon
        self.predictor = MLPRegressor(hidden_layer_sizes=(50, 50))
        
    def train_predictor(self, past_inputs, past_outputs, future_outputs):
        """訓練預測器"""
        # 準備訓練數據
        X = np.column_stack([past_inputs, past_outputs])
        y = future_outputs
        self.predictor.fit(X, y)
    
    def predict_and_control(self, current_state, reference):
        """預測並計算控制信號"""
        # 預測未來輸出
        predicted = self.predictor.predict(current_state.reshape(1, -1))
        
        # 計算控制誤差
        error = reference - predicted[0]
        
        # 簡單比例控制
        control_signal = 0.5 * error
        return control_signal
```

---

## 📈 頻域分析

### 波德圖與頻率響應
```python
# 繪製波德圖
ctrl.bode_plot(H)
plt.show()

# 獲取頻率響應數據
omega = np.logspace(-2, 2, 1000)  # 頻率範圍
mag, phase, omega = ctrl.frequency_response(H, omega)

# 轉換為dB
mag_db = 20 * np.log10(np.abs(mag))
phase_deg = np.angle(mag) * 180 / np.pi
```

### 時頻分析結合
```python
from scipy.signal import spectrogram

def time_frequency_analysis(signal, fs):
    """
    時頻分析
    """
    f, t, Sxx = spectrogram(signal, fs)
    plt.pcolormesh(t, f, 10 * np.log10(Sxx))
    plt.ylabel('Frequency [Hz]')
    plt.xlabel('Time [sec]')
    plt.colorbar(label='Power [dB]')
    return f, t, Sxx

# 範例
fs = 1000  # 採樣頻率
t = np.linspace(0, 5, fs*5)
signal_data = np.sin(2*np.pi*10*t) + 0.5*np.sin(2*np.pi*50*t)
f, t_spec, Sxx = time_frequency_analysis(signal_data, fs)
```

---

## 🎯 控制器設計

### PID控制器
```python
def design_pid_controller(Kp, Ki, Kd, system):
    """
    設計PID控制器
    """
    # PID傳遞函數
    num_pid = [Kd, Kp, Ki]
    den_pid = [1, 0]
    pid_controller = ctrl.tf(num_pid, den_pid)
    
    # 閉環系統
    closed_loop = ctrl.feedback(pid_controller * system)
    
    return pid_controller, closed_loop

# 範例
plant = ctrl.tf([1], [1, 2, 1])  # 二階系統
pid, closed_system = design_pid_controller(1, 0.5, 0.1, plant)
```

### 現代控制 - LQR設計
```python
def lqr_design(A, B, Q, R):
    """
    線性二次調節器設計
    """
    # 求解Riccati方程
    P = ctrl.care(A, B, Q, R)
    
    # 計算最優增益
    K = np.linalg.inv(R) @ B.T @ P
    
    return K, P

# 範例
A = np.array([[0, 1], [-2, -3]])
B = np.array([[0], [1]])
Q = np.eye(2)  # 狀態權重
R = np.array([[1]])  # 控制權重

K_optimal, P = lqr_design(A, B, Q, R)
```

---

## 📊 時間序列數據處理

### 數據預處理
```python
def preprocess_timeseries(data, window_size=10):
    """
    時間序列數據預處理
    """
    # 移動平均濾波
    smoothed = np.convolve(data, np.ones(window_size)/window_size, mode='valid')
    
    # 去趨勢
    from scipy.signal import detrend
    detrended = detrend(data)
    
    # 標準化
    normalized = (data - np.mean(data)) / np.std(data)
    
    return smoothed, detrended, normalized

# 異常檢測
def detect_anomalies(data, threshold=3):
    """
    基於統計的異常檢測
    """
    z_scores = np.abs((data - np.mean(data)) / np.std(data))
    anomalies = z_scores > threshold
    return anomalies, z_scores
```

### 系統穩定性分析
```python
def stability_analysis(system):
    """
    穩定性分析
    """
    # 獲取極點
    poles = ctrl.pole(system)
    
    # 檢查穩定性
    is_stable = np.all(np.real(poles) < 0)
    
    # 穩定邊界
    stability_margin = -np.max(np.real(poles))
    
    return is_stable, poles, stability_margin

# 根軌跡分析
def root_locus_analysis(system, k_range=None):
    """
    根軌跡分析
    """
    if k_range is None:
        k_range = np.logspace(-2, 2, 1000)
    
    rlist, klist = ctrl.root_locus(system, kvect=k_range, plot=True)
    return rlist, klist
```

---

## 🔄 實時控制實現

### 實時數據處理
```python
class RealTimeController:
    def __init__(self, controller, sampling_time=0.01):
        self.controller = controller
        self.dt = sampling_time
        self.error_history = []
        self.output_history = []
    
    def update(self, reference, measurement):
        """
        實時更新控制器
        """
        error = reference - measurement
        self.error_history.append(error)
        
        # 數值積分和微分
        if len(self.error_history) > 1:
            integral = np.sum(self.error_history) * self.dt
            derivative = (self.error_history[-1] - self.error_history[-2]) / self.dt
        else:
            integral = derivative = 0
        
        # PID控制
        control_output = (self.Kp * error + 
                         self.Ki * integral + 
                         self.Kd * derivative)
        
        self.output_history.append(control_output)
        return control_output

# 使用範例
controller = RealTimeController(None)
controller.Kp, controller.Ki, controller.Kd = 1.0, 0.1, 0.05
```

---

## 📊 性能評估指標

### 控制系統性能指標
```python
def performance_metrics(reference, output, time_vector):
    """
    計算控制系統性能指標
    """
    error = reference - output
    
    # 積分絕對誤差 (IAE)
    iae = np.trapz(np.abs(error), time_vector)
    
    # 積分平方誤差 (ISE)
    ise = np.trapz(error**2, time_vector)
    
    # 積分時間絕對誤差 (ITAE)
    itae = np.trapz(time_vector * np.abs(error), time_vector)
    
    # 超調量
    steady_state = np.mean(output[-50:])  # 假設後50點為穩態
    overshoot = (np.max(output) - steady_state) / steady_state * 100
    
    # 上升時間 (10% 到 90%)
    rise_start = np.where(output >= 0.1 * steady_state)[0][0]
    rise_end = np.where(output >= 0.9 * steady_state)[0][0]
    rise_time = time_vector[rise_end] - time_vector[rise_start]
    
    return {
        'IAE': iae,
        'ISE': ise,
        'ITAE': itae,
        'Overshoot': overshoot,
        'Rise_Time': rise_time
    }
```

---

## 🛠️ 實用工具函數

### 數據可視化
```python
def plot_control_results(time, reference, output, control_signal):
    """
    繪製控制結果
    """
    fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(10, 8))
    
    # 系統響應
    ax1.plot(time, reference, 'r--', label='Reference', linewidth=2)
    ax1.plot(time, output, 'b-', label='Output', linewidth=2)
    ax1.set_ylabel('Amplitude')
    ax1.set_title('System Response')
    ax1.legend()
    ax1.grid(True)
    
    # 控制信號
    ax2.plot(time, control_signal, 'g-', linewidth=2)
    ax2.set_xlabel('Time (s)')
    ax2.set_ylabel('Control Signal')
    ax2.set_title('Control Signal')
    ax2.grid(True)
    
    plt.tight_layout()
    plt.show()

# 頻域比較
def compare_frequency_response(systems, labels, omega_range=None):
    """
    比較多個系統的頻率響應
    """
    if omega_range is None:
        omega_range = np.logspace(-2, 2, 1000)
    
    plt.figure(figsize=(12, 8))
    
    for i, (sys, label) in enumerate(zip(systems, labels)):
        mag, phase, omega = ctrl.frequency_response(sys, omega_range)
        
        plt.subplot(2, 1, 1)
        plt.semilogx(omega, 20*np.log10(np.abs(mag)), label=label)
        plt.ylabel('Magnitude (dB)')
        plt.grid(True)
        plt.legend()
        
        plt.subplot(2, 1, 2)
        plt.semilogx(omega, np.angle(mag)*180/np.pi, label=label)
        plt.ylabel('Phase (degrees)')
        plt.xlabel('Frequency (rad/s)')
        plt.grid(True)
        plt.legend()
    
    plt.tight_layout()
    plt.show()
```

---

## 📝 快速參考表

### 常用函數速查
| 功能     | 函數                        | 範例                                            |
| -------- | --------------------------- | ----------------------------------------------- |
| 傳遞函數 | `ctrl.tf()`                 | `H = ctrl.tf([1], [1, 2, 1])`                   |
| 狀態空間 | `ctrl.ss()`                 | `sys = ctrl.ss(A, B, C, D)`                     |
| 步階響應 | `ctrl.step_response()`      | `t, y = ctrl.step_response(H)`                  |
| 頻率響應 | `ctrl.frequency_response()` | `mag, phase, w = ctrl.frequency_response(H, w)` |
| 極點計算 | `ctrl.pole()`               | `poles = ctrl.pole(H)`                          |
| 閉環系統 | `ctrl.feedback()`           | `T = ctrl.feedback(G, H)`                       |
| 波德圖   | `ctrl.bode_plot()`          | `ctrl.bode_plot(H)`                             |
| 根軌跡   | `ctrl.root_locus()`         | `ctrl.root_locus(H)`                            |

### 常見系統類型
```python
# 一階系統
first_order = ctrl.tf([K], [T, 1])  # K/(Ts + 1)

# 二階系統  
second_order = ctrl.tf([wn**2], [1, 2*zeta*wn, wn**2])  # wn²/(s² + 2ζwn·s + wn²)

# 積分器
integrator = ctrl.tf([1], [1, 0])  # 1/s

# 微分器
differentiator = ctrl.tf([1, 0], [1])  # s

# 時間延遲近似 (Pade)
delay_time = 0.5  # 延遲時間
pade_approx = ctrl.pade(delay_time, 2)  # 2階Pade近似
```

---

### 💡 最佳實踐提示

1. **系統識別**: 先用簡單模型，再逐步增加複雜度
2. **控制器調整**: 從比例控制開始，再加入積分和微分項
3. **穩定性**: 始終檢查閉環系統的穩定性
4. **性能**: 使用多個指標評估系統性能
5. **實時性**: 考慮計算複雜度和實時約束

這個cheat sheet涵蓋了Python control套件在時間序列分析和機器學習結合應用中的主要功能，提供實用的代碼範例和最佳實踐建議。