# TDD-006: Value Objects

## Ticket 資訊

| 欄位 | 值 |
|------|-----|
| **Ticket ID** | TDD-006 |
| **標題** | 實作 Value Objects（SortMode、Cursor、PagingInfo） |
| **類型** | Domain Model |
| **優先級** | P0 |
| **所屬 Feature** | LiveChat & PrematchComment |
| **依賴 Ticket** | - |
| **Story Point** | 2 |
| **估時（Mid-level）** | 2 天 |

## 描述

定義所有 Value Objects（SortMode、Cursor、PagingInfo）。

## 需求

1. 定義 SortMode Value Object（top / newest）
2. 定義 Cursor Value Object（分頁游標）
3. 定義 PagingInfo Value Object（hasMore、nextCursor）
4. 實作 Equatable（透過值比較）
5. 確保完全不可變

## 驗收條件

- [ ] SortMode Value Object 定義完成
- [ ] Cursor Value Object 定義完成
- [ ] PagingInfo Value Object 定義完成
- [ ] 所有 Value Object 完全不可變
- [ ] Equatable 實作正確（透過值比較）
- [ ] Unit Test 覆蓋率 ≥ 80%

## 相關文件

- Domain Model 定義：`output/LiveChat&PrematchComment/04_Domain Model/01_domain_model.md`

