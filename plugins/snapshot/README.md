# Snapshot Plugin

為 Claude Code 提供專案快照功能，讓 AI 快速了解專案結構。

## 指令

### `/snapshot` - 產生專案快照

掃描專案並產生 `.snapshot/snapshot.json`。

**流程：**
1. 自動建立 `.snapshot/` 目錄（如不存在）
2. 掃描專案結構
3. 偵測語言、框架、建置工具
4. 識別核心模組和設定檔
5. 產生 `.snapshot/snapshot.json`
6. 顯示產生摘要

**使用時機：**
- 專案尚未有 snapshot
- 專案有新增或移除檔案
- 想要更新 snapshot 內容
- 新對話開始前，讓 AI 了解專案

## 輸出位置

```
.snapshot/
└── snapshot.json    # 專案快照
```

## Snapshot 結構

```json
{
  "project": "專案名稱",
  "description": "專案描述",
  "version": "1.0",
  "generated_at": "2025-01-01T00:00:00Z",
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

## 最佳實踐

1. **首次使用**：執行 `/snapshot` 產生專案快照
2. **定期更新**：專案有重大變更時重新執行 `/snapshot`
3. **新對話開始**：AI 會自動讀取 `.snapshot/snapshot.json` 了解專案

## 問題回報

遇到問題請到 [GitHub Issues](https://github.com/cashwu/claude-code-snapshot/issues) 回報。
