# Linux.sb 表情包工具 Pro

一个为 [Linux.sb](https://linux.sb/) 论坛设计的油猴脚本（Tampermonkey），在编辑器表情按钮左侧添加自定义表情包图库，支持多分组管理、帖子图片快速收藏，提升回帖时的表情包使用体验。

## ✨ 功能特性

- **自定义表情包图库**：在编辑器工具栏添加独立按钮，点击后弹出面板，可插入 Markdown 图片语法。
- **双分组管理**：默认提供“我的”和“sbmeme”两个分组，可自由切换，方便分类存储。
- **图片添加与删除**：通过面板内的添加按钮输入图片 URL 和描述，或直接从帖子内容中悬浮图片点击“+”快速收藏。
- **帖子图片收集**：鼠标悬浮在帖子内容中的图片上，左上角显示“+”按钮，一键添加到“我的”分组。
- **插入到光标位置**：点击面板中的缩略图，自动在编辑器光标处插入 `![描述](URL)`，支持 CodeMirror / 隐藏 div / textarea 多种编辑器模式。
- **本地持久化**：使用 `localStorage` 保存各分组数据，刷新页面不丢失。
- **顶部提示**：所有操作反馈均为顶部居中半透明提示，自动消失，不打断操作。
- **主题适配**：面板背景使用 `var(--panel)`，文字颜色使用 `var(--text)`，自动适配论坛主题。
- **自动更新**：通过油猴的更新机制，脚本可自动检查并提示新版本。

## 📦 安装

1. 安装 [Tampermonkey](https://www.tampermonkey.net/) 或 [Violentmonkey](https://violentmonkey.github.io/) 浏览器扩展。
2. 点击扩展图标，选择“创建新脚本”。
3. 将本仓库中的 `script.user.js` 文件内容完整复制粘贴到编辑器中。
4. 保存脚本（`Ctrl + S`）。
5. 打开 [Linux.sb](https://linux.sb/) 任意帖子页面，在回复框的编辑器工具栏中即可看到新增的表情包按钮。

## 🚀 使用方法

### 打开表情包面板

点击编辑器工具栏中表情按钮左侧的图片图标按钮（红色边框图标），即可展开表情包面板。

### 切换分组

面板顶部分组标签栏显示“我的”和“sbmeme”，点击标签即可切换当前分组。

### 添加表情包

1. 点击面板左上角的红色加号按钮，展开添加区域。
2. 输入图片 URL（必填）和描述（可选）。
3. 点击“确认添加”按钮，图片即保存到当前分组。

### 删除表情包

鼠标悬浮在面板中的缩略图上，右上角出现“✕”按钮，点击即可删除该图片。

### 插入表情包到编辑器

点击任意缩略图，脚本会在编辑器光标位置插入对应的 Markdown 图片代码。

### 从帖子中收集图片

在帖子正文中，将鼠标悬浮在任意图片上，左上角会出现“+”按钮，点击即可将图片添加到“我的”分组（若已存在则提示重复）。

## ⚙️ 分组配置

脚本顶部的 `GROUP_NAMES`、`GROUP_LABELS`、`GROUP_STORAGE_KEYS` 和 `DEFAULT_IMAGES` 定义了分组信息。如需添加新分组，只需修改这四个常量：

```javascript
const GROUP_NAMES = ['my', 'sbmeme'];                     // 分组标识列表
const GROUP_LABELS = { my: '我的', sbmeme: 'sbmeme' };    // 分组显示名称
const GROUP_STORAGE_KEYS = {
    my: 'nb-emoji-my-v1',
    sbmeme: 'nb-emoji-sbmeme-v1',
};
const DEFAULT_IMAGES = {
    my: [
        { url: 'https://example.com/default.webp', desc: '默认图片' },
    ],
    sbmeme: [
        // 添加默认图片
    ],
};
```
例如添加新分组 `meme2`：

1. 在 `GROUP_NAMES` 数组中添加 `'meme2'`。
2. 在 `GROUP_LABELS` 中添加 `meme2: '新分组'`。
3. 在 `GROUP_STORAGE_KEYS` 中添加 `meme2: 'nb-emoji-meme2-v1'`。
4. 在 `DEFAULT_IMAGES` 中添加 `meme2: []`（或默认图片数组）。

保存脚本后刷新页面，新分组会自动出现在标签栏中。

## 🗄️ 数据存储

- 每个分组使用独立的 `localStorage` 键存储图片数组。
- 存储格式为 JSON：`[{ "url": "图片URL", "desc": "图片描述" }, ...]`
- 可通过浏览器开发者工具手动编辑或导出数据。

## 🔄 自动更新

本脚本使用油猴（Tampermonkey）的自动更新功能。在脚本头部配置了 `@updateURL` 和 `@downloadURL`，指向本仓库的 `script.user.js` 文件。当仓库中的脚本版本号高于当前安装版本时，油猴会提示用户更新。

### 更新检查设置

- Tampermonkey 默认每天检查一次更新。
- 用户可手动点击扩展菜单中的“检查更新”立即触发。
- 若用户禁用了自动更新，需要手动更新。

## ❓ 常见问题

### 点击表情包按钮没有反应？

- 请确保脚本已启用，且页面匹配 `https://linux.sb/*`。
- 检查控制台是否有报错，可能是编辑器结构变更导致插入失败，但不影响面板打开。

### 插入的图片没有出现在编辑器中？

- 脚本会优先尝试 CodeMirror 实例，其次隐藏 div，最后回退到 `textarea[name="body"]`。如果仍无效，请在本仓库提交 Issue，并提供控制台错误信息。

### 为什么分栏切换后表情包面板会关闭？

- 这是脚本早期版本的问题，已在 v1.1 中修复，请确保使用最新版本。

### 如何备份我的表情包数据？

- 可在浏览器开发者工具 Console 中执行 `localStorage.getItem('nb-emoji-my-v1')` 获取 JSON 数据并保存。

## 🛠️ 技术细节

- **脚本版本**：v1.1
- **兼容浏览器**：Chrome / Edge / Firefox（需支持 ES6+）
- **依赖**：无外部库，纯原生 JavaScript
- **编辑器兼容**：EasyMDE / CodeMirror / 原生 textarea / 隐藏 div

## 🤝 贡献

欢迎提交 Issue 或 Pull Request 来改进这个项目。如果你有新的功能想法或发现问题，请随时在 GitHub 仓库中提出。

## 📄 许可证

本项目采用 MIT 许可证，详情请参阅 [LICENSE](LICENSE) 文件。

---

**作者**：雪王  
**仓库地址**：https://github.com/你的用户名/linux-sb-emoji-tool