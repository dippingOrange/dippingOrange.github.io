# 我的网站

一个纯静态的个人网站：没有构建步骤，没有依赖，双击 `index.html` 就能看。
托管在 GitHub Pages 上，成本为零。

```
my-site/
├── index.html          首页：自我介绍 + 文章列表
├── about.html          关于页
├── 404.html            找不到页面时显示
├── .nojekyll           告诉 GitHub Pages 不要用 Jekyll 处理
├── README.md           本文件，上传时可以不带
├── assets/
│   ├── style.css       全部样式，配色改顶部变量
│   └── favicon.svg     地址栏小图标
└── posts/
    ├── _template.html      新文章模板（复制它来写文章）
    ├── why-a-website.html
    └── build-log-001.html
```

---

## 一、本地预览

直接双击 `index.html`，浏览器就打开了。

想用本地服务器（更接近线上效果，路径问题能提前暴露）：

```powershell
cd my-site
python -m http.server 8000
```

然后访问 http://localhost:8000 。

---

## 二、第一次上传到 GitHub

### 1. 建仓库

登录 GitHub → 右上角 `+` → **New repository**

| 字段 | 填什么 |
|---|---|
| Repository name | `你的用户名.github.io` ← **必须一字不差，全小写** |
| Description | 随便，可以不填 |
| Public / Private | **Public**（免费账号的 Pages 只支持公开仓库） |
| Add a README file | **不勾**（避免和本地文件冲突） |

点 **Create repository**。

> 例子：如果你的 GitHub 用户名是 `ayu`，仓库名就必须是 `ayu.github.io`。
> 名字不对的话，网址会变成 `ayu.github.io/仓库名`，多一层路径。

### 2. 上传文件

在新仓库页面上找到 **uploading an existing file** 这个链接，点进去。

把这个文件夹**里面的内容**拖进去：`index.html`、`about.html`、`404.html`、
`_template.html`、`assets/` 文件夹、`posts/` 文件夹。

⚠️ 拖的是 `my-site` 里面的东西，**不是 `my-site` 文件夹本身**。
否则网址会变成 `用户名.github.io/my-site/`。

`.nojekyll` 是隐藏文件，拖拽可能带不上。如果没上去，在仓库里
**Add file → Create new file**，文件名填 `.nojekyll`，内容留空，提交即可。

提交前在下面填一句说明，比如 `first commit`，然后点 **Commit changes**。

### 3. 打开 Pages

仓库页 → **Settings** → 左侧 **Pages**

- **Source**：`Deploy from a branch`
- **Branch**：`main`，目录选 `/ (root)`
- 点 **Save**

### 4. 等一两分钟

回到 Pages 设置页，顶部会出现一行绿色的
`Your site is live at https://你的用户名.github.io/`。

打开它。如果显示 404，先等满两分钟再刷新，多数情况是还在构建。

---

## 三、以后怎么更新

三种方式，从简到繁：

1. **网页上传**：进仓库 → `Add file` → `Upload files` → 覆盖同名文件 → 提交
2. **GitHub Desktop**：图形界面，把本地文件夹和仓库关联，之后点几下就同步
3. **命令行**（推荐，学会一次终身受用）：

```powershell
cd my-site
git init
git add .
git commit -m "更新文章"
git branch -M main
git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
git push -u origin main
```

之后每次更新只要 `git add . && git commit -m "..." && git push`。

---

## 四、写新文章

1. 在 `posts/` 目录里复制 `_template.html`，改个名字：
   `2026-11-my-post.html`
   （文件名只用小写字母、数字、连字符，不要用空格和中文）
2. 打开它，文件顶部有一段注释，列出要改的五个地方
3. 回到 `index.html`，在 `<ul class="post-list">` 里加一条，**最新的放最上面**：

```html
<li>
  <a href="posts/2026-11-my-post.html">
    <span class="title">文章标题</span>
    <time datetime="2026-11-01">2026.11.01</time>
  </a>
</li>
```

4. 提交、推送、等一分钟

---

## 五、上线前要改的地方

用编辑器的全局替换最快（VS Code：`Ctrl + Shift + H`）。

- [ ] 所有 `你的名字` —— 出现在 `index.html`、`about.html`、`404.html`、
      两篇文章、`_template.html`、各页的 `<title>`
- [ ] 所有 `you@example.com` —— 换成你的邮箱
- [ ] 所有 `yourname` / `你的用户名` —— 换成你的 GitHub 用户名
- [ ] 首页 hero 那两句话（`index.html` 里的 `.hero` 段）
- [ ] `about.html` 里的内容
- [ ] 示例文章的日期
- [ ] 删掉或改写两篇示例文章

想改站点标题，就把 `<title>` 里的文字也一起换掉。

---

## 六、换配色

打开 `assets/style.css`，最上面这一块：

```css
:root {
  --bg:      #fdfcf9;   /* 背景 */
  --fg:      #1f1f1c;   /* 正文 */
  --muted:   #74716a;   /* 次要文字 */
  --line:    #e6e2d9;   /* 分隔线 */
  --accent:  #35594e;   /* 链接、选中色 */
}
```

改这五个值，整站跟着变。深色模式有三行在下面的
`@media (prefers-color-scheme: dark)` 里，一起改会更协调。

---

## 七、常见问题

**打开是 404**
按顺序检查：仓库名是不是 `用户名.github.io`；仓库是不是 Public；
Pages 里的 Branch 是不是 `main` + `/ (root)`；是不是还没等够两分钟。

**改了文件但页面没变**
浏览器缓存。`Ctrl + Shift + R` 强制刷新，或者开无痕窗口。

**样式全没了，只剩白底黑字**
`assets/style.css` 路径不对。CSS 必须在仓库根目录的 `assets/` 里，
HTML 里写的是 `assets/style.css`（文章页写 `../assets/style.css`）。

**能用中文文件名吗**
技术上能，但网址里会出现一长串 `%E4%BD%A0...`，而且容易出问题。别用。

**图片放哪**
放 `assets/` 里，文章里用 `<img src="../assets/照片.jpg" alt="描述">`。

**别人能改我的站吗**
不能。仓库是只读的，只有你能提交。

---

## 八、什么时候买域名

建议标准：**写满 5 篇之后。**

在这之前，`用户名.github.io` 完全够用。写满了，说明这件事你能持续做，
再花那一百块买域名，绑上去只要五分钟：仓库 Settings → Pages →
Custom domain 里填域名，然后去域名商那里加两条 DNS 记录（GitHub 有文档）。

---

## 一条规矩

**先上线，再装修。**

样式调整每次不超过两小时，超时就去写文章。
网站的价值九成在内容，一成在技术。
