# Eddyter — API Reference & Integration Guide

> Eddyter is a plug-and-play, AI-native rich text editor built on Meta's Lexical framework. Drop it into React, vanilla JavaScript, Angular, Svelte, Vue, Laravel, or React Native in about 10 minutes — without managing infrastructure, storage, or AI.

**Compatibility:** React 18.2+ / 19.x (Next.js App & Pages Router, Vite, CRA, any Webpack-based React framework). Also available as a framework-agnostic SDK (`@eddyter/core`, ES modules or `<script>` tag), and as official wrappers for Angular 17–21 (`@eddyter/angular`), Svelte 5 (`@eddyter/svelte`), Vue 3 / Nuxt / Quasar (`@eddyter/vue`), Laravel 10–13 (`eddyter/laravel` Composer package), and React Native 0.72+ for iOS & Android (`@eddyter/react-native`).

**Stack:** React, Next.js, TypeScript. Built on Meta's Lexical framework. Per-framework docs: https://eddyter.com/docs (React), /docs/sdk, /docs/angular, /docs/svelte, /docs/vue, /docs/laravel, /docs/react-native.

---

## Pricing & Plans

Eddyter offers five tiers designed to scale from side projects to production SaaS applications.

| Plan | Price | Storage | API Keys | What's Included |
|---|---|---|---|---|
| **Free** | $0/month | 100 MB | 1 | 2,500 editor loads per month. Core rich text editor for evaluation and personal projects. Eddyter branding in footer. |
| **Starter** | $12/month | 1 GB | 1 | 5,000 editor loads per month, core rich text editing, secure media uploads, stable editor infrastructure, community support. Powered-by-Eddyter branding. Best for side projects and early-stage products. |
| **Pro** | $29/month | 2 GB | 3 | Everything in Starter plus unlimited editor loads, premium toolbar options, advanced table features, priority support, no Eddyter branding. |
| **AI (BYOK)** | $39/month | 3 GB | 4 | Everything in Pro plus AI features powered by your own API key (Bring Your Own Key). You provide your OpenAI/Anthropic key; Eddyter handles the integration layer. |
| **AI Pro (Managed)** | $59/month | 5 GB | 5 | Everything in Pro plus fully managed AI — hosting, storage, scaling, and AI processing all handled by Eddyter. Includes 1,000 AI credits per month. No AI provider setup required. |

**Enterprise:** Custom plans with enterprise-grade security, dedicated support, and higher limits are available. Contact sales for details.

**Free Trial:** New accounts get two weeks of free premium access with all AI features enabled.

**Starter vs Free:** Starter raises the limits from 2,500 to 5,000 editor loads per month and from 100 MB to 1 GB of storage — the minimum tier for a production side project or MVP.

**BYOK (Bring Your Own Key):** On the AI (BYOK) plan, you supply your own LLM API key (OpenAI, Anthropic, etc.). Eddyter routes AI requests through your key, giving you full control over costs and model selection while Eddyter handles the editor-side integration.

**Add-ons available on paid plans:** Upgrade storage, upgrade AI credits, buy extra API keys.

**Compare plans in detail:** https://eddyter.com/pricing

---

## Getting Started

Integrate Eddyter into your React application in 10 minutes. Supports Next.js (App & Pages), Vite, and Webpack-based projects.

### 1. Installation

Install the core package via your preferred package manager:

```bash
npm install eddyter
```

Or with yarn / pnpm:

```bash
yarn add eddyter
# or
pnpm add eddyter
```

**npm:** https://www.npmjs.com/package/eddyter

### 2. Style Integration

Import the required CSS in your root file:

```tsx
import 'eddyter/style.css';
```

Place this import in `app/layout.tsx` (Next.js App Router), `pages/_app.tsx` (Pages Router), or your root `App.tsx`.

**Important:** Use `'eddyter/style.css'` — NOT `'eddyter/dist/style.css'`. The stylesheet is required for proper rendering of tables, toolbars, and all editor components. Missing styles will cause visual issues.

### 3. Get Your API Key

1. Create an account at [eddyter.com](https://eddyter.com).
2. Use the Free plan, or choose a paid plan on the [Pricing](https://eddyter.com/pricing) page.
3. Navigate to [License Keys](https://eddyter.com/user/license-key) in your dashboard.
4. Copy your API key.

Use `"YOUR_API_KEY"` as a placeholder in code examples. The key comes from the Eddyter dashboard.

### 4. Add the Editor to Your App

**Recommended approach for Next.js:** Use the `"use client"` directive at the top of your component file. Only fall back to `dynamic()` import with `ssr: false` if `"use client"` doesn't work.

```tsx
"use client";

import React from 'react';
import {
  ConfigurableEditorWithAuth,
  EditorProvider,
  defaultEditorConfig
} from 'eddyter';
import 'eddyter/style.css'; // REQUIRED — the editor is unstyled without this import

export default function Editor() {
  const apiKey = process.env.NEXT_PUBLIC_EDITOR_API_KEY!;

  // Current logged-in user for comments feature
  const currentUser = {
    id: 'user-123',
    name: 'John Doe',
    email: 'john@example.com',
    avatar: 'https://example.com/avatar.jpg' // optional
  };

  const handleContentChange = (html: string) => {
    console.log('Editor content:', html);
    // Save to state, database, etc.
  };

  return (
    <EditorProvider
      defaultFontFamilies={defaultEditorConfig.defaultFontFamilies}
      currentUser={currentUser}
    >
      <ConfigurableEditorWithAuth
        apiKey={apiKey}
        onChange={handleContentChange}
        initialContent="<p>Start writing...</p>"
        mentionUserList={['Alice', 'Bob', 'Charlie']}
        onAuthSuccess={() => console.log('Editor ready!')}
        onAuthError={(error) => console.error('Auth failed:', error)}
      />
    </EditorProvider>
  );
}
```

**Environment Variable:** Store your API key in environment variables (e.g. `NEXT_PUBLIC_EDITOR_API_KEY`). Never commit API keys to version control.

**Fallback (if `"use client"` doesn't work):**

```tsx
import dynamic from 'next/dynamic';

const Editor = dynamic(() => import('./Editor'), { ssr: false });
```

---

## Framework Integrations

The sections above cover the React package (`eddyter`). The same editor ships for other stacks with identical features and the same API-key authentication. Each framework has its own docs page: https://eddyter.com/docs/sdk, /docs/angular, /docs/svelte, /docs/vue, /docs/laravel, /docs/react-native. Across every framework: pass your `apiKey`, the editor validates it against the Eddyter server, `onAuthSuccess`/`onAuthError` (or the equivalent event) fire on the result, and an optional `customVerifyKey` lets you proxy verification through your own backend (web packages only — not available on React Native).

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

### Angular — `@eddyter/angular` (Angular 17–21)

Standalone `<eddyter>` component with `@angular/forms` `ControlValueAccessor` support (`[(ngModel)]` or `[(value)]`).

```bash
npm install @eddyter/angular
```

```ts
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { Eddyter } from '@eddyter/angular';

@Component({
  standalone: true,
  selector: 'app-basic-editor',
  imports: [FormsModule, Eddyter],
  template: `
    <eddyter
      [apiKey]="apiKey"
      [(value)]="content"
      mode="edit"
      [toolbar]="{ mode: 'static' }"
      (ready)="onReady()"
      (authSuccess)="onAuthSuccess()"
      (authError)="onAuthError($event)"
    />
  `,
})
export class BasicEditorComponent {
  apiKey = '<YOUR_API_KEY>';
  content = '<p></p>';
  onReady() {}
  onAuthSuccess() {}
  onAuthError(message: string) { console.error(message); }
}
```

Import `@import '@eddyter/angular/styles.css';` in `src/styles.css`. Global styles must be in the initial bundle — importing only inside a lazy-loaded chunk is not enough for production builds.

### Svelte — `@eddyter/svelte` (Svelte 5)

Runes-based `<Eddyter>` component for SvelteKit and standalone Vite apps.

```bash
npm install @eddyter/svelte
```

```svelte
<script lang="ts">
  import Eddyter from '@eddyter/svelte';

  const apiKey = import.meta.env.VITE_EDDYTER_API_KEY ?? '';
  let content = $state('<p></p>');
</script>

<Eddyter
  apiKey={apiKey}
  value={content}
  mode="edit"
  toolbar={{ mode: 'static' }}
  onChange={(html) => (content = html)}
  onReady={() => console.log('ready')}
  onAuthSuccess={() => console.log('auth ok')}
  onAuthError={(err) => console.error(err)}
/>
```

The component imports the SDK stylesheet internally; add `import '@eddyter/core/style.css';` only if styles are missing in production.

### Vue — `@eddyter/vue` (Vue 3, Nuxt, Quasar)

`<Eddyter>` with `v-model` and ref-based instance access.

```bash
npm install @eddyter/vue
```

```vue
<script setup lang="ts">
import { ref } from 'vue';
import { Eddyter } from '@eddyter/vue';

const apiKey = import.meta.env.VITE_EDDYTER_API_KEY ?? '';
const content = ref('<p></p>');
</script>

<template>
  <Eddyter
    v-model="content"
    :api-key="apiKey"
    mode="edit"
    :toolbar="{ mode: 'static' }"
    @ready="() => console.log('ready')"
    @auth-success="() => console.log('auth ok')"
    @auth-error="(e) => console.error(e)"
  />
</template>
```

The component imports the SDK stylesheet automatically; add `import '@eddyter/core/style.css';` in `main.ts` only if styles are missing in production.

### Laravel — `eddyter/laravel` Composer package

Official Laravel integration with a Blade component, hidden textarea for form binding, and CDN-loaded SDK. No React, Vite, or Node.js required.

```bash
composer require eddyter/laravel
php artisan eddyter:install
```

```env
EDDYTER_API_KEY=your-api-key-here
```

```blade
{{-- layouts/app.blade.php --}}
<head>
    <meta name="csrf-token" content="{{ csrf_token() }}">
    @eddyterStyles
</head>
<body>
    @yield('content')
    @eddyterScripts
</body>

{{-- posts/create.blade.php --}}
<form method="POST" action="{{ route('posts.store') }}">
    @csrf
    <x-eddyter name="body" :value="old('body')" height="500px" />
    <button type="submit">Save</button>
</form>
```

`php artisan eddyter:install --config` also publishes `config/eddyter.php`; add `--force` to overwrite published files. The combined `@eddyterAssets` directive remains available when separate asset placement is not possible.

On submit, the hidden textarea syncs with the editor HTML so `$request->input('body')` contains the rich text content. The package transports HTML but does not sanitize it; apply your application's allowlist or sanitizer when content is not fully trusted. For dynamic DOM (Livewire, Turbo, HTMX), call `EddyterLaravel.initAll()` after new HTML is inserted.

### React Native — `@eddyter/react-native` (iOS & Android)

`<Eddyter>` loads the published `@eddyter/core` build inside a `WebView` and bridges events, updates, and commands to React Native. Supports React Native 0.72+ with React 18+.

```bash
npm install @eddyter/react-native react-native-webview react-native-device-info react-native-blob-util react-native-share
cd ios && pod install
```

The four `react-native-*` packages are native peer dependencies: `react-native-webview` (>= 13) hosts the editor, `react-native-device-info` reads your bundle id / applicationId for license binding, and `react-native-blob-util` + `react-native-share` handle PDF / DOCX export. Expo apps need a development build (Expo Go does not include these native modules). No stylesheet import is needed.

```tsx
import { useRef, useState } from 'react';
import { Eddyter, type EddyterRef } from '@eddyter/react-native';

export function NoteEditor({ note }) {
  const editorRef = useRef<EddyterRef>(null);
  const [html, setHtml] = useState(note.html);

  return (
    <Eddyter
      ref={editorRef}
      apiKey="YOUR_API_KEY"
      user={{ id: '1', name: 'Sam' }}
      documentId={note.id}
      initialContent={note.html}
      onChange={setHtml}
      onError={(error) => console.warn('Eddyter:', error)}
      containerStyle={{ flex: 1 }}
    />
  );
}
```

- **License binding:** native apps have no web domain, so the license key is bound to your app's iOS bundle id / Android applicationId. The component reads it from the compiled app; if it can't be determined, `onError` is called. There is no `customVerifyKey` on React Native.
- **Content:** passing `onChange` output back into `initialContent` never reloads the editor; any other value re-initializes it. To switch documents, change the component `key`.
- **Updating options:** visual and configuration props (`mode`, `darkMode`, `fitToPage`, `uiStyle`, fonts, classes, `style`, `toolbar`, `editor`, `defaultFontFamilies`, `mentionUserList`, `emailMergeTags*`, `documentId`) apply in place without reloading the WebView. Changing `apiKey` or `user` re-creates the editor and keeps the latest content.
- **Ref methods (`EddyterRef`):** `saveVersion(options?)` (resolves to `{ skipped: true, reason: 'unavailable' }` when the editor isn't ready or version history isn't on your plan), `getContent()`, `reload()`.
- **Native-only props:** `containerStyle`, `exportMode` (`'share' | 'save' | 'both'`), `exportFilePrefix`, `onExportSuccess`, `onExportError`, `onLinkPress` (defaults to `Linking.openURL`), `sdkVersion`, `showStatus`, `webViewProps`.
- **Permissions:** speech-to-text needs `NSMicrophoneUsageDescription` (iOS) and `RECORD_AUDIO` + `MODIFY_AUDIO_SETTINGS` (Android). Saving exports to Downloads on Android 9 and below needs `WRITE_EXTERNAL_STORAGE`.
- **Network:** the device needs network access. The editor loads on first launch and the license key is validated online.

### Custom key verification (any framework)

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

---

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

---

## Authentication

Standard API key verification enables premium features and AI capabilities.

### How It Works

1. Pass your API key to the `apiKey` prop.
2. The editor validates the key against Eddyter's server.
3. Features are enabled based on your subscription plan.
4. `onAuthSuccess` fires when validation succeeds.
5. `onAuthError` fires if validation fails.

### Custom Verification (Optional)

Provide your own verification function to validate keys through your backend:

```tsx
<ConfigurableEditorWithAuth
  apiKey={apiKey}
  customVerifyKey={async (key) => {
    const response = await fetch('/api/verify-editor-key', {
      method: 'POST',
      body: JSON.stringify({ key })
    });
    const data = await response.json();
    return {
      success: data.valid,
      message: data.message
    };
  }}
/>
```

---

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

---

## Configuration

Tailor every aspect of the editor to fit your application's specific needs.

### Feature availability (which toolbar buttons / features are enabled)

Feature availability is **not** configured in your code. It is controlled by the
**plan tied to your license key** and managed from your Eddyter dashboard.
`ConfigurableEditorWithAuth` has **no** `config`, `toolbarOptions`,
`floatingMenuOptions`, `features`, or `aiOptions` prop — do not pass one. This
includes AI features (chat, autocomplete, improve). When you set `apiKey`, the editor
validates it and loads the enabled features for that plan automatically; any
feature the plan doesn't include renders as locked.

```tsx
// Correct: pass only the apiKey. Enabled features come from the plan.
<ConfigurableEditorWithAuth apiKey="YOUR_API_KEY" onChange={(html) => save(html)} />
```

To turn features on/off, change the plan's configuration in the Eddyter
dashboard — not in your integration code.

### Toolbar Behavior

Configure toolbar positioning with the `toolbar` prop. In sticky mode you can set `offset` and `zIndex`. In static mode those values are ignored.

```tsx
// Sticky toolbar below a 64px fixed header
<ConfigurableEditorWithAuth
  apiKey="YOUR_API_KEY"
  toolbar={{ mode: "sticky", offset: 64, zIndex: 1200 }}
/>
```

**Defaults:** `{ mode: "sticky", offset: 20, zIndex: 1000 }`

**Note:** The `toolbar` prop is an OBJECT, not an array.

### Editor Scroll Area

In static toolbar mode, use `editor.maxHeight` to make only the editor text area scrollable:

```tsx
// Static toolbar with scrollable content area
<ConfigurableEditorWithAuth
  apiKey="YOUR_API_KEY"
  toolbar={{ mode: "static" }}
  editor={{ maxHeight: 600 }}
/>
```

### Responsive Layout — Embedding in Flex / Grid Columns

The editor is fully responsive out of the box: it shrinks to whatever width its
container gives it, and toolbar buttons that don't fit collapse into a `⋯ More`
menu (on touch screens the toolbar scrolls horizontally instead). You do **not**
need to add any CSS for the standard case — just render it and it fits.

**The one rule when placing it inside your OWN flex or grid layout** (e.g. an
editor next to a preview/sidebar panel): the ancestor that holds the editor must
be allowed to shrink. By default flex and grid items have `min-width: auto`,
which means they refuse to shrink below their content and will push the editor
(and its toolbar) wider than the screen — the editor looks like it "overflows"
or is "cut off" instead of fitting.

Fix it on the **host** container (one line):

```tsx
// FLEX: give the column that contains the editor `min-w-0`
<div className="flex gap-4">
  <div className="flex-1 min-w-0">   {/* ← min-w-0 lets the editor shrink */}
    <ConfigurableEditorWithAuth apiKey="YOUR_API_KEY" />
  </div>
  <aside className="w-[400px] shrink-0">Preview…</aside>
</div>
```

```tsx
// GRID: size the editor's track with minmax(0, 1fr), NOT 1fr
<div className="grid grid-cols-[minmax(0,1fr)_400px] gap-4">
  <div><ConfigurableEditorWithAuth apiKey="YOUR_API_KEY" /></div>
  <aside>Preview…</aside>
</div>
```

Plain CSS equivalents: add `min-width: 0` to the flex/grid item that wraps the
editor, or use `grid-template-columns: minmax(0, 1fr) 400px` instead of
`1fr 400px`. This rule applies at every ancestor level between the editor and the
page — if you nest several flex/grid wrappers, each one needs to be allowed to
shrink (`min-w-0` on the item, or `minmax(0,1fr)` on the grid track).

**Self-diagnosing:** if the editor ever does overflow its container, it logs a
`[Eddyter]` warning to the browser console at runtime that names the exact
offending ancestor element and the one-line fix. Check the console first.

**Note:** this is a standard CSS flexbox/grid behavior (the `min-width: auto`
default), not specific to Eddyter — the same rule applies to any wide child
(code blocks, tables, charts) placed in a flex/grid column. The editor already
carries `min-width: 0`, `overflow-x: clip`, and `contain: inline-size`
internally, so it only surfaces when a host wrapper reintroduces the constraint.

---

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

---

## API Reference

### `<EditorProvider>`

Provides context and configuration for the editor. Must wrap the editor component to enable all features.

| Prop | Type | Required | Default | Description |
|---|---|---|---|---|
| `children` | `ReactNode` | Yes | — | The wrapped content (your editor component). |
| `defaultFontFamilies` | `string[]` | No | Built-in list | Override the list of available font families. Use `defaultEditorConfig.defaultFontFamilies` for the standard set. |
| `currentUser` | `CurrentUser` | No | — | User info for the comments feature. Object with `id`, `name`, and optional `email` and `avatar` fields. |
| `apiKey` | `string` | No | — | API key for read-only/preview mode contexts. |
| `mentionUserList` | `string[]` | No | `[]` | User names available for @mention autocomplete. |
| `enableLinkPreview` | `boolean` | No | `true` | Show link previews on hover for content inside the provider. |

### `<ConfigurableEditorWithAuth>`

The core editor component with built-in subscription verification and AI service integration.

| Prop | Type | Required | Default | Description |
|---|---|---|---|---|
| `apiKey` | `string` | **Yes** | — | Your Eddyter subscription key. |
| `initialContent` | `string` | No | `""` | Default HTML content to populate the editor. Use `initialContent`, NOT `value`. |
| `onChange` | `(html: string, meta?: { mode: "document" \| "email" }) => void` | No | — | Callback that fires on every content change, returning the current HTML string. The second argument says which surface produced it — `"email"` when the email builder is open. It is additive: `onChange={setHtml}` keeps working. See Email Templates. |
| `mode` | `"edit" \| "preview"` | No | `"edit"` | `"edit"` enables full editing; `"preview"` renders content read-only. |
| `toolbar` | `{ mode?: "sticky" \| "static"; offset?: number; zIndex?: number }` | No | `{ mode: "sticky", offset: 20, zIndex: 1000 }` | Toolbar positioning config. This is an OBJECT, not an array. |
| `editor` | `{ maxHeight?: React.CSSProperties["maxHeight"] }` | No | — | Editor area config. Use `maxHeight` in static toolbar mode to constrain the scrollable content area. |
| `uiFontFamily` | `string` | No | `"Geist"` | Font family for the editor's **UI chrome** (toolbar labels, menus, dialogs, sidebars) so it matches your app's typography. Does NOT change document content — that is chosen in the editor's font picker. Pass a single family name, not a CSS stack. See UI Font under Theming. |
| `contentFontFamily` | `string` | No | `"DM Sans"` | Starting font for the **document text**, shown as selected in the font picker. A default, not a lock — the user can pick another font and their choice wins. Applies to newly typed text only. NOT the same as `defaultFontFamilies` (which is the list of fonts offered). See UI Font / Content Font under Theming. |
| `mentionUserList` | `string[]` | No | `[]` | List of user names available for @mention autocomplete. |
| `emailMergeTags` | `EmailMergeTag[]` | No | `[]` | Personalization fields offered in the email builder. Each `value` is the inner name WITHOUT affixes (`"FNAME"`, not `"{{FNAME}}"`). Empty or omitted hides all merge-tag UI. See Email Templates. |
| `emailMergeTagPrefix` | `string` | No | `"{{"` | Typed to open the merge-tag menu, and wrapped around the inserted tag. Set to `"*|"` for Mailchimp syntax. |
| `emailMergeTagSuffix` | `string` | No | `"}}"` | Set to `"\|*"` for Mailchimp syntax. |
| `onAuthSuccess` | `() => void` | No | — | Fires when API key validation succeeds. |
| `onAuthError` | `(error: string) => void` | No | — | Fires when API key validation fails. |
| `customVerifyKey` | `(key: string) => Promise<{ success: boolean; message?: string }>` | No | — | Custom async function to verify the API key through your own backend. |
| `documentId` | `string` | No | — | Stable id that scopes version history to this document. Pass your record's primary key. See Version History below. |
| `defaultFontFamilies` | `string[]` | No | Built-in list | Font names offered in the font picker. |
| `darkMode` | `boolean` | No | Auto | `true` forces dark, `false` forces light. Omitted, it follows a `dark` class on the host's `<html>` or `<body>`. |
| `fitToPage` | `boolean` | No | `false` | Constrains the editor width and centres it like a document page. |
| `uiStyle` | `"default" \| "compact"` | No | `"default"` | Visual style only. `"compact"` has a bottom-bordered toolbar and transparent, borderless content. |
| `className` | `string` | No | — | Class for the editor wrapper. |
| `style` | `React.CSSProperties` | No | — | Style for the editor wrapper. |
| `containerClassName` | `string` | No | — | Class for the preview container (preview mode only). |
| `contentClassName` | `string` | No | — | Class for the editor content area. |
| `floatingToolbarClassName` | `string` | No | — | Class for floating elements and the portal container. |
| `onPreviewClick` | `() => void` | No | — | Click handler in preview mode, useful for switching to edit mode. |
| `onEditorReady` | `() => void` | No | — | React Native bridge: fires when the editor has loaded. |
| `onFocus` | `() => void` | No | — | React Native bridge: fires on focus. |
| `onBlur` | `() => void` | No | — | React Native bridge: fires on blur. |
| `onHeightChange` | `(height: number) => void` | No | — | React Native bridge: fires when the editor height changes. |
| `enableReactNativeBridge` | `boolean` | No | Auto | Enables the React Native WebView bridge. Detected automatically inside a WebView. |

### Version History

Server-backed, plan-gated document versioning. The editor ships the whole UI — a toolbar toggle and a side panel where users **save, name, preview, and restore** versions — and stores snapshots on Eddyter's backend, keyed by your account and a `documentId`. There is **no automatic/timed autosave**; a version is created only when Save is clicked.

**1. The one requirement — a correct per-document `documentId`.** It must uniquely and stably identify each document (pass your record's primary key, reuse it on reopen so history reconnects):

```tsx
<ConfigurableEditorWithAuth apiKey={apiKey} documentId={note.id} … />
```

URL fallback caveat: with no `documentId` the editor derives an id from the page URL (and warns). That's only safe when **every document has its own URL** (one editor per page). In a single-page/master-detail app where many documents share one URL (e.g. `/notes`), the fallback hashes them all to the SAME id and their histories COLLIDE — always pass a real per-record `documentId` there.

**2. Two ways a version gets created:**
- The panel's built-in **Save** button — works on its own for documents whose correct `documentId` is known at mount (existing records). No extra code.
- **`saveVersion()` from your own Save button** — the robust path for real apps. Required for brand-new records (whose id only exists after the first save) and whenever you want your app's Save button to also snapshot. Most integrations should wire this:

```tsx
import { useRef } from "react";
import { ConfigurableEditorWithAuth, EditorProvider, type EddyterEditorHandle } from "eddyter";

const editorRef = useRef<EddyterEditorHandle>(null);

async function handleSave() {
  // 1) Persist content to your backend first → get the record id (created here for a NEW record).
  const saved = await saveToYourBackend({ id: note?.id, content });
  // 2) Snapshot a version — pass the id explicitly so a new record's first save
  //    captures version 1. Do it BEFORE unmounting the editor (see gotcha below).
  try {
    await editorRef.current?.saveVersion({ documentId: saved.id });
  } catch (err) {
    console.error("Version save failed:", err); // non-fatal
  }
}

<EditorProvider apiKey={apiKey}>
  <ConfigurableEditorWithAuth
    key={note?.id ?? "new-note"}   // remount when switching documents
    ref={editorRef}
    apiKey={apiKey}
    documentId={note?.id}
    initialContent={content}
    onChange={setContent}
  />
</EditorProvider>
```

`saveVersion(opts?)` signature:

```ts
saveVersion(opts?: {
  label?: string;       // optional name shown in the history panel
  documentId?: string;  // overrides the prop for this call (e.g. a just-created record's id)
}): Promise<
  | { version: VersionMeta; deduped: boolean; documentId: string } // success
  | { skipped: true; reason: "unavailable" }                       // history not active
>
```

For a brand-new record whose id doesn't exist at mount, pass it at save time via `saveVersion({ documentId })` so the first save captures the original content as version 1.

Notes:
- **No autosave / dedup**: no timed snapshots — versions come from the panel's Save button or your `saveVersion()` call. Unchanged content returns `deduped: true` and creates nothing, so it's safe to save repeatedly.
- **Restore appends, never destroys**: restoring writes the old version as the new latest and backs up current content.
- **Plan-gated**: the toggle/panel appear only when the plan includes version history; otherwise a locked upsell shows.
- **Timing gotcha** (`saveVersion()` only): it reads the *mounted* editor. If your Save flow switches to a read-only/preview view, call `saveVersion()` BEFORE unmounting — otherwise it returns `{ skipped: "unavailable" }` and no version is created.

### Email Templates

Plan-gated. A second editing surface for building marketing emails, reached from the envelope button in the toolbar. It is a lazily-loaded chunk: nothing of it downloads until someone opens email mode.

**Integration is one prop you already have.** There is no separate email component, no extra config:

```tsx
import { ConfigurableEditorWithAuth, EditorProvider } from 'eddyter';
import 'eddyter/style.css';

function Composer() {
  const [documentHtml, setDocumentHtml] = useState('');
  const [emailHtml, setEmailHtml] = useState('');

  return (
    <EditorProvider apiKey={process.env.NEXT_PUBLIC_EDDYTER_KEY!}>
      <ConfigurableEditorWithAuth
        apiKey={process.env.NEXT_PUBLIC_EDDYTER_KEY!}
        onChange={(html, meta) => {
          // meta.mode tells you which surface produced this HTML.
          if (meta?.mode === 'email') setEmailHtml(html);
          else setDocumentHtml(html);
        }}
      />
    </EditorProvider>
  );
}
```

**Last-active-wins.** `onChange` reports whichever surface is being edited. Store both in one field and the last edit overwrites the other — build an email, switch to Document, type one character, and your field holds document HTML. Split on `meta.mode` as above, or give email its own column.

#### What you get back

The email **body**: a full-width page table carrying the page background, wrapping a centred 600px card, with the section tables stacked inside it. The page background and centring are already handled.

```html
<table role="presentation" width="100%" bgcolor="#f4f4f5" data-ed-email-shell="1">
  <tbody><tr><td align="center" style="padding:0">
    <!--[if mso]><table role="presentation" width="600" align="center"><tr><td><![endif]-->
    <div data-ed-email-card="1" style="max-width:600px;margin:0 auto">
      <table role="presentation" width="100%" data-ed-email-section="1">...</table>
    </div>
    <!--[if mso]></td></tr></table><![endif]-->
  </td></tr></tbody>
</table>
```

Wrap it in a document with `wrapEmailDocument` before sending. Do not hand-write the `<head>`. `wrapEmailDocument` and `EMAIL_RESPONSIVE_CSS` require an `eddyter` release newer than 1.4.27; in 1.4.27 and older, these imports do not exist.

```ts
import { wrapEmailDocument } from 'eddyter';

// Any sending library takes the result as-is:
await sgMail.send({
  to,
  from,
  subject,
  html: wrapEmailDocument(emailHtml, { title: subject }),
});
```

```ts
wrapEmailDocument(bodyHtml: string, options?: { title?: string; lang?: string }): string
```

`title` and `lang` are both escaped, so a user-supplied subject can be passed straight through. It is exported from the package root and is a pure string function — no editor instance, no React, and importing it does not pull the email chunk into your main bundle.

**Three things can only live in a `<head>`, and each fails silently in exactly one client:**

| In the `<head>` | Without it |
| --- | --- |
| The responsive stylesheet (`EMAIL_RESPONSIVE_CSS`) | **Gmail does not stack multi-column rows on a phone.** Gmail applies a `<style>` only from `<head>` — it retains the copy at the top of the exported fragment and ignores it. Verified in the Gmail Android app: the same email stacked with the wrapper and stayed side-by-side without it. |
| `<o:PixelsPerInch>96</o:PixelsPerInch>` | Outlook's Word engine assumes 120 DPI, so every declared width renders 25% too large — the 600px card becomes 750px and overflows. |
| `x-apple-disable-message-reformatting` | iOS Mail raises small text to its own minimum, breaking a layout built to a fixed width. |

**Already hand-writing a shell?** It almost certainly lacks the responsive stylesheet, because earlier versions of this page told you to add only a doctype and `<head>`. Columns in those emails do not stack in Gmail. Either switch to `wrapEmailDocument`, or add the CSS to your own `<head>`:

```ts
import { EMAIL_RESPONSIVE_CSS } from 'eddyter';
// ...
`<style>${EMAIL_RESPONSIVE_CSS}</style>`
```

Do NOT wrap the body in a second centring table or background cell — the export already has one, and nesting them produces a 600px email with a 300px-wide coloured band.

#### Merge tags (personalization)

**The host owns the field list; Eddyter neither invents nor stores it.** The editor has no knowledge of what `FNAME` means or which fields a given email platform supports, so the integrating application declares them and passes them in as a prop — typically mapped from the ESP's merge fields, a CRM, or the app's own database columns, and often per-account where end users have custom fields.

End users **pick from the supplied list**; there is no UI for them to create a field. (This matches TinyMCE, whose `mergetags_list` is likewise developer configuration with no end-user editor.) The escape hatch is that an undeclared placeholder typed by hand still exports verbatim — see below.

```tsx
<ConfigurableEditorWithAuth
  apiKey={process.env.NEXT_PUBLIC_EDDYTER_KEY!}
  emailMergeTags={[
    { value: 'FNAME',   name: 'First name', sample: 'Alex',   group: 'Contact' },
    { value: 'COMPANY', name: 'Company',    sample: 'Acme',   group: 'Contact' },
    { value: 'unsubscribe_url', name: 'Unsubscribe URL', group: 'System' },
  ]}
  onChange={(html, meta) => {
    if (meta?.mode === 'email') setEmailHtml(html);
  }}
/>
```

| Prop | Type | Default | Notes |
| --- | --- | --- | --- |
| `emailMergeTags` | `EmailMergeTag[]` | `[]` | Omitted or empty hides every merge-tag control — no menu, no panel row, no `{}` button. |
| `emailMergeTagPrefix` | `string` | `"{{"` | Typed to open the field menu **and** wrapped around the inserted tag. |
| `emailMergeTagSuffix` | `string` | `"}}"` | |

```ts
interface EmailMergeTag {
  value: string;    // inner name WITHOUT affixes — "FNAME", never "{{FNAME}}"
  name?: string;    // label in the picker; defaults to `value`
  sample?: string;  // stands in for the tag in Preview
  group?: string;   // optional heading in the picker
}
```

- **The list is normally fetched, not hard-coded.** `emailMergeTags={mergeTags}` where `mergeTags` came from `fetch('/api/merge-fields')` is the expected shape. A fetched array is a new reference every render, but it does NOT need `useMemo`: the internal config is rebuilt from the array's **contents**, not its identity. While the request is in flight the list is empty, and an empty list hides every merge-tag control — so the menu appearing a moment after mount is expected.
- **`value` excludes the affixes.** `"{{FNAME}}"` is rejected with a console warning; the editor adds the prefix/suffix, so accepting it would send `{{{{FNAME}}}}`.
- **The prefix is the trigger.** `emailMergeTagPrefix="*|"` + `emailMergeTagSuffix="|*"` means users type `*|` and get `*|FNAME|*`. One setting, so the trigger and the syntax cannot drift apart.
- **A tag behaves as one character.** Backspace removes the whole thing; the caret never lands inside it; it inherits bold/colour/size from the surrounding text, so a tag in a bold red heading exports bold and red.
- **Tags work in URLs.** Select a button or link and the styles panel shows its URL with a `{}` button. `{{unsubscribe_url}}` as the whole href and `https://shop.com/o/{{ORDER_ID}}` both survive the export untouched.
- **Unknown placeholders are left alone.** Text like `{{ total | money }}` that is not in your list stays plain text and exports verbatim. The panel counts them; nothing is auto-corrected and nothing blocks the export. This is the escape hatch for Twig/Liquid filter syntax a flat field list cannot express.
- **`onChange` never substitutes.** You always receive the raw placeholders — your email platform fills them in. The builder's **Preview** button substitutes each `sample` for display only (a field with no `sample` shows as `[First name]`).

To render a filled-in copy yourself, the same helper the builder uses is exported:

```ts
import { substituteMergeTags } from 'eddyter';

// keyed by the inner `value`, not the affixed text; values are HTML-escaped
const preview = substituteMergeTags(emailHtml, { FNAME: 'Alex', COMPANY: 'Acme' });

// non-default affixes
substituteMergeTags(emailHtml, { FNAME: 'Alex' }, { prefix: '*|', suffix: '|*' });
```

#### Constraints

- **No persistence.** The email canvas is in-memory for the session; a reload gives a blank canvas. Your app stores the HTML it received.
- **No re-editing.** Email HTML is for sending, not loading back. Passing it as `initialContent` is refused with a console warning — the document editor's table importer would flatten a built email into an editable data table.
- **Images need absolute `https://` URLs.** Upload through the editor (which returns CDN URLs); `data:` URIs are stripped by most clients and rejected here.
- **No web fonts.** Outlook, Gmail and Yahoo ignore `@font-face`, so the builder offers only stacks that work everywhere.
- **Keep the total under ~102KB.** Gmail clips beyond that and hides the rest behind "View entire message".

#### Starter templates

Six designs in a gallery that opens on the first switch to email mode, and from **Templates** above the canvas. They ship no photographs (each picture is an empty placeholder you fill from your own media, so a sent email never depends on a URL Eddyter hosts) and no links (buttons have an empty `href`, so the author is prompted for the real one). Picking a template or "Start from blank" asks before replacing existing work; both are undoable with Ctrl/Cmd+Z.

#### Narrow containers

The builder measures its own width, not the window's. Below 1000px the styles panel moves into a sheet opened from **Styles**; below 720px the block palette joins it under **Blocks** and tapping a block inserts it instead of dragging. No feature is dropped at any width.

### `CurrentUser` Type

```tsx
interface CurrentUser {
  id: string;       // Unique user identifier
  name: string;     // Display name
  email?: string;   // Optional user email
  avatar?: string;  // Optional avatar URL
}
```

### Helper Exports

| Export | Type | Description |
|---|---|---|
| `defaultEditorConfig` | `object` | Default values (e.g. `defaultFontFamilies`). Note: feature toggles inside it are set by your plan, not from code. |
| `EddyterEditorHandle` | `type` | Ref handle type for `<ConfigurableEditorWithAuth>`, exposing `saveVersion()` for version history. |
| `useEditor` | `hook` | Access the editor context. |
| `useHtmlView`, `HtmlViewProvider` | `hook`, `component` | Access and provide HTML view state. |
| `verifyApiKey` | `function` | Verify an API key programmatically. |
| `isVersionSaved` | `function` | Tells a saved `saveVersion()` result from a skipped one. |
| `ContentPreview` | `component` | Displays saved editor content outside the editor. |
| `LinkPreviewHover` | `component` | Standalone link preview on hover. |
| `EddyterMark` | `component` | The animated Eddyter mark, for loading and error states. |
| `substituteMergeTags` | `function` | Fills merge tags in email HTML. |
| `wrapEmailDocument` | `function` | Wraps exported email HTML in a sendable document. Requires a release newer than 1.4.27. |
| `EMAIL_RESPONSIVE_CSS` | `string` | The responsive stylesheet for hand-written shells. Requires a release newer than 1.4.27. |
| `useReactNativeBridge`, `isReactNativeWebView` | `hook`, `function` | React Native WebView bridge helpers. |

---

## Code Examples

### Basic Editor

```tsx
"use client";

import { ConfigurableEditorWithAuth, EditorProvider } from 'eddyter';
import 'eddyter/style.css';

export default function BasicEditor() {
  return (
    <EditorProvider>
      <ConfigurableEditorWithAuth
        apiKey="YOUR_API_KEY"
        onAuthSuccess={() => console.log('Ready!')}
      />
    </EditorProvider>
  );
}
```

### Editor with State Management

```tsx
"use client";

import { useState } from 'react';
import { ConfigurableEditorWithAuth, EditorProvider } from 'eddyter';
import 'eddyter/style.css';

export default function EditorWithState() {
  const [content, setContent] = useState('<p>Start writing...</p>');

  return (
    <EditorProvider>
      <ConfigurableEditorWithAuth
        apiKey="YOUR_API_KEY"
        initialContent={content}
        onChange={setContent}
      />
    </EditorProvider>
  );
}
```

### Minimal Setup

Which features/toolbar buttons are available is decided by the plan tied to your
license key (managed in the Eddyter dashboard) — not by props. A minimal
integration is just the `apiKey`:

```tsx
"use client";

import { ConfigurableEditorWithAuth, EditorProvider } from 'eddyter';
import 'eddyter/style.css';

export default function MinimalEditor() {
  return (
    <EditorProvider>
      <ConfigurableEditorWithAuth
        apiKey="YOUR_API_KEY"
        toolbar={{ mode: "sticky", offset: 0 }}
      />
    </EditorProvider>
  );
}
```

### Full-Featured Editor with Comments and Mentions

```tsx
"use client";

import { useState } from 'react';
import {
  ConfigurableEditorWithAuth,
  EditorProvider,
  defaultEditorConfig
} from 'eddyter';
import 'eddyter/style.css';

export default function FullEditor() {
  const [content, setContent] = useState('');

  const currentUser = {
    id: 'user-001',
    name: 'Jane Smith',
    email: 'jane@example.com',
    avatar: 'https://example.com/jane.jpg'
  };

  return (
    <EditorProvider
      defaultFontFamilies={defaultEditorConfig.defaultFontFamilies}
      currentUser={currentUser}
    >
      <ConfigurableEditorWithAuth
        apiKey={process.env.NEXT_PUBLIC_EDITOR_API_KEY!}
        initialContent={content}
        onChange={setContent}
        mentionUserList={['Alice', 'Bob', 'Charlie', 'David']}
        toolbar={{ mode: "sticky", offset: 64, zIndex: 1200 }}
        onAuthSuccess={() => console.log('Editor authenticated!')}
        onAuthError={(err) => console.error('Auth failed:', err)}
      />
    </EditorProvider>
  );
}
```

### Preview / Read-Only Mode

```tsx
"use client";

import { ConfigurableEditorWithAuth, EditorProvider } from 'eddyter';
import 'eddyter/style.css';

export default function PreviewMode({ content }: { content: string }) {
  return (
    <EditorProvider>
      <ConfigurableEditorWithAuth
        apiKey="YOUR_API_KEY"
        initialContent={content}
        mode="preview"
      />
    </EditorProvider>
  );
}
```

---

## Theming

### CSS Variable Customization

The editor exposes CSS variables under the `--cteditorf47ac10b-*` namespace. Apply overrides inside a `.eddyter-scope` selector to customize colors, backgrounds, borders, and more.

```css
.eddyter-scope {
  --cteditorf47ac10b-background: #ffffff;
  --cteditorf47ac10b-foreground: #1a1a1a;
  --cteditorf47ac10b-primary: #2563eb;
  --cteditorf47ac10b-border: #e5e7eb;
  /* ... additional variables */
}
```

### UI Font

Match the editor's chrome to your app's typography with the `uiFontFamily` prop:

```tsx
<ConfigurableEditorWithAuth apiKey={apiKey} uiFontFamily="Poppins" />
```

This changes **UI chrome only** — toolbar labels, dropdown items, dialogs, sidebars, toasts. **Document content is unaffected**; the content font is chosen by the end user in the editor's font-family picker and defaults to DM Sans. Do not use `uiFontFamily` to try to set the document font.

**Loading is automatic — you normally do not add a font link yourself:**

- A system font (Arial, Georgia, Verdana, Times New Roman, …) is used directly, with no network request.
- Any Google Fonts family is fetched automatically, including accented and non-Latin names (e.g. `"Caacupé One"`).
- A font your page already declares via `@font-face` (a licensed, Adobe Fonts, or self-hosted font) is detected and used as-is, with no request to Google.

**Value rules:**

- Pass a **single family name**, not a CSS stack. `uiFontFamily="Poppins"` is correct; `uiFontFamily="Poppins, sans-serif"` is rejected.
- CSS generic keywords (`serif`, `sans-serif`, `monospace`, `system-ui`) are not supported — the value is treated as a family name.
- An unrecognised or malformed name is ignored and the editor falls back to Geist, so a typo degrades quietly rather than breaking the layout.

For fonts outside the editor's built-in list, only the regular weight is fetched, so bold chrome is synthesised by the browser. This is cosmetic and affects nothing functionally.

### Content Font

`uiFontFamily` (above) styles the interface. `contentFontFamily` sets the starting font for the **document text** the user writes:

```tsx
<ConfigurableEditorWithAuth
  apiKey={apiKey}
  uiFontFamily="Poppins"     // toolbar, menus, dialogs
  contentFontFamily="Lora"   // the text users write
/>
```

**Do not confuse the three font props:**

- `contentFontFamily` (`string`) — the starting font for document text.
- `uiFontFamily` (`string`) — the font of the editor's interface.
- `defaultFontFamilies` (`string[]`) — the LIST of fonts offered in the picker.

**Behaviour:**

- The font shows as the selected value in the toolbar's font picker, and is applied to text as the user types.
- It is a **default, not a lock**. The user can pick any other font from the picker and their choice wins.
- If the user has **pinned** their own default font (a per-browser preference set from the picker), the pin takes precedence over this prop.
- It applies to **newly typed text only**. Content loaded via `initialContent` that carries no font of its own keeps rendering in the editor's base font (DM Sans), so a pre-existing document can display both fonts until its text is rewritten.
- Loading and value rules are the same as `uiFontFamily`: system fonts and Google Fonts families are handled automatically, a single family name is required (not a CSS stack), and an unrecognised name is ignored.

### Force Theme

If your application supports only one theme:

- **Dark-only:** Target `.eddyter-scope` with dark-mode variables.
- **Light-only:** Target `.dark .eddyter-scope.dark` with light-mode variables.

See the full CSS variable list with default values at https://eddyter.com/docs#theming.

---

## Common Pitfalls

| Mistake | Fix |
|---|---|
| Importing `'eddyter/dist/style.css'` | Use `'eddyter/style.css'` — no `dist/` prefix. |
| Using `value` prop for content | Use `initialContent` — there is no `value` prop. |
| Passing `toolbar` as an array | `toolbar` is an OBJECT: `{ mode, offset, zIndex }`. |
| Missing `"use client"` in Next.js | Add `"use client"` at the top of your editor component file. |
| Committing API key to source code | Store in environment variables (e.g. `NEXT_PUBLIC_EDITOR_API_KEY`). |
| Reading `error.message` in `onAuthError` | `onAuthError` receives a `string`, not an `Error`. |
| Passing `aiOptions={{ autocomplete: true }}` (or any AI/feature prop) | No such prop exists. Turn AI features on for the API key in the dashboard ([License Keys](https://eddyter.com/user/license-key) → edit key → "AI & smart features"); the plan must include AI. |
| Wrapping editor without `EditorProvider` | `ConfigurableEditorWithAuth` must be inside `<EditorProvider>`. |
| Editor overflows / is cut off inside a flex or grid column | The host flex/grid item won't shrink (`min-width: auto` default). Add `min-w-0` to the flex item that wraps the editor, or size the grid track with `minmax(0, 1fr)` instead of `1fr`. See "Responsive Layout" above; the editor also logs a `[Eddyter]` console warning naming the exact ancestor to fix. |
| `saveVersion()` returns `{ skipped: "unavailable" }` | The editor was unmounted before the call (e.g. you switched to preview/read-only in your Save handler). Call `saveVersion()` while the editor is still mounted, then unmount. Also check the plan includes version history. |
| Version history panel not appearing | It's plan-gated — the account's plan must include version history (otherwise a locked upsell shows). The toggle lives in the toolbar (edit mode only). |
| Your stored HTML turned into document HTML after building an email | `onChange` reports whichever surface is active, so the last edit wins. Split on `meta.mode === "email"` or store the email in its own field. |
| The email is 600px wide but its coloured bands are half that | You wrapped the export in your own centring table or background cell. It already ships a full-width page table around a 600px card — wrap it with `wrapEmailDocument` and nothing else. |
| Two-column rows do not stack on a phone in Gmail (but do in Apple Mail) | Your sending shell has no responsive `<style>` in its `<head>`. Gmail applies a `<style>` only from `<head>` and ignores the copy carried at the top of the exported fragment. Use `wrapEmailDocument`, or add `EMAIL_RESPONSIVE_CSS` to your own `<head>`. |
| Every width in Outlook is ~25% too large and the layout overflows | The `<head>` is missing the `<o:PixelsPerInch>96</o:PixelsPerInch>` block, so the Word engine assumes 120 DPI. `wrapEmailDocument` includes it. |
| Feeding saved email HTML back as `initialContent` | Not supported and refused with a console warning. Email HTML is for sending; the document editor's table importer would flatten it into an editable data table. |
| Expecting Eddyter to supply a built-in list of merge tags | There is no built-in list and no dashboard setting for one. The integrating app declares the fields via the `emailMergeTags` prop, built from its own ESP/CRM/database. Eddyter only renders and exports what it is given. |
| Merge tags arriving as `{{{{FNAME}}}}` | `emailMergeTags[].value` is the inner name only — `"FNAME"`, not `"{{FNAME}}"`. The editor adds the prefix and suffix. A value carrying its own affixes is dropped with a console warning. |
| No merge-tag menu appears when typing `{{` | Either `emailMergeTags` is empty/omitted (which hides the whole feature by design), or `emailMergeTagPrefix` is set to something else — the prefix IS the trigger. |
| A typed `{{ total \| money }}` never became a pill | Only declared tags convert. Undeclared placeholders stay plain text on purpose and export verbatim, so platform filter syntax keeps working; the styles panel counts them as a warning, not an error. |
| Expecting `onChange` to return filled-in values | It always returns raw placeholders — substitution is your email platform's job. Use the exported `substituteMergeTags(html, values)` if you need a filled copy for a test send. |
| Email builder toggle missing | Plan-gated like version history — the plan must include Email Templates, otherwise the envelope button shows as locked. |

---

## Migrating from Legacy Editors

Eddyter is designed as a drop-in replacement for legacy WYSIWYG editors. If you're migrating from TinyMCE, CKEditor, Quill, or similar:

1. **Content compatibility:** Eddyter accepts standard HTML via `initialContent`, so existing stored content works out of the box.
2. **Event model:** Replace `onChange` / `onEditorChange` callbacks with Eddyter's `onChange` prop, which returns an HTML string.
3. **Toolbar mapping:** Most toolbar features (bold, italic, tables, lists, links, images) have direct equivalents. Which features are enabled is set by your plan in the Eddyter dashboard, not in code.
4. **AI upgrade:** Unlike legacy editors, AI features (chat, autocomplete, refinement, image generation) are built in — no plugins or third-party integrations required.

---

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
