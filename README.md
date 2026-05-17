# Dong's Reading

一个个人读书博客的源代码。技术栈是 **Hugo + [Stack 主题](https://stack.jimmycai.com/)**，部署在 **GitHub Pages**。

每本书写四份东西：**拆书稿**（讲给朋友）、**书摘**、**书评**、**读后感**。拆书稿是首页可见的主帖，其它三份挂在它底部、有自己的 URL、但不在首页列表里出现。

---

## 一次性环境配置

只需要在第一次跑这个项目时做一遍。

### 1. 装 Hugo（macOS）

```bash
brew install hugo
hugo version   # 需要 0.139.0 以上
```

其它系统：见 [Hugo 安装文档](https://gohugo.io/installation/)。

### 2. 把项目推上 GitHub

```bash
# 在 reading-notes/ 目录下
git init
git add .
git commit -m "init: Dong's Reading"
git branch -M main
git remote add origin https://github.com/<你的用户名>/reading-notes.git
git push -u origin main
```

### 3. 安装 Stack 主题（作为 git submodule）

```bash
git submodule add https://github.com/CaiJimmy/hugo-theme-stack.git themes/hugo-theme-stack
git commit -m "add: hugo-theme-stack as submodule"
git push
```

### 4. 在 GitHub 仓库里打开 Pages

GitHub 仓库 → **Settings** → **Pages** → **Build and deployment** → **Source** 选 **GitHub Actions**。

下一次 push 之后，仓库上的 Actions 会自动跑 `.github/workflows/hugo.yml`，把站点构建并部署到：

> `https://<你的用户名>.github.io/reading-notes/`

### 5. 把 `hugo.toml` 里的 baseURL 改一下

```toml
baseURL = "https://<你的用户名>.github.io/reading-notes/"
```

---

## 日常工作流

### 本地预览

```bash
hugo server -D       # -D 表示带上 draft 草稿一起预览
```

打开 <http://localhost:1313/> 看效果，改文件会热刷新。

### 加一本新书

每本书需要 4 个文件，分别对应"拆书稿 / 书摘 / 书评 / 读后感"。我准备了 4 套 archetype 一键生成。

把 `<slug>` 换成英文/拼音的短串（不要中文、不要空格），比如 `tan-lan-de-duo-ba-an` 或 `pyramid-principle`：

```bash
# 主帖：拆书稿（会出现在首页）
hugo new --kind book post/<slug>/index.md

# 三份附录（自动设 hidden: true，不会出现在首页）
hugo new --kind book-quotes     post/<slug>-quotes/index.md
hugo new --kind book-review     post/<slug>-review/index.md
hugo new --kind book-reflection post/<slug>-reflection/index.md
```

然后：

1. 改 `post/<slug>/index.md` 顶部 frontmatter（书名、分类、标签、评分、作者）。
2. 在 `post/<slug>/` 文件夹里放一个 `cover.svg` 当封面——可以照搬《贪婪的多巴胺》那张，改改颜色和文字就行。
3. 把每个文件 frontmatter 里的 `draft: true` 改成 `false` 才会发布。

#### 关于分类色（建议保持一致）

| 分类 | 色号（Stack 600 色） | 用途 |
|---|---|---|
| 心理学 / 神经科学 | `#534AB7`（紫） | |
| 商业 / 经管 | `#0F6E56`（绿） | |
| 小说 / 文学 | `#993C1D`（橘） | |
| 历史 / 社会 | `#185FA5`（蓝） | |
| 科技 / 工具 | `#888780`（灰） | |
| 自助 / 成长 | `#D4537E`（粉） | |

新书的 `cover.svg` 沿用同分类的色号，整个博客视觉会很统一。

### 发布

```bash
git add .
git commit -m "add: <书名>"
git push
```

GitHub Actions 会自动重新构建并部署，1~2 分钟后在线生效。

---

## 目录结构

```
reading-notes/
├── README.md                      你正在看的这份
├── hugo.toml                      Hugo 配置 + Stack 主题参数
├── .gitignore
├── .github/workflows/hugo.yml     GitHub Pages 部署流水线
├── archetypes/                    新书脚手架模板
│   ├── default.md
│   ├── book.md                    主帖（拆书稿）
│   ├── book-quotes.md             附录·书摘
│   ├── book-review.md             附录·书评
│   └── book-reflection.md         附录·读后感
├── content/
│   ├── _index.md                  首页元数据
│   ├── about/index.md             关于页
│   └── post/
│       ├── tan-lan-de-duo-ba-an/             ← 主帖（首页可见）
│       │   ├── index.md
│       │   └── cover.svg
│       ├── tan-lan-de-duo-ba-an-quotes/      ← 附录（hidden）
│       ├── tan-lan-de-duo-ba-an-review/      ← 附录（hidden）
│       └── tan-lan-de-duo-ba-an-reflection/  ← 附录（hidden）
└── themes/
    └── hugo-theme-stack/          submodule 安装后才会有
```

## 维护备忘

- Stack 主题升级：`cd themes/hugo-theme-stack && git pull origin master && cd ../.. && git commit -am "chore: update stack theme"`
- Hugo 升级：`brew upgrade hugo`，同步把 `.github/workflows/hugo.yml` 里的 `HUGO_VERSION` 改成新版本。
- 想换主题：用 Hugo + `theme = "..."` 这一行的好处是任何 [Hugo 主题](https://themes.gohugo.io/)都能换上去，content 不动。

## License

内容（content/ 下的 .md 文件）：[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)。
代码（hugo.toml、workflow、archetypes 等）：MIT。
