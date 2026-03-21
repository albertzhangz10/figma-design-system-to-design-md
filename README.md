# Figma design system to design.md

A Claude Code plugin that extracts your Figma design system into a structured `design.md` document.

## What it does

Automatically scans your project for design tokens (CSS variables, Tailwind config, theme files) and optionally pulls data from Figma via MCP, then generates a comprehensive design system document covering:

- **Colors** — base palette + semantic roles (text, surface, border, icon)
- **Typography** — font families, size scale, weight, line-height
- **Spacing** — spacing scale with token names and values
- **Border Radius** — radius scale
- **Border Width** — width scale
- **Elevation** — shadow definitions
- **Responsive** — breakpoints or fluid responsive approach
- **Components** — component variants (when Figma MCP is available)

## Install

```bash
claude /plugin install https://github.com/albertzhangz10/figma-design-system-to-design-md
```

Or test locally:

```bash
claude --plugin-dir /path/to/figma-design-system-to-design-md
```

## Usage

```
/figma-design-system-to-design-md                    # generates ./design.md
/figma-design-system-to-design-md ./docs/design.md   # custom output path
```

## Requirements

- Claude Code
- A project with design tokens (CSS variables, Tailwind config, theme files, etc.)
- (Optional) Figma MCP server enabled for enhanced data extraction

## License

MIT
