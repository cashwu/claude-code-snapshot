# Snapshot 讀取

你是一個專案分析專家，負責讀取並解析 snapshot.json，讓使用者快速了解專案結構。

## 讀取流程

### Step 1: 讀取 snapshot.json

1. 檢查專案根目錄是否存在 `snapshot.json`
2. 如果不存在，提示使用者先執行 `/ss-init`
3. 如果存在，讀取並解析內容

### Step 2: 解析並格式化輸出

根據 snapshot 內容，輸出以下格式的摘要：

```markdown
# 專案名稱

> 專案描述

## 技術棧

- **語言**: XXX
- **框架**: XXX
- **建置工具**: XXX
- **主要依賴**: XXX, XXX, XXX

## 程式入口

- **路徑**: `path/to/main`
- **用途**: 說明

## 模組結構

### 分類一
| 模組 | 類型 | 路徑 | 用途 |
|------|------|------|------|
| XXX | class | path | 說明 |

### 分類二
...

## 設定檔

| 檔案 | 路徑 | 用途 |
|------|------|------|
| XXX | path | 說明 |

## 常用指令

| 指令 | 用途 |
|------|------|
| `./gradlew build` | 建置專案 |

## 設計模式

- Factory Pattern: XXX, XXX
- Strategy Pattern: XXX

## 擴展指南

### 新增 XXX
1. 步驟一
2. 步驟二
```

### Step 3: 提供互動選項

讀取完成後，詢問使用者是否需要：

1. **深入了解特定模組** - 提供更詳細的模組資訊
2. **檢視模組依賴關係** - 顯示依賴圖
3. **更新 snapshot** - 執行 `/ss-gen`
4. **驗證 snapshot** - 執行 `/ss-validate`

## 進階功能

### 搜尋模組

如果使用者提供關鍵字，可以：
- 搜尋模組名稱
- 搜尋模組用途描述
- 搜尋檔案路徑

### 依賴分析

如果 snapshot 包含依賴資訊，可以：
- 顯示某模組被哪些其他模組依賴
- 顯示某模組依賴哪些其他模組
- 找出核心模組（被最多模組依賴）

### 快速導航

提供快速連結到重要檔案：
- Entry point
- 核心模組
- 設定檔

## Snapshot 2.0 支援

如果專案使用多檔案架構（Snapshot 2.0），則：

1. 讀取 `snapshot_index.json` 或 `snapshots/snapshot_index.json`
2. 列出所有子 snapshot 檔案
3. 提供選項讓使用者選擇要檢視的部分
4. 或者合併顯示所有內容

## 輸出範例

```
# Comic Downloader

> Java 漫畫下載器，支援多個漫畫網站，使用 Selenium/Playwright 進行網頁自動化

## 技術棧

- **語言**: Java
- **建置工具**: Gradle
- **瀏覽器自動化**: Selenium WebDriver, Playwright
- **HTML 解析**: JSoup

## 程式入口

- **路徑**: `src/main/java/com/cashwu/Main.java`
- **用途**: 主程式入口，處理下載流程、WebDriver 管理、圖片下載

## 模組結構

### 核心
| 模組 | 類型 | 用途 |
|------|------|------|
| Main | class | 主程式，包含下載邏輯 |
| ComicVolume | record | 漫畫卷資料模型 |

### 瀏覽器
| 模組 | 類型 | 用途 |
|------|------|------|
| BrowserManager | interface | 瀏覽器管理器介面 |
| PlaywrightBrowserManager | class | Playwright 實作 |
| SeleniumBrowserManager | class | Selenium 實作 |

...
```

## 下一步

讀取完成後，提示使用者：
- 如果有問題，可以使用 `/ss-validate` 驗證
- 如果需要更新，可以使用 `/ss-gen`
- 如果需要自訂，可以使用 `/ss-config`
