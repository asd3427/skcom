群益證券聽牌套件 (skcom)
========================

此套件用來降低群益證券 API (SKCOM.dll) 的使用門檻，目前可蒐集歷史與即時報價資訊，也提供輔助工具引導安裝 API 元件，
透過輔助安裝工具會安裝 VC++ 可轉發套件中較為安全的版本，比依照官方文件安裝更理想

此套件僅相容 Windows 64 位元, 無法確保在其它環境正常運作, 驗證環境如下:

- Windows 10 64位元, Anaconda 2019.03 Python 3.7 version
- Windows 10 64位元, Python 3.5

**注意事項**

- **此套件並非群益證券開發, 使用問題請透過 GitHub Issue 回報**
- **在群益證券開戶, 並且開通 API 使用權限後才能使用**

功能
====

0.9.0 首次發布

- 蒐集日 K 資料
- 接收即時撮合結果
- 必要環境輔助安裝工具 (Visual C++ 可轉發套件與 SKCOM.dll)

環境安裝
========

安裝相依套件, 安裝前會提示要求管理者權限

.. code:: powershell

  (base) PS>pip install skcom
  (base) PS>python -m skcom.samples.setup
  安裝 Visual C++ 2010 可轉發套件
  Visual C++ 2010 可轉發套件已安裝, 版本: 10.0.40219.325
  安裝與註冊群益 API 元件
  群益 API 元件已安裝, 版本: 2.13.16.0

使用 Ticks 監聽範例
===================

.. code:: powershell

  (base) PS>python -m skcom.samples.ticks
  登入成功
  連線成功
  連線就緒
  [2330 台積電] 時間:09:00:00.530 買:0.00 賣:0.00 成:233.50 單量:3594 總量:3594
  [2330 台積電] 時間:09:00:05.543 買:233.00 賣:233.50 成:233.50 單量:87 總量:3681
  [2330 台積電] 時間:09:00:10.558 買:233.00 賣:233.50 成:233.50 單量:3 總量:3684
  [2330 台積電] 時間:09:00:15.573 買:233.00 賣:233.50 成:233.00 單量:31 總量:3715
  [2330 台積電] 時間:09:00:20.588 買:233.00 賣:233.50 成:233.50 單量:20 總量:3735
  [2330 台積電] 時間:09:00:25.603 買:233.00 賣:233.50 成:233.00 單量:15 總量:3750
  [2330 台積電] 時間:09:00:30.618 買:233.00 賣:233.50 成:233.00 單量:22 總量:3772
  [2330 台積電] 時間:09:00:35.633 買:233.00 賣:233.50 成:233.50 單量:6 總量:3778
  [2330 台積電] 時間:09:00:40.649 買:233.00 賣:233.50 成:233.00 單量:8 總量:3786
  [2330 台積電] 時間:09:00:45.661 買:233.00 賣:233.50 成:233.00 單量:52 總量:3838
  ...
  偵測到 Ctrl+C, 結束監聽
  斷線
  監聽結束

使用日 K 監聽範例
=================

.. code:: powershell

  (base) PS>python -m skcom.samples.kline
  登入成功
  連線成功
  連線就緒
  [2330 台積電] 的日K資料
  >> 日期:2019-05-17 開:249.00 收:241.50 高:249.00 低:241.50 量:38585
  >> 日期:2019-05-20 開:242.50 收:238.00 高:243.00 低:238.00 量:39105
  >> 日期:2019-05-21 開:233.50 收:234.00 高:236.00 低:232.50 量:79971
  >> 日期:2019-05-22 開:236.50 收:238.00 高:240.50 低:235.50 量:34587
  >> 日期:2019-05-23 開:233.50 收:230.00 高:233.50 低:230.00 量:58651
  ...
  偵測到 Ctrl+C, 結束監聽
  斷線
  監聽結束

交易日重要時機
===============

- 09:00 開盤, T < 09:00:00.000 會產生大量試撮 Ticks, 不可計入成交量
- 13:25 準備收盤, 13:25:00.000 <= T < 13:30:00.000 會產生大量試撮 Ticks, 不可計入成交量
- 13:30 收盤, 13:30:00.000 會有最後一筆撮合, 要計入成交量
- 14:30 零股撮合
- 14:37 零股撮合 Ticks 事件觸發, 這時候才能收到零股交易資料
- 14:40 系統疑似統計中, 查詢個股資訊沒有回應
- 14:45 日 K 資料出現當日交易, 系統恢復正常
