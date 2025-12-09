# Snapshot 初始化

你是一個專案分析專家，負責為專案建立 snapshot.json「專案地圖」，讓 AI 可以快速了解專案結構。

## 初始化流程

### Step 1: 偵測專案類型

掃描專案根目錄，根據以下檔案判斷專案類型：

| 檔案 | 專案類型 |
|------|----------|
| `build.gradle` 或 `pom.xml` | Java (Spring) |
| `build.gradle.kts` + `AndroidManifest.xml` | Kotlin (Android) |
| `requirements.txt` 或 `pyproject.toml` 或 `setup.py` | Python |
| `package.json` + (`next.config.js` 或 `next.config.ts` 或 `next.config.mjs`) | JavaScript/TypeScript (Next.js) |
| `package.json` (無 Next.js) | JavaScript/TypeScript |
| `*.csproj` 或 `*.sln` | C# (.NET Core) |

如果無法判斷，詢問使用者專案類型。

### Step 2: 掃描專案結構

根據專案類型，掃描以下內容：

#### Java (Spring)
- 主程式入口: 尋找含有 `public static void main` 的類別
- 模組分析: 掃描 `@Controller`、`@Service`、`@Repository`、`@Entity`、`@Component` 註解的類別
- 設定檔: `application.properties`、`application.yml`、`build.gradle`、`pom.xml`
- 目錄結構: `src/main/java`、`src/main/resources`、`src/test`

#### Kotlin (Android)
- 主程式入口: 尋找 `MainActivity` 或 `Application` 類別
- 模組分析: 掃描 `Activity`、`Fragment`、`ViewModel`、`Repository` 類別
- 設定檔: `build.gradle.kts`、`AndroidManifest.xml`、`gradle.properties`
- 目錄結構: `app/src/main/kotlin` 或 `app/src/main/java`

#### Python
- 主程式入口: 尋找 `main.py`、`app.py`、`__main__.py` 或含有 `if __name__ == "__main__"` 的檔案
- 模組分析: 掃描所有 `.py` 檔案，分析類別和函數
- 設定檔: `requirements.txt`、`pyproject.toml`、`setup.py`、`setup.cfg`
- 目錄結構: 依專案實際結構

#### JavaScript/TypeScript (Next.js)
- 主程式入口: `pages/_app.tsx`、`app/layout.tsx`
- 模組分析: 掃描 `pages/`、`app/`、`components/`、`lib/`、`hooks/`、`api/`
- 設定檔: `package.json`、`next.config.js`、`tsconfig.json`、`.env`
- 目錄結構: Next.js 標準結構

#### C# (.NET Core)
- 主程式入口: `Program.cs`
- 模組分析: 掃描 `Controllers/`、`Services/`、`Models/`、`Data/`（DbContext）
- 設定檔: `*.csproj`、`appsettings.json`、`Program.cs`
- 目錄結構: .NET Core 標準結構

### Step 3: 產生 snapshot.json

根據掃描結果，產生以下格式的 `snapshot.json`：

```json
{
  "project": "專案名稱（從 package.json、build.gradle 等取得）",
  "description": "專案描述",
  "version": "1.0",
  "tech_stack": {
    "language": "主要語言",
    "framework": "使用的框架",
    "build_tool": "建置工具",
    "dependencies": ["主要依賴"]
  },
  "entry_point": {
    "path": "主程式路徑",
    "purpose": "主程式用途說明"
  },
  "modules": {
    "模組名稱": {
      "path": "路徑",
      "type": "類型（class/interface/record/component 等）",
      "purpose": "用途說明",
      "key_methods": ["重要方法"],
      "dependencies": ["依賴的其他模組"]
    }
  },
  "config_files": {
    "檔案名稱": {
      "path": "路徑",
      "purpose": "用途說明",
      "settings": ["重要設定項目"]
    }
  },
  "output": {
    "pattern": "輸出檔案路徑模式",
    "example": "範例"
  },
  "commands": {
    "build": "建置指令",
    "run": "執行指令",
    "test": "測試指令"
  },
  "design_patterns": ["使用的設計模式"],
  "extension_guide": {
    "擴展說明": ["步驟"]
  }
}
```

### Step 4: 建立 SNAPSHOT-GUIDE.md（如果不存在）

如果專案根目錄沒有 `SNAPSHOT-GUIDE.md`，建立一個簡單的指南檔案。

## 輸出

1. 將 `snapshot.json` 寫入專案根目錄
2. 如果是新建立，同時建立 `SNAPSHOT-GUIDE.md`
3. 顯示產生的 snapshot 摘要

## 注意事項

- 只分析主要程式碼，忽略 `node_modules`、`build`、`dist`、`.git`、`__pycache__`、`bin`、`obj` 等目錄
- 重要類別/模組才需要記錄，不需要列出所有檔案
- 描述要簡潔明瞭，讓 AI 能快速理解
- 如果專案結構複雜，可以使用 Snapshot 2.0 多檔案架構

## 下一步

初始化完成後，提示使用者：
- 使用 `/ss-read` 讀取並驗證 snapshot
- 使用 `/ss-gen` 重新產生或更新 snapshot
- 使用 `/ss-validate` 驗證 snapshot 與實際專案是否一致
