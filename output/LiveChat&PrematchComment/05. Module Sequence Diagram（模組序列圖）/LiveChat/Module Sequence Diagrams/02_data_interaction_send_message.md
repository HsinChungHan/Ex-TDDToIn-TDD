# 發送聊天訊息流程

## Flow 資訊

| 欄位 | 值 |
|------|-----|
| **feature** | LiveChat |
| **flow_id** | LC-SUB-001 |
| **flow_type** | Sub |
| **flow_name** | 用戶留 comment 時登入與 nickname 檢查 |
| **parent_flow_id** | LC-FULL-001 |
| **parent_flow_name** | 用戶進入與離開聊天室（含 WebSocket 相依） |
| **original_annotation** | @flow: Sub |

## 模組說明

| 模組名稱 | 職責 |
|---------|------|
| **LiveDetailView** | 直播詳情頁面 |
| **LiveChatFeature** | TCA Reducer，管理聊天室相關的 State 和 Action |
| **SendChatMessageUseCase** | 發送聊天訊息 |
| **LiveChatRepository** | Domain 資料來源的抽象介面（聊天室相關） |
| **LiveChatClient** | HTTP 通訊（聊天室相關） |
| **ChatAPI** | 後端 endpoint 定義（聊天室相關） |
| **PersonalPage Package (External)** | 登入流程（外部 Package） |
| **FComSharedFlow Package (External)** | Nickname 建立流程（外部 Package） |

## 流程說明

| 流程步驟 | 說明 |
|---------|------|
| **1. 點擊輸入框與登入檢查** | 1. 用戶點擊輸入框準備輸入留言<br>2. 檢查登入狀態<br>3. 未登入則跳轉到 PersonalPage 完成登入 |
| **2. 輸入並送出留言** | 1. 用戶輸入並送出 Chat<br>2. 檢查 nickname 是否存在 |
| **3. 有 nickname 時發送** | 1. 已有 nickname，直接送出留言<br>2. 顯示新留言 |
| **4. 無 nickname 時發送** | 1. 調用 FComSharedFlow Package 建立 nickname<br>2. 建立成功後送出留言<br>3. 顯示 Chat 在最新的位置並 highlight 3 秒 |

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
        participant SendChatMessageUseCase
    end
    box rgb(240,240,240) Data & Infrastructure Layer
        participant LiveChatRepository
        participant LiveChatClient
        participant ChatAPI
    end
    participant PersonalPagePackage as PersonalPage Package (External)
    participant FComSharedFlowPackage as FComSharedFlow Package (External)
    participant Server

    Note over User,LiveDetailView: 用戶點擊輸入框
    User->>LiveDetailView: 點擊輸入框
    LiveDetailView->>LiveChatFeature: tapInputField
    LiveChatFeature->>LiveChatFeature: 檢查登入狀態（透過 Main App）
    alt 使用者已登入
        LiveChatFeature->>LiveChatFeature: 已登入，允許輸入留言
    else 使用者未登入
        LiveChatFeature->>PersonalPagePackage: route(to: .personalPage)
        PersonalPagePackage->>PersonalPagePackage: 完成 user 登入流程
        PersonalPagePackage-->>LiveChatFeature: 登入成功（回跳至原頁面）
    end

    Note over User,LiveDetailView: 用戶輸入並送出 Chat
    User->>LiveDetailView: 輸入並送出 Chat
    LiveDetailView->>LiveChatFeature: sendMessage(content: String)
    LiveChatFeature->>SendChatMessageUseCase: execute(input: SendChatMessageInput)
    SendChatMessageUseCase->>SendChatMessageUseCase: 檢查 nickname（透過 Main App）
    alt nickname 存在
        SendChatMessageUseCase->>LiveChatRepository: sendMessage(chatroomId: String, content: String, messageType: MessageType)
        LiveChatRepository->>LiveChatClient: sendMessage(chatroomId: String, content: String, messageType: MessageType)
        LiveChatClient->>ChatAPI: POST /chat/match/comment
        ChatAPI->>Server: POST /chat/match/comment { refId, content, parentId? }
        alt 留言成功
            Server-->>ChatAPI: 201 Created（回傳 comment 資料）
            ChatAPI-->>LiveChatClient: Message DTO
            LiveChatClient-->>LiveChatRepository: Message DTO
            LiveChatRepository-->>SendChatMessageUseCase: Message Entity
            SendChatMessageUseCase-->>LiveChatFeature: Output(message: Message)
            LiveChatFeature-->>LiveDetailView: 更新 State
            LiveDetailView-->>User: 顯示新留言
        else 留言失敗
            Server-->>ChatAPI: 4xx/5xx Error
            ChatAPI-->>LiveChatClient: Error
            LiveChatClient-->>LiveChatRepository: Error
            LiveChatRepository-->>SendChatMessageUseCase: Error
            SendChatMessageUseCase-->>LiveChatFeature: Output(error: Error)
            LiveChatFeature-->>LiveDetailView: 更新 State
            LiveDetailView-->>User: 顯示錯誤 Toast
        end
    else nickname 不存在
        SendChatMessageUseCase->>FComSharedFlowPackage: CreatNickName API
        FComSharedFlowPackage-->>User: 顯示 create nickName pop-up
        User->>FComSharedFlowPackage: 使用者輸入 nickname
        FComSharedFlowPackage->>FComSharedFlowPackage: FComSharedFlow Package 建立 nickname
        FComSharedFlowPackage-->>SendChatMessageUseCase: nickname 建立成功
        SendChatMessageUseCase->>LiveChatRepository: sendMessage(chatroomId: String, content: String, messageType: MessageType)
        LiveChatRepository->>LiveChatClient: sendMessage(chatroomId: String, content: String, messageType: MessageType)
        LiveChatClient->>ChatAPI: POST /chat/{matchId}/message
        ChatAPI->>Server: POST /chat/{matchId}/message { content, messageType: TEXT }
        alt 留言成功
            Server-->>ChatAPI: 201 Created
            ChatAPI-->>LiveChatClient: Message DTO
            LiveChatClient-->>LiveChatRepository: Message DTO
            LiveChatRepository-->>SendChatMessageUseCase: Message Entity
            SendChatMessageUseCase-->>LiveChatFeature: Output(message: Message)
            LiveChatFeature-->>LiveDetailView: 更新 State
            LiveDetailView-->>User: 顯示 Chat 在最新的位置，並 highlight 3 秒
        else 留言失敗
            Server-->>ChatAPI: 4xx/5xx Error
            ChatAPI-->>LiveChatClient: Error
            LiveChatClient-->>LiveChatRepository: Error
            LiveChatRepository-->>SendChatMessageUseCase: Error
            SendChatMessageUseCase-->>LiveChatFeature: Output(error: Error)
            LiveChatFeature-->>LiveDetailView: 更新 State
            LiveDetailView-->>User: 顯示錯誤 Toast
        end
    end
```

