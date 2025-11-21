# TDD-043: LoadRepliesUseCase

## Ticket 資訊

| 欄位 | 值 |
|------|-----|
| **Ticket ID** | TDD-043 |
| **標題** | 實作 LoadRepliesUseCase |
| **類型** | UseCase |
| **優先級** | P0 |
| **所屬 Feature** | PrematchComment |
| **依賴 Ticket** | TDD-030 |
| **Story Point** | 5 |
| **估時（Mid-level）** | 5 天 |

## 描述

實作 LoadRepliesUseCase。

## 需求

1. 實作 UseCase 商業邏輯
2. 整合 PrematchCommentRepository 呼叫
3. 實作 Input/Output Model 驗證
4. 實作 Error Handling
5. 支援分頁載入（每次最多 5 筆）
6. 支援 cursor 機制

## 驗收條件

- [ ] UseCase 商業邏輯實作完成
- [ ] 分頁邏輯實作完成
- [ ] 所有 Test Scenarios 通過（Basic / Loop / Error / Edge Case）
- [ ] Unit Test 覆蓋率 ≥ 90%
- [ ] Integration Test 通過

## 相關文件

- UseCase 定義：`output/LiveChat&PrematchComment/03_Module Responsibility/01_module_responsibility.md`
- Input/Output Model：`output/LiveChat&PrematchComment/07_UseCase Input & Output Model/01_usecase_input_output.md`
- Test Scenarios：`output/LiveChat&PrematchComment/10_Test Scenarios/01_test_scenarios.md`

