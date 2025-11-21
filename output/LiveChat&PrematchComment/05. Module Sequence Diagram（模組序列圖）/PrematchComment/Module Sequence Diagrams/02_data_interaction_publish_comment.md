# 發送留言流程

## Flow 資訊

| 欄位 | 值 |
|------|-----|
| **feature** | PrematchComment |
| **flow_id** | PC-SUB-002 |
| **flow_type** | Sub |
| **flow_name** | 用戶發 Comment - Reply（含登入與 nickname 檢查） |
| **parent_flow_id** | PC-FULL-001 |
| **parent_flow_name** | 用戶進入 Upcoming Race Page, Prematch Comment Page 與 Top |
| **original_annotation** | @flow: Sub |

## 模組說明

| 模組名稱 | 職責 |
|---------|------|
| **PrematchCommentView** | 賽前留言頁面 |
| **PrematchCommentFeature** | TCA Reducer，管理評論相關的 State 和 Action |
| **PublishCommentUseCase** | 發送留言或回覆 |
| **PrematchCommentRepository** | Domain 資料來源的抽象介面（評論相關） |
| **PrematchCommentClient** | HTTP 通訊（評論相關） |
| **PrematchCommentAPI** | 後端 endpoint 定義（評論相關） |
| **PersonalPage Package (External)** | 登入流程（外部 Package） |
| **FComSharedFlow Package (External)** | Nickname 建立流程（外部 Package） |

## 流程說明

| 流程步驟 | 說明 |
|---------|------|
| **1. 點擊輸入框與登入檢查** | 1. 用戶點擊輸入框準備輸入留言或回覆<br>2. 檢查登入狀態<br>3. 未登入則跳轉到 PersonalPage 完成登入 |
| **2. 輸入並送出留言** | 1. 用戶輸入並送出 Comment/Reply<br>2. 檢查 nickname 是否存在 |
| **3. 有 nickname 時發送** | 1. 已有 nickname，直接送出留言<br>2. 顯示新留言 |
| **4. 無 nickname 時發送** | 1. 調用 FComSharedFlow Package 建立 nickname<br>2. 建立成功後送出留言<br>3. 顯示新留言 |

## 序列圖

```mermaid
sequenceDiagram
    autonumber
    actor User
    box rgb(207,232,255) UI Layer
        participant PrematchCommentView
    end
    box rgb(255,250,205) Domain Layer
        participant PrematchCommentFeature
        participant PublishCommentUseCase
    end
    box rgb(240,240,240) Data & Infrastructure Layer
        participant PrematchCommentRepository
        participant PrematchCommentClient
        participant PrematchCommentAPI
    end
    participant PersonalPagePackage as PersonalPage Package (External)
    participant FComSharedFlowPackage as FComSharedFlow Package (External)
    participant Server

    Note over User,PrematchCommentView: 用戶點擊輸入框
    User->>PrematchCommentView: 點擊輸入框
    PrematchCommentView->>PrematchCommentFeature: tapInputField
    PrematchCommentFeature->>PrematchCommentFeature: 檢查登入狀態（透過 Main App）
    alt 使用者已登入
        PrematchCommentFeature->>PrematchCommentFeature: 已登入，允許輸入留言
    else 使用者未登入
        PrematchCommentFeature->>PersonalPagePackage: route(to: .personalPage)
        PersonalPagePackage->>PersonalPagePackage: 完成 user 登入流程
        PersonalPagePackage-->>PrematchCommentFeature: 登入成功（回跳至原頁面）
    end

    Note over User,PrematchCommentView: 用戶輸入並送出 Comment/Reply
    User->>PrematchCommentView: 輸入並送出 Comment/Reply
    PrematchCommentView->>PrematchCommentFeature: publishComment(content: String, parentId: String?)
    PrematchCommentFeature->>PublishCommentUseCase: execute(input: PublishCommentInput)
    PublishCommentUseCase->>PublishCommentUseCase: 檢查 nickname（透過 Main App）
    alt nickname 存在
        PublishCommentUseCase->>PrematchCommentRepository: publishComment(refId: String, content: String, parentId: String?)
        PrematchCommentRepository->>PrematchCommentClient: publishComment(refId: String, content: String, parentId: String?)
        PrematchCommentClient->>PrematchCommentAPI: POST /chat/match/comment
        PrematchCommentAPI->>Server: POST /chat/match/comment { refId, content, parentId? }
        alt 留言成功
            Server-->>PrematchCommentAPI: 201 Created（回傳 comment 資料）
            PrematchCommentAPI-->>PrematchCommentClient: Comment DTO
            PrematchCommentClient-->>PrematchCommentRepository: Comment DTO
            PrematchCommentRepository-->>PublishCommentUseCase: Comment Entity
            PublishCommentUseCase-->>PrematchCommentFeature: Output(comment: Comment)
            PrematchCommentFeature-->>PrematchCommentView: 更新 State
            PrematchCommentView-->>User: 顯示新留言
        else 留言失敗
            Server-->>PrematchCommentAPI: 4xx/5xx Error
            PrematchCommentAPI-->>PrematchCommentClient: Error
            PrematchCommentClient-->>PrematchCommentRepository: Error
            PrematchCommentRepository-->>PublishCommentUseCase: Error
            PublishCommentUseCase-->>PrematchCommentFeature: Output(error: Error)
            PrematchCommentFeature-->>PrematchCommentView: 更新 State
            PrematchCommentView-->>User: 顯示錯誤 Toast
        end
    else nickname 不存在
        PublishCommentUseCase->>FComSharedFlowPackage: CreatNickName API
        FComSharedFlowPackage-->>User: 顯示 create nickName pop-up
        User->>FComSharedFlowPackage: 使用者輸入 nickname
        FComSharedFlowPackage->>FComSharedFlowPackage: FComSharedFlow Package 建立 nickname
        FComSharedFlowPackage-->>PublishCommentUseCase: nickname 建立成功
        PublishCommentUseCase->>PrematchCommentRepository: publishComment(refId: String, content: String, parentId: String?)
        PrematchCommentRepository->>PrematchCommentClient: publishComment(refId: String, content: String, parentId: String?)
        PrematchCommentClient->>PrematchCommentAPI: POST /chat/match/comment
        PrematchCommentAPI->>Server: POST /chat/match/comment { refId, content, parentId? }
        alt 留言成功
            Server-->>PrematchCommentAPI: 201 Created（回傳 comment 資料）
            PrematchCommentAPI-->>PrematchCommentClient: Comment DTO
            PrematchCommentClient-->>PrematchCommentRepository: Comment DTO
            PrematchCommentRepository-->>PublishCommentUseCase: Comment Entity
            PublishCommentUseCase-->>PrematchCommentFeature: Output(comment: Comment)
            PrematchCommentFeature-->>PrematchCommentView: 更新 State
            PrematchCommentView-->>User: 顯示新留言
        else 留言失敗
            Server-->>PrematchCommentAPI: 4xx/5xx Error
            PrematchCommentAPI-->>PrematchCommentClient: Error
            PrematchCommentClient-->>PrematchCommentRepository: Error
            PrematchCommentRepository-->>PublishCommentUseCase: Error
            PublishCommentUseCase-->>PrematchCommentFeature: Output(error: Error)
            PrematchCommentFeature-->>PrematchCommentView: 更新 State
            PrematchCommentView-->>User: 顯示錯誤 Toast
        end
    end
```

