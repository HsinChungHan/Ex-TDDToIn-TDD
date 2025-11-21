# TDD-048: BlockUserUseCase

## Ticket 資訊

| 欄位 | 值 |
|------|-----|
| **Ticket ID** | TDD-048 |
| **標題** | 實作 BlockUserUseCase |
| **類型** | UseCase |
| **優先級** | P1 |
| **所屬 Feature** | LiveChat |
| **依賴 Ticket** | TDD-031 |
| **Story Point** | 3 |
| **估時（Mid-level）** | 3 天 |

## 描述

實作 BlockUserUseCase。

## 需求

1. 實作 UseCase 商業邏輯
2. 整合 LiveChatRepository 呼叫
3. 實作 Input/Output Model 驗證
4. 實作 Error Handling
5. 實作將用戶加入黑名單 DB 的邏輯
6. 實作 WebSocket 重連後同步 blocked user 清單的邏輯

## 驗收條件

- [ ] UseCase 商業邏輯實作完成
- [ ] 黑名單管理邏輯實作完成
- [ ] 所有 Test Scenarios 通過（Basic / Branch / Error）
- [ ] Unit Test 覆蓋率 ≥ 90%
- [ ] Integration Test 通過

## 相關文件

- UseCase 定義：`output/LiveChat&PrematchComment/03_Module Responsibility/01_module_responsibility.md`
- Input/Output Model：`output/LiveChat&PrematchComment/07_UseCase Input & Output Model/01_usecase_input_output.md`
- Test Scenarios：`output/LiveChat&PrematchComment/10_Test Scenarios/01_test_scenarios.md`

