# Dynaconf Cheat Sheet - 機器學習與時間序列分析

## 🚀 快速開始

### 安裝
```bash
pip install dynaconf
pip install python-dotenv  # 如果要使用 .env 檔案
```

### 初始化專案
```bash
dynaconf init -f toml  # 使用 TOML 格式
# 或
dynaconf init -f yaml  # 使用 YAML 格式
```

## 📁 專案結構

```
project/
├── config.py            # Dynaconf 設定檔
├── settings.toml        # 主要設定檔
├── .secrets.toml        # 敏感資料（加入 .gitignore）
├── .env                 # 環境變數
└── configs/
    ├── development.toml # 開發環境設定
    ├── production.toml  # 生產環境設定
    └── testing.toml     # 測試環境設定
```

## ⚙️ 基本設定

### config.py
```python
from dynaconf import Dynaconf

settings = Dynaconf(
    envvar_prefix="DYNACONF",
    settings_files=['settings.toml', '.secrets.toml'],
    environments=True,
    load_dotenv=True,
    env_switcher="ENV_FOR_DYNACONF",
)
```

## 🤖 機器學習 + 時間序列分析配置

### settings.toml
```toml
[default]
# 專案基本設定
project_name = "ml_timeseries_project"
random_seed = 42
debug = false

# 資料路徑
[default.paths]
data_dir = "data/"
raw_data = "@format {this.data_dir}/raw/"
processed_data = "@format {this.data_dir}/processed/"
models_dir = "models/"
logs_dir = "logs/"
results_dir = "results/"

# 時間序列設定
[default.timeseries]
date_column = "date"
target_column = "value"
frequency = "D"  # D=daily, H=hourly, M=monthly
seasonal_periods = 7  # 週期性
forecast_horizon = 30  # 預測期間

# 特徵工程
[default.features]
lag_features = [1, 7, 14, 30]
rolling_windows = [7, 14, 30]
rolling_stats = ["mean", "std", "min", "max"]
use_holiday_features = true
use_fourier_features = true
fourier_order = 5

# 資料預處理
[default.preprocessing]
handle_missing = "interpolate"  # forward_fill, backward_fill, interpolate
outlier_method = "iqr"  # iqr, zscore, isolation_forest
outlier_threshold = 1.5
scale_method = "standard"  # standard, minmax, robust
train_test_split = 0.8
validation_split = 0.2

# 模型設定
[default.models]
# 傳統時間序列模型
[default.models.arima]
enabled = true
auto_arima = true
seasonal = true
stepwise = true
max_p = 5
max_q = 5
max_d = 2

[default.models.prophet]
enabled = true
yearly_seasonality = true
weekly_seasonality = true
daily_seasonality = false
changepoint_prior_scale = 0.05

# 機器學習模型
[default.models.random_forest]
enabled = true
n_estimators = 100
max_depth = 10
min_samples_split = 5
n_jobs = -1

[default.models.xgboost]
enabled = true
n_estimators = 100
learning_rate = 0.1
max_depth = 6
subsample = 0.8
colsample_bytree = 0.8

[default.models.lightgbm]
enabled = true
num_leaves = 31
learning_rate = 0.05
n_estimators = 100
boosting_type = "gbdt"

# 深度學習模型
[default.models.lstm]
enabled = true
units = [128, 64, 32]
dropout = 0.2
batch_size = 32
epochs = 100
early_stopping_patience = 10
optimizer = "adam"
learning_rate = 0.001

[default.models.gru]
enabled = true
units = [100, 50]
dropout = 0.2
batch_size = 32
epochs = 100

# 模型評估
[default.evaluation]
metrics = ["mae", "rmse", "mape", "r2"]
cv_folds = 5
cv_method = "timeseries"  # timeseries, blocking
backtesting_windows = 3

# 開發環境
[development]
debug = true
log_level = "DEBUG"
sample_size = 10000  # 使用部分資料加速開發

[development.models]
quick_mode = true  # 使用較少的參數加速訓練

# 生產環境
[production]
debug = false
log_level = "INFO"
use_gpu = true
parallel_jobs = 8

[production.models]
ensemble = true  # 使用模型集成
save_predictions = true
```

## 🔧 單純時間序列分析配置

### timeseries_only_settings.toml
```toml
[default]
# 基本設定
project_name = "timeseries_analysis"
random_seed = 42

# 資料設定
[default.data]
file_path = "data/timeseries.csv"
date_column = "date"
value_columns = ["sales", "revenue"]  # 可多變量
date_format = "%Y-%m-%d"
frequency = "D"
missing_value_strategy = "interpolate"

# 探索性分析
[default.eda]
plot_components = true
check_stationarity = true
seasonal_decompose = true
decompose_model = "additive"  # additive, multiplicative

# 統計檢定
[default.statistical_tests]
adf_test = true  # Augmented Dickey-Fuller
kpss_test = true  # KPSS
acf_pacf_lags = 40

# 時間序列模型
[default.models]
# ARIMA 設定
[default.models.arima]
auto = true
seasonal = true
m = 12  # 季節性週期
stepwise = true
trace = true
error_action = "ignore"
suppress_warnings = true

# SARIMA 設定
[default.models.sarima]
order = [1, 1, 1]  # (p, d, q)
seasonal_order = [1, 1, 1, 12]  # (P, D, Q, m)
enforce_stationarity = false
enforce_invertibility = false

# 指數平滑
[default.models.exponential_smoothing]
trend = "add"  # add, mul, None
seasonal = "add"  # add, mul, None
seasonal_periods = 12
use_boxcox = true

# VAR (向量自迴歸)
[default.models.var]
maxlags = 15
ic = "aic"  # aic, bic, hqic

# 預測設定
[default.forecast]
periods = 30
confidence_intervals = [80, 95]
return_confidence = true

# 視覺化
[default.visualization]
plot_size = [12, 6]
style = "seaborn"
save_plots = true
plot_format = "png"
dpi = 300
```

## 💡 實用程式碼片段

### 1. 載入設定
```python
from config import settings

# 基本使用
data_path = settings.paths.raw_data
model_type = settings.models.xgboost.enabled

# 動態存取
model_name = "lightgbm"
model_config = settings.models[model_name]
```

### 2. 環境切換
```python
# 透過環境變數
# export ENV_FOR_DYNACONF=production

# 或在程式碼中
from config import settings
settings.setenv('production')

# 臨時切換
with settings.using_env('development'):
    debug = settings.debug  # True
```

### 3. 動態更新設定
```python
# 運行時更新
settings.set('models.lstm.epochs', 200)

# 批量更新
settings.update({
    'models.lstm.batch_size': 64,
    'models.lstm.learning_rate': 0.0001
})
```

### 4. 驗證設定
```python
# 在 config.py 中加入驗證
from dynaconf import Dynaconf, Validator

settings = Dynaconf(
    validators=[
        Validator('random_seed', must_exist=True, is_type_of=int),
        Validator('models.lstm.epochs', gte=1, lte=1000),
        Validator('preprocessing.train_test_split', gte=0.5, lte=0.95),
    ]
)
```

### 5. 機器學習管線整合
```python
# ml_pipeline.py
from config import settings
import pandas as pd
from sklearn.model_selection import train_test_split

class MLPipeline:
    def __init__(self):
        self.config = settings
        
    def load_data(self):
        df = pd.read_csv(f"{self.config.paths.raw_data}/data.csv")
        return df
        
    def preprocess(self, df):
        # 使用設定中的參數
        X_train, X_test = train_test_split(
            df, 
            test_size=1-self.config.preprocessing.train_test_split,
            random_state=self.config.random_seed
        )
        return X_train, X_test
        
    def train_models(self, X_train, y_train):
        models = {}
        
        if self.config.models.xgboost.enabled:
            from xgboost import XGBRegressor
            models['xgboost'] = XGBRegressor(
                **self.config.models.xgboost.to_dict()
            )
            
        return models
```

### 6. 時間序列特徵工程
```python
def create_time_features(df):
    config = settings.features
    
    # 延遲特徵
    for lag in config.lag_features:
        df[f'lag_{lag}'] = df[settings.timeseries.target_column].shift(lag)
    
    # 滾動統計
    for window in config.rolling_windows:
        for stat in config.rolling_stats:
            df[f'rolling_{stat}_{window}'] = (
                df[settings.timeseries.target_column]
                .rolling(window=window)
                .agg(stat)
            )
    
    return df
```

## 🔒 安全最佳實踐

### .secrets.toml
```toml
[default]
# API 金鑰
api_keys.weather_api = "your-secret-key"
api_keys.database_url = "postgresql://user:pass@localhost/db"

# 憑證
credentials.aws_access_key = "xxx"
credentials.aws_secret_key = "yyy"
```

### .env
```bash
# 環境變數
DYNACONF_DATABASE_URL="@format {env[DATABASE_URL]}"
DYNACONF_API_KEY="@format {env[API_KEY]}"
```

## 📊 進階技巧

### 1. 參數組合
```toml
# 使用 @format 進行字串插值
[default]
base_path = "/home/user/project"
data_path = "@format {this.base_path}/data"
model_path = "@format {this.base_path}/models/{env[USER]}"
```

### 2. 條件設定
```python
# 根據環境動態調整
if settings.current_env == "production":
    settings.models.lstm.epochs = 500
else:
    settings.models.lstm.epochs = 50
```

### 3. 設定繼承
```toml
# base.toml
[default.base_model]
optimizer = "adam"
loss = "mse"

# settings.toml
[default.models.lstm]
"@extends" = "base_model"
units = [128, 64]
```

## 🎯 常見模式

### 實驗追蹤
```python
# 將設定保存用於實驗重現
import json
from datetime import datetime

experiment_config = {
    'timestamp': datetime.now().isoformat(),
    'settings': settings.as_dict(),
    'git_hash': get_git_hash()
}

with open('experiments/exp_001.json', 'w') as f:
    json.dump(experiment_config, f)
```

### 超參數搜尋
```python
# 動態更新超參數
from sklearn.model_selection import GridSearchCV

param_grid = {
    'n_estimators': [100, 200, 300],
    'max_depth': [5, 10, 15]
}

for params in ParameterGrid(param_grid):
    settings.update({'models.random_forest': params})
    # 訓練模型...
```

## 🐛 除錯技巧

```python
# 查看所有設定
print(settings.as_dict())

# 查看特定設定來源
print(settings.get_fresh('models.lstm.epochs'))

# 列出所有環境
print(settings.list_envs())

# 驗證設定
settings.validators.validate()
```