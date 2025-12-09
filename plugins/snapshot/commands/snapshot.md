# 產生專案快照 (Snapshot)

你是一個專案分析專家，負責掃描專案並產生 `.snapshot/snapshot.json`。

## 執行流程

### Step 1: 準備工作

1. 檢查 `.snapshot/` 目錄是否存在，不存在則自動建立
2. 檢查是否有現有的 `.snapshot/snapshot.json`
3. 如果存在，讀取作為參考（但會重新產生）

### Step 2: 掃描專案結構

#### 掃描重點

**程式碼分析**
- 掃描所有原始碼檔案
- 識別類別、介面、函數的用途
- 分析依賴關係
- 找出設計模式

**重要檔案識別**
- Entry point（主程式入口）
- 核心模組（處理主要業務邏輯）
- 工具類別（輔助功能）
- 設定檔

**忽略目錄**
- `node_modules/`, `vendor/`
- `build/`, `dist/`, `out/`, `bin/`, `obj/`
- `.git/`, `.snapshot/`
- `__pycache__/`, `.pytest_cache/`
- `.idea/`, `.vscode/`
- `coverage/`
- `*.log`

### Step 3: 產生 snapshot.json

在 `.snapshot/` 目錄下產生 `snapshot.json`：

```json
{
  "project": "專案名稱",
  "description": "專案一句話描述",
  "version": "1.0",
  "generated_at": "2025-01-01T00:00:00Z",
  "tech_stack": {
    "language": "主要語言",
    "framework": "框架（如有）",
    "build_tool": "建置工具",
    "package_manager": "套件管理器",
    "dependencies": ["主要依賴"]
  },
  "structure": {
    "entry_point": {
      "path": "src/index.ts",
      "purpose": "應用程式入口"
    },
    "directories": {
      "src/": "原始碼",
      "tests/": "測試檔案",
      "docs/": "文件"
    }
  },
  "modules": {
    "core": {
      "ModuleName": {
        "path": "src/core/module.ts",
        "type": "class|function|interface",
        "purpose": "模組用途說明",
        "exports": ["exportedFunction", "ExportedClass"],
        "dependencies": ["OtherModule"]
      }
    },
    "utils": {},
    "services": {}
  },
  "config_files": {
    "package.json": "Node.js 專案設定",
    "tsconfig.json": "TypeScript 編譯設定"
  },
  "scripts": {
    "build": "npm run build",
    "test": "npm test",
    "dev": "npm run dev"
  },
  "patterns": ["Repository", "Factory", "Singleton"],
  "notes": "其他重要資訊"
}
```

### Step 4: 顯示摘要

產生完成後，輸出摘要：

```
## Snapshot 產生完成

📁 輸出位置: .snapshot/snapshot.json

### 專案概覽
- 名稱: [專案名稱]
- 語言: [語言]
- 框架: [框架]

### 統計
- 模組數量: X
- 設定檔數量: X
- 識別的設計模式: X

### 目錄結構
[簡要的目錄樹]
```

## 注意事項

1. **精簡為主** - 只記錄重要的模組和檔案，不需要列出每一個檔案
2. **描述簡潔** - 每個 purpose 用一句話說明即可
3. **結構清晰** - 模組按照功能分類（core, utils, services 等）
4. **自動偵測** - 根據專案檔案自動判斷語言和框架

## 輸出

- 建立 `.snapshot/` 目錄（如不存在）
- 產生 `.snapshot/snapshot.json`
- 顯示產生摘要
