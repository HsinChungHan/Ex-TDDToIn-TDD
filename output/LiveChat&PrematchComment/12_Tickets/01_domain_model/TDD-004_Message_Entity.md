# TDD-004: Message Entity

## Ticket 資訊

| 欄位 | 值 |
|------|-----|
| **Ticket ID** | TDD-004 |
| **標題** | 實作 Message Entity |
| **類型** | Domain Model |
| **優先級** | P0 |
| **所屬 Feature** | LiveChat |
| **依賴 Ticket** | - |
| **Story Point** | 1 |
| **估時（Mid-level）** | 1 天 |

## 描述

定義 Message Entity Domain Model。

## 需求

1. 定義 Message 結構
2. 實作 Identifiable 和 Equatable
3. 定義所有必要欄位（id、content、messageNo、authorId 等）
4. 定義 MessageType enum

## 驗收條件

- [ ] Message Entity 定義完成
- [ ] MessageType enum 定義完成
- [ ] 所有欄位類型正確
- [ ] Equatable 實作正確（透過 id 比較）
- [ ] Unit Test 覆蓋率 ≥ 80%

## 相關文件

- Domain Model 定義：`output/LiveChat&PrematchComment/04_Domain Model/01_domain_model.md`

