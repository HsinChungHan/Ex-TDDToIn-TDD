# Event Status 訂閱與通知流程

## Flow 資訊

| 欄位 | 值 |
|------|-----|
| **feature** | PrematchComment |
| **flow_id** | PC-SUB-001 |
| **flow_type** | Sub |
| **flow_name** | Event Status 訂閱與通知流程 |
| **parent_flow_id** | PC-FULL-001 |
| **parent_flow_name** | 用戶進入 Upcoming Race Page, Prematch Comment Page 與 Top |
| **original_annotation** | @flow: Sub |

## 模組說明

| 模組名稱 | 職責 |
|---------|------|
| **PrematchCommentView** | 賽前留言頁面 |
| **PrematchCommentFeature** | TCA Reducer，管理評論相關的 State 和 Action |
| **FactsCenter Package (External)** | Event Status 訂閱與通知（外部 Package） |

## 流程說明

| 流程步驟 | 說明 |
|---------|------|
| **1. Event Status 訂閱** | 1. 用戶進入 Race Detail Page<br>2. FactsCenter Package（外部）自動透過 WebSocket 訂閱 Event Status |
| **2. Event Status 變更通知** | 1. Prematch Comment Page 處於開啟狀態<br>2. Server 推送 EventStatusChanged 事件<br>3. FactsCenter Package 透過 interface 通知 PrematchCommentFeature |
| **3. 處理通知** | 1. 若 event status 有改變，且為 match_started：自動關閉 Prematch Comment Page，導回 Race Detail Page<br>2. 若 event status 沒有改變：APP 維持原畫面 |

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
    end
    participant FactsCenterPackage as FactsCenter Package (External)
    participant Server

    Note over User,PrematchCommentView: 用戶進入 Race Detail Page
    User->>PrematchCommentView: 進入 Race Detail Page
    FactsCenterPackage->>Server: WebSocket 訂閱 Event Status
    Note over FactsCenterPackage: 外部 Package，內部實作不在此 TDD 範圍內

    Note over User,PrematchCommentView: Prematch Comment Page 處於開啟狀態
    Note over User,PrematchCommentView: Event Status 變更通知
    Server-->>FactsCenterPackage: EventStatusChanged
    FactsCenterPackage->>PrematchCommentFeature: eventStatus(didChange status: Int)
    Note over FactsCenterPackage,PrematchCommentFeature: FactsCenter Package 透過 interface 通知 PrematchCommentFeature
    alt 若 event status 有改變，且為 match_started
        PrematchCommentFeature->>PrematchCommentView: 關閉 Prematch Comment Page
        PrematchCommentView-->>User: 導回 Race Detail Page
        Note over PrematchCommentFeature: 下次再打開 Chat Room 時，會是 match ongoing 的 chat room
    else 若 event status 沒有改變
        PrematchCommentFeature->>PrematchCommentFeature: APP 維持原畫面
    end
```

