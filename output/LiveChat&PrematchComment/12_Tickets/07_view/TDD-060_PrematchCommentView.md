# TDD-060: PrematchCommentView

## Ticket 資訊

| 欄位 | 值 |
|------|-----|
| **Ticket ID** | TDD-060 |
| **標題** | 實作 PrematchCommentView |
| **類型** | View |
| **優先級** | P0 |
| **所屬 Feature** | PrematchComment |
| **依賴 Ticket** | TDD-050 |
| **Story Point** | 13 |
| **估時（Mid-level）** | 13 天 |

## 描述

實作 PrematchCommentView。

## 需求

1. 實作 UI 元件
2. 整合 PrematchCommentFeature（TCA）
3. 實作 UI 狀態綁定
4. 實作使用者互動處理
5. 實作 Loading/Error 狀態顯示
6. 實作留言列表顯示（支援 top/newest 切換）
7. 實作回覆列表顯示（支援展開/收起）
8. 實作分頁載入更多

## 驗收條件

- [ ] UI 元件實作完成
- [ ] Feature 整合完成
- [ ] 所有使用者互動流程測試通過
- [ ] Loading/Error 狀態顯示正確
- [ ] UI Test 覆蓋率 ≥ 70%

## 相關文件

- Module Sequence Diagram：`output/LiveChat&PrematchComment/05. Module Sequence Diagram（模組序列圖）/PrematchComment/Module Sequence Diagrams/`

## 調整因子說明

- 基礎估時：複雜 View = 13 SP
- +2 SP：複雜的列表 UI（留言列表、回覆列表）
- +2 SP：需要處理多種 Loading/Error 狀態

