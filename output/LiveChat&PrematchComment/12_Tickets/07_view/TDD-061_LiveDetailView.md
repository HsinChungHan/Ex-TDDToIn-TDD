# TDD-061: LiveDetailView

## Ticket 資訊

| 欄位 | 值 |
|------|-----|
| **Ticket ID** | TDD-061 |
| **標題** | 實作 LiveDetailView |
| **類型** | View |
| **優先級** | P0 |
| **所屬 Feature** | LiveChat |
| **依賴 Ticket** | TDD-051 |
| **Story Point** | 13 |
| **估時（Mid-level）** | 13 天 |

## 描述

實作 LiveDetailView。

## 需求

1. 實作 UI 元件
2. 整合 LiveChatFeature（TCA）
3. 實作 UI 狀態綁定
4. 實作使用者互動處理
5. 實作 Loading/Error 狀態顯示
6. 實作聊天室訊息列表顯示
7. 實作即時訊息接收和顯示
8. 實作歷史訊息載入（滾動到頂部）
9. 實作用戶操作選單（封鎖、跳轉個人主頁）

## 驗收條件

- [ ] UI 元件實作完成
- [ ] Feature 整合完成
- [ ] 所有使用者互動流程測試通過
- [ ] WebSocket 訊息即時顯示正確
- [ ] Loading/Error 狀態顯示正確
- [ ] UI Test 覆蓋率 ≥ 70%

## 相關文件

- Module Sequence Diagram：`output/LiveChat&PrematchComment/05. Module Sequence Diagram（模組序列圖）/LiveChat/Module Sequence Diagrams/`

## 調整因子說明

- 基礎估時：複雜 View = 13 SP
- +2 SP：複雜的列表 UI（聊天訊息列表）
- +2 SP：需要處理多種 Loading/Error 狀態
- +1 SP：WebSocket 即時訊息處理

