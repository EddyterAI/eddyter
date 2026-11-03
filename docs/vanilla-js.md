# Eddyter for Vanilla JS

> How to install and use the Eddyter AI rich text editor with Vanilla JS. Authentication uses an API key from the Eddyter dashboard, and the plan tied to that key decides which features are enabled.

The detailed API reference, theming, and email guides are written for React: see https://eddyter.com/llms-full.txt. Prop names in the feature list below may differ here, so follow the usage on this page.

## Install and use

### Vanilla JS SDK — `@eddyter/core` (bundler)

Framework-agnostic. Works with Vite, Webpack, esbuild, Parcel, or any ES-module bundler.

```bash
npm install @eddyter/core
```

```ts
import { init } from '@eddyter/core';
import '@eddyter/core/style.css';

const instance = init({
  container: '#editor',          // CSS selector or HTMLElement
  apiKey: process.env.EDITOR_API_KEY!,
  mode: 'edit',
  initialContent: '<p>Start writing…</p>',
  onChange: (html) => console.log(html),
  onReady: () => console.log('Ready'),
  onAuthSuccess: () => console.log('Authenticated'),
  onAuthError: (err) => console.error(err),
});

instance.update({ darkMode: true }); // update without remounting
// instance.destroy();               // cleanup when removing the editor
```

### Vanilla JS SDK — `@eddyter/core` (`<script>` tag, no build step)

Load a single IIFE bundle and call `Eddyter.init()` from plain HTML or any server-rendered stack (Laravel, Django, Rails, WordPress). The IIFE bundle **injects its stylesheet automatically** — no CSS import needed.

```html
<div id="editor"></div>
<script src="https://unpkg.com/@eddyter/core/dist/editor.iife.js"></script>
<script>
  const instance = Eddyter.init({
    container: '#editor',
    apiKey: 'YOUR_API_KEY',
    initialContent: '<p>Start writing...</p>',
    onChange: (html) => console.log('Content:', html),
    onReady: () => console.log('Editor ready!'),
  });
</script>
```

Pin a version in production: `https://unpkg.com/@eddyter/core@1.0.4/dist/editor.iife.js` (or jsDelivr / self-hosted `editor.iife.js`).

## Custom key verification

Provide a `customVerifyKey` that resolves to `{ success, message }` to validate keys through your own backend instead of the default endpoint. Example (SDK; the same option exists as a prop/input/binding on every wrapper):

```ts
init({
  container: '#editor',
  apiKey,
  customVerifyKey: async (key) => {
    const res = await fetch('/api/verify-editor-key', {
      method: 'POST',
      body: JSON.stringify({ key }),
    });
    const data = await res.json();
    return { success: data.valid, message: data.message };
  },
});
```

## AI Agents & MCP

Eddyter has a Model Context Protocol server so coding agents can check a license, generate setup code, and change editor features for the key they are connected to. Overview: https://eddyter.com/mcp

**Hosted server (no install):** `https://mcp.eddyter.com/mcp`, with OAuth sign-in. Each token is scoped to one license key.

- **Claude.ai / Claude Desktop:** Settings → Connectors → Add custom connector → paste the URL. Also listed at https://claude.ai/directory/eddyter.
- **Cursor:** https://cursor.directory/plugins/eddyter
- **Codex:** `codex mcp add eddyter --url https://mcp.eddyter.com/mcp`
- **Claude Code plugin:** `/plugin marketplace add EddyterAI/eddyter-mcp`, then `/plugin install eddyter@eddyter`

**Local server:** `npx -y eddyter-mcp` with `EDDYTER_LICENSE_KEY` set. `update_config` is off locally unless `EDDYTER_ALLOW_WRITES=true`. Source: https://github.com/EddyterAI/eddyter-mcp

| Tool | Mode | What it does |
|---|---|---|
| `verify_status` | read | Whether the key is valid and active, with plan, expiry, subscription, BYOK status, and warnings. |
| `get_config` | read | Which editor features are enabled, off but allowed, or locked by plan. |
| `get_usage` | read | AI credits remaining, usage this period, and days until reset. |
| `get_integration_snippet` | read | SSR-safe setup code for a framework. |
| `update_config` | write | Turns editor features on or off, clamped to the plan and live with no redeploy. It affects every user of that key, so confirm with the account owner first. |

## Features

A comprehensive toolset for modern content creation, from basic formatting to advanced AI generation.

### Classic Formatting

- **Basic Formatting** — Bold, italic, underline, strikethrough, subscript, superscript.
- **Text Colors** — Text color and background highlight with color picker.
- **Font Controls** — 20+ font families with adjustable font sizes and line height.
- **Text Alignment** — Left, center, right, and justify alignment.

### Lists and Structure

- **Bulleted Lists** — Custom bullet styles with proper nesting.
- **Numbered Lists** — Decimal, alpha, and roman numeral formats.
- **Checklists** — Interactive checkboxes with strikethrough.
- **Headings** — H1–H6 heading levels.
- **Note Panels** — Callout/note blocks for tips, warnings, and info.

### Tables

- **Table Operations** — Insert/delete rows and columns, merge cells.
- **Cell Resizing** — Drag to resize columns and rows.
- **Header Styling** — Distinct header row styling.
- **Context Menu** — Right-click menu for quick actions.

### Media Support

- **Images** — Drag-drop upload with 8-point resize handles.
- **Videos** — YouTube/Vimeo embed with responsive players.
- **File Attachments** — Upload and attach downloadable files.
- **Link Management** — Insert links with floating editor and preview.

### Collaboration

- **Comments** — Inline commenting powered by the `currentUser` prop.
- **@Mentions** — Tag users with the `mentionUserList` prop.
- **Version History** — Server-backed, plan-gated document versions with save/preview/restore in a panel. Pass a correct per-document `documentId`, and call `saveVersion({ documentId })` from your Save button (required for new records / single-page apps; the panel's own Save button covers existing records). See Version History under API Reference.

### Email Templates (Plan-gated)

- **Drag-and-drop email builder** — An envelope button in the toolbar switches between the document editor and an email builder: block palette, 600px canvas, styles panel.
- **Six starter templates** — A gallery opens on the first switch (newsletter, product launch, welcome, promotion, event invite, plain note). Every block stays editable; "Start from blank" clears the canvas.
- **Email-safe HTML** — Nested `<table role="presentation">` layout, inline styles, `align`/`bgcolor` attributes and absolute image URLs. Normal rich-text HTML does not survive Outlook; this does.
- **Merge tags / personalization** — The host declares fields via `emailMergeTags`; users insert them by typing the prefix (`{{` by default, configurable — `*|` gives Mailchimp syntax). Tags behave as one unit, inherit surrounding formatting, and work inside button and link URLs. See Email Templates under API Reference.
- **Delivered through `onChange`** — No new prop. `meta.mode === "email"` identifies email output. See Email Templates under API Reference.
- **One-call sending shell** — `wrapEmailDocument(html, { title })` wraps the exported body in the document a mail client needs, carrying the three head-only pieces that each break one client: the responsive stylesheet Gmail needs to stack columns, Outlook's DPI override, and iOS Mail's reformatting opt-out.

### Markdown

The toolbar's Markdown menu is on by default and can be hidden in the API key settings.

- **Copy as Markdown** — Copies the document or selection as GitHub Flavored Markdown.
- **Download as .md** — Saves the document, named after its first heading.
- **Import Markdown** — Paste Markdown or choose a `.md` / `.txt` file (up to 2 MB), then insert at the cursor or replace the document. Undoable in one step.

Styling Markdown can't represent is removed but the text is kept. Embeds, videos, buttons and social icons become links, columns are flattened, and charts become tables. Imported Markdown is sanitized. There is no developer API for Markdown; it is a UI feature.

### AI Power (Premium)

Available on the AI (BYOK) and AI Pro (Managed) plans.

AI features are switched on in the dashboard, not in code: open [License Keys](https://eddyter.com/user/license-key), edit the API key, and use the "AI & smart features" section (AI Chat, Improve, Auto correction, Speech to text, Autocomplete). There is **no** `aiOptions`, `ai`, `enableAI`, or `autocomplete` prop on any Eddyter component — do not pass one. Once enabled, AI appears automatically for that key.

- **Smart Chat** — In-editor AI assistant for research, drafting, and creative ideas.
- **Smart Autocomplete** — Predictive text suggestions as you type.
- **Refinement** — Instantly improve tone, fix grammar, or change content length.
- **Gen-AI Images** — Create custom visuals from text prompts inside your document.

### Slash Commands

Type `/` at the start of a line to open the quick formatting menu. Provides fast access to headings, lists, tables, images, embeds, and more without reaching for the toolbar.

## Keyboard Shortcuts

Speed up your workflow with standard keyboard shortcuts.

### Text Formatting

| Action | Shortcut |
|---|---|
| Bold | Ctrl/Cmd + B |
| Italic | Ctrl/Cmd + I |
| Underline | Ctrl/Cmd + U |
| Insert Link | Ctrl/Cmd + K |

### General

| Action | Shortcut |
|---|---|
| Undo | Ctrl/Cmd + Z |
| Redo | Ctrl/Cmd + Y |
| Select All | Ctrl/Cmd + A |
| Paste | Ctrl/Cmd + V |

### Quick Access

| Action | Shortcut |
|---|---|
| Slash Commands | Type `/` at line start |

## Support & Resources

- **Documentation** — Full docs: https://eddyter.com/docs
- **Live Demo** — Try the editor: https://eddyter.com
- **Get API Key** — Manage license: https://eddyter.com/user/license-key
- **Pricing** — Compare plans: https://eddyter.com/pricing
- **Support** — Contact the team: https://eddyter.com/contact (signed-in users can open a ticket at https://eddyter.com/user/support)
- **Tutorials** — Video walkthroughs: https://eddyter.com/tutorials
- **Release Notes** — Shipped updates: https://eddyter.com/release-notes
- **npm Package** — https://www.npmjs.com/package/eddyter
- **MCP Server** — https://eddyter.com/mcp

### Video Tutorials

- **What is Eddyter? Why Developers Are Switching to This AI Editor (2026):** https://youtu.be/oNHBa-DImZc
- **Integrate Eddyter in 30 Minutes Using AI Tools (Cursor, Claude, Lovable):** https://youtu.be/5lTjRFjUWgs

### Bug Reports & Support

Eddyter's repository is private. Report bugs through the in-product bug reporter, a support ticket at https://eddyter.com/user/support, or https://eddyter.com/contact — not through GitHub.

### License

Eddyter is licensed under the MIT License. Security, privacy, and compliance are our core technical principles.
