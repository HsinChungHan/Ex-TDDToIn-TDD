# TDD-005: ChatroomInfo Entity

## Ticket 資訊

| 欄位 | 值 |
|------|-----|
| **Ticket ID** | TDD-005 |
| **標題** | 實作 ChatroomInfo Entity |
| **類型** | Domain Model |
| **優先級** | P0 |
| **所屬 Feature** | LiveChat |
| **依賴 Ticket** | - |
| **Story Point** | 1 |
| **估時（Mid-level）** | 1 天 |

## 描述

定義 ChatroomInfo Entity Domain Model。

## 需求

1. 定義 ChatroomInfo 結構
2. 實作 Identifiable 和 Equatable
3. 定義所有必要欄位（chatroomId、lastMessageNo）

## 驗收條件

- [ ] ChatroomInfo Entity 定義完成
- [ ] 所有欄位類型正確
- [ ] Equatable 實作正確（透過 chatroomId 比較）
- [ ] Unit Test 覆蓋率 ≥ 80%

## 相關文件

- Domain Model 定義：`output/LiveChat&PrematchComment/04_Domain Model/01_domain_model.md`

