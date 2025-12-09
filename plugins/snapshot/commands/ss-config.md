# Snapshot 設定

你是一個專案分析專家，負責管理 snapshot 的產生設定。

## 設定檔位置

設定檔存放在專案根目錄：`.snapshot-config.json`

## 設定流程

### Step 1: 檢查現有設定

1. 檢查是否存在 `.snapshot-config.json`
2. 如果存在，讀取並顯示現有設定
3. 如果不存在，使用預設設定

### Step 2: 顯示設定選項

```markdown
# Snapshot 設定

## 目前設定

| 設定項目 | 目前值 | 說明 |
|----------|--------|------|
| 專案類型 | auto | auto/java/kotlin/python/javascript/csharp/custom |
| 輸出格式 | single | single/multi |
| 包含目錄 | ["src"] | 要掃描的目錄 |
| 排除模式 | [...] | 要排除的檔案/目錄 |
| 自訂範本 | null | 自訂的 snapshot 範本 |

## 可修改項目

1. 專案類型
2. 輸出格式
3. 包含目錄
4. 排除模式
5. 自訂範本
6. 重設為預設值
```

### Step 3: 處理使用者選擇

根據使用者選擇，更新對應的設定。

## 設定檔格式

```json
{
  "version": "1.0",
  "projectType": "auto",
  "outputFormat": "single",
  "include": [
    "src",
    "app",
    "lib"
  ],
  "exclude": [
    "node_modules",
    "build",
    "dist",
    ".git",
    "__pycache__",
    "*.test.*",
    "*.spec.*",
    "*Test.java",
    "*Tests.java"
  ],
  "customTemplate": null,
  "options": {
    "includePrivateMethods": false,
    "includeTestFiles": false,
    "maxDepth": 5,
    "minImportance": "medium"
  }
}
```

## 設定項目說明

### projectType（專案類型）

| 值 | 說明 |
|----|------|
| `auto` | 自動偵測專案類型 |
| `java` | Java (Spring) 專案 |
| `kotlin` | Kotlin (Android) 專案 |
| `python` | Python 專案 |
| `javascript` | JavaScript/TypeScript (Next.js) 專案 |
| `csharp` | C# (.NET Core) 專案 |
| `custom` | 使用自訂範本 |

### outputFormat（輸出格式）

| 值 | 說明 |
|----|------|
| `single` | 單一 snapshot.json 檔案 |
| `multi` | 多檔案架構（Snapshot 2.0） |

### include（包含目錄）

指定要掃描的目錄列表。預設會根據專案類型自動設定。

範例：
```json
{
  "include": [
    "src/main/java",
    "src/main/resources"
  ]
}
```

### exclude（排除模式）

指定要排除的檔案或目錄模式。支援 glob 語法。

預設排除：
- `node_modules/`
- `build/`、`dist/`、`out/`
- `.git/`
- `__pycache__/`
- `*.test.*`、`*.spec.*`
- `*Test.java`、`*Tests.java`

### customTemplate（自訂範本）

可以指定自訂的 snapshot 結構範本。

範例：
```json
{
  "customTemplate": {
    "modules": {
      "controllers": {
        "pattern": "**/*Controller.java",
        "type": "controller"
      },
      "services": {
        "pattern": "**/*Service.java",
        "type": "service"
      }
    }
  }
}
```

### options（進階選項）

| 選項 | 預設值 | 說明 |
|------|--------|------|
| `includePrivateMethods` | false | 是否包含私有方法 |
| `includeTestFiles` | false | 是否包含測試檔案 |
| `maxDepth` | 5 | 最大掃描深度 |
| `minImportance` | medium | 最低重要性（low/medium/high） |

## 自訂範本

### 建立自訂範本

1. 建立 `templates/` 目錄
2. 建立 `my-template.json` 範本檔案
3. 在設定檔中指定使用

### 範本格式

```json
{
  "name": "my-custom-template",
  "description": "我的自訂範本",
  "detectFiles": ["my-config.json"],
  "structure": {
    "entry_point": {
      "patterns": ["main.js", "index.js", "app.js"],
      "purpose": "應用程式入口"
    },
    "modules": {
      "controllers": {
        "patterns": ["**/controllers/**/*.js"],
        "type": "controller"
      },
      "models": {
        "patterns": ["**/models/**/*.js"],
        "type": "model"
      }
    },
    "config_files": {
      "patterns": ["*.config.js", ".env*"]
    }
  }
}
```

## 多檔案架構設定（Snapshot 2.0）

當 `outputFormat` 設為 `multi` 時，會產生以下結構：

```
snapshots/
├── snapshot_index.json   # 主索引
├── core.json             # 核心模組
├── services.json         # 服務模組
├── controllers.json      # 控制器
└── utils.json            # 工具類
```

### 分割規則設定

```json
{
  "outputFormat": "multi",
  "multiFileConfig": {
    "indexFile": "snapshots/snapshot_index.json",
    "splitBy": "module",
    "minModulesPerFile": 3
  }
}
```

## 匯出/匯入設定

### 匯出設定

將目前設定匯出為檔案，方便在其他專案使用。

### 匯入設定

從檔案匯入設定，快速套用到目前專案。

## 輸出

1. 顯示目前設定
2. 提供修改選項
3. 儲存更新後的設定到 `.snapshot-config.json`

## 下一步

設定完成後，提示使用者：
- 執行 `/ss-gen` 使用新設定產生 snapshot
- 執行 `/ss-init` 重新初始化（如果變更了專案類型）
