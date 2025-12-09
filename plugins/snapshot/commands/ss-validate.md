# Snapshot 驗證

你是一個專案分析專家，負責驗證 snapshot.json 是否與實際專案結構一致。

## 驗證流程

### Step 1: 讀取 snapshot.json

1. 檢查專案根目錄是否存在 `snapshot.json`
2. 如果不存在，提示使用者先執行 `/ss-init`
3. 讀取並解析 snapshot 內容

### Step 2: 掃描實際檔案

根據 snapshot 中記錄的所有路徑，檢查：

1. **檔案存在性** - 檔案是否真的存在
2. **路徑正確性** - 路徑是否正確
3. **類型一致性** - 類型標註是否正確（class/interface/record 等）

### Step 3: 找出差異

比對 snapshot 與實際專案，產生差異報告：

#### 差異類型

1. **遺失的檔案** - snapshot 中有記錄但檔案不存在
2. **新增的檔案** - 檔案存在但 snapshot 未記錄
3. **路徑錯誤** - 路徑不正確
4. **類型錯誤** - 類型標註不正確
5. **過時的描述** - 檔案內容與描述不符

### Step 4: 輸出驗證報告

```markdown
# Snapshot 驗證報告

## 驗證結果: ✅ 通過 / ⚠️ 有警告 / ❌ 失敗

### 統計
- 檢查的模組數: X
- 檢查的設定檔數: X
- 問題數: X

---

## ❌ 遺失的檔案

以下檔案在 snapshot 中有記錄，但實際不存在：

| 模組 | 路徑 | 建議 |
|------|------|------|
| XXX | path/to/file.java | 從 snapshot 移除 |

## ⚠️ 未記錄的檔案

以下重要檔案存在但未在 snapshot 中記錄：

| 檔案 | 路徑 | 建議 |
|------|------|------|
| NewService.java | path/to/NewService.java | 加入 snapshot |

## ⚠️ 可能過時的描述

以下模組的描述可能需要更新：

| 模組 | 問題 | 建議 |
|------|------|------|
| XXX | 新增了方法 | 更新 key_methods |

---

## 建議操作

1. 執行 `/ss-gen` 更新 snapshot
2. 手動檢查並確認變更
```

### Step 5: 提供修復選項

驗證完成後，詢問使用者是否要：

1. **自動修復** - 執行 `/ss-gen` 自動更新 snapshot
2. **手動修復** - 顯示需要修改的內容，讓使用者自行編輯
3. **忽略** - 暫時忽略問題

## 驗證檢查項目

### Entry Point 驗證
- [ ] 路徑存在
- [ ] 包含 main 方法或入口函數

### 模組驗證
- [ ] 所有路徑存在
- [ ] 類型標註正確
- [ ] key_methods 中的方法存在

### 設定檔驗證
- [ ] 所有路徑存在
- [ ] 設定項目存在

### 指令驗證
- [ ] 建置指令可執行
- [ ] 執行指令可執行

## 深度驗證（可選）

如果使用者要求深度驗證，額外檢查：

1. **方法簽名** - 檢查 key_methods 中的方法簽名是否正確
2. **依賴關係** - 檢查 dependencies 是否正確
3. **設計模式** - 檢查 design_patterns 是否還在使用
4. **註解一致性** - 檢查類別/函數的註解是否與描述一致

## 自動掃描新檔案

掃描以下目錄，找出可能需要加入 snapshot 的新檔案：

### Java (Spring)
- `src/main/java/**/*.java`
- 排除 `*Test.java`、`*Tests.java`

### Kotlin (Android)
- `app/src/main/**/*.kt`
- 排除 `*Test.kt`

### Python
- `**/*.py`
- 排除 `test_*.py`、`*_test.py`

### JavaScript/TypeScript
- `src/**/*.{js,jsx,ts,tsx}`
- 排除 `*.test.{js,ts}`、`*.spec.{js,ts}`

### C# (.NET)
- `**/*.cs`
- 排除 `*Tests.cs`

## 輸出

1. 顯示驗證報告
2. 列出建議操作
3. 提供修復選項

## 下一步

驗證完成後，提示使用者：
- 如果有問題，執行 `/ss-gen` 更新 snapshot
- 如果沒問題，snapshot 是最新狀態
