# Claude Meta

`claude-*` 系列專案給 AI 使用的跨 repo 工作入口：用來整理多個獨立 Claude Code 擴充 repo 的共同脈絡。

　

## 為什麼需要這一層

- **保留獨立 repo 的協調方式**：
  - 這些內容 repo 常常要一起改，但又必須能各自獨立連結、發佈與維護
  - monorepo 會在架構上把它們綁死；meta 層則是在保留獨立性的前提下協調它們
- **讓 AI 拿到完整的全貌**：
  - 在這裡工作的 AI 可以一次看見所有 repo，並按照共用規範做判斷
  - 如果跨 repo 的修改只從單一 repo 內部發動，AI 容易漏掉其他 repo 需要同步調整的地方
  - 這裡主要靠脈絡與規範防漂移，而不是靠檢查腳本
- **可複用的架構**：
  - 「一個 meta 層協調 N 個獨立 repo」這種做法，之後也可以用在 Claude 工具以外的地方（例如：前端、後端、App 等專案叢集）

　

## 專案組成

| repo | 擴充類型 | 路徑 |
| --- | --- | --- |
| `claude-skills` | LLM 觸發的 skill | `../claude-skills` |
| `claude-hooks` | 事件觸發的 hook | `../claude-hooks` |
| `claude-agents` | subagent 定義 | `../claude-agents` |
| `claude-mcps` | MCP server | `../claude-mcps` |
| `claude-meta` | 協調層（本 repo） | `.` |

　

## 運作方式

- **用指向保留實際檔案位置**：
  - claude-meta 不「裝著」其他 repo，而是「指向」它們；實際檔案留在各自的 `~/Developer/claude-*`，這一層只負責把它們放進同一個視野
  - 其他 repo 仍位在相對路徑 `../claude-*`；搜尋範圍若只放在 `.`，`Grep`／`Glob` 就只會看到本 repo
- **在同一個地方看多個 repo**：
  - VS Code 端用一份 multi-root workspace 並列五個 repo，讓它們各自保有獨立的版控視圖
- **共用規範各有來源**：
  - 命名、來源與授權、git 等規範，都定義在對應的 skill 或 README 裡
- **改一個 repo 時順手確認相關內容**：
  - 改其中一個內容 repo，常會連帶需要調整 `claude-skills` 裡對應的 `*-author` skill 文件與範例

　

## 判斷準則

- **先貼合自己的工作流**：
  - 這些 repo 同時服務四個目標：個人工具、可分享的工具箱、教學示範、能力探索
  - 但當「貼合維護者自己的工作流」與「外人也好用」衝突時，前者優先
  - 寫死的個人偏好與中文 README 都是刻意的；保留分享的可能，但不把個人工作流改成通用設計
- **先做出可用版本**：
  - 這些專案通常先把眼前需要的版本做出來，再保留後續調整空間
  - 平台支援（目前 GitHub Pages）、專案邊界（目前四種擴充類型）、eval 機制（客觀輸出型）都照這個方式處理
