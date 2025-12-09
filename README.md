# Claude Code Snapshot

> 基於 [tzangms (海總理)](https://tzangms.com) 的 Vibe Coding 方法論

Snapshot Plugin 讓 AI 可以快速產生和維護專案的 `snapshot.json`「專案地圖」，大幅減少 AI 理解專案結構的時間。

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
| `/snapshot` | 掃描專案並產生 `.snapshot/snapshot.json` |
| `/snapshot-link` | 在 `CLAUDE.md` 加入 snapshot 參考提示 |

## 快速開始

```
/snapshot
```

AI 會自動：
1. 建立 `.snapshot/` 目錄
2. 偵測專案類型（語言、框架）
3. 掃描專案結構
4. 產生 `.snapshot/snapshot.json`

## 輸出位置

```
your-project/
├── .snapshot/
│   └── snapshot.json    # 專案快照（給 AI 看的）
├── src/
└── ...
```

## 支援的專案類型

| 類型 | 偵測方式 | 主要分析項目 |
|------|----------|--------------|
| **Java (Spring)** | `build.gradle`, `pom.xml` | @Controller, @Service, @Repository, @Entity |
| **Kotlin (Android)** | `build.gradle.kts`, `AndroidManifest.xml` | Activity, Fragment, ViewModel, Repository |
| **Python** | `requirements.txt`, `pyproject.toml` | 模組、類別、函數 |
| **JavaScript/TypeScript** | `package.json` | components, API routes, hooks |
| **C# (.NET)** | `*.csproj`, `*.sln` | Controllers, Services, Models |
| **其他** | 自動偵測 | 根據檔案結構分析 |

## Snapshot 結構範例

```json
{
  "project": "my-app",
  "description": "專案描述",
  "version": "1.0",
  "generated_at": "2025-01-01T00:00:00Z",
  "tech_stack": {
    "language": "TypeScript",
    "framework": "Next.js",
    "build_tool": "npm",
    "dependencies": ["react", "next", "prisma"]
  },
  "structure": {
    "entry_point": {
      "path": "src/app/page.tsx",
      "purpose": "首頁入口"
    },
    "directories": {
      "src/app/": "App Router 頁面",
      "src/components/": "React 元件",
      "src/lib/": "工具函數"
    }
  },
  "modules": {
    "components": {
      "Header": {
        "path": "src/components/Header.tsx",
        "purpose": "網站導覽列"
      }
    },
    "services": {
      "auth": {
        "path": "src/lib/auth.ts",
        "purpose": "認證邏輯"
      }
    }
  },
  "config_files": {
    "next.config.js": "Next.js 設定",
    "tailwind.config.js": "Tailwind CSS 設定"
  },
  "scripts": {
    "dev": "npm run dev",
    "build": "npm run build"
  }
}
```

## 最佳實踐

1. **首次使用**：執行 `/snapshot` 產生專案快照
2. **定期更新**：專案有重大變更時重新執行 `/snapshot`
3. **新對話開始**：AI 會自動讀取 `.snapshot/snapshot.json`

## 參考資料

- [Vibe Coding: Snapshot 2.0](https://tzangms.com/vibe-coding-snapshot-2-0/)
- [Vibe Coding snapshot - 小海報 61](https://tzangms.substack.com/p/vibe-coding-snapshot-61)

## 授權

MIT License

## 作者

- [Cash Wu](https://github.com/cashwu)
