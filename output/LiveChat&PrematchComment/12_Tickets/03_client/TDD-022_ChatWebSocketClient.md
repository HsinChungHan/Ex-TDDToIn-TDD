# TDD-022: ChatWebSocketClient

## Ticket 資訊

| 欄位 | 值 |
|------|-----|
| **Ticket ID** | TDD-022 |
| **標題** | 實作 ChatWebSocketClient（WebSocket） |
| **類型** | Client |
| **優先級** | P0 |
| **所屬 Feature** | LiveChat |
| **依賴 Ticket** | TDD-011 |
| **Story Point** | 13 |
| **估時（Mid-level）** | 13 天 |

## 描述

實作 ChatWebSocketClient（WebSocket）。

## 需求

1. 實作 WebSocket 通訊邏輯
2. 實作訂閱 / 取消訂閱
3. 實作訊息接收與發送
4. 實作連線管理（重連、心跳等）
5. 實作指數退避重連策略

## 驗收條件

- [ ] WebSocket 連線管理實作完成
- [ ] 訂閱 / 取消訂閱實作完成
- [ ] 訊息接收與發送實作完成
- [ ] 重連機制實作完成
- [ ] 心跳機制實作完成
- [ ] Unit Test 覆蓋率 ≥ 80%
- [ ] Integration Test 通過

## 相關文件

- API Spec：`output/LiveChat&PrematchComment/08_API Spec & Mapping/01_api_spec.md`
- Module Responsibility：`output/LiveChat&PrematchComment/03_Module Responsibility/01_module_responsibility.md`

## 調整因子說明

- 基礎估時：複雜 Client = 8 SP
- +3 SP：WebSocket
- +2 SP：複雜 Reconnect 邏輯

