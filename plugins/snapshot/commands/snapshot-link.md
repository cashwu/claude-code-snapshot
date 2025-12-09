# 在 CLAUDE.md 加入 Snapshot 參考

你負責在專案的 `CLAUDE.md` 檔案中加入 snapshot 參考提示。

## 執行流程

### Step 1: 檢查 CLAUDE.md

1. 檢查專案根目錄是否有 `CLAUDE.md` 檔案
2. 如果存在，讀取檔案內容

### Step 2: 判斷是否需要加入

檢查檔案內容是否已包含 `.snapshot/snapshot.json` 字串：

- **已包含** → 輸出提示訊息，不做任何修改
- **未包含** → 繼續 Step 3

### Step 3: 加入提示文字

**如果 CLAUDE.md 不存在：**

建立新檔案，內容為：

```markdown
## 專案架構

如需了解專案架構，請參考 `.snapshot/snapshot.json`。
```

**如果 CLAUDE.md 已存在：**

在檔案最後加入（前面空一行）：

```markdown

## 專案架構

如需了解專案架構，請參考 `.snapshot/snapshot.json`。
```

### Step 4: 輸出結果

**成功加入時：**
```
已在 CLAUDE.md 加入 snapshot 參考提示
```

**已存在時：**
```
CLAUDE.md 已包含 snapshot 參考，無需重複加入
```

**建立新檔案時：**
```
已建立 CLAUDE.md 並加入 snapshot 參考提示
```

## 注意事項

1. **不要覆蓋** - 絕對不能覆蓋 CLAUDE.md 原有內容
2. **避免重複** - 檢查關鍵字避免重複加入
3. **保持格式** - 確保加入的內容前後有適當的空行
