# Zhixie's Page

窝嘚网站，托管于 **GitHub Pages**。纯原生 HTML / CSS / JS，零框架、零构建、零后端。

- 主站：<https://uzjhf-836.github.io/>
- 博客：<https://uzjhf-836.github.io/blogs/index.html>
- 项目：<https://uzjhf-836.github.io/project/index.html>
- 国象：<https://uzjhf-836.github.io/chess/index.html>

## 向导

- 如果你想投稿文章
[点我投稿](#投稿)
- 如果你要反馈bug
[点我跳转到issues](https://www.github.com/uzjhf-836/uzjhf-836.github.io/issues)
## 站点栏目

| 栏目 | 路径 | 内容 |
| --- | --- | --- |
| 首页 | `/index.html` | Profile 卡片、MyTools 下载、联系方式、国象入口 |
| 项目 | `/project/index.html` | 作品展示：MyTools / Edit text / Claude Time Generate / Cear |
| 博客 | `/blogs/index.html` | 数据驱动的文章列表 + 单模板文章页 |
| 国际象棋 | `/chess/index.html` | 人机对弈网页版国际象棋「残局大师版」 |
| 404 | `/404.html` | 毛玻璃报错页 + 引导提 issue |

## 全站基础

- **字体**：HarmonyOS Sans SC（自托管于 `/font/`）+ Inter（Google Fonts）回退
- **双主题**：浅色 / 深色，默认深色；`<body>` 加 `.light` 类切换，偏好存 cookie `theme`，支持 URL 参数强制主题（`?light` / `?dark` / `?1` / `?0`，不写 cookie）
- **通用组件**：毛玻璃卡片（`.card-light`）+ 鼠标跟随光效、主题切换涟漪扩散动画、Cookie 同意横幅（三选：全部 / 仅必要 / 拒绝）、齿轮设置下拉菜单、渐入动画
- **样式分层**：`css/style.css` 全站共享；博客分区独立使用 `css/blogs.css`

## 目录结构

```
├── index.html          # 首页
├── 404.html
├── css/                # 全站样式（style.css）+ 博客样式（blogs.css）
├── blogs/              # 博客子系统（见下）
├── project/            # 项目展示页
├── chess/              # 国际象棋子站（见下）
├── egg/                # 彩蛋 / 隐藏小页面
├── font/               # 自托管字体（HarmonyOS Sans SC 等）
├── sources/            # 静态素材（favicon 等）
└── docs/               # 文档/笔记（markdown）
```

## 博客（blogs/）

数据驱动的静态博客。

```
blogs/
├── index.html        # 列表页：读 文章.js 渲染卡片
├── article.html      # 文章页通用模板（?id=N 取文章）
├── articles/         # 正文文件（N.md）
├── 文章.js           # 文章元数据 + 样式徽标定义
├── badge-colors.css  # 徽标配色
├── create_article.py # 文章生成脚本（本地专用）
└── test_server.py    # 本地预览服务器（本地专用）
```

### 运作原理

- **列表页**：`文章.js` 数据驱动渲染，支持 `?sort=` 排序（最新/最旧/标题/编号/收藏/随机）、`?pin=0` 关闭置顶优先
- **文章页**：单模板 `article.html?id=N` → `fetch('/blogs/articles/N.md')` → `marked` 渲染，正文支持 **Markdown 混写原生 HTML**
- **阅读体验**：总字数、预计阅读时间（按字数自动估算或手动填写）、整页阅读进度条 + 返回顶部按钮、移动端悬浮按钮

### 文章记录格式（`文章.js`）

```
["标题", "介绍", "日期", [样式], [标签], "#编号", 阅读按钮, 跳转URL, 标题开关, 阅读时间, 时间开关]
```

| 字段 | 说明 |
| --- | --- |
| 阅读按钮 | `1` 显示「点击阅读」，`0` 隐藏 |
| 跳转URL | `"None"` 用默认编号页，其他字符串为外链/自定义跳转 |
| 标题开关 | `1` 自动设 `<title>标题 - Zhixie's Blogs</title>`，`0` 保留硬编码 |
| 阅读时间 | 填分钟数；`时间开关` `1` = 按字数自动估算，`0` = 用填的分钟数 |

样式徽标定义在 `文章.js` 的 `STYLE_BADGES`（默认 `固定` / `新` / `热`，可扩展）。

> 想投稿见文末 [投稿](#投稿)：在仓库 Issues 里贴文章 markdown，人工审核后发布。
> 生成 `articles/N.md`、注册 `文章.js` 这些是作者本地的事，读者用不着管。

## 国际象棋

[想玩点我](https://uzjhf-836.github.io/chess/index.html)
网页版人机对弈国际象棋「**残局大师版（终极强化 AI）**」，玩家执白 vs AI 执黑。单文件纯前端实现（约 2300 行），无任何库依赖。

- **五档 AI 难度**（AI:1 新手 ~ AI:4pro 深度 5 全力搜索）
- **计时**：无限 / 15 / 10 / 5 / 1 分钟 / 30 秒，对局中可切换
- **自定义棋局**：Canvas 可视化摆盘编辑器，任意设置起始局面
- **升变 / 预走棋（Premove）/ 吃子比分 / AI 台词气泡 / 6 套主题 + 自定义配色 / R 键重开**
- **AI 实现**：Minimax + Alpha-Beta 剪枝，辅以迭代加深、杀手走法、MVV-LVA 排序；内置 KRK 残局引擎与子力 + 位置表（PST）+ 兵型评估

目录：`index.html`（版本列表页）→ `v1.15.0.html`（主游戏）；`old/` 归档 15 个历史版本，`revised/` 存 5 个「只屑修改版」衍生版本。

## 彩蛋（egg/）

隐藏趣味页面：`touch.html`（首页名片可点入）、`start.html`、`dpsk4f.html`、`诗-语音.html`。

## 本地预览

克隆后在仓库根目录起个静态服务器即可（有 Python 就行）：

```bash
python -m http.server 8000
```

然后打开 <http://localhost:8000/blogs/index.html>

> 博客正文用 `fetch()` 拉 `.md`，**直接双击 html（file://）会被浏览器 CORS 拦截**，必须走 http 服务。

## 部署

push 到 `main` 分支即可，GitHub Pages 自动构建发布：

```bash
git add .
git commit -m "..."
git push
```

## 投稿

欢迎投稿博客文章！在仓库 **Issues** 里发一篇，把文章 Markdown 正文贴进去即可：

<https://github.com/uzjhf-836/uzjhf-836.github.io/issues/new>

人工审核后发布，正文内容原样保留（含 Markdown 语法与原生 HTML）。

## 致谢

> WHXCHESS:对[chess](#国际象棋)的开发[![Bilibili_space](https://img.shields.io/badge/<His-bilibili_space-blue?logo=bilibili)](https://space.bilibili.com/2114495614?)

> HarmonyOS Sans:网站的字体（[授权协议](/font/harmonyos-sans/LICENSE_HarmonyOS_Sans.txt)）

> Chess Alpha:国象棋子字体（仅个人非商用，[LICENSE](/font/alpha/LICENSE_Chess_Alpha.txt)）

> 我:对网站的开发