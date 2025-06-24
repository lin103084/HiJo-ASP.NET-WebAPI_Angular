[HiJo專題報告簡報](https://www.canva.com/design/DAGrR3uOTvU/tiLcPJzIJmQj9jCLxMbmXg/edit?utm_content=DAGrR3uOTvU&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)
# HiJo 專案整體架構與部署介紹

**HiJo** 是一個現代化的交友活動平台，採用 **前後端分離架構** 開發，  
搭配 **Docker 容器化部署** 提供穩定、可擴充、可監控的營運環境。  
系統設計聚焦在高彈性、模組化與維運效率，支援會員配對、活動報名、VIP 方案、聊天室與金流整合等功能。

---

## 🧱 架構總覽

### 🔹 前端 Frontend
- **技術：** Angular 16（模組化架構）
- **UI 套件：** Bootstrap
- **路由規劃：**
  - `/auth`: 登入／註冊／忘記密碼
  - `/members`: 個人資料、配對、聊天室
  - `/events`: 活動列表、詳細、報名流程
  - `/vip`: VIP 方案購買與狀態查詢

### 🔹 後端 Backend
- **技術：** ASP.NET Core 8 Web API
- **分層架構：**
  - Controllers：接收請求
  - Services：商業邏輯
  - DTOs / ViewModels / Wraps：資料傳遞模型
- **功能模組：**
  - JWT + RefreshToken 驗證機制
  - 寄送 Email 驗證與報名成功通知（含 QRCode）
  - 活動報名與金流整合（綠界金流）
  - 聊天室功能（SignalR）
  - VIP 升級與訂單管理
  - Redis：統計線上人數
  - Prometheus + Grafana：系統監控
  - Serilog + ELK：Log 分析追蹤

---

## 📦 容器化部署（Docker）

### 📁 資料夾規劃
- `/home/tommy/hijo-api/`：後端 ASP.NET Core 專案（Dockerfile 已建置）
- `/home/tommy/hijo-web/`：前端 Angular 打包後 dist
- `/home/tommy/logs/`：儲存 Serilog 輸出 JSON log
- `/home/tommy/metrics/`：Prometheus 指標輸出（metrics.txt）
- `/home/tommy/certs/`：憑證（server.pfx、server.crt、server.key）
- `/home/tommy/secrets/`：JWT 金鑰與 DB 連線字串（user-secrets）
- `/home/tommy/nginx/`：前端 Nginx 設定檔
- `/home/tommy/prometheus/`：Prometheus 設定檔
- `/home/tommy/elk/`：Logstash + Elasticsearch + Kibana 設定

### 🐳 docker-compose.yml 範圍
整合部署以下服務：
- ASP.NET Core Web API
- Angular + Nginx 靜態頁面
- Redis
- Prometheus + Grafana
- ELK Stack（Elasticsearch, Logstash, Kibana）

---

## 🛡️ 安全與擴充
- **JWT + RefreshToken：** 安全身份驗證與續期機制
- **HttpOnly Cookie 傳遞 RefreshToken**，防止 XSS 竊取
- **Https 憑證部署前後端共用憑證**，提升通訊加密安全性
- **Log 分析／線上人數統計／健康監控** 可快速擴充新模組並進行 DevOps 整合

---

## 📈 成果與價值

- 完整支援會員、配對、聊天室、活動報名與金流流程
- 支援自動寄信、QRCode 驗證、VIP 角色切換與訂單流程
- 容器化部署可快速還原環境與版本，適合實際上線或雲端平台部署

---

