# 初始化聊天室流程

## Flow 資訊

| 欄位 | 值 |
|------|-----|
| **feature** | LiveChat |
| **flow_id** | LC-FULL-001 |
| **flow_type** | Full |
| **flow_name** | 用戶進入與離開聊天室（含 WebSocket 相依） |
| **parent_flow_id** | null |
| **parent_flow_name** | null |
| **original_annotation** | @flow: Full |

## 模組說明

| 模組名稱 | 職責 |
|---------|------|
| **LiveDetailView** | 直播詳情頁面 |
| **LiveChatFeature** | TCA Reducer，管理聊天室相關的 State 和 Action |
| **JoinChatroomUseCase** | 加入聊天室並載入歷史訊息 |
| **LeaveChatroomUseCase** | 離開聊天室並清理快取 |
| **LiveChatRepository** | Domain 資料來源的抽象介面（聊天室相關） |
| **LiveChatClient** | HTTP 通訊（聊天室相關） |
| **ChatWebSocketClient** | WebSocket 通訊（即時訊息） |
| **ChatAPI** | 後端 endpoint 定義（聊天室相關） |

## 流程說明

| 流程步驟 | 說明 |
|---------|------|
| **1. 進入頁面與 WebSocket 訂閱** | 1. 用戶進入 Live Detail Page<br>2. 自動建立 WebSocket 連線 |
| **2. 取得聊天室資訊** | 1. 向伺服器請求聊天室 ID 和最後訊息編號<br>2. 成功則儲存資訊，失敗則隱藏 Live Chat Bar |
| **3. 加入聊天室** | 1. 用戶點擊 Join the chat 按鈕<br>2. 加入聊天室 |
| **4. 載入歷史訊息** | 1. 透過 backward API 載入舊訊息<br>2. 將歷史訊息與 WebSocket 訊息合併並去重 |
| **5. 離開聊天室** | 1. 用戶關閉聊天室<br>2. 離開聊天室並清除快取 |
| **6. 離開頁面** | 1. 用戶離開 Live Detail Page<br>2. 取消 WebSocket 訂閱 |

## 序列圖

```mermaid
sequenceDiagram
    autonumber
    actor User
    box rgb(207,232,255) UI Layer
        participant LiveDetailView
    end
    box rgb(255,250,205) Domain Layer
        participant LiveChatFeature
        participant JoinChatroomUseCase
        participant LeaveChatroomUseCase
    end
    box rgb(240,240,240) Data & Infrastructure Layer
        participant LiveChatRepository
        participant LiveChatClient
        participant ChatWebSocketClient
        participant ChatAPI
    end
    participant Server

    Note over User,LiveDetailView: 用戶進入 Live Detail Page
    User->>LiveDetailView: 進入 Live Detail Page
    LiveDetailView->>LiveChatFeature: onAppear
    LiveChatFeature->>LiveChatRepository: subscribeWebSocket()
    LiveChatRepository->>ChatWebSocketClient: subscribe()
    ChatWebSocketClient->>ChatAPI: WebSocket 訂閱
    ChatAPI->>Server: wss://www.encorebet.net/chat/websocket/web-chat
    LiveChatRepository->>JoinChatroomUseCase: execute(input: JoinChatroomInput)
    JoinChatroomUseCase->>LiveChatRepository: getChatroomInfo(refId: String)
    LiveChatRepository->>LiveChatClient: getChatroomInfo(refId: String)
    LiveChatClient->>ChatAPI: GET /chat/match/{refId}
    ChatAPI->>Server: GET /chat/match/{refId}
    Server-->>ChatAPI: { chatroomId, lastMessageNo }
    ChatAPI-->>LiveChatClient: ChatroomInfo DTO
    LiveChatClient-->>LiveChatRepository: ChatroomInfo DTO
    LiveChatRepository-->>JoinChatroomUseCase: ChatroomInfo Entity
    alt 成功取得 chatroomId
        JoinChatroomUseCase->>JoinChatroomUseCase: 儲存 chatroomId 與 lastMessageNo
        JoinChatroomUseCase-->>LiveChatFeature: Output(chatroomInfo: ChatroomInfo)
        LiveChatFeature-->>LiveDetailView: 更新 State
    else 無法取得 chatroomId
        JoinChatroomUseCase-->>LiveChatFeature: Output(error: Error)
        LiveChatFeature->>LiveChatFeature: 隱藏 Live Chat Bar
        LiveChatFeature-->>LiveDetailView: 更新 State
    end

    Note over User,LiveDetailView: 用戶開啟聊天視窗
    User->>LiveDetailView: 點擊 Join the chat
    LiveDetailView->>LiveChatFeature: joinChatroom()
    LiveChatFeature->>JoinChatroomUseCase: execute(input: JoinChatroomInput)
    JoinChatroomUseCase->>LiveChatRepository: joinChatroom(chatroomId: String)
    LiveChatRepository->>LiveChatClient: joinChatroom(chatroomId: String)
    LiveChatClient->>ChatAPI: POST /chat/match/join
    ChatAPI->>Server: POST /chat/match/join
    Server-->>ChatAPI: 200 OK
    ChatAPI-->>LiveChatClient: Success
    LiveChatClient-->>LiveChatRepository: Success
    LiveChatRepository-->>JoinChatroomUseCase: Success
    JoinChatroomUseCase->>LiveChatRepository: loadHistoricalMessages(chatroomId: String, lastMessageNo: Int)
    LiveChatRepository->>LiveChatClient: loadHistoricalMessages(chatroomId: String, lastMessageNo: Int)
    LiveChatClient->>ChatAPI: POST /chat/match/backward
    ChatAPI->>Server: POST /chat/match/backward { chatroomId, lastMessageNo }
    Server-->>ChatAPI: historicalMessages
    ChatAPI-->>LiveChatClient: [Message] DTO
    LiveChatClient-->>LiveChatRepository: [Message] DTO
    LiveChatRepository-->>JoinChatroomUseCase: [Message] Entity
    JoinChatroomUseCase->>JoinChatroomUseCase: 將 historicalMessages 與 websocketMessages 合併後依 messageNo 去重
    JoinChatroomUseCase-->>LiveChatFeature: Output(messages: [Message])
    LiveChatFeature-->>LiveDetailView: 更新 State

    Note over User,LiveDetailView: 用戶關閉聊天視窗
    User->>LiveDetailView: 點擊聊天室外部置灰區域或 bar
    LiveDetailView->>LiveChatFeature: leaveChatroom()
    LiveChatFeature->>LeaveChatroomUseCase: execute(input: LeaveChatroomInput)
    LeaveChatroomUseCase->>LiveChatRepository: leaveChatroom(chatroomId: String)
    LiveChatRepository->>LiveChatClient: leaveChatroom(chatroomId: String)
    LiveChatClient->>ChatAPI: POST /chat/match/leave
    ChatAPI->>Server: POST /chat/match/leave
    Server-->>ChatAPI: 200 OK
    ChatAPI-->>LiveChatClient: Success
    LiveChatClient-->>LiveChatRepository: Success
    LiveChatRepository-->>LeaveChatroomUseCase: Success
    LeaveChatroomUseCase->>LeaveChatroomUseCase: 清除 cache 的 messages
    LeaveChatroomUseCase->>LiveChatRepository: checkAndCleanBlacklist()
    LiveChatRepository->>LiveChatRepository: 比較黑名單紀錄的加入時間與當前時間
    LiveChatRepository->>LiveChatRepository: 若超過 4 小時則自動移除黑名單紀錄
    LiveChatRepository-->>LeaveChatroomUseCase: Success
    LeaveChatroomUseCase-->>LiveChatFeature: Output()
    LiveChatFeature-->>LiveDetailView: 更新 State

    Note over User,LiveDetailView: 用戶離開 Live Detail Page
    User->>LiveDetailView: 離開 Live Detail Page
    LiveDetailView->>LiveChatFeature: onDisappear
    LiveChatFeature->>LiveChatRepository: unsubscribeWebSocket()
    LiveChatRepository->>ChatWebSocketClient: unsubscribe()
    ChatWebSocketClient->>ChatAPI: WebSocket 取消訂閱
    ChatAPI->>Server: WebSocket 取消訂閱
```

