---
name: figma-design-system-to-design-md
description: "Figma design system to design.md — Extract Figma design system into a structured design.md. Use when user says 'generate design.md', 'extract design system', 'design tokens to markdown', 'create design doc from Figma', 'Figma design system to design.md', or wants to document their design system from token files and Figma."
user-invocable: true
allowed-tools: Read, Grep, Glob, Bash, Write, Edit, mcp__Figma__get_variable_defs, mcp__Figma__get_metadata, mcp__Figma__get_screenshot, mcp__Figma__get_design_context
argument-hint: "[output-path]"
---

# Figma Design System → design.md

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

- **Only include sections where data was found.** Do not generate empty tables.
- Mark sections with insufficient data as `> TODO: [what needs to be added]`
- Use the exact CSS variable names / token names from the source files
- Include hex values for all colors
- For semantic color tokens that reference other tokens, show both the token reference AND the resolved hex value
- Tables must be properly formatted markdown
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

---

## Colors

### Base

| Token | Hex |
|-------|-----|

### [Color Group Name] (repeat for each group: Primary, Neutral, Success, Error, Warning, etc.)

| Token | Hex | Notes |
|-------|-----|-------|

### Semantic Color Roles

#### Text

| Token | Maps to | Hex | Usage |
|-------|---------|-----|-------|

#### Surface

| Token | Maps to | Hex | Usage |
|-------|---------|-----|-------|

#### Border

| Token | Maps to | Hex | Usage |
|-------|---------|-----|-------|

#### Icon

| Token | Maps to | Hex | Usage |
|-------|---------|-----|-------|

---

## Typography

### Font Families

| Usage | Font |
|-------|------|

### Heading

| Level | Size | Line Height | Weight |
|-------|------|-------------|--------|

### Body

| Level | Size | Line Height | Weight |
|-------|------|-------------|--------|

### Caption

| Level | Size | Line Height |
|-------|------|-------------|

---

## Spacing

| Token | Value |
|-------|-------|

---

## Border Radius

| Token | Value |
|-------|-------|

---

## Border Width

| Token | Value |
|-------|-------|

---

## Elevation

| Name | Value |
|------|-------|

---

## Responsive

- **Minimum supported width**: [value]px
- **Approach**: [fluid / breakpoint-based / hybrid]
- [additional responsive rules]

---

## Components

> TODO: [guidance on what to add]

---

## Overview

> TODO: Design intent and visual personality (to be written by designer)

---

## Do's and Don'ts

> TODO: Design judgment rules (to be written by designer)
```

---

## Important Rules

1. **Never guess or fabricate values.** Only use data from actual files or Figma MCP responses.
2. **Preserve original token names exactly**, including any typos in the source (e.g. `netural` instead of `neutral`). Note typos in a comment but do not correct them in the token name.
3. **Resolve token references.** When a semantic token maps to another token (e.g. `var(--color-netural-900)`), show both the reference and the final hex value.
4. **Detect the responsive approach.** Check if the project uses fixed breakpoints, fluid responsive, or a hybrid. Read the minimum viewport width from config files.
5. **Be framework-agnostic.** This skill works with any CSS framework (Tailwind, styled-components, vanilla CSS, etc.) and any JS framework (React, Vue, Svelte, etc.).
6. **Ask before writing.** Always confirm with the user before saving the file.
