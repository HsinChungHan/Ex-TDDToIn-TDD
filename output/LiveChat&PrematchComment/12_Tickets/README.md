# Tickets

本目錄包含所有開發 Ticket 和估時資訊。

## 文件列表

### Domain Model
- `01_domain_model/TDD-001_Comment_Entity.md`
- `01_domain_model/TDD-002_CommentMeta_Entity.md`
- `01_domain_model/TDD-003_UserInfo_Entity.md`
- `01_domain_model/TDD-004_Message_Entity.md`
- `01_domain_model/TDD-005_ChatroomInfo_Entity.md`
- `01_domain_model/TDD-006_ValueObjects.md`

### API
- `02_api/TDD-010_PrematchCommentAPI.md`
- `02_api/TDD-011_ChatAPI.md`

### Client
- `03_client/TDD-020_PrematchCommentClient.md`
- `03_client/TDD-021_LiveChatClient.md`
- `03_client/TDD-022_ChatWebSocketClient.md`

### Repository
- `04_repository/TDD-030_PrematchCommentRepository.md`
- `04_repository/TDD-031_LiveChatRepository.md`

### UseCase
- `05_usecase/TDD-040_ReloadCommentListUseCase.md`
- `05_usecase/TDD-041_PublishCommentUseCase.md`
- `05_usecase/TDD-042_ToggleLikeUseCase.md`
- `05_usecase/TDD-043_LoadRepliesUseCase.md`
- `05_usecase/TDD-044_NavigateToProfileUseCase.md`
- `05_usecase/TDD-045_SendChatMessageUseCase.md`
- `05_usecase/TDD-046_JoinChatroomUseCase.md`
- `05_usecase/TDD-047_LeaveChatroomUseCase.md`
- `05_usecase/TDD-048_BlockUserUseCase.md`

### Feature
- `06_feature/TDD-050_PrematchCommentFeature.md`
- `06_feature/TDD-051_LiveChatFeature.md`

### View
- `07_view/TDD-060_PrematchCommentView.md`
- `07_view/TDD-061_LiveDetailView.md`

## Ticket 開發順序

### Phase 1: Domain Model（優先）
1. TDD-001 ~ TDD-006：Domain Model 定義

### Phase 2: Data & Infrastructure Layer
2. TDD-010 ~ TDD-011：API 定義
3. TDD-020 ~ TDD-022：Client 實作
4. TDD-030 ~ TDD-031：Repository 實作

### Phase 3: Domain Layer
5. TDD-040 ~ TDD-048：UseCase 實作
6. TDD-050 ~ TDD-051：Feature 實作

### Phase 4: UI Layer
7. TDD-060 ~ TDD-061：View 實作

## 估時說明

- **Story Point (SP)**：使用 Fibonacci 數列（1, 2, 3, 5, 8, 13, 21）
- **工時轉換**：
  - Junior（0-2 年）：1 SP = 1.5 天
  - Mid-level（2-5 年）：1 SP = 1 天
  - Senior（5+ 年）：1 SP = 0.5 天

## 依賴關係

Ticket 必須按照依賴順序開發：
- Domain Model → API → Client → Repository → UseCase → Feature → View

## 總估時統計

### 按層級統計

| 層級 | Ticket 數量 | 總 Story Point | Mid-level 總工時 |
|------|------------|--------------|----------------|
| **Domain Model** | 6 | 7 SP | 7 天 |
| **API** | 2 | 8 SP | 8 天 |
| **Client** | 3 | 21 SP | 21 天 |
| **Repository** | 2 | 13 SP | 13 天 |
| **UseCase** | 9 | 62 SP | 62 天 |
| **Feature** | 2 | 21 SP | 21 天 |
| **View** | 2 | 26 SP | 26 天 |
| **總計** | **26** | **158 SP** | **158 天** |

### 按 Feature 統計

| Feature | Ticket 數量 | 總 Story Point | Mid-level 總工時 |
|---------|------------|--------------|----------------|
| **PrematchComment** | 14 | 75 SP | 75 天 |
| **LiveChat** | 12 | 83 SP | 83 天 |

### 注意事項

1. **估時僅供參考**：實際開發時間可能因工程師經驗、專案複雜度、技術債務等因素而有所不同
2. **需要定期調整**：根據實際開發情況，應定期調整估時規則
3. **考慮並行開發**：某些 Ticket 可以並行開發（如 Domain Model 的 Entity 可以並行）
4. **考慮風險緩衝**：建議在總估時基礎上增加 20-30% 的緩衝時間
5. **總估時（含緩衝）**：158 天 × 1.25 = **197.5 天**（約 40 週，以 Mid-level 工程師計算）

