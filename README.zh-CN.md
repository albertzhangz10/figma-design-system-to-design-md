# Figma 设计系统转 design.md

### 将你的 Figma 设计令牌转化为结构化、可读的设计系统文档

**🇨🇳 中文** | **[🇺🇸 English](README.md)**

<p>
  <img src="https://img.shields.io/badge/Claude_Code-black?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code">
  <img src="https://img.shields.io/badge/Figma_MCP-F24E1E?style=flat-square&logo=figma&logoColor=white" alt="Figma MCP">
  <img src="https://img.shields.io/badge/License-All%20Rights%20Reserved-red?style=flat-square" alt="All Rights Reserved">
</p>

> 别再手动写设计系统文档了。让 AI 读取你的 token 文件，自动生成文档。

**[立即在线体验 — 无需任何配置](https://figmadesignmd.com/)**

一个 Claude Code 技能插件，自动从项目中提取设计令牌（CSS 变量、Tailwind 配置、主题文件），并可选通过 Figma MCP 补充数据，生成一份涵盖完整设计系统的 `design.md` 文档。设计师和 PM 等非技术用户可以直接使用[网页版](https://figmadesignmd.com/) — 粘贴 Figma 链接即可获得 design.md。

## 痛点

设计系统分散在 Figma 和代码中，但文档要么不存在，要么躺在过时的 wiki 页面里。每次 token 变更，文档就落后了。开发者只能猜颜色角色、间距规则和字体层级。

## 解决方案

一条命令，一份文档，始终与你的实际 token 文件同步。

```
/figma-design-system-to-design-md
```

## 生成内容

| 板块 | 数据来源 | 自动化 |
|------|---------|:------:|
| **Colors（颜色）** — 基础色板 + 语义角色（文本、背景面、边框、图标） | Token 文件 | ✅ |
| **Typography（排版）** — 字体家族、字号层级、字重、行高 | Token 文件 | ✅ |
| **Spacing（间距）** — 间距系统及 token 名称与值 | Token 文件 | ✅ |
| **Border Radius（圆角）** — 圆角系统 | Token 文件 | ✅ |
| **Border Width（边框宽度）** — 边框宽度系统 | Token 文件 | ✅ |
| **Elevation（阴影）** — 阴影定义 | Tailwind / Figma MCP | ✅ |
| **Responsive（响应式）** — 最小宽度、流式 vs 断点策略 | 配置文件 | ✅ |
| **Components（组件）** — 变体、尺寸、状态 | Figma MCP | 🔶 可选 |
| **Overview（概述）** — 设计意图与视觉个性 | — | ✍️ 手动 |
| **Do's and Don'ts（规范）** — 判断性设计规则 | — | ✍️ 手动 |

## 工作流程

```
第 1 步  检测 token 来源（tokens.css、theme.ts、tailwind.config.js 等）
第 2 步  解析并分类所有 token
第 3 步 （可选）通过 Figma MCP 拉取补充数据
第 4 步  生成结构化 design.md
第 5 步  与用户确认后保存
```

### Token 来源检测

插件会自动搜索以下文件：

| 类型 | 匹配模式 |
|------|---------|
| CSS token | `**/tokens.css`、`**/variables.css`、`**/theme.css` |
| JSON/JS token | `**/tokens.json`、`**/tokens.ts`、`**/theme.ts` |
| 样式配置 | `tailwind.config.*`、`**/globals.css` |

### 框架无关

适用于任何技术栈：

| 框架 | 样式方案 | Token 格式 |
|------|---------|-----------|
| React / Next.js | Tailwind、styled-components、CSS Modules | CSS 变量、JS 对象 |
| Vue / Nuxt | Tailwind、Vuetify、UnoCSS | CSS 变量、JSON |
| Svelte | Tailwind、原生 CSS | CSS 变量 |
| 任意 | 任意 | 任何可解析的 token 格式 |

## 安装

```bash
# 通过 marketplace
claude plugin install figma-design-system-to-design-md

# 通过 GitHub
claude /plugin install https://github.com/albertzhangz10/figma-design-system-to-design-md
```

## 使用

```
/figma-design-system-to-design-md                    # 生成 ./design.md
/figma-design-system-to-design-md ./docs/design.md   # 自定义输出路径
```

### 本地开发测试

```bash
claude --plugin-dir /path/to/figma-design-system-to-design-md
```

## 前置条件

| 条件 | 必需 | 说明 |
|------|:----:|------|
| Claude Code | ✅ | 推荐使用最新版本 |
| 项目中有 token 文件 | ✅ | CSS 变量、Tailwind 配置、主题文件等 |
| Figma MCP Server | 🔶 | 可选 — 开启后可提取组件信息和阴影样式 |

### 开启 Figma MCP（可选）

1. 打开 Figma 桌面端（确保最新版本）
2. 菜单 → Preferences → Enable Dev Mode MCP Server
3. 重启 Claude Code
4. 在 Figma 中打开设计系统文件

## 版权声明

Copyright (c) 2026 albertzhangz10. 保留所有权利。

未经版权所有者书面许可，不得以任何形式使用、复制、修改或分发本项目的任何部分。

## 致谢

由 [albertzhangz10](https://github.com/albertzhangz10) 开发。

`design.md` 的格式与结构参考了 [Google Stitch design.md](https://stitch.withgoogle.com/docs/design-md/overview)，感谢他们的工作。
