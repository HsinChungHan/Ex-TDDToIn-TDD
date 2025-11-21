# API Spec & Mapping

## PrematchCommentAPI

### GET /chat/match/comment/info/{refId}

**所屬 UseCase**：ReloadCommentListUseCase（初始化時）

**HTTP Method**：GET

**URL**：`/chat/match/comment/info/{refId}`

**Request**：
- Path Parameter：`refId: String`

**Response DTO**：
```swift
struct CommentMetaResponseDTO: Codable {
    let commentCount: Int
    let betCount: Int
    let refId: String
}
```

**Mapping**：
```swift
extension CommentMeta {
    init(from dto: CommentMetaResponseDTO) {
        self.id = dto.refId
        self.commentCount = dto.commentCount
        self.betCount = dto.betCount
        self.refId = dto.refId
    }
}
```

**呼叫鏈**：ReloadCommentListUseCase → PrematchCommentRepository → PrematchCommentClient → PrematchCommentAPI

---

### GET /chat/match/comment/popular

**所屬 UseCase**：ReloadCommentListUseCase（mode = .top）

**HTTP Method**：GET

**URL**：`/chat/match/comment/popular`

**Request**：
- Query Parameter：`refId: String`
- Query Parameter：`cursor: String?`（可選，用於分頁）

**Response DTO**：
```swift
struct CommentListResponseDTO: Codable {
    let comments: [CommentDTO]
    let pagingInfo: PagingInfoDTO?
}

struct CommentDTO: Codable {
    let id: String
    let content: String
    let likeCount: Int
    let authorId: String
    let authorNickname: String?
    let parentId: String?
    let createdAt: String  // ISO 8601
    let refId: String
}

struct PagingInfoDTO: Codable {
    let hasMore: Bool
    let nextCursor: String?
}
```

**Mapping**：
```swift
extension Comment {
    init(from dto: CommentDTO) {
        self.id = dto.id
        self.content = dto.content
        self.likeCount = dto.likeCount
        self.authorId = dto.authorId
        self.authorNickname = dto.authorNickname
        self.parentId = dto.parentId
        self.createdAt = ISO8601DateFormatter().date(from: dto.createdAt) ?? Date()
        self.refId = dto.refId
    }
}

extension PagingInfo {
    init(from dto: PagingInfoDTO) {
        self.hasMore = dto.hasMore
        self.nextCursor = dto.nextCursor.map { Cursor(value: $0) }
    }
}
```

**呼叫鏈**：ReloadCommentListUseCase → PrematchCommentRepository → PrematchCommentClient → PrematchCommentAPI

---

### GET /chat/match/comment/newest

**所屬 UseCase**：ReloadCommentListUseCase（mode = .newest）

**HTTP Method**：GET

**URL**：`/chat/match/comment/newest`

**Request**：
- Query Parameter：`refId: String`
- Query Parameter：`cursor: String?`（可選，用於分頁）

**Response DTO**：同 `GET /chat/match/comment/popular`

**Mapping**：同 `GET /chat/match/comment/popular`

**呼叫鏈**：ReloadCommentListUseCase → PrematchCommentRepository → PrematchCommentClient → PrematchCommentAPI

---

### POST /chat/match/comment

**所屬 UseCase**：PublishCommentUseCase

**HTTP Method**：POST

**URL**：`/chat/match/comment`

**Request DTO**：
```swift
struct PublishCommentRequestDTO: Codable {
    let refId: String
    let content: String
    let parentId: String?  // 可選，用於回覆
}
```

**Response DTO**：
```swift
struct CommentResponseDTO: Codable {
    let id: String
    let content: String
    let likeCount: Int
    let authorId: String
    let authorNickname: String?
    let parentId: String?
    let createdAt: String
    let refId: String
}
```

**Mapping**：
```swift
extension Comment {
    init(from dto: CommentResponseDTO) {
        self.id = dto.id
        self.content = dto.content
        self.likeCount = dto.likeCount
        self.authorId = dto.authorId
        self.authorNickname = dto.authorNickname
        self.parentId = dto.parentId
        self.createdAt = ISO8601DateFormatter().date(from: dto.createdAt) ?? Date()
        self.refId = dto.refId
    }
}
```

**呼叫鏈**：PublishCommentUseCase → PrematchCommentRepository → PrematchCommentClient → PrematchCommentAPI

---

### POST /chat/match/comment/like

**所屬 UseCase**：ToggleLikeUseCase

**HTTP Method**：POST

**URL**：`/chat/match/comment/like`

**Request DTO**：
```swift
struct ToggleLikeRequestDTO: Codable {
    let commentId: String
}
```

**Response DTO**：同 `POST /chat/match/comment` 的 Response

**Mapping**：同 `POST /chat/match/comment`

**呼叫鏈**：ToggleLikeUseCase → PrematchCommentRepository → PrematchCommentClient → PrematchCommentAPI

---

### GET /chat/match/comment/replies

**所屬 UseCase**：LoadRepliesUseCase

**HTTP Method**：GET

**URL**：`/chat/match/comment/replies`

**Request**：
- Query Parameter：`commentId: String`
- Query Parameter：`cursor: String?`（可選，用於分頁）

**Response DTO**：同 `GET /chat/match/comment/popular`

**Mapping**：同 `GET /chat/match/comment/popular`

**呼叫鏈**：LoadRepliesUseCase → PrematchCommentRepository → PrematchCommentClient → PrematchCommentAPI

---

## ChatAPI

### GET /chat/match/{refId}

**所屬 UseCase**：JoinChatroomUseCase

**HTTP Method**：GET

**URL**：`/chat/match/{refId}`

**Request**：
- Path Parameter：`refId: String`

**Response DTO**：
```swift
struct ChatroomInfoResponseDTO: Codable {
    let chatroomId: String
    let lastMessageNo: Int
}
```

**Mapping**：
```swift
extension ChatroomInfo {
    init(from dto: ChatroomInfoResponseDTO) {
        self.chatroomId = dto.chatroomId
        self.lastMessageNo = dto.lastMessageNo
    }
}
```

**呼叫鏈**：JoinChatroomUseCase → LiveChatRepository → LiveChatClient → ChatAPI

---

### POST /chat/match/join

**所屬 UseCase**：JoinChatroomUseCase

**HTTP Method**：POST

**URL**：`/chat/match/join`

**Request DTO**：
```swift
struct JoinChatroomRequestDTO: Codable {
    let chatroomId: String
}
```

**Response**：200 OK（無 body）

**呼叫鏈**：JoinChatroomUseCase → LiveChatRepository → LiveChatClient → ChatAPI

---

### POST /chat/match/backward

**所屬 UseCase**：JoinChatroomUseCase

**HTTP Method**：POST

**URL**：`/chat/match/backward`

**Request DTO**：
```swift
struct LoadHistoricalMessagesRequestDTO: Codable {
    let chatroomId: String
    let lastMessageNo: Int
}
```

**Response DTO**：
```swift
struct MessageListResponseDTO: Codable {
    let messages: [MessageDTO]
}

struct MessageDTO: Codable {
    let id: String
    let content: String
    let messageNo: Int
    let authorId: String
    let authorNickname: String?
    let messageType: String
    let createdAt: String
    let chatroomId: String
}
```

**Mapping**：
```swift
extension Message {
    init(from dto: MessageDTO) {
        self.id = dto.id
        self.content = dto.content
        self.messageNo = dto.messageNo
        self.authorId = dto.authorId
        self.authorNickname = dto.authorNickname
        self.messageType = MessageType(rawValue: dto.messageType) ?? .text
        self.createdAt = ISO8601DateFormatter().date(from: dto.createdAt) ?? Date()
        self.chatroomId = dto.chatroomId
    }
}
```

**呼叫鏈**：JoinChatroomUseCase → LiveChatRepository → LiveChatClient → ChatAPI

---

### POST /chat/match/leave

**所屬 UseCase**：LeaveChatroomUseCase

**HTTP Method**：POST

**URL**：`/chat/match/leave`

**Request DTO**：
```swift
struct LeaveChatroomRequestDTO: Codable {
    let chatroomId: String
}
```

**Response**：200 OK（無 body）

**呼叫鏈**：LeaveChatroomUseCase → LiveChatRepository → LiveChatClient → ChatAPI

---

### POST /chat/{matchId}/message

**所屬 UseCase**：SendChatMessageUseCase

**HTTP Method**：POST

**URL**：`/chat/{matchId}/message`

**Request DTO**：
```swift
struct SendMessageRequestDTO: Codable {
    let content: String
    let messageType: String  // "TEXT"
}
```

**Response DTO**：同 `POST /chat/match/backward` 的 MessageDTO

**Mapping**：同 `POST /chat/match/backward`

**呼叫鏈**：SendChatMessageUseCase → LiveChatRepository → LiveChatClient → ChatAPI

---

### WebSocket: wss://www.encorebet.net/chat/websocket/web-chat

**所屬 UseCase**：JoinChatroomUseCase（訂閱）、LeaveChatroomUseCase（取消訂閱）

**Protocol**：WebSocket

**URL**：`wss://www.encorebet.net/chat/websocket/web-chat`

**Subscribe**：
- 連線後自動訂閱
- 無需額外訂閱訊息

**Message Type**：
```swift
struct WebSocketMessageDTO: Codable {
    let type: String  // "message"
    let payload: MessageDTO
}
```

**Unsubscribe**：
- 關閉 WebSocket 連線即取消訂閱

**Mapping**：
```swift
extension Message {
    init(from dto: MessageDTO) {
        // 同 HTTP API 的 Mapping
    }
}
```

**呼叫鏈**：JoinChatroomUseCase → LiveChatRepository → ChatWebSocketClient → ChatAPI

---

## Mapping 原則

### 1. Repository 負責所有 Mapping

- 所有 DTO → Domain Model 的轉換都在 Repository 層完成
- UseCase 和 Feature 永遠不接觸 DTO

### 2. 使用 Extension 實作 Mapping

- 為每個 Domain Model 建立 `init(from dto: DTO)` extension
- 保持 Domain Model 的純淨性

### 3. 處理可選值和預設值

- 日期字串轉換失敗時使用當前時間
- Optional 欄位正確處理 nil 情況

### 4. 保持 Mapping 邏輯簡單

- 避免複雜的業務邏輯
- 複雜轉換應在 UseCase 層處理

