# Claude Code Snapshot

> 基於 [tzangms (海總理)](https://tzangms.com) 的 Vibe Coding 方法論

Snapshot Skill 讓 AI 可以快速產生和維護專案的 `snapshot.json`「專案地圖」，大幅減少 AI 理解專案結構的時間。

## 什麼是 Snapshot？

Snapshot 是一個給 AI 看的「專案地圖」檔案，讓 AI 能快速了解專案的結構和功能，而不需要逐一掃描所有檔案。

### 為什麼需要 Snapshot？

當專案變大後，與 AI 協作開發會遇到以下問題：

1. **搜尋變慢** - AI 需要不斷掃描檔案來理解專案
2. **重複開發** - AI 不知道既有功能，可能重新實作已存在的功能
3. **Context 佔用** - 大量檔案內容佔用寶貴的 context 空間

Snapshot 的解決方案：
- 提供精簡的專案概覽
- 讓 AI 知道「有什麼」、「在哪裡」、「做什麼用」
- 大幅減少搜尋和理解時間

## 安裝

```bash
# 新增 marketplace
/plugin marketplace add cashwu/claude-code-snapshot

# 安裝 plugin
/plugin install snapshot
```

## 指令

| 指令 | 說明 |
|------|------|
| `/ss-init` | 初始化 snapshot，偵測專案類型並產生初始 `snapshot.json` |
| `/ss-gen` | 重新掃描專案並更新 `snapshot.json` |
| `/ss-read` | 讀取並解析現有的 snapshot，顯示專案概覽 |
| `/ss-validate` | 驗證 snapshot 與實際專案結構是否一致 |
| `/ss-config` | 自訂 snapshot 產生設定 |

## 支援的專案類型

| 類型 | 偵測方式 | 主要分析項目 |
|------|----------|--------------|
| **Java (Spring)** | `build.gradle`, `pom.xml` | @Controller, @Service, @Repository, @Entity |
| **Kotlin (Android)** | `build.gradle.kts`, `AndroidManifest.xml` | Activity, Fragment, ViewModel, Repository |
| **Python** | `requirements.txt`, `pyproject.toml` | 模組、類別、函數 |
| **JavaScript/TypeScript (Next.js)** | `package.json`, `next.config.js` | pages, components, API routes, hooks |
| **C# (.NET Core)** | `*.csproj`, `*.sln` | Controllers, Services, Models, DbContext |
| **自訂** | 使用者定義 | 使用者定義的規則 |

## Snapshot 結構範例

```json
{
  "project": "my-app",
  "description": "專案描述",
  "version": "1.0",
  "tech_stack": {
    "language": "Java",
    "framework": "Spring Boot",
    "build_tool": "Gradle"
  },
  "entry_point": {
    "path": "src/main/java/com/example/Application.java",
    "purpose": "Spring Boot 應用程式入口"
  },
  "modules": {
    "controllers": {
      "UserController": {
        "path": "src/main/java/com/example/controller/UserController.java",
        "type": "controller",
        "purpose": "處理使用者相關 API"
      }
    },
    "services": {
      "UserService": {
        "path": "src/main/java/com/example/service/UserService.java",
        "type": "service",
        "purpose": "使用者業務邏輯"
      }
    }
  },
  "config_files": {
    "application.properties": {
      "path": "src/main/resources/application.properties",
      "purpose": "應用程式設定"
    }
  },
  "commands": {
    "build": "./gradlew build",
    "run": "./gradlew bootRun",
    "test": "./gradlew test"
  }
}
```

## 快速開始

### 1. 初始化 Snapshot

```
/ss-init
```

AI 會自動偵測專案類型，掃描專案結構，並產生 `snapshot.json`。

### 2. 讀取 Snapshot

```
/ss-read
```

顯示專案概覽，包括技術棧、模組結構、設定檔等。

### 3. 更新 Snapshot

當專案有變更時：

```
/ss-gen
```

重新掃描並更新 `snapshot.json`。

### 4. 驗證 Snapshot

確認 snapshot 是否與實際專案一致：

```
/ss-validate
```

### 5. 自訂設定

如果需要調整產生規則：

```
/ss-config
```

## 進階功能

### Snapshot 2.0 多檔案架構

當專案很大時，可以將單一 snapshot 拆分為多個檔案：

```
snapshots/
├── snapshot_index.json   # 主索引
├── core.json             # 核心模組
├── services.json         # 服務模組
└── utils.json            # 工具類
```

在 `/ss-config` 中設定 `outputFormat: "multi"` 即可啟用。

### 自訂範本

可以建立自訂範本來支援其他框架或語言：

1. 複製 `templates/custom.json`
2. 修改偵測規則和模組定義
3. 在 `/ss-config` 中指定使用自訂範本

## 參考資料

- [Vibe Coding: Snapshot 2.0](https://tzangms.com/vibe-coding-snapshot-2-0/)
- [Vibe Coding snapshot - 小海報 61](https://tzangms.substack.com/p/vibe-coding-snapshot-61)

## 授權

MIT License

## 作者

- [Cash Wu](https://github.com/cashwu)
