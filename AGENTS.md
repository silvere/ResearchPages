# ResearchPages · Repository Guidelines

ResearchPages 是一个零依赖的纯静态站点，部署在 GitHub Pages。**每个深度调研主题一个自包含的子站**，根 `index.html` 是"调研台"首页，把所有主题站按领域分区索引。模式脱胎于姊妹项目 `../BooksReader`。

用户通过 `/rp <主题>` 技能触发新调研（技能文件在 `~/.claude/commands/rp.md`，是薄入口）；本文件是唯一的规范来源（single source of truth），流程细节以这里为准。

## 目录结构与命名

```
ResearchPages/
├── index.html          # 调研台首页（领域分区 + 报告卡片）
├── AGENTS.md           # 本文件
├── README.md           # 简介 + 已发布报告列表
├── .nojekyll           # 必须保留：让 Pages 直接服务中文路径与 assets/
└── <主题中文名>/        # 每主题一个自包含目录，如 具身智能/
    ├── index.html      # 总览页（固定）：结论先行 + 核心信息图
    ├── <定制页 3-5 个>  # 按主题性质定制，kebab-case 按用途命名
    ├── sources.html    # 参考来源页（固定）：全部引用 + 访问日期
    └── assets/
        └── style.css   # 本站独立视觉系统
```

- 中间定制页常用命名：`landscape.html`（全景/格局）、`players.html`（关键玩家）、`tech.html`（技术拆解）、`timeline.html`（时间线）、`debate.html`（争议与反方）、`cases.html`（案例）、`data.html`（数据与证据）。按主题性质选 3-5 个，不强求一致。
- 站内一律相对链接；每站自包含，不跨站引用资源。
- 报告编号全局递增：`RP 01`、`RP 02`……与首页卡片、总览页 eyebrow 保持一致。

## 每主题调研 SOP（四阶段闭环）

### Phase A — 扇出调研
并行派 3-5 个 `researcher` agent（一条消息多个工具调用），按互补角度扇出，例如：

1. 全景与格局：领域定义、市场/学术现状、主要流派
2. 关键玩家：公司/机构/人物，各自路线与最新动作
3. 数据与证据：核心数字、基准、财务/融资/论文数据（必须带来源 URL 和日期）
4. 争议与反方：主要分歧点、看空观点、失败案例
5. 时间线与最新动态：近 12-24 个月大事记

要求每个 agent 返回**结构化发现 + 一手来源 URL**，不接受无出处的断言。

### Phase B — 事实核验
从调研结果中挑出将写入站点的**关键结论**（具体数字、日期、排名、因果断言），批量交 `fact-checker` agent 做证伪式核查。未通过核验的：能修正则修正，不能修正则标注"存疑"或直接删除。宁可少一个论点，不放一个错误。

### Phase C — 建站
1. 根据调研结果设计中间页面结构（3-5 页），先定信息架构再动手写。
2. 建主题目录，写 `assets/style.css`：独立主色调（与已有各站区分），复用全局视觉语言（见下）。
3. 写页面：总览页必须**结论先行**（一句话核心判断 + 一张核心 SVG 信息图），各页顶部 `nav.nav` 联动。
4. `sources.html` 列出全部引用来源：标题、URL、访问日期、用在哪一页。

### Phase D — 上架
1. 根 `index.html` 对应领域分区新增报告卡片（领域不存在则新建分区）。
2. `README.md` 报告列表加一行。
3. 本地预览验证（见下），然后 commit + push，Pages 自动上线。

**Phase D 是硬性收尾**：站点建完默认立即 commit + push，无需等待用户确认——没推上 `main` 的站等于没交付。每次开工前先 `git status`：若发现上一会话遗留的完整未提交站点，先按其编号单独补交，本次主题编号顺延。

## 视觉语言

- UTF-8、`lang="zh-CN"`、语义化标签、两空格缩进；类名 lowercase kebab-case；主题色进 `:root` 自定义属性。
- 全局气质是"档案/情报简报"（DOSSIER 感）：mono 小字 eyebrow + 报告编号、中文衬线大标题（Songti/Noto Serif SC）、密实的信息版式——与 BooksReader 的报刊读书感区分。
- 每站独立主色与视觉身份，改动只进本站 `assets/style.css`，不跨站复制大段样式。
- 鼓励内嵌 SVG 信息图（带 `role="img"` 和 `aria-label`）与轻量可交互组件（纯内联 JS，无外部依赖）。
- 图标用 data URI SVG favicon（参照首页写法），不引外部资源。

## 本地验证与发布

```bash
cd ~/Code/ResearchPages
python3 -m http.server 8000
```

打开 `http://127.0.0.1:8000/`，检查：改动页面的渲染、站内相对链接、交互组件、响应式布局、浏览器控制台无报错。

- 推 `main` 即发布（GitHub Pages）。
- Commit 用 Conventional Commit 风格，如：`feat: RP 01 具身智能调研站上架`、`fix: 修正首页卡片链接`。
- `.nojekyll` 永远不删。
