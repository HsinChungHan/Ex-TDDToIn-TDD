# LiveChat & PrematchComment TDD

本目錄包含 LiveChat 和 PrematchComment Feature 的完整 TDD（Technical Design Document）文件。

## 本地預覽

使用 MkDocs 本地預覽：

```bash
# 安裝依賴
pip install -r ../../requirements.txt

# 啟動本地伺服器
mkdocs serve
```

然後在瀏覽器中開啟 `http://127.0.0.1:8000` 即可預覽。

## 部署

本 TDD 文件使用 MkDocs 生成，並可部署到 GitHub Pages。

### 自動部署

當推送到 `main` 分支時，GitHub Actions 會自動部署到 GitHub Pages。

### 手動部署

```bash
# 建置網站
mkdocs build

# 部署到 GitHub Pages
mkdocs gh-deploy
```

## 文件結構

- `00_Overview/` - 整體概述
- `01_Integrated Service-Level Sequence Diagram/` - 整合服務層級序列圖
- `02_Architecture/` - 架構圖
- `03_Module Responsibility/` - 模組職責說明
- `04_Domain Model/` - Domain Model 定義
- `05. Module Sequence Diagram（模組序列圖）/` - 模組序列圖
- `06_Feature State & Action (TCA)/` - Feature State 和 Action 定義
- `07_UseCase Input & Output Model/` - UseCase Input/Output Model
- `08_API Spec & Mapping/` - API 規格和 Mapping
- `09_Error Handling/` - 錯誤處理策略
- `10_Test Scenarios/` - 測試場景
- `11_Risks & Questions/` - 風險和問題
- `12_Tickets/` - 開發 Tickets

