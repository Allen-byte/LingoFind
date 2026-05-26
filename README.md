<p align="center">
  <img src="./public/icon.png" alt="LingoFind Logo" width="120" style="border-radius: 24px; box-shadow: 0 8px 30px rgba(0,0,0,0.2);" />
</p>

<h3 align="center">LingoFind（语搜）</h3>

<p align="center">
  <strong>用日常说话的方式，搜索你电脑里的文件</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/React-19-blue?logo=react" />
  <img src="https://img.shields.io/badge/Tauri-v2-24C6C1?logo=tauri" />
  <img src="https://img.shields.io/badge/Rust-2021-CE412B?logo=rust" />
  <img src="https://img.shields.io/badge/AI-DeepSeek-4D6BFE" />
  <img src="https://img.shields.io/badge/TailwindCSS-v4-06B6D4?logo=tailwindcss" />
</p>

---

## 这是什么

LingoFind 是一个 Windows 桌面端的**本地文件搜索工具**。和系统自带搜索不同，你可以用**大白话**搜文件——就像跟人说话一样。比如：

- "上周下载的 PDF 文档"
- "最近三天改过的 Python 代码"
- "最大的 5 个视频"
- "桌面上的图片"

它会理解你的意思，然后帮你找到文件。不用记复杂的搜索语法。

除了搜索，它还能帮你**整理杂乱的文件夹**——自动分类、找重复文件、批量移动，做错了还能撤销。

---

## 功能

### 智能搜索

- **自然语言输入**：用日常说话的方式搜索，不用学任何语法
- **AI + 本地双引擎**：配置了 DeepSeek API Key 就用 AI 解析查询；没配置也能用内置的规则引擎，离线照样搜
- **自动识别搜索条件**：系统会自动从你的话里提取关键词、文件类型、时间范围、排序方式
- **文件预览**：搜到文件后可以在侧边栏预览图片、代码（带语法高亮）、基本文件信息
- **批量操作**：选中多个文件，一键移动或导出搜索结果（CSV / JSON）
- **搜索历史**：自动记录最近搜索过的内容

### 文件整理

- **一键扫描**：选一个文件夹，自动分析里面有什么、哪些是重复的
- **智能分类**：按图片、文档、代码、视频、压缩包等 10 个类别自动分组
- **重复检测**：找出文件名和大小都一样（文件名 + 大小双重校验，非哈希比对）的重复文件
- **批量整理**：把文件按类别移到对应文件夹，或按月份归档
- **操作可撤销**：整理之后如果觉得不满意，可以一键恢复

### 知识管理

- **文件标签**：给文件打标签，之后按标签查找
- **文件笔记**：给任意文件写备注，支持 Markdown，笔记会持久保存
- **文件空间**：按年份、月份、类型、标签浏览所有已索引的文件

### 其他

- **天气预报**：顶部栏显示城市天气（调用心知天气 API）
- **自动更新**：检测新版本并支持下载安装
- **深色界面**：半透明毛玻璃卡片风格，随时间段变换背景色调

---

## 界面

> 截图放在 `docs/images/` 目录下：
> - `search_dashboard.png` — 搜索主界面
> - `file_organizer.png` — 文件整理面板
> - `knowledge_browser.png` — 文件空间浏览
> - `file_preview.png` — 文件预览（图片 / 代码 / 标签 / 笔记）

---

## 技术栈

| 层 | 用的什么 |
|---|---|
| 桌面框架 | [Tauri v2](https://tauri.app/)（Rust 后端 + Web 前端） |
| 前端 | React 19 + TypeScript + TailwindCSS v4 |
| 状态管理 | Zustand |
| 动画 | Framer Motion |
| 3D 背景 | Three.js + React Three Fiber |
| AI | DeepSeek API（`deepseek-v4-flash`，可选） |
| 文件扫描 | Rust `walkdir` |
| 网络请求 | Rust `reqwest` |

---

## 怎么跑起来

### 你需要准备

- **Node.js** 18+
- **Rust** 最新稳定版
- **Windows**：Visual Studio 生成工具（C++ 桌面开发）

### 开发模式

```bash
# 1. 克隆项目
git clone https://github.com/yourusername/LingoFind.git
cd LingoFind

# 2. 装前端依赖
npm install

# 3. 启动（前端 + Rust 后端一起）
npm run tauri dev
```

### 打包

```bash
npm run tauri build
```

打好包的文件在 `src-tauri/target/release/bundle/` 里。

---

## 怎么用

### 搜索文件

1. 先点左侧导航栏底部的 **索引健康**，添加你要搜索的文件夹路径
2. 等扫描完成（顶部栏会显示索引进度）
3. 回到 **搜索** 页面，在搜索框里用大白话输入你想找什么
4. 结果出来后可以预览、打开、打标签、写笔记、导出

### 配置 AI（可选）

点右上角齿轮 → **AI 设置** → 填入 DeepSeek API Key。不填也能搜（仅依靠本地解析规则搜索），填了之后对复杂查询的理解会更准。

### 整理文件夹

1. 点左侧 **整理** → 选一个文件夹 → **开始扫描**
2. 看看分类结果，勾选你要操作的文件
3. 点 **移动到** 选择一个目标文件夹，或直接点 **自动整理**

---

## 索引器特性

- 自动跳过隐藏文件和系统目录（`.git`、`node_modules`、`target`、`dist`、Windows 系统目录等）
- 单个索引上限 50,000 个文件，最大深度 30 层
- 文本文件自动提取前 200 字节内容用于内容搜索
- 索引结果缓存到本地，下次启动更快

---

## 开源协议

[MIT](LICENSE)
