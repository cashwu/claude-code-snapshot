# Snapshot Plugin

為 Claude Code 提供專案快照功能，讓 AI 快速了解專案結構。

## 指令說明

### `/ss-init` - 初始化 Snapshot

首次建立 `snapshot.json`。

**流程：**
1. 偵測專案類型（Java、Kotlin、Python、JavaScript、C#）
2. 選擇對應的範本
3. 掃描專案結構
4. 產生初始 `snapshot.json`
5. 建立 `SNAPSHOT-GUIDE.md`（如果不存在）

**使用時機：**
- 專案尚未有 `snapshot.json`
- 想要重新初始化 snapshot

---

### `/ss-gen` - 產生/更新 Snapshot

重新掃描專案並更新 `snapshot.json`。

**流程：**
1. 讀取現有 `snapshot.json`
2. 掃描專案結構
3. 比對變更
4. 產生更新後的 `snapshot.json`
5. 顯示變更摘要

**使用時機：**
- 專案有新增或移除檔案
- 想要更新 snapshot 內容

---

### `/ss-read` - 讀取 Snapshot

讀取並解析現有的 snapshot，顯示專案概覽。

**輸出內容：**
- 專案基本資訊
- 技術棧
- 模組結構
- 設定檔列表
- 常用指令
- 設計模式

**使用時機：**
- 想要快速了解專案結構
- 新成員加入專案
- 開始新的對話，讓 AI 了解專案

---

### `/ss-validate` - 驗證 Snapshot

檢查 snapshot 與實際專案結構是否一致。

**檢查項目：**
- 檔案存在性
- 路徑正確性
- 類型一致性
- 找出未記錄的新檔案

**使用時機：**
- 確認 snapshot 是否過時
- 專案重構後驗證

---

### `/ss-config` - 自訂設定

管理 snapshot 的產生設定。

**可設定項目：**
- 專案類型（auto/java/kotlin/python/javascript/csharp/custom）
- 輸出格式（single/multi）
- 包含/排除的目錄
- 自訂範本

**設定檔位置：** `.snapshot-config.json`

---

## 範本

此 plugin 提供以下預設範本：

| 範本 | 檔案 | 說明 |
|------|------|------|
| Java (Spring) | `java-spring.json` | Spring Boot 專案 |
| Kotlin (Android) | `kotlin-android.json` | Android 專案 |
| Python | `python.json` | Django/Flask/FastAPI 等 |
| JavaScript (Next.js) | `javascript-nextjs.json` | Next.js/React 專案 |
| C# (.NET) | `csharp-dotnet.json` | .NET Core 專案 |
| 自訂 | `custom.json` | 自訂範本基礎 |

## 建立自訂範本

1. 複製 `templates/custom.json`
2. 根據你的框架修改：
   - `detectFiles` - 設定偵測檔案
   - `structure.modules` - 設定模組分類
   - `config_files` - 設定設定檔
3. 在 `/ss-config` 中指定使用

範例：

```json
{
  "name": "my-framework",
  "detectFiles": ["my-config.js"],
  "structure": {
    "modules": {
      "controllers": {
        "patterns": ["**/controllers/**/*.js"],
        "type": "controller",
        "purpose": "控制器"
      }
    }
  }
}
```

## 最佳實踐

1. **首次使用**：執行 `/ss-init` 初始化
2. **定期更新**：專案有重大變更時執行 `/ss-gen`
3. **新對話開始**：執行 `/ss-read` 讓 AI 了解專案
4. **驗證一致性**：偶爾執行 `/ss-validate` 確認

## 問題回報

遇到問題請到 [GitHub Issues](https://github.com/cashwu/claude-code-snapshot/issues) 回報。
