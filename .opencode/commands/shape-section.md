---
description: Define section UX spec and auto-generate data/types
agent: build
---

# Shape Section

You are helping the user define the specification for a section of their product. This is a conversational process to establish the scope of functionality, user flows, and UI requirements — then automatically generate the spec and sample data.

## Step 1: Check Prerequisites

First, verify that `/product/product-roadmap.md` exists. If it doesn't:

"I don't see a product roadmap defined yet. Please run `/product-roadmap` first to define your product sections, then come back to shape individual sections."

Stop here if the roadmap doesn't exist.

## Step 2: Identify the Target Section

Read `/product/product-roadmap.md` to get the list of available sections.

If there's only one section, auto-select it. If there are multiple sections, use the `question` tool to ask which section the user wants to work on:

"Which section would you like to define the specification for?"

Present the available sections as options.

## Step 3: Gather Initial Input

Once the section is identified, invite the user to share any initial thoughts:

"Let's define the scope and requirements for **[Section Title]**.

Do you have any notes or ideas about what this section should include? Share any thoughts about the features, user flows, or UI patterns you're envisioning. If you're not sure yet, we can start with questions."

Wait for their response. The user may provide raw notes or ask to proceed with questions.

## Step 4: Ask Clarifying Questions

Before questioning, use the `skill` tool to load `prd-discovery`.

Use the `question` tool to ask targeted questions across all categories below. Aim for 8-12 total questions (grouped 1-2 at a time), and ask follow-ups when answers are vague.

- **Main user actions/tasks** - What can users do in this section?
- **Information to display** - What data and content needs to be shown?
- **Key user flows** - What are the step-by-step interactions?
- **UI patterns/options** - Preferred layout and interaction patterns
- **States** - Empty, loading, error, success, and long-list scenarios
- **Permissions** - Any role-based visibility/actions
- **Responsive behavior** - What changes on mobile vs desktop
- **Accessibility expectations** - Keyboard, labels, contrast, focus behavior
- **Scope boundaries** - What should be explicitly excluded?

Example questions (adapt based on their input and the section):
- "What are the main actions a user can take in this section?"
- "What information must always be visible on first load?"
- "Walk me through the main user flow - what happens step by step?"
- "Which layout fits best for this section: table/list, card grid, split-pane, or kanban-style board?"
- "Do you need more than one view (list/detail/create/edit)? If yes, which one is priority one?"
- "What should users see when there is no data yet?"
- "What error states should the UI account for (validation, permission denied, failed action)?"
- "Are any actions restricted to specific user roles?"
- "On mobile, should this be condensed, stacked, or tabbed?"
- "What's intentionally out of scope for this section?"

Focus on user experience and interface requirements only - no backend or database implementation details.

## Step 5: Ask About Shell Configuration

If a shell design has been created for this project (check if `/src/shell/components/AppShell.tsx` exists), ask the user about shell usage:

"Should this section's screen designs be displayed **inside the app shell** (with navigation header), or should they be **standalone pages** (without the shell)?

Most sections use the app shell, but some pages like public-facing views, landing pages, or embedded widgets should be standalone."

Use the `question` tool with options:
- "Inside app shell" - The default for most in-app sections
- "Standalone (no shell)" - For public pages, landing pages, or embeds

If no shell design exists yet, skip this question and default to using the shell.

## Step 6: Auto-Proceed — Create Spec and Sample Data

Once you have enough information from the clarifying questions, **immediately proceed** without asking for approval. Do all of the following in sequence:

### 6a: Create the Spec File

Create the file at `product/sections/[section-id]/spec.md` with this exact format:

```markdown
# [Section Title] Specification

## Overview
[2-3 sentence summary of what this section does]

## User Flows
- [Flow 1]
- [Flow 2]
- [Flow 3]
[Add all flows discussed]

## UI Requirements
- [Requirement 1]
- [Requirement 2]
- [Requirement 3]
[Add all requirements discussed]

## Configuration
- shell: [true/false]
```

**Important:**
- Set `shell: true` if the section should display inside the app shell (this is the default)
- Set `shell: false` if the section should display as a standalone page without the shell
- The section-id is the slug version of the section title (lowercase, hyphens instead of spaces)
- Don't add features that weren't discussed. Don't leave out features that were discussed.

### 6b: Generate Sample Data and Types

Immediately after writing the spec, run the full sample data generation process for this section:

1. **Check for global data shape** — Read `/product/data-shape/data-shape.md` if it exists. Use entity names and relationships as a guide for consistency.

2. **Analyze the spec** — Determine what data entities are implied by the user flows, what fields each entity needs, and what actions can be taken (these become callback props).

3. **Create `product/sections/[section-id]/data.json`** with:
   - A `_meta` section with human-readable descriptions of each entity and their relationships
   - Realistic, believable sample data (not "Lorem ipsum" or "Test 123")
   - 5-10 sample records for main entities
   - Varied content: mix short/long text, different statuses
   - Edge cases: at least one empty array, one long description
   - TypeScript-friendly structure with consistent field names

   Required `_meta` structure:
   ```json
   {
     "_meta": {
       "models": {
         "entityName": "Plain-language description of what this entity represents."
       },
       "relationships": [
         "Description of how models connect to each other"
       ]
     }
   }
   ```

4. **Create `product/sections/[section-id]/types.ts`** with:
   - Data interfaces inferred from sample data (strings, numbers, booleans, arrays, nested objects)
   - Union types for status/enum fields based on the spec
   - A Props interface named `[SectionName]Props` with data as props and optional callback props for each action
   - JSDoc comments on callback props
   - PascalCase for interface names, camelCase for property names

### 6c: Inform the User

After all files are created, present a summary:

"I've created the following for **[Section Title]**:

1. **Spec** — `product/sections/[section-id]/spec.md`
2. **Sample Data** — `product/sections/[section-id]/data.json` ([X] records)
3. **TypeScript Types** — `product/sections/[section-id]/types.ts`

Here's a quick summary of the spec:

**Overview:** [2-3 sentence summary]

**User Flows:** [Brief list]

**Sample data includes:** [Brief description of entities and record counts]

Feel free to review these files. Let me know if you'd like to adjust anything in the spec or sample data. When you're ready, run `/design-screen` to create the screen design for this section."

## Important Notes

- Be conversational and helpful, not robotic
- Ask follow-up questions when answers are vague
- Focus on UX and UI - don't discuss backend, database, or API details
- Keep the spec concise - only include what was discussed, no bloat
- The format must match exactly for the app to parse it correctly
- Do NOT present a draft for approval — go straight to writing the files after gathering enough info
- If the user requests changes after reviewing, update the relevant files immediately
