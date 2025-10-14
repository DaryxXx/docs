# Mintlify Field Guide

Authoritative reference for writing and styling documentation in this repo with Mintlify. Keep this document close whenever you update `*.mdx` files or `docs.json`.

## Quick Reference

| Topic | Link |
| --- | --- |
| Product overview | https://mintlify.com |
| Docs home | https://mintlify.com/docs |
| Configuration reference | https://mintlify.com/docs/reference/config |
| Components catalog | https://mintlify.com/docs/meta/components |
| Custom components guide | https://mintlify.com/docs/writing-content/custom-components |
| CLI usage | https://mintlify.com/docs/meta/mintlify-cli |

## Repo Essentials

- Content lives in root-level `.mdx` files (one page per file).
- Global configuration uses `docs.json` (brand, navigation, colors, integrations, redirects).
- Static assets (images, logos) sit under `images/` or `logo/`.
- Local preview with `npm install` (if needed) and `npm run dev` or `mintlify dev`.

## Frontmatter & Metadata

Every page can declare YAML frontmatter at the top of the `.mdx` file:

| Key | Purpose |
| --- | --- |
| `title` | Page title shown in UI and metadata. |
| `description` | Short meta description used for SEO and previews. |
| `icon` | Optional icon (from the Mintlify icon set) for navigation. |
| `slug` | Override the URL slug if the filename should not be used. |
| `image` | Social preview image path. |
| `sidebarTitle` | Shorter label in sidebar/tabs. |
| `hidden` | Boolean to remove the page from navigation while keeping it accessible. |
| `toc` | Control on-page table of contents (`true`, `false`, or custom depth). |

Aim to keep titles in sentence case and descriptions under ~150 characters.

## docs.json Configuration

`docs.json` controls global styling, navigation, and integrations. Key sections:

- `name`, `logo`, `favicon`: Brand identity.
- `colors.primary`, `colors.accent`, `background`: Theme palette. Use hex values.
- `theme`: Dark/light defaults, typography, layout toggles.
- `navigation`: Tabs or sidebar groups. You can mix tabs with nested groups.
- `topbar`: Social links, GitHub/Discord buttons.
- `integrations`: Analytics, chat widgets, MCP servers, etc.
- `redirects`: Legacy URL handling.

When editing navigation, ensure every slug listed in `pages` matches an `.mdx` filename (without extension) or explicit `slug`.

### Tabs vs Sidebar Navigation

**Tabs layout (current preference)**

```json
{
  "navigation": {
    "tabs": [
      {
        "tab": "Overview",
        "icon": "home",
        "groups": [
          { "group": "Start", "pages": ["overview", "welcome"] }
        ]
      }
    ]
  }
}
```

**Simple sidebar**

```json
{
  "navigation": [
    { "group": "Intro", "pages": ["overview"] },
    { "group": "Developers", "pages": ["sdk", "api"] }
  ]
}
```

## Writing Guidelines

- Lead with a short summary (1–3 sentences) followed by scannable sections.
- Favor active voice, concise paragraphs, and meaningful headings.
- Use widgets (callouts, cards, steps) for structure instead of long prose.
- Keep language neutral and product-focused; avoid marketing fluff unless intentional.
- Link related pages using relative paths (`/infinite-web-arena`).
- For code, always supply a language hint (```` ```ts ````) to enable syntax highlighting.

## Core Components & Widgets

Mintlify ships a rich MDX component library. Importing is not required for built-ins—just use the tags.

### Callouts (for highlights & warnings)

| Component | Use case | Example |
| --- | --- | --- |
| `<Info>` | Neutral information or context. | ```mdx\n<Info>Subnet 36 is the incentive layer for web agents.</Info>\n``` |
| `<Tip>` | Best practices or pro tips. | ```mdx\n<Tip>Use sandboxed browsers when running validators.</Tip>\n``` |
| `<Note>` | Supplemental details. | ```mdx\n<Note>Weights update every epoch.</Note>\n``` |
| `<Warning>` | Potential pitfalls. | ```mdx\n<Warning>Do not hardcode task flows; pages change frequently.</Warning>\n``` |
| `<Danger>` | Critical blockers. | ```mdx\n<Danger>Never expose production credentials in tutorials.</Danger>\n``` |

Callouts support optional `title` props for custom headings.

### Cards & Feature Grids

Two common patterns exist in this repo:

- `<Cards columns={3}>` with nested `<Card title="..." href="...">`.
- `<CardGroup cols={2}>` with nested `<Card icon="..." title="..." color="#hex">`.

Use cards when you need high-level summaries or CTAs.

```mdx
<Cards columns={3}>
  <Card title="Adaptive autonomy">
    Handles UI drift without scripts.
  </Card>
  <Card title="Full-stack execution">
    Navigates, fills forms, and finalizes workflow.
  </Card>
  <Card title="Global coverage">
    Works across e-commerce, finance, and SaaS portals.
  </Card>
</Cards>
```

### Sequential Content

- `<Steps>` with nested `<Step title="...">` to describe ordered flows.
- `<Checklist>` to present completion lists.
- `<Timeline>` if you need chronological milestones (less common but available).

```mdx
<Steps>
  <Step title="Submit agent">
    Package the model and publish from your miner node.
  </Step>
  <Step title="Validation run">
    Validators execute the agent against IWA tasks.
  </Step>
  <Step title="Scoring">
    Results update on-chain weights for emissions.
  </Step>
</Steps>
```

### Collapsible & Tabbed Content

- `:::tabs` / `:::tab` blocks for language or persona switches.
- `<AccordionGroup>` with nested `<Accordion title="...">` for FAQs.
- `<Details>` / `<Summary>` (native HTML) also works for lightweight toggles.

````mdx
:::tabs
:::tab Web
Use `<Tip>` and `<Cards>` for product overviews.
:::
:::tab CLI
Document installation, commands, and flags.
:::
:::
````

### Media & Embeds

- `<Image src="/images/diagram.png" alt="Architecture diagram" />`
- `<Video src="https://cdn.example.com/demo.mp4" />`
- `<Frame>` to wrap responsive iframes (e.g., Figma, Loom).
- `<Embed src="https://www.loom.com/share/...">` for supported providers.

Always include `alt` text on images. For wide diagrams, wrap with `<Frame>` to manage overflow.

### Code, API, and Data Blocks

- Use fenced code blocks with language hints for snippets.
- `:::code-group` or `<CodeGroup>` help compare variations (CLI vs API).
- `<API>` + `<Endpoint>` components render API references automatically when you define OpenAPI specs.
- `<Table>` (standard Markdown table) handles tabular data; Mintlify styles it automatically.

### Buttons & Badges

- `<Button href="https://...">` for primary CTAs.
- `<Button variant="secondary">` or `variant="outline"` to adjust styling.
- `<Badge>` highlights statuses (e.g., `<Badge color="green">GA</Badge>`).
- `<Tag>` works similarly for labeling features.

### Layout Utilities

- `<Columns>` / `<Column>` splits content into responsive columns.
- `<TwoColumn>` with `<Column>` explicitly defines left/right layouts.
- `<Hero>` sets a banner with headline, subtext, and actions (useful for landing pages).
- `<Split>` creates alternating image/text sections.

## Styling & Tone Consistency

- Reuse existing color tokens and icon names to keep styling consistent.
- Prefer vector assets (`.svg`) for logos; raster images at least 2x resolution for retina.
- Keep CTA copy short (`Start building`, `View leaderboard`).
- Avoid mixing `CardGroup` styles on the same page unless you need both icon and text variants.
- Check dark mode by toggling the Mintlify preview or using browser devtools.

## Custom React Components

You can import custom React components at the top of an `.mdx` file:

```mdx
import Demo from '../components/Demo';

<Demo feature="iwa" />
```

Keep shared components under a `components/` directory and ensure they are SSR-safe. Avoid accessing browser-only APIs directly.

## CLI & Local Workflow

1. Install dependencies (`npm install`) if the project uses local scripts.
2. Run `npm run dev` (or `mintlify dev`) from repo root.
3. Visit `http://localhost:3000` to preview docs with hot reload.
4. Use `mintlify lint` to validate frontmatter and navigation when available.

Mintlify automatically builds previews for feature branches. Keep PRs scoped to logical documentation changes for easier review.

## Additional Tips

- Store reusable snippets under `my-templates.mdx` or similar and copy when needed.
- If you add new assets, update `docs.json` `assets` or `logo` paths if relevant.
- Document any custom components in this guide so future contributors understand usage patterns.
- When introducing new navigation groups, confirm the order in `docs.json` matches the intended sidebar hierarchy.
- Run a quick `rg "<"` scan if you need examples of specific components already used in this repo.

## Changelog

- 2025-10-14: Initial version compiled with component inventory, styling patterns, and workflow pointers.
