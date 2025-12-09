# 安裝指南

## 前置需求

- [Claude Code CLI](https://claude.ai/code) 已安裝
- Claude Code 版本需支援 plugin 功能

## 安裝步驟

### 方法一：從 Marketplace 安裝（推薦）

1. 新增 marketplace：

```bash
/plugin marketplace add cashwu/claude-code-snapshot
```

2. 安裝 snapshot plugin：

```bash
/plugin install snapshot
```

3. 驗證安裝：

```bash
/plugin list
```

應該會看到 `snapshot` 在已安裝的 plugin 列表中。

### 方法二：從本地安裝

1. Clone 此 repo：

```bash
git clone https://github.com/cashwu/claude-code-snapshot.git
```

2. 在 Claude Code 中安裝：

```bash
/plugin install /path/to/claude-code-snapshot/plugins/snapshot
```

## 驗證安裝

安裝完成後，可以使用以下指令驗證：

```bash
# 顯示 snapshot 說明
/ss-init
```

## 解除安裝

```bash
/plugin uninstall snapshot
```

## 更新

```bash
/plugin update snapshot
```

## 疑難排解

### Plugin 無法安裝

1. 確認 Claude Code 版本支援 plugin 功能
2. 確認 marketplace 已正確新增
3. 嘗試重新啟動 Claude Code

### 指令無法執行

1. 確認 plugin 已正確安裝：`/plugin list`
2. 確認指令名稱正確（使用 `/ss-` 前綴）
3. 檢查錯誤訊息並回報問題

## 問題回報

如果遇到問題，請到 [GitHub Issues](https://github.com/cashwu/claude-code-snapshot/issues) 回報。
