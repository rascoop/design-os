---
description: Capture section screenshots with Playwright CLI
agent: build
---

# Screenshot Screen Design

You are helping the user capture screenshots of section screen designs for documentation.

Use `playwright-cli` (not Playwright MCP) because it is more token-efficient for this workflow.

## Step 0: Check Playwright CLI Availability

Before proceeding, verify `playwright-cli` is available by running:

```bash
playwright-cli --help
```

If it is not installed, tell the user to install it:

```bash
npm install -g @playwright/cli@latest
```

Then stop and ask them to run `/screenshot-design` again.

## Step 1: Identify the Screen Design

Determine which screen design to screenshot.

Read `/product/product-roadmap.md` and inspect `src/sections/` for available screen designs.

- If only one exists, auto-select it.
- If multiple exist, use the `question` tool to ask the user which one to capture.

## Step 2: Start Dev Server

Start the dev server yourself using Bash (`npm run dev`) and keep it running while capturing screenshots.

Do not ask the user to start it.

Default URL is `http://localhost:5173`. If the server output shows a different port, use that port.

## Step 3: Capture With Playwright CLI

Before browser steps, use the `skill` tool to load `playwright-cli`.

Use a named session so commands share browser state. Example session: `design-os`.

For target route:

`http://localhost:[port]/sections/[section-id]/screen-designs/[screen-design-name]`

Run these actions in order:

1. Open/navigate with playwright-cli session
2. Resize viewport to desktop width (`1280x900` recommended)
3. Hide the top navigation by clicking `[data-hide-header]` via `playwright-cli eval`
4. Capture screenshot directly to destination path

Example command pattern:

```bash
playwright-cli -s=design-os open "http://localhost:5173"
playwright-cli -s=design-os goto "http://localhost:5173/sections/[section-id]/screen-designs/[screen-design-name]"
playwright-cli -s=design-os resize 1280 900
playwright-cli -s=design-os eval "() => document.querySelector('[data-hide-header]')?.click()"
playwright-cli -s=design-os screenshot --filename="product/sections/[section-id]/[filename].png"
```

Use full-page behavior when available in current CLI version. If not available, use the best possible viewport capture.

## Step 4: Naming

Use this naming convention:

`[screen-design-name]-[variant].png`

Examples:

- `invoice-list.png`
- `invoice-list-dark.png`
- `invoice-detail.png`
- `invoice-form-empty.png`

## Step 5: Confirm Completion

Tell the user where the screenshot was saved:

`product/sections/[section-id]/[filename].png`

If useful, offer additional captures:

- dark mode
- mobile viewport
- empty/loading/error states

## Important Notes

- Prefer `playwright-cli` over Playwright MCP for this command
- Start the dev server yourself
- Save screenshots directly into `product/sections/[section-id]/`
- Use descriptive filenames
- Close the Playwright CLI session when done if no more captures are needed
