# 產生專案快照 (Snapshot)

你是一個專案分析專家，負責掃描專案並產生專案快照。根據專案規模和架構，可能產生單一檔案或分檔輸出。

## 執行流程

### Step 1: 準備工作

1. 檢查 `.snapshot/` 目錄是否存在，不存在則自動建立
2. 檢查是否有現有的 snapshot 檔案
3. 如果存在，讀取作為參考（但會重新產生）

### Step 1.5: 架構偵測（決定是否分檔）

在掃描專案前，先偵測專案架構來決定輸出模式：

#### 多專案架構偵測

檢查以下檔案來判斷是否為多專案架構：

| 專案類型 | 偵測檔案 | 偵測方式 |
|---------|---------|---------|
| **Java/Kotlin Multi-module** | `settings.gradle` 或 `settings.gradle.kts` | 檢查 `include` 語句數量 |
| **.NET Multi-project** | `*.sln` | 解析 sln 檔案，計算 `*.csproj` 數量 |
| **JS/TS Monorepo** | `pnpm-workspace.yaml`、`lerna.json`、或 `package.json` 中的 `workspaces` | 檢查 workspace 設定 |

#### 大型專案偵測

如果不是多專案架構，則統計模組數量：
- 計算所有將被識別的程式單元（controllers, services, repositories, components, hooks 等）
- 如果總數超過 **50 個**，標記為大型專案

#### 決定輸出模式 (split_mode)

根據偵測結果決定 `split_mode`：

| 條件 | split_mode | 輸出結構 |
|------|------------|---------|
| 多專案架構（2+ 子專案） | `projects` | `index.json` + `projects/*.json` |
| 大型單一專案（50+ 模組） | `modules` | `index.json` + `modules/*.json` |
| 一般專案 | `none` | 單一 `snapshot.json` |

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

### Step 3: 產生 Snapshot

根據 Step 1.5 決定的 `split_mode` 產生對應的輸出：

---

#### 模式 A: `split_mode = none`（一般專案）

在 `.snapshot/` 目錄下產生單一 `snapshot.json`：

```json
{
  "project": "專案名稱",
  "description": "專案一句話描述",
  "version": "1.0",
  "generated_at": "2025-01-01T00:00:00Z",
  "split_mode": "none",
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

---

#### 模式 B: `split_mode = projects`（多專案架構）

建立以下目錄結構：

```
.snapshot/
├── index.json
└── projects/
    ├── project-a.json
    ├── project-b.json
    └── ...
```

**index.json 結構：**

```json
{
  "project": "專案名稱",
  "description": "專案一句話描述",
  "version": "1.0",
  "generated_at": "2025-01-01T00:00:00Z",
  "split_mode": "projects",
  "tech_stack": {
    "language": "主要語言",
    "framework": "框架（如有）",
    "build_tool": "建置工具"
  },
  "structure": {
    "directories": {
      "project-a/": "子專案 A 描述",
      "project-b/": "子專案 B 描述"
    }
  },
  "projects": {
    "project-a": {
      "path": "projects/project-a.json",
      "root_dir": "project-a/",
      "description": "子專案 A 的用途",
      "type": "application|library|service"
    },
    "project-b": {
      "path": "projects/project-b.json",
      "root_dir": "project-b/",
      "description": "子專案 B 的用途",
      "type": "application|library|service"
    }
  },
  "config_files": {
    "settings.gradle": "Gradle 多模組設定"
  },
  "scripts": {
    "build": "./gradlew build"
  },
  "notes": "這是多專案架構，請根據需要讀取對應的子專案 snapshot"
}
```

**projects/project-a.json 結構（每個子專案獨立的 snapshot）：**

```json
{
  "project": "project-a",
  "parent": "父專案名稱",
  "description": "子專案描述",
  "version": "1.0",
  "generated_at": "2025-01-01T00:00:00Z",
  "tech_stack": {
    "language": "Java",
    "framework": "Spring Boot"
  },
  "structure": {
    "entry_point": {
      "path": "project-a/src/main/java/App.java",
      "purpose": "應用程式入口"
    },
    "directories": {
      "src/main/java/": "原始碼",
      "src/test/java/": "測試"
    }
  },
  "modules": {
    "controllers": { ... },
    "services": { ... }
  },
  "config_files": {
    "build.gradle": "Gradle 建置設定"
  }
}
```

---

#### 模式 C: `split_mode = modules`（大型單一專案）

建立以下目錄結構：

```
.snapshot/
├── index.json
└── modules/
    ├── controllers.json
    ├── services.json
    ├── repositories.json
    └── ...
```

**index.json 結構：**

```json
{
  "project": "專案名稱",
  "description": "專案一句話描述",
  "version": "1.0",
  "generated_at": "2025-01-01T00:00:00Z",
  "split_mode": "modules",
  "tech_stack": {
    "language": "主要語言",
    "framework": "框架",
    "build_tool": "建置工具",
    "dependencies": ["主要依賴"]
  },
  "structure": {
    "entry_point": {
      "path": "src/main/java/App.java",
      "purpose": "應用程式入口"
    },
    "directories": {
      "src/controllers/": "API 控制器",
      "src/services/": "業務邏輯"
    }
  },
  "modules_index": {
    "controllers": {
      "path": "modules/controllers.json",
      "count": 25,
      "description": "REST API 控制器"
    },
    "services": {
      "path": "modules/services.json",
      "count": 30,
      "description": "業務邏輯服務"
    },
    "repositories": {
      "path": "modules/repositories.json",
      "count": 20,
      "description": "資料存取層"
    }
  },
  "config_files": {
    "application.yml": "Spring Boot 設定"
  },
  "scripts": {
    "build": "./gradlew build"
  },
  "patterns": ["Repository", "Service", "DTO"],
  "notes": "這是大型專案，模組依類型分檔。請根據需要讀取對應的模組檔案"
}
```

**modules/controllers.json 結構：**

```json
{
  "module_type": "controllers",
  "parent_project": "專案名稱",
  "generated_at": "2025-01-01T00:00:00Z",
  "count": 25,
  "items": {
    "UserController": {
      "path": "src/controllers/UserController.java",
      "type": "class",
      "purpose": "使用者相關 API",
      "exports": ["getUser", "createUser", "updateUser"],
      "dependencies": ["UserService"]
    },
    "ProductController": {
      "path": "src/controllers/ProductController.java",
      "type": "class",
      "purpose": "商品相關 API",
      "exports": ["getProducts", "createProduct"],
      "dependencies": ["ProductService"]
    }
  }
}
```

### Step 4: 顯示摘要

產生完成後，根據 `split_mode` 輸出對應的摘要：

#### split_mode = none（單檔輸出）

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

#### split_mode = projects（多專案分檔）

```
## Snapshot 產生完成（多專案架構）

📁 輸出位置: .snapshot/
├── index.json（主索引）
└── projects/
    ├── [project-a].json
    ├── [project-b].json
    └── ...

### 專案概覽
- 名稱: [專案名稱]
- 架構: 多專案（Multi-module/Multi-project）
- 子專案數量: X

### 子專案列表
| 專案 | 類型 | 說明 |
|------|------|------|
| project-a | application | 子專案 A 描述 |
| project-b | library | 子專案 B 描述 |

### 使用方式
1. 先讀取 `index.json` 了解專案架構
2. 根據需要讀取對應的子專案 snapshot
```

#### split_mode = modules（大型專案分檔）

```
## Snapshot 產生完成（大型專案）

📁 輸出位置: .snapshot/
├── index.json（主索引）
└── modules/
    ├── controllers.json
    ├── services.json
    └── ...

### 專案概覽
- 名稱: [專案名稱]
- 語言: [語言]
- 框架: [框架]

### 模組統計
| 模組類型 | 數量 | 說明 |
|----------|------|------|
| controllers | 25 | REST API 控制器 |
| services | 30 | 業務邏輯服務 |
| repositories | 20 | 資料存取層 |
| **總計** | **75** | |

### 使用方式
1. 先讀取 `index.json` 了解專案架構
2. 根據需要讀取對應的模組檔案（如：修改 API 就讀取 `controllers.json`）
```

## 注意事項

1. **精簡為主** - 只記錄重要的模組和檔案，不需要列出每一個檔案
2. **描述簡潔** - 每個 purpose 用一句話說明即可
3. **結構清晰** - 模組按照功能分類（core, utils, services 等）
4. **自動偵測** - 根據專案檔案自動判斷語言和框架
5. **自動分檔** - 多專案架構或大型專案會自動分檔輸出

## 輸出

根據專案規模和架構，可能產生以下輸出：

### 一般專案（split_mode = none）
- 建立 `.snapshot/` 目錄（如不存在）
- 產生 `.snapshot/snapshot.json`

### 多專案架構（split_mode = projects）
- 建立 `.snapshot/` 目錄（如不存在）
- 建立 `.snapshot/projects/` 子目錄
- 產生 `.snapshot/index.json`（主索引）
- 產生 `.snapshot/projects/*.json`（每個子專案一個檔案）

### 大型專案（split_mode = modules）
- 建立 `.snapshot/` 目錄（如不存在）
- 建立 `.snapshot/modules/` 子目錄
- 產生 `.snapshot/index.json`（主索引）
- 產生 `.snapshot/modules/*.json`（每個模組類型一個檔案）

最後顯示產生摘要。
