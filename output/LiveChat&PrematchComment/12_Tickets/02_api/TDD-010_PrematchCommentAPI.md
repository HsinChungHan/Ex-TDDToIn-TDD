# TDD-010: PrematchCommentAPI

## Ticket 資訊

| 欄位 | 值 |
|------|-----|
| **Ticket ID** | TDD-010 |
| **標題** | 實作 PrematchCommentAPI 定義 |
| **類型** | API |
| **優先級** | P0 |
| **所屬 Feature** | PrematchComment |
| **依賴 Ticket** | TDD-001, TDD-002, TDD-006 |
| **Story Point** | 3 |
| **估時（Mid-level）** | 3 天 |

## 描述

實作 PrematchCommentAPI API 定義。

## 需求

1. 定義所有 Endpoints（URL、Method、Request/Response DTO）
2. 定義 Error Response 格式
3. 定義 API 文件註解
4. Endpoints：
   - `GET /chat/match/comment/info/{refId}`
   - `GET /chat/match/comment/popular`
   - `GET /chat/match/comment/newest`
   - `GET /chat/match/comment/replies`
   - `POST /chat/match/comment`
   - `POST /chat/match/comment/like`

## 驗收條件

- [ ] 所有 Endpoints 定義完成
- [ ] Request/Response DTO 定義完成
- [ ] API 文件註解完整
- [ ] Error Response 格式定義完成

## 相關文件

- API Spec：`output/LiveChat&PrematchComment/08_API Spec & Mapping/01_api_spec.md`
- Module Responsibility：`output/LiveChat&PrematchComment/03_Module Responsibility/01_module_responsibility.md`

