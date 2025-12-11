# Snapshot Plugin

產生專案快照，使用 TOON 格式讓 AI 快速了解專案結構。

## 指令

### `/snapshot` - 產生專案快照

掃描專案並產生 TOON 格式快照，比 JSON 節省約 40% token。

**功能：**
- 識別路由對應（URL → handler → db → template）
- 識別模組及相依性
- 自動偵測專案架構並分檔

## TOON 格式

TOON (Token-Oriented Object Notation) 專為 LLM 優化。

**語法：**
- 縮排表示巢狀
- `[N]` 陣列長度
- `{field1,field2}` 欄位標題
- `|` 分隔多值

**範例：**

```toon
proj: project-name
desc: short desc
tech:
  lang: TypeScript
  fw: Next.js
  db: PostgreSQL
entry: src/app/layout.tsx
r[2]{route,handler,tables,tpl}:
  GET /users,UserCtrl.list,users,UserList
  POST /api/users,UserCtrl.create,users,
m.ctrl[1]{name,path,desc,deps}:
  UserCtrl,src/ctrl/user.ts,user api,UserSvc
m.svc[1]{name,path,desc,deps}:
  UserSvc,src/svc/user.ts,user logic,UserRepo
cfg[2]: package.json,tsconfig.json
```

## 縮寫對照

| 縮寫 | 原意 | 縮寫 | 原意 |
|------|------|------|------|
| proj | project | m | modules |
| r | routes | cfg | config |
| ctrl | controllers | svc | services |
| repo | repositories | cmp | components |
| tpl | template | deps | dependencies |

## 自動分檔

| 條件 | split | 輸出 |
|------|-------|------|
| 一般專案 | none | `.snapshot/snapshot.toon` |
| 多專案架構 | projects | `index.toon` + `projects/*.toon` |
| 大型專案（50+ 模組） | modules | `index.toon` + `modules/*.toon` |

## 問題回報

[GitHub Issues](https://github.com/cashwu/claude-code-snapshot/issues)
