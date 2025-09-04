# FAQ
- 原資料要不要做transform
- DWT 要拆幾次，哪些要餵給arima，哪些要餵給深度學習模型，要用哪種小波變換?
    在Time Series Forecasting using Hybrid ARIMA and 
    ANN Models based on DWT Decomposition中
    他將高頻訊號給arima，低頻訊號給ANN模型，可能兩種實驗都可以做做
    小波變換的選擇db1, db2, db4, db8或者平均 都可以做做實驗試試效果，比較不同資料間的共同性，**也許可以找到一個最佳的選項或者是算法來選擇哪種小波變換**
- ARIMA參數要怎麼調
  - ACF/PACF
  - AIC/BIC
- 選用哪個model，要輸入哪些東西
  - Dlinear
  - tsmixer
- 預測多少步
    可能要去看看time-series-library裡面的long term short term是怎麼定義的
- 怎麼處理多變量
    可以把所有變量都用小波變換拆開後做兩個實驗
    - 只丟殘差部分
        如果只把殘差丟進去，等於強制假設「非線性 = 原始序列 − 線性部分」。
        但現實世界的數據（像 GIS response time）可能不是簡單的「線性 + 非線性」，而是更複雜的交互。
    - 殘差和arima預測值和歷史值都丟

# 待辦清單
- 搞清楚time-series-library的data flow還有setting
- 想辦法在experiment裡面的train函數加上
  - yeo-johnson transform
  - WT
    - db1, db2, db4, average
    - generate arima prediction and residual on every single feature 
  - 看要丟哪些東西進去model，然後可能要更改model的張量
  - inverse yeo-johnson transform