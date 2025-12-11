# 產生專案快照 (Snapshot)

產生精簡的專案快照，使用 TOON 格式節省 token。

## TOON 格式說明

TOON (Token-Oriented Object Notation) 專為 LLM 優化，比 JSON 節省約 40% token。

**語法規則：**
- 縮排表示巢狀結構
- `[N]` 標示陣列長度
- `{field1,field2}` 宣告欄位標題
- 多值用 `|` 分隔
- 空值直接留空

## 縮寫對照

| 縮寫 | 原意 | 縮寫 | 原意 |
|------|------|------|------|
| proj | project | m | modules |
| desc | description | r | routes |
| tech | tech_stack | cfg | config |
| lang | language | ctrl | controllers |
| fw | framework | svc | services |
| db | database | repo | repositories |
| tpl | template | cmp | components |
| deps | dependencies | tbl | table |

## 執行流程

### Step 1: 準備工作

1. 檢查 `.snapshot/` 目錄，不存在則建立
2. 檢查現有 snapshot 檔案

### Step 2: 架構偵測

| 專案類型 | 偵測檔案 |
|---------|---------|
| Java/Kotlin Multi-module | `settings.gradle(.kts)` 含 `include` |
| .NET Multi-project | `*.sln` 含多個 `*.csproj` |
| JS/TS Monorepo | `pnpm-workspace.yaml`、`lerna.json`、`workspaces` |

決定 `split`：
- `projects`: 多專案架構（2+ 子專案）
- `modules`: 大型專案（50+ 模組）
- `none`: 一般專案

### Step 3: 掃描專案

**掃描重點**
- 識別路由對應（URL→handler→db→template）
- 識別模組及相依性
- 找出 Entry point

**忽略目錄**
`node_modules/`, `vendor/`, `build/`, `dist/`, `.git/`, `.snapshot/`, `__pycache__/`, `.idea/`, `coverage/`

### Step 4: 產生 Snapshot

---

#### 模式 A: `split: none`

產生 `.snapshot/snapshot.toon`：

```toon
proj: project-name
desc: short desc
v: 1.0
ts: 2025-01-01T00:00:00Z
split: none
tech:
  lang: TypeScript
  fw: Next.js
  db: PostgreSQL
entry: src/app/layout.tsx
r[6]{route,handler,tables,tpl}:
  GET /,page.tsx,,RootLayout
  GET /users,UserPage,users,UserList
  GET /users/:id,UserDetailPage,users|profiles,UserDetail
  POST /api/users,UserCtrl.create,users,
  PUT /api/users/:id,UserCtrl.update,users,
  DELETE /api/users/:id,UserCtrl.delete,users,
m.ctrl[2]{name,path,desc,deps}:
  UserCtrl,src/ctrl/user.ts,user api,UserSvc
  ProductCtrl,src/ctrl/product.ts,product api,ProductSvc
m.svc[2]{name,path,desc,deps}:
  UserSvc,src/svc/user.ts,user logic,UserRepo|EmailSvc
  ProductSvc,src/svc/product.ts,product logic,ProductRepo
m.repo[2]{name,path,desc,deps}:
  UserRepo,src/repo/user.ts,users tbl,
  ProductRepo,src/repo/product.ts,products tbl,
m.cmp[2]{name,path,desc,deps}:
  UserList,src/cmp/UserList.tsx,user list ui,
  UserDetail,src/cmp/UserDetail.tsx,user detail ui,
cfg[3]: package.json,tsconfig.json,.env
patterns[2]: Repository,MVC
```

---

#### 模式 B: `split: projects`

```
.snapshot/
├── index.toon
└── projects/
    ├── project-a.toon
    └── project-b.toon
```

**index.toon：**

```toon
proj: project-name
desc: short desc
v: 1.0
ts: 2025-01-01T00:00:00Z
split: projects
tech:
  lang: Java
  fw: Spring Boot
subs[2]{name,file,dir,desc}:
  project-a,projects/project-a.toon,project-a/,sub-project A
  project-b,projects/project-b.toon,project-b/,sub-project B
cfg[1]: settings.gradle
```

**projects/project-a.toon：**

```toon
proj: project-a
parent: parent-proj
desc: sub-project desc
tech:
  lang: Java
  fw: Spring Boot
entry: src/main/java/App.java
r[2]{route,handler,tables,tpl}:
  GET /api/users,UserCtrl.list,users,
  POST /api/users,UserCtrl.create,users,
m.ctrl[1]{name,path,desc,deps}:
  UserCtrl,src/ctrl/UserCtrl.java,user api,UserSvc
m.svc[1]{name,path,desc,deps}:
  UserSvc,src/svc/UserSvc.java,user logic,UserRepo
m.repo[1]{name,path,desc,deps}:
  UserRepo,src/repo/UserRepo.java,users tbl,
cfg[1]: build.gradle
```

---

#### 模式 C: `split: modules`

```
.snapshot/
├── index.toon
└── modules/
    ├── ctrl.toon
    ├── svc.toon
    └── repo.toon
```

**index.toon：**

```toon
proj: project-name
desc: short desc
v: 1.0
ts: 2025-01-01T00:00:00Z
split: modules
tech:
  lang: Java
  fw: Spring Boot
  db: MySQL
entry: src/main/java/App.java
r[3]{route,handler,tables,tpl}:
  GET /api/users,UserCtrl.list,users,
  POST /api/users,UserCtrl.create,users,
  GET /api/products,ProductCtrl.list,products,
mIdx[3]{type,file,count}:
  ctrl,modules/ctrl.toon,25
  svc,modules/svc.toon,30
  repo,modules/repo.toon,20
cfg[1]: application.yml
patterns[2]: Repository,Service
```

**modules/ctrl.toon：**

```toon
type: ctrl
parent: project-name
items[2]{name,path,desc,deps}:
  UserCtrl,src/ctrl/UserCtrl.java,user api,UserSvc
  ProductCtrl,src/ctrl/ProductCtrl.java,product api,ProductSvc
```

### Step 5: 顯示摘要

```
## Snapshot 產生完成

📁 .snapshot/snapshot.toon
- routes: X
- modules: X
```

## 注意事項

1. **精簡為主** - 只記錄重要模組
2. **英文描述** - 用最短英文描述（省 token）
3. **路由優先** - 優先記錄 URL→handler→db→template 對應

## 輸出

| split | 輸出檔案 |
|-------|---------|
| none | `.snapshot/snapshot.toon` |
| projects | `index.toon` + `projects/*.toon` |
| modules | `index.toon` + `modules/*.toon` |
