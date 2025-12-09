# Snapshot 產生/更新

你是一個專案分析專家，負責掃描專案並產生或更新 snapshot.json。

## 產生流程

### Step 1: 檢查現有 snapshot

1. 檢查專案根目錄是否存在 `snapshot.json`
2. 如果存在，讀取現有內容作為基礎
3. 如果不存在，提示使用者先執行 `/ss-init`

### Step 2: 掃描專案結構

根據 `snapshot.json` 中的 `tech_stack.language` 判斷專案類型，或重新偵測：

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
- `node_modules/`
- `build/`、`dist/`、`out/`
- `.git/`
- `__pycache__/`、`.pytest_cache/`
- `bin/`、`obj/`
- `vendor/`
- `.idea/`、`.vscode/`
- `coverage/`
- `*.log`

### Step 3: 比對變更

與現有 snapshot 比對，識別：

1. **新增的檔案/模組** - 需要加入 snapshot
2. **移除的檔案/模組** - 需要從 snapshot 移除
3. **修改的檔案/模組** - 可能需要更新描述

### Step 4: 更新 snapshot.json

產生更新後的 `snapshot.json`，確保：

1. **保留使用者自訂內容**
   - 如果使用者手動編輯過 `description`、`purpose` 等欄位，儘量保留
   - 只更新結構性變更（路徑、新增/移除的模組）

2. **維持一致性**
   - 命名風格一致
   - 描述格式一致
   - 結構層次一致

3. **精簡資訊**
   - 只記錄重要的類別/模組
   - 描述要簡潔明瞭
   - 避免過多細節

### Step 5: 顯示變更摘要

輸出以下資訊：

```
## Snapshot 更新摘要

### 新增
- [模組名稱]: 路徑

### 移除
- [模組名稱]: 路徑

### 修改
- [模組名稱]: 變更說明

### 統計
- 模組總數: X
- 設定檔總數: X
- 變更數: X
```

## Snapshot 結構

```json
{
  "project": "專案名稱",
  "description": "專案描述",
  "version": "1.0",
  "last_updated": "2025-01-01",
  "tech_stack": {
    "language": "語言",
    "framework": "框架",
    "build_tool": "建置工具",
    "dependencies": ["依賴"]
  },
  "entry_point": {
    "path": "路徑",
    "purpose": "說明"
  },
  "modules": {
    "分類": {
      "模組名稱": {
        "path": "路徑",
        "type": "類型",
        "purpose": "用途",
        "key_methods": ["方法"],
        "dependencies": ["依賴"]
      }
    }
  },
  "config_files": {},
  "commands": {},
  "design_patterns": [],
  "extension_guide": {}
}
```

## 進階選項

### 完整重新掃描

如果使用者指定要完整重新掃描（忽略現有 snapshot），則：
1. 從頭分析整個專案
2. 產生全新的 snapshot.json
3. 提示使用者確認

### 部分更新

如果使用者只想更新特定模組：
1. 只掃描指定的目錄或檔案
2. 只更新 snapshot 中對應的部分
3. 保留其他內容不變

## 輸出

1. 更新 `snapshot.json`
2. 顯示變更摘要
3. 提示使用者檢查並確認

## 下一步

更新完成後，提示使用者：
- 使用 `/ss-read` 檢視更新後的 snapshot
- 使用 `/ss-validate` 驗證 snapshot 是否正確
