# Snapshot Plugin

為 Claude Code 提供專案快照功能，讓 AI 快速了解專案結構。

## 指令

### `/snapshot` - 產生專案快照

掃描專案並產生專案快照。根據專案規模和架構，可能產生單一檔案或分檔輸出。

**流程：**
1. 自動建立 `.snapshot/` 目錄（如不存在）
2. 偵測專案架構（多專案/大型專案/一般專案）
3. 掃描專案結構
4. 偵測語言、框架、建置工具
5. 識別核心模組和設定檔
6. 根據架構產生對應的 snapshot 檔案
7. 顯示產生摘要

**使用時機：**
- 專案尚未有 snapshot
- 專案有新增或移除檔案
- 想要更新 snapshot 內容
- 新對話開始前，讓 AI 了解專案

## 自動分檔功能

Snapshot 會根據專案規模和架構自動決定輸出模式：

### 觸發條件

| 條件 | 輸出模式 | 說明 |
|------|---------|------|
| 多專案架構 | `projects` | Java multi-module、.NET multi-project、JS/TS monorepo |
| 大型單一專案 | `modules` | 模組數量超過 50 個 |
| 一般專案 | `none` | 單一檔案輸出 |

### 多專案架構偵測

- **Java/Kotlin**: `settings.gradle` 或 `settings.gradle.kts` 中有 `include` 語句
- **.NET**: `*.sln` 檔案中包含多個 `*.csproj`
- **JS/TS**: `pnpm-workspace.yaml`、`lerna.json`、或 `package.json` 中的 `workspaces`

## 輸出位置

### 一般專案（split_mode = none）

```
.snapshot/
└── snapshot.json    # 專案快照
```

### 多專案架構（split_mode = projects）

```
.snapshot/
├── index.json           # 主索引
└── projects/
    ├── project-a.json   # 子專案 A
    ├── project-b.json   # 子專案 B
    └── ...
```

### 大型專案（split_mode = modules）

```
.snapshot/
├── index.json           # 主索引
└── modules/
    ├── controllers.json # 控制器模組
    ├── services.json    # 服務模組
    └── ...
```

## Snapshot 結構

### 單檔輸出（snapshot.json）

```json
{
  "project": "專案名稱",
  "description": "專案描述",
  "version": "1.0",
  "generated_at": "2025-01-01T00:00:00Z",
  "split_mode": "none",
  "tech_stack": {
    "language": "主要語言",
    "framework": "框架",
    "build_tool": "建置工具",
    "package_manager": "套件管理器",
    "dependencies": ["主要依賴"]
  },
  "structure": {
    "entry_point": { "path": "...", "purpose": "..." },
    "directories": { "src/": "原始碼" }
  },
  "modules": {
    "core": { "ModuleName": { "path": "...", "purpose": "..." } }
  },
  "config_files": {},
  "scripts": {},
  "patterns": [],
  "notes": ""
}
```

### 分檔輸出（index.json）

```json
{
  "project": "專案名稱",
  "description": "專案描述",
  "version": "1.0",
  "generated_at": "2025-01-01T00:00:00Z",
  "split_mode": "projects|modules",
  "tech_stack": { ... },
  "structure": { ... },
  "projects": { ... },        // split_mode = projects 時
  "modules_index": { ... },   // split_mode = modules 時
  "config_files": {},
  "scripts": {},
  "notes": "根據需要讀取對應的子檔案"
}
```

## 最佳實踐

1. **首次使用**：執行 `/snapshot` 產生專案快照
2. **定期更新**：專案有重大變更時重新執行 `/snapshot`
3. **新對話開始**：AI 會自動讀取 `.snapshot/index.json` 或 `.snapshot/snapshot.json` 了解專案
4. **分檔專案**：先讀取 `index.json`，再根據需要讀取對應的子檔案

## 問題回報

遇到問題請到 [GitHub Issues](https://github.com/cashwu/claude-code-snapshot/issues) 回報。
