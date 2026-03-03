# AI 模型效果横比 - Mermaid 转 PNG 工具实现

> 同一 Prompt 下，6 大 AI 模型的代码生成能力对比实验

## 项目概述

本项目是一个 **AI 模型代码生成能力横向对比实验**，通过向 6 个主流 AI 模型提供**完全相同的 Prompt**，要求它们生成一个 **Shadcn/ui 风格的单 HTML 页面**，实现将 Mermaid 架构图转换为可导出 PNG 的功能。

### 实验目标

- **评估不同模型的代码生成质量**
- **对比技术方案选择的差异**
- **分析实现思路和工程能力**
- **为技术选型提供参考依据**

## 目录结构

```
Mermaid2Html2PNG/
├── index.html                 # 对比展示主页（Shadcn/ui 风格）
├── Prompt.txt                 # 原始提示词
├── README.md                  # 本文件
│
├── Claude Sonnet 4.6.html     # Claude 实现
├── Claude Sonnet 4.6.png      # Claude 实现预览
│
├── Kimi K2.5.html            # Kimi 实现
├── KIMI k2.5.png             # Kimi 实现预览
│
├── DeepSeek R1.html          # DeepSeek 实现
├── DeepSeek R1.png           # DeepSeek 实现预览
│
├── Gemini 3.1 Pro.html       # Gemini 实现
├── Gemini 3.1 Pro.png        # Gemini 实现预览
│
├── GPT Free.html             # GPT 实现
├── GPT Free.png              # GPT 实现预览
│
├── GLM-5.html                # GLM-5 实现
└── GLM-5.png                 # GLM-5 实现预览
```

## 原始 Prompt

<details>
<summary>点击展开完整 Prompt（所有模型使用相同的输入）</summary>

```
请你根据下方提供的Mermaid书写的系统架构图的代码帮我构建一个包含一份适合放入软著的shadcn/ui的简约风格的软件架构图的单HTML页面该页面支持导出这张软件架构图。

以下是供参考的Mermaid代码:

flowchart TB
    %% 前端层
    subgraph "前端层 - 微信小程序/uni-app"
        direction TB
        UI["交互展示模块<br/>政策检索、智能问答、分类展示"]
        FE_LOGIC["业务逻辑模块<br/>检索解析、问答管理、操作记录"]
        API_ADAPT["接口适配模块<br/>请求封装、响应解析、异常处理"]
    end

    %% 后端层
    subgraph "后端层 - Supabase BaaS"
        direction TB
        DATA_SERVICE["数据服务模块<br/>政策数据、用户数据、日志管理"]
        BE_LOGIC["业务逻辑模块<br/>政策管理、智能检索、智能问答"]
        PERM_CTRL["权限管控模块<br/>RBAC用户权限管理"]
    end

    %% 外部服务层
    subgraph "外部服务层"
        direction TB
        GOV_DATA["政务数据服务<br/>政策数据同步、增量更新"]
        AI_SERVICE["AI智能服务<br/>语义解析、自然语言理解与生成"]
    end

    %% 前后端连接
    UI --> FE_LOGIC
    UI --> API_ADAPT
    FE_LOGIC --> API_ADAPT
    API_ADAPT --> BE_LOGIC
    BE_LOGIC --> DATA_SERVICE
    BE_LOGIC --> PERM_CTRL

    %% 后端与外部服务连接
    BE_LOGIC --> GOV_DATA
    BE_LOGIC --> AI_SERVICE

    %% 配色
    style UI fill:#e1f5fe
    style FE_LOGIC fill:#e1f5fe
    style API_ADAPT fill:#e1f5fe
    style DATA_SERVICE fill:#e8f5e8
    style BE_LOGIC fill:#e8f5e8
    style PERM_CTRL fill:#e8f5e8
    style GOV_DATA fill:#fff3e0
    style AI_SERVICE fill:#fff3e0
```

</details>

## 模型实现对比总览

| 模型 | 技术方案 | 渲染方式 | 导出格式 | UI 框架 | 特点 |
|------|---------|---------|---------|---------|------|
| **Claude Sonnet 4.6** | 纯 CSS/HTML | 手工绘制 | PNG | 原生 CSS | 精致分层设计，视觉效果最佳 |
| **Kimi K2.5** | Mermaid | 库渲染 | PNG + SVG | Tailwind CSS | 功能完整，文档详尽 |
| **DeepSeek R1** | Mermaid | 库渲染 | PNG | 原生 CSS | 圆润阴影，Shadcn 风格按钮 |
| **Gemini 3.1 Pro** | Mermaid | 库渲染 | PNG + SVG | Tailwind CSS | classDef 样式定义 |
| **GPT Free** | 纯 SVG | 原生绘制 | PNG + SVG | Tailwind CSS | 矢量精度最高 |
| **GLM-5** | 纯 CSS/HTML | 手工绘制 | PNG | Tailwind CSS | 图标丰富，Tailwind风格 |

## 快速开始

### 方式一：直接打开（推荐）

1. 双击 `index.html` 查看对比展示页面
2. 点击任意模型卡片的 **"查看实现"** 按钮跳转

### 方式二：本地服务器

```bash
# 进入项目目录
cd Mermaid2Html2PNG

# 启动本地服务器（Python）
python -m http.server 8080

# 或 Node.js
npx serve .

# 浏览器访问
open http://localhost:8080/index.html
```

### 方式三：单独查看模型实现

直接打开任意 HTML 文件：

```bash
open "Claude Sonnet 4.6.html"
open "Kimi K2.5.html"
open "DeepSeek R1.html"
open "Gemini 3.1 Pro.html"
open "GPT Free.html"
open "GLM-5.html"
```

## 功能特性

### 1. 对比展示主页 (index.html)

- **模型卡片展示**：6 个 AI 模型的实现效果对比，包含预览图、技术标签、功能描述
- **Prompt 一键复制**：展开原始 Prompt 区域后，点击"复制"按钮即可复制完整 Prompt 到剪贴板
- **技术方案深度分析**：统计图表展示渲染方案分布、导出能力、UI 框架选择等
- **快速跳转**：点击任意卡片的"查看实现"按钮，直接进入对应模型页面
- **Shadcn/ui 风格**：统一使用 Tailwind CSS + Inter 字体，简约现代的设计语言

### 2. 各模型实现页面

每个模型页面均包含：
- **返回首页按钮**：固定在右上角，支持一键返回对比主页
- **导出功能**：支持 PNG 导出，部分模型支持 SVG 导出
- **架构图渲染**：Mermaid 渲染或原生绘制的高清架构图
- **响应式设计**：适配桌面端和移动端浏览

## 技术方案统计

### 渲染方案分布

| 方案类型 | 模型数量 | 具体模型 |
|---------|---------|---------|
| Mermaid 库渲染 | 3 | Kimi, DeepSeek, Gemini |
| 纯 CSS/HTML | 2 | Claude, GLM-5 |
| 纯 SVG 绘制 | 1 | GPT |

### 导出格式支持

| 导出格式 | 支持数量 | 模型 |
|---------|---------|------|
| PNG 单格式 | 3 | Claude, DeepSeek, GLM-5 |
| PNG + SVG 双格式 | 3 | Kimi, Gemini, GPT |

### UI 框架选择

| 框架 | 使用数量 | 模型 |
|-----|---------|------|
| Tailwind CSS | 4 | Kimi, Gemini, GPT, GLM-5 |
| 原生 CSS | 2 | Claude, DeepSeek |

## 技术栈汇总

### 使用的库

- [Tailwind CSS](https://tailwindcss.com/) - 原子化 CSS 框架
- [Mermaid](https://mermaid.js.org/) - 流程图渲染库
- [html2canvas](https://html2canvas.hertzen.com/) - DOM 转 Canvas
- [Inter Font](https://rsms.me/inter/) - 字体
- [Lucide Icons](https://lucide.dev/) - 图标库

### 核心技术

- HTML5
- CSS3 (Flexbox, Grid, Custom Properties)
- JavaScript (ES6+)
- SVG

## 使用建议

### 对于软著文档

所有实现均满足软著文档要求：
- ✅ 清晰的架构图展示
- ✅ 可导出高清 PNG
- ✅ 专业的视觉风格
- ✅ 完整的技术说明

### 推荐选择

1. **首选 Claude**：视觉效果最专业，适合正式文档
2. **次选 Kimi**：功能最完整，适合需要 SVG 的场景
3. **备选 DeepSeek**：风格还原度最高，Shadcn 生态友好

## 许可证

本项目用于 AI 模型能力对比研究，各模型生成的代码归相应模型服务提供商所有。

## 致谢

感谢以下 AI 模型参与本次对比实验：

- **Claude Sonnet 4.6** by Anthropic
- **Kimi K2.5** by Moonshot AI
- **DeepSeek R1** by DeepSeek
- **Gemini 3.1 Pro** by Google
- **GPT Free** by OpenAI
- **GLM-5** by 智谱AI

---

> 💡 **提示**：打开 `index.html` 即可查看完整的模型对比展示页面！点击"查看原始 Prompt"可展开并一键复制完整 Prompt，点击任意模型卡片可查看详细实现，各模型页面右上角均有"返回首页"按钮。
