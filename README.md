# Mori

一个单文件的 Markdown 笔记应用：边写边排版，不需要切换「编辑 / 预览」模式。界面采用三栏布局：导航、笔记列表和编辑器；编辑时始终隐藏 Markdown 标记。

## 文件

- `index.html`：完整应用，CSS、JavaScript 全部内嵌。

## 即时渲染

- 标题、列表、待办、引用、代码块、分割线在输入时直接生效
- 粗体、斜体、删除线、行内代码、链接即时排版
- 编辑时也不显示 Markdown 标记，标题、列表、引用等始终保持排版效果
- 支持中文输入法组合输入

支持的语法：标题、无序/有序列表、待办、引用、代码块、分割线、粗体、斜体、删除线、行内代码、链接。暂不支持表格和图片；链接用 `⌘/Ctrl + 点击` 打开。

## 界面

- 左栏：所有笔记、最近编辑、收藏、最近删除，以及外观与同步设置
- 中栏：搜索和笔记列表；右键笔记可收藏、置顶、导出或移到最近删除
- 右栏：标题、即时排版正文和格式工具栏
- 浅色外观使用暖纸白与柔和鼠尾草绿，深色模式使用协调的灰绿色

## 快捷键

| 快捷键 | 作用 |
| --- | --- |
| `⌘/Ctrl + N` | 新建笔记 |
| `⌘/Ctrl + K` | 搜索 |
| `⌘/Ctrl + S` | 同步 |
| `⌘/Ctrl + B` | 粗体 |
| `⌘/Ctrl + I` | 斜体 |
| `⌘/Ctrl + ⇧ + K` | 插入链接 |
| `⌘/Ctrl + ⇧ + F` | 收藏 |
| `⌘/Ctrl + Z` / `⇧ + Z` | 撤销 / 重做 |

## 跨设备同步

Mori 直接调用 GitHub Contents API，把每篇笔记保存为私有仓库指定目录下的 `.md` 文件。正文为普通 Markdown，文件头用 HTML 注释保存少量元数据；其他 Markdown 编辑器仍可正常阅读正文。

1. 在 GitHub 创建一个**私有仓库**（例如 `yourname/private-notes`）。
2. 创建 fine-grained personal access token：Repository access 只选该私有仓库，Repository permissions → Contents 设为 `Read and write`。
3. 打开 Mori → 设置 → 填写 `owner/repo`、笔记目录和 token，点击「保存并连接」。
4. 在另一台设备打开同一个网页，输入相同仓库和 token，即可同步。

默认设置指向 `elyar-adil/paper-notes-data` 下的 `mori-notes/` 目录；建议使用你自己的私有仓库。令牌默认仅保存在当前会话。勾选「记住令牌」时，令牌会通过 Web Crypto AES-GCM 加密后存入 IndexedDB；应用不会把令牌写入源码、笔记仓库或 `localStorage`。令牌只会发送到 `api.github.com`。

应用拒绝连接公开仓库，以免误把笔记公开。每篇笔记使用独立 `.md` 文件；编辑后自动保存到本机，连接 GitHub 后会延迟合并同步。

## 部署

站点由 GitHub Actions 部署：推送到 `main` 后，`.github/workflows/pages.yml` 会把 `index.html` 作为站点根目录发布。

- 源码：`https://github.com/elyar-adil/mori`
- 页面：`https://elyar-adil.github.io/mori/`

也可以直接用浏览器打开 `index.html`，不需要安装依赖或启动服务器。

## 隐私提示

笔记在首次使用时只保存在当前浏览器。连接 GitHub 后，笔记内容会写入所配置的私有仓库，并通过 GitHub 在已授权设备之间同步。不要在不信任的设备上选择记住令牌。
