---
name: figma-design-system-to-design-md
description: "Figma design system to design.md — Extract Figma design system into a structured design.md. Use when user says 'generate design.md', 'extract design system', 'design tokens to markdown', 'create design doc from Figma', 'Figma design system to design.md', or wants to document their design system from token files and Figma."
user-invocable: true
allowed-tools: Read, Grep, Glob, Bash, Write, Edit, mcp__Figma__get_variable_defs, mcp__Figma__get_metadata, mcp__Figma__get_screenshot, mcp__Figma__get_design_context
argument-hint: "[output-path]"
---

# Figma Design System → design.md

> **Web version available:** Non-technical users (designers, PMs) can use the web app at [figmadesignmd.com](https://figmadesignmd.com) — paste a Figma URL and get a design.md without any CLI setup.

You are a design system extraction agent. Your job is to analyze the user's project and generate a comprehensive `design.md` document that describes their design system.

## Workflow

Follow these steps **in order**. Do not skip steps.

### Step 1: Determine Output Path

- If the user provided `$ARGUMENTS`, use that as the output file path
- Otherwise, ask the user where to save `design.md` (default: project root `./design.md`)

### Step 2: Detect Token Sources

Search the project for design token files. Check these common patterns **in parallel**:

**CSS token files:**
- `**/tokens.css`
- `**/design-tokens.css`
- `**/variables.css`
- `**/theme.css`

**JSON/JS token files:**
- `**/tokens.json`
- `**/design-tokens.json`
- `**/tokens.js`, `**/tokens.ts`
- `**/theme.js`, `**/theme.ts`

**Style config files:**
- `tailwind.config.*`
- `styled-components` theme files
- `**/globals.css`

Read all detected files. These are your primary data sources.

### Step 3: Try Figma MCP (Optional Enhancement)

Attempt to connect to Figma MCP for additional data:

1. Call `mcp__Figma__get_variable_defs` to pull Figma variables
2. Call `mcp__Figma__get_metadata` to understand file structure
3. If Figma MCP is not available or returns an error, **proceed without it** — token files from Step 2 are sufficient

**Important:** Figma MCP is an enhancement, not a requirement. The skill must work without it.

### Step 4: Analyze and Classify Tokens

Parse all collected data and classify into these categories:

1. **Colors** — base palette + semantic roles (text, surface, border, icon)
2. **Typography** — font families, size scale, weight, line-height, letter-spacing
3. **Spacing** — spacing scale with token names and values
4. **Border Radius** — radius scale
5. **Border Width** — width scale
6. **Elevation** — shadow definitions (from CSS, Tailwind config, or Figma)
7. **Responsive** — breakpoints or fluid responsive approach, minimum supported width
8. **Components** — component variants, sizing, states (if available from Figma)

### Step 5: Generate design.md

Generate the document using the template below. Rules:

- **Only include sections where data was found.** Do not generate empty sections.
- Mark sections with insufficient data as `> TODO: [what needs to be added]`
- Use the exact CSS variable names / token names from the source files
- Include hex values for all colors, with a usage description for each
- For semantic color tokens that reference other tokens, show both the token reference AND the resolved hex value
- Use **bullet lists** (not tables) for all token listings — this matches the Stitch DESIGN.md format
- All section headers must use `##`
- Add a metadata header showing data sources

### Step 6: Confirm with User

Before writing the file, show the user:
1. A summary of what was detected (number of colors, typography levels, spacing values, etc.)
2. Which sections will be included vs marked as TODO
3. Ask if they want to adjust anything before saving

Then write the file to the output path.

---

## design.md Template

```markdown
# [Project Name] Design System

> Data sources: [list files that were read, e.g. `feats/tokens.css`, `tailwind.config.js`]
> Generated: [date]

## Overview

[2–3 sentence summary: visual character, intended product, key design conventions. E.g. "A focused, minimal dark interface for a developer productivity tool. Clean lines, low visual noise, high information density."]

## Colors

- **Primary** (#hex): CTAs, active states, key interactive elements
- **Secondary** (#hex): Supporting UI, chips, secondary actions
- **Surface** (#hex): Page backgrounds
- **On-surface** (#hex): Primary text on dark backgrounds
- **Error** (#hex): Validation errors, destructive actions
[repeat for each color token — include hex value and usage role]

## Typography

- **Headline Font**: [Font family], semi-bold
- **Body Font**: [Font family], regular, 14–16px
- **Label Font**: [Font family], medium, 12px, uppercase for section headers

[Add a brief note on the relationship between headline and body fonts, and any rules about weight usage.]

## Elevation

[Describe how depth is conveyed. Either:]
- Flat: "This design uses no shadows. Depth is conveyed through border contrast and surface color variation (surface, surface-container, surface-bright)."
- Shadows: list each level as `- **Shadow-sm**: 0 1px 2px rgba(0,0,0,0.05)`

[If elevation is used, specify the shadow properties (spread, blur, color) and which components should be elevated.]

## Spacing

- **space-1**: `4px`
- **space-2**: `8px`
- **space-4**: `16px`
[repeat for each spacing token, sorted by value ascending]

## Border Radius

- **radius-sm**: `4px`
- **radius-md**: `8px`
- **radius-pill**: `100px`
[repeat for each radius token]

## Border Width

- **border-thin**: `1px`
- **border-thick**: `2px`
[repeat for each border width token]

## Responsive

- **Minimum supported width**: [value]px
- **Approach**: [fluid / breakpoint-based / hybrid]
- [additional breakpoint values if applicable]

## Components

- **Buttons**: Rounded ([radius]), primary uses brand blue fill, secondary uses outline variant
- **Cards**: [radius] corner radius, [elevation or flat description]
- **Inputs**: 1px border, surface-variant background, [radius] corner radius
[Focus on the components most relevant to the project. For each: name, variants, sizing, and key visual properties.]

## Do's and Don'ts

- Do use the primary color sparingly, only for the most important action per view
- Don't mix rounded and sharp corners in the same view
- Do maintain 4:1 contrast ratio for all text
- Don't use more than two font weights on a single screen
[4–6 practical guidelines covering color usage, typography rules, spacing conventions, and component behavior]
```

---

## Important Rules

1. **Never guess or fabricate values.** Only use data from actual files or Figma MCP responses.
2. **Preserve original token names exactly**, including any typos in the source (e.g. `netural` instead of `neutral`). Note typos in a comment but do not correct them in the token name.
3. **Resolve token references.** When a semantic token maps to another token (e.g. `var(--color-netural-900)`), show both the reference and the final hex value.
4. **Use bullet lists, not tables.** Every token should be a bullet point with inline values and usage descriptions. This matches the Stitch DESIGN.md format that AI agents consume.
5. **Every color needs a usage role.** Don't just list hex values — describe what each color is for (e.g. "CTAs, active states" or "Page backgrounds").
6. **Detect the responsive approach.** Check if the project uses fixed breakpoints, fluid responsive, or a hybrid. Read the minimum viewport width from config files.
7. **Be framework-agnostic.** This skill works with any CSS framework (Tailwind, styled-components, vanilla CSS, etc.) and any JS framework (React, Vue, Svelte, etc.).
8. **Ask before writing.** Always confirm with the user before saving the file.
