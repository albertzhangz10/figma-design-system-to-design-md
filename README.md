# Figma design system to design.md

### Turn your Figma design tokens into a structured, readable design system document

**[🇨🇳 中文](README.zh-CN.md)** | **🇺🇸 English**

<p>
  <img src="https://img.shields.io/badge/Claude_Code-black?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code">
  <img src="https://img.shields.io/badge/Figma_MCP-F24E1E?style=flat-square&logo=figma&logoColor=white" alt="Figma MCP">
  <img src="https://img.shields.io/github/license/albertzhangz10/figma-design-system-to-design-md?style=flat-square&color=green" alt="MIT License">
</p>

> Stop manually documenting your design system. Let AI read your tokens and generate the docs for you.

**[Try it now on the web — no setup required](https://figmadesignmd.com/)**

A Claude Code skill plugin that automatically extracts design tokens from your project (CSS variables, Tailwind config, theme files) and optionally enriches them via Figma MCP, then generates a comprehensive `design.md` covering your entire design system. Non-technical users (designers, PMs) can use the [web version](https://figmadesignmd.com/) directly — just paste a Figma URL and get your design.md.

## The Problem

Design systems live in Figma and code, but the documentation lives nowhere — or worse, in an outdated wiki page. Every time tokens change, the docs fall behind. Developers guess at color roles, spacing scales, and typography levels.

## The Solution

One command. One document. Always in sync with your actual token files.

```
/figma-design-system-to-design-md
```

## What It Generates

| Section | Source | Auto |
|---------|--------|:----:|
| **Colors** — base palette + semantic roles (text, surface, border, icon) | Token files | ✅ |
| **Typography** — font families, size scale, weight, line-height | Token files | ✅ |
| **Spacing** — spacing scale with token names and values | Token files | ✅ |
| **Border Radius** — radius scale | Token files | ✅ |
| **Border Width** — width scale | Token files | ✅ |
| **Elevation** — shadow definitions | Tailwind / Figma MCP | ✅ |
| **Responsive** — minimum width, fluid vs breakpoint approach | Config files | ✅ |
| **Components** — variants, sizing, states | Figma MCP | 🔶 Optional |
| **Overview** — design intent and personality | — | ✍️ Manual |
| **Do's and Don'ts** — judgment rules | — | ✍️ Manual |

## How It Works

```
Step 1  Detect token sources (tokens.css, theme.ts, tailwind.config.js, etc.)
Step 2  Parse and classify all tokens into categories
Step 3  (Optional) Pull additional data from Figma MCP
Step 4  Generate structured design.md
Step 5  Confirm with user before saving
```

### Token Source Detection

The skill automatically searches for:

| Type | Patterns |
|------|----------|
| CSS tokens | `**/tokens.css`, `**/variables.css`, `**/theme.css` |
| JSON/JS tokens | `**/tokens.json`, `**/tokens.ts`, `**/theme.ts` |
| Style config | `tailwind.config.*`, `**/globals.css` |

### Framework Agnostic

Works with any stack:

| Framework | Styling | Tokens |
|-----------|---------|--------|
| React / Next.js | Tailwind, styled-components, CSS Modules | CSS variables, JS objects |
| Vue / Nuxt | Tailwind, Vuetify, UnoCSS | CSS variables, JSON |
| Svelte | Tailwind, vanilla CSS | CSS variables |
| Any | Any | Any format with parseable tokens |

## Install

```bash
# Via marketplace
claude plugin install figma-design-system-to-design-md

# Via GitHub
claude /plugin install https://github.com/albertzhangz10/figma-design-system-to-design-md
```

## Usage

```
/figma-design-system-to-design-md                    # generates ./design.md
/figma-design-system-to-design-md ./docs/design.md   # custom output path
```

### Local Development

```bash
claude --plugin-dir /path/to/figma-design-system-to-design-md
```

## Requirements

| Requirement | Required | Notes |
|-------------|:--------:|-------|
| Claude Code | ✅ | Latest version recommended |
| Token files in project | ✅ | CSS variables, Tailwind config, theme files, etc. |
| Figma MCP Server | 🔶 | Optional — enables component extraction and effect styles |

### Enabling Figma MCP (Optional)

1. Open Figma desktop app (latest version)
2. Menu → Preferences → Enable Dev Mode MCP Server
3. Restart Claude Code
4. Open your design system file in Figma

## License

MIT License - see the [LICENSE](LICENSE) file for details.

## Credits

Built by [albertzhangz10](https://github.com/albertzhangz10).

Inspired by [Google Stitch design.md](https://stitch.withgoogle.com/docs/design-md/overview) — the `design.md` format and structure are based on their work.
