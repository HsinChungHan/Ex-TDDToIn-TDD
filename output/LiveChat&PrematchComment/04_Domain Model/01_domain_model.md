# Domain Model 定義

## Entity（實體）

### Comment

留言實體，代表一則留言或回覆。

```swift
struct Comment: Identifiable, Equatable {
    let id: String  // 唯一識別碼
    let content: String  // 留言內容
    var likeCount: Int  // 按讚數（可變）
    let authorId: String  // 作者 ID
    let authorNickname: String?  // 作者暱稱（可選）
    let parentId: String?  // 父留言 ID（可選，用於回覆）
    let createdAt: Date  // 建立時間
    let refId: String  // 賽事參考 ID
    
    // 透過 id 來比較
    static func == (lhs: Comment, rhs: Comment) -> Bool {
        lhs.id == rhs.id
    }
}
```

**判斷依據**：
- ✅ 有唯一識別碼（id）
- ✅ 需要追蹤物件的狀態變化（likeCount）
- ✅ 業務語意上這是「一則留言」

### CommentMeta

留言統計資訊實體，代表賽事的留言統計。

```swift
struct CommentMeta: Identifiable, Equatable {
    let id: String  // 唯一識別碼（通常為 refId）
    let commentCount: Int  // 留言數量
    let betCount: Int  // 投注數量
    let refId: String  // 賽事參考 ID
    
    // 透過 id 來比較
    static func == (lhs: CommentMeta, rhs: CommentMeta) -> Bool {
        lhs.id == rhs.id
    }
}
```

**判斷依據**：
- ✅ 有唯一識別碼（id，通常為 refId）
- ✅ 需要追蹤物件的狀態變化（commentCount、betCount）
- ✅ 業務語意上這是「一個賽事的留言統計」

### UserInfo

用戶資訊實體，代表用戶的基本資訊。

```swift
struct UserInfo: Identifiable, Equatable {
    let id: String  // 唯一識別碼（用戶 ID）
    let nickname: String?  // 暱稱（可選）
    let avatar: String?  // 頭像 URL（可選）
    
    // 透過 id 來比較
    static func == (lhs: UserInfo, rhs: UserInfo) -> Bool {
        lhs.id == rhs.id
    }
}
```

**判斷依據**：
- ✅ 有唯一識別碼（id）
- ✅ 需要追蹤物件的狀態變化（nickname、avatar）
- ✅ 業務語意上這是「一個用戶」

### Message

聊天訊息實體，代表一則聊天訊息。

```swift
struct Message: Identifiable, Equatable {
    let id: String  // 唯一識別碼
    let content: String  // 訊息內容
    let messageNo: Int  // 訊息編號（用於排序和去重）
    let authorId: String  // 作者 ID
    let authorNickname: String?  // 作者暱稱（可選）
    let messageType: MessageType  // 訊息類型（TEXT 等）
    let createdAt: Date  // 建立時間
    let chatroomId: String  // 聊天室 ID
    
    // 透過 id 來比較
    static func == (lhs: Message, rhs: Message) -> Bool {
        lhs.id == rhs.id
    }
}
```

**判斷依據**：
- ✅ 有唯一識別碼（id）
- ✅ 需要追蹤物件的狀態變化
- ✅ 業務語意上這是「一則聊天訊息」

### ChatroomInfo

聊天室資訊實體，代表聊天室的基本資訊。

```swift
struct ChatroomInfo: Identifiable, Equatable {
    let id: String  // 唯一識別碼（通常為 chatroomId）
    let chatroomId: String  // 聊天室 ID
    let lastMessageNo: Int  // 最後訊息編號
    let refId: String  // 賽事參考 ID
    
    // 透過 id 來比較
    static func == (lhs: ChatroomInfo, rhs: ChatroomInfo) -> Bool {
        lhs.id == rhs.id
    }
}
```

**判斷依據**：
- ✅ 有唯一識別碼（id，通常為 chatroomId）
- ✅ 需要追蹤物件的狀態變化（lastMessageNo）
- ✅ 業務語意上這是「一個聊天室」

## Value Object（值物件）

### SortMode

排序模式值物件，用於區分留言的排序方式。

```swift
struct SortMode: Equatable {
    let value: String  // "top" 或 "newest"
    
    static let top = SortMode(value: "top")
    static let newest = SortMode(value: "newest")
    
    // 兩個 SortMode 如果 value 相同，就視為相同
    // Swift 自動實現 Equatable
}
```

**判斷依據**：
- ✅ 沒有唯一識別碼
- ✅ 只關心物件的「值」，不關心「是哪一個」
- ✅ 完全不可變（使用 `let`）
- ✅ 業務語意上這是「排序方式」

### Cursor

分頁游標值物件，用於追蹤分頁位置。

```swift
struct Cursor: Equatable {
    let value: String?  // 游標值（可選）
    
    // 兩個 Cursor 如果 value 相同，就視為相同
    // Swift 自動實現 Equatable
}
```

**判斷依據**：
- ✅ 沒有唯一識別碼
- ✅ 只關心物件的「值」，不關心「是哪一個」
- ✅ 完全不可變（使用 `let`）
- ✅ 業務語意上這是「分頁游標」
- ✅ 可被多個 Feature 共享使用（放在 SharedValueObjects）

### PagingInfo

分頁資訊值物件，用於描述分頁狀態。

```swift
struct PagingInfo: Equatable {
    let hasMore: Bool  // 是否還有更多資料
    let nextCursor: Cursor?  // 下一頁游標（可選）
    
    // 兩個 PagingInfo 如果所有屬性相同，就視為相同
    // Swift 自動實現 Equatable
}
```

**判斷依據**：
- ✅ 沒有唯一識別碼
- ✅ 只關心物件的「值」，不關心「是哪一個」
- ✅ 完全不可變（使用 `let`）
- ✅ 業務語意上這是「分頁資訊」
- ✅ 可被多個 Feature 共享使用（放在 SharedValueObjects）

### MessageType

訊息類型值物件，用於區分訊息類型。

```swift
struct MessageType: Equatable {
    let value: String  // "TEXT" 等
    
    static let text = MessageType(value: "TEXT")
    
    // 兩個 MessageType 如果 value 相同，就視為相同
    // Swift 自動實現 Equatable
}
```

**判斷依據**：
- ✅ 沒有唯一識別碼
- ✅ 只關心物件的「值」，不關心「是哪一個」
- ✅ 完全不可變（使用 `let`）
- ✅ 業務語意上這是「訊息類型」

## Domain Model 關係

### 語意化關係

- **Comment** `has author` **UserInfo**（留言有作者）
- **Comment** `references` **Comment**（回覆引用父留言）
- **Comment** `belongs to` **CommentMeta**（留言屬於留言統計）
- **Message** `has author` **UserInfo**（訊息有作者）
- **Message** `belongs to` **ChatroomInfo**（訊息屬於聊天室）
- **PagingInfo** `contains` **Cursor**（分頁資訊包含游標）

### 關係說明表格

| 關係 | 語意化關係 | 標準 UML 關係 | 關係說明 | 實作方式 | 方向 |
|------|-----------|-------------|---------|---------|------|
| Comment → UserInfo | has author | Association | 留言有作者 | `authorId: String`（引用 UserInfo.id） | 單向 |
| Comment → Comment | references | Association | 回覆引用父留言 | `parentId: String?`（引用 Comment.id） | 單向 |
| Comment → CommentMeta | belongs to | Association | 留言屬於留言統計 | `refId: String`（引用 CommentMeta.refId） | 單向 |
| Message → UserInfo | has author | Association | 訊息有作者 | `authorId: String`（引用 UserInfo.id） | 單向 |
| Message → ChatroomInfo | belongs to | Association | 訊息屬於聊天室 | `chatroomId: String`（引用 ChatroomInfo.chatroomId） | 單向 |
| PagingInfo → Cursor | contains | Composition | 分頁資訊包含游標 | `nextCursor: Cursor?`（直接包含） | 單向 |

## 可共享的 Value Object

以下 Value Object 可被多個 Feature 共享使用，應放在 `SharedValueObjects` 中：

- **Cursor**：分頁游標
- **PagingInfo**：分頁資訊

## Domain Model vs DTO 對照

| 特性 | Domain Model | DTO |
|------|-------------|-----|
| **用途** | 業務領域的資料模型 | API 傳輸的資料結構 |
| **存在位置** | Domain Model Layer | Client Layer ↔ API Layer |
| **可被使用** | Feature、UseCase、Repository | Client、API |
| **不可被使用** | - | Feature、UseCase、Repository |
| **轉換** | - | Repository 負責 DTO → Domain Model |
| **建模依據** | 業務語意 | API schema |
| **Swift 類型** | `struct`（Entity / Value Object） | `struct`（通常） |

## 在 TCA 中的使用

### Feature State 中的 Domain Model

```swift
struct PrematchCommentState {
    var comments: [Comment]  // ✅ Entity 陣列
    var pagingInfo: PagingInfo?  // ✅ Value Object
    var cursor: Cursor?  // ✅ Value Object
    var sortMode: SortMode  // ✅ Value Object
    // var apiResponse: CommentResponseDTO  // ❌ 禁止
}
```

### UseCase Input / Output 中的 Domain Model

```swift
struct ReloadCommentListInput {
    let refId: String
    let mode: SortMode  // ✅ Value Object
    let cursor: Cursor?  // ✅ Value Object
    // let requestDTO: CommentRequestDTO  // ❌ 禁止
}

struct ReloadCommentListOutput {
    let comments: [Comment]  // ✅ Entity 陣列
    let pagingInfo: PagingInfo  // ✅ Value Object
}
```

### Repository 中的 Domain Model

```swift
func getComments(refId: String, mode: SortMode) async throws -> [Comment] {
    let dto = try await client.getComments(refId, mode: mode.value)
    return dto.map { Comment(from: $0) }  // DTO → Domain Model
}
```

