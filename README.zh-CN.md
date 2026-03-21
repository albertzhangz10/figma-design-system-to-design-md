# Figma 设计系统转 design.md

一个 Claude Code 插件，将你的 Figma 设计系统提取为结构化的 `design.md` 文档。

[English](./README.md)

## 功能介绍

自动扫描项目中的设计令牌文件（CSS 变量、Tailwind 配置、主题文件等），并可选通过 Figma MCP 拉取补充数据，生成涵盖以下内容的设计系统文档：

- **Colors（颜色）** — 基础色板 + 语义角色（文本、背景面、边框、图标）
- **Typography（排版）** — 字体家族、字号层级、字重、行高
- **Spacing（间距）** — 间距系统及 token 名称与值
- **Border Radius（圆角）** — 圆角系统
- **Border Width（边框宽度）** — 边框宽度系统
- **Elevation（阴影）** — 阴影定义
- **Responsive（响应式）** — 断点或无极响应式策略
- **Components（组件）** — 组件变体、尺寸、状态（需 Figma MCP）

## 安装

```bash
claude /plugin install https://github.com/albertzhangz10/figma-design-system-to-design-md
```

本地测试：

```bash
claude --plugin-dir /path/to/figma-design-system-to-design-md
```

## 使用方式

```
/figma-design-system-to-design-md                    # 生成 ./design.md
/figma-design-system-to-design-md ./docs/design.md   # 自定义输出路径
```

## 前置条件

- Claude Code
- 项目中有设计令牌文件（CSS 变量、Tailwind 配置、主题文件等）
- （可选）开启 Figma MCP Server 以获取更丰富的设计数据

## 开源协议

MIT
