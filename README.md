# HNUSEC

湖南大学信息安全协会官网与学习指南。

Fiat Lux · Just for Fun✨

这份指南始于 2025 CTF 新生杯。我们希望把同学们的学习资源、实践经验和大学生活建议留下来，让刚来到湖南大学网络空间安全学院的同学更容易找到方向，也找到一起学习的人。

## 从这里开始

- **访问官网**：[www.hnusec.net](https://www.hnusec.net/)
- **阅读新生指南**：[Hello, World!](https://www.hnusec.net/docs/guide)
- **关注 GitHub 组织**：[HNUinfoSec](https://github.com/HNUinfoSec)，点击 Follow 关注我们，浏览组织中的项目。
- **加入交流群**：[2026HNUSec迎新群](https://qm.qq.com/q/JSaru1wB4C)，QQ群号 **571381107**。

欢迎提交学习笔记、题解和经验分享，也欢迎帮忙修正错别字、补充资料或反馈失效链接。你可以通过 [Issue](https://github.com/HNUinfoSec/HNUSEC/issues) 提出建议，或提交 Pull Request 参与完善。

## 本地运行

项目使用 Next.js、React、Fumadocs 和 Tailwind CSS。需要 Node.js 20.9 或更高版本，以及 npm。

```bash
git clone https://github.com/HNUinfoSec/HNUSEC.git
cd HNUSEC
npm ci
npm run dev
```

访问 <http://localhost:3000> 查看效果。

常用命令：

| 命令 | 用途 |
| --- | --- |
| `npm run dev` | 启动开发服务器，预览修改 |
| `npm run build` | 构建网站，检查编译和类型错误 |
| `npm run start` | 启动已构建的网站，需要先完成 build |
| `npm run lint:md` | 检查 Markdown 文件格式 |
| `npm run fix:md` | 自动修复 Markdown 文件的部分格式问题 |

注意：现有 `lint:md` 和 `fix:md` 脚本只处理 `.md` 文件。检查 MDX 文章时，请运行 `npx markdownlint "content/docs/**/*.mdx"`。

## 项目结构

```text
app/                  页面与布局
  (home)/             官网首页
  docs/               文档页面
  friends/            友链页面
content/docs/         MDX 文章与目录配置
config/friends.ts     友链资料
lib/layout.shared.tsx 公共导航配置
public/               图片等静态资源
```

## 如何提交文章

### 1. 准备贡献分支

先在 GitHub 上 **Fork 本仓库**。将下方的 `YOUR_USERNAME` 替换为你的 GitHub 用户名，clone 自己的 Fork 并创建分支：

```bash
git clone https://github.com/YOUR_USERNAME/HNUSEC.git
cd HNUSEC
git switch -c docs/your-topic
```

安装依赖与启动预览的方式见上方“本地运行”。如果前面已 clone 官方仓库，请在另一个目录中 clone 你的 Fork。

### 2. 编写文章

所有文章位于 `content/docs/`，使用 MDX 格式，支持 Markdown 与 React 组件。

在对应分类下新建文件，例如 `content/docs/security/your-article.mdx`，并在顶部添加 frontmatter：

```yaml
---
title: 文章标题
description: 用一句话介绍这篇文章
---
```

- 新文件与目录名使用小写字母和连字符，如 `your-article.mdx`。
- 图片放在 `public/` 下，在文章中通过站点路径引用。例如 `public/images/example.png` 对应 `/images/example.png`。
- 代码块注明语言；工具教程说明适用版本、环境与必要的前置知识。
- 引用资料附上来源；分享个人经验时，可以注明作者与写作时间。

### 3. 更新目录

在文章所在目录的 `meta.json` 中，将文件名（不含 `.mdx`）加入 `pages` 数组。数组顺序就是导航顺序，保留已有条目。

例如，在 `content/docs/security/` 中添加文章后，对应配置为：

```json
{
  "title": "Security",
  "pages": ["index", "llm-security", "start", "your-article"]
}
```

如果新增分类，请创建该分类的 `index.mdx` 和 `meta.json`，并将分类目录名加入上一级 `meta.json` 的 `pages` 数组。顶层分类在 `content/docs/meta.json` 中配置。

### 4. 预览并提交 Pull Request

在本地打开文章，确认目录、链接、图片和代码块显示正常，并运行：

```bash
npx markdownlint "content/docs/**/*.mdx"
npm run build
```

用 `git diff` 检查改动后，暂存并提交本次修改的文件，将贡献分支推送到自己的 Fork。以下以仅修改文章和目录配置为例；如果新增图片，也请暂存对应的 `public/` 文件。

```bash
git add content/docs/
git commit -m "docs: 添加文章标题"
git push -u origin docs/your-topic
```

然后在 GitHub 上向 `HNUinfoSec/HNUSEC` 的 `main` 分支发起 Pull Request，简要说明文章主题或修改内容。

## 如何添加友链

在 `config/friends.ts` 的 `friends` 数组中追加个人资料，例如：

```typescript
{
  name: "博客名称",
  url: "https://your-blog.com",
  avatar: "/avatars/your-avatar.jpg",
  description: "你的个性签名",
  grade: "2025级",
  tags: ["Web", "AI", "Security"],
  bio: "简短的个人介绍"
}
```

- `name`、`url`、`description`、`grade` 和 `tags` 为必填项；`avatar` 和 `bio` 可省略。
- 不提供 `avatar` 时，页面会使用名字的首个字符作为头像。
- 头像放在 `public/avatars/` 下，建议使用方形图片，页面显示尺寸为 48 × 48 像素。
- 如果列表中没有你的年级，需要同时更新 `config/friends.ts` 的 `Friend.grade` 类型和 `app/friends/page.tsx` 的 `grades` 数组。
- 技术方向不限于安全，欢迎分享你感兴趣的领域。

在本地访问 <http://localhost:3000/friends> 检查展示与年级筛选，确认博客链接有效后，提交包含资料与头像修改的 Pull Request。
