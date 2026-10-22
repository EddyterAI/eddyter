<!--
  This file is the organization profile shown at github.com/EddyterAI.
  Lines marked TODO need a real link or confirmed fact before publishing.
-->

<p align="center">
  <a href="https://www.eddyter.com">
    <!-- TODO: replace with a 10–15 second GIF of the slash menu, AI rewrite and a table -->
    <img src="https://dsckj64r0usqg.cloudfront.net/assets/laptop-mockup.png" alt="Eddyter, the AI rich text editor for React, Next.js, Vue, Angular, Svelte and Laravel" width="760">
  </a>
</p>

<h1 align="center">Eddyter</h1>

<p align="center">
  <strong>The plug-and-play AI rich text editor for your app.</strong><br>
  Integrate in 10 minutes. AI, storage and infrastructure are fully managed.
</p>

<p align="center">
  <a href="https://www.eddyter.com">Live demo</a> ·
  <a href="https://eddyter.com/docs">Docs</a> ·
  <a href="https://github.com/EddyterAI/examples">Examples</a> ·
  <a href="https://www.eddyter.com/user/license-key">Free API key</a> ·
  <a href="https://github.com/EddyterAI/eddyter/discussions">Community</a>
</p>

<p align="center">
  Works with <b>React</b> · <b>Next.js</b> · <b>Vue</b> · <b>Angular</b> · <b>Svelte</b> · <b>Laravel</b> · <b>React Native</b> · <b>Vanilla JS</b>
</p>

<!-- TODO: uncomment once translated READMEs exist in the eddyter repo
<p align="center">
  <a href="https://github.com/EddyterAI/eddyter/blob/main/README.md">English</a> ·
  <a href="https://github.com/EddyterAI/eddyter/blob/main/README.zh-CN.md">简体中文</a> ·
  <a href="https://github.com/EddyterAI/eddyter/blob/main/README.es.md">Español</a> ·
  <a href="https://github.com/EddyterAI/eddyter/blob/main/README.pt-BR.md">Português</a> ·
  <a href="https://github.com/EddyterAI/eddyter/blob/main/README.ja.md">日本語</a>
</p>
-->

---

## What is Eddyter?

Eddyter is a ready-made rich text editor you add to your product with one component. It includes AI writing tools, comments, tables, media, Markdown, version history and an email template builder, so your team doesn't have to build, host or maintain an editor.

It's used for blogs and CMS platforms, CRMs and helpdesks, SaaS dashboards, learning platforms and internal tools.

▶️ [What is Eddyter? (90 seconds)](https://youtu.be/oNHBa-DImZc) · [Integrate Eddyter step by step](https://youtu.be/5lTjRFjUWgs)

## Why teams choose Eddyter

**The editor isn't your core product. Shipping is.**

| | |
|---|---|
| **One subscription, not three vendors** | AI, storage and infrastructure in one place. |
| **10 minutes, not 100 hours** | Don't spend a quarter building and maintaining an editor. |
| **Your choice of AI** | Bring your own API keys, or use ours. Switch models any time. |
| **Sensible defaults** | It works out of the box. Turn features on or off from your dashboard, without code changes. |

## What's inside

| Feature | What you get |
|---|---|
| AI writing | Ask anything, rewrite, shorten, expand, fix grammar and change tone, with switchable models |
| Quick actions | One-click changes to selected text |
| Voice input | Speak, and get clean, editable text |
| Inline comments and @mentions | Feedback on any text selection |
| Image generation | Built in, with OpenAI, Grok or Gemini |
| Tables, media and embeds | Resizable tables, images, video, files and link previews |
| Markdown | Import, export and shortcuts |
| Version history | Save, preview and restore document versions |
| Email templates | Drag-and-drop builder with email-safe HTML and merge tags |

## Quick start

**1. Get a free API key** at [eddyter.com](https://www.eddyter.com/user/license-key)

**2. Install**

```bash
npm install eddyter
```

**3. Add the editor**

```tsx
'use client'; // Needed for the Next.js App Router. Harmless in plain React or Vite.

import { useState } from 'react';
import { ConfigurableEditorWithAuth, EditorProvider } from 'eddyter';
import 'eddyter/style.css';

export default function App() {
  const [html, setHtml] = useState('');

  return (
    <EditorProvider>
      <ConfigurableEditorWithAuth
        apiKey={process.env.NEXT_PUBLIC_EDDYTER_API_KEY!}
        onChange={setHtml}
      />
    </EditorProvider>
  );
}
```

That's the full integration. Content arrives as HTML in `onChange`, ready to save in your own database.

[See the full React example →](https://github.com/EddyterAI/examples)

## Framework support

| Framework | Package |
|---|---|
| React | [`eddyter`](https://www.npmjs.com/package/eddyter) |
| Next.js | [`eddyter`](https://www.npmjs.com/package/eddyter) with the `'use client'` directive |
| Vue / Nuxt | [`@eddyter/vue`](https://www.npmjs.com/package/@eddyter/vue) |
| Angular | [`@eddyter/angular`](https://www.npmjs.com/package/@eddyter/angular) |
| Svelte | [`@eddyter/svelte`](https://www.npmjs.com/package/@eddyter/svelte) |
| Laravel (Blade, Livewire) | [`eddyter/laravel`](https://github.com/EddyterAI/laravel) |
| React Native | [`@eddyter/react-native`](https://www.npmjs.com/package/@eddyter/react-native) |
| Vanilla JS / other | [`@eddyter/core`](https://www.npmjs.com/package/@eddyter/core) |
| AI coding tools (Claude, Cursor, Codex) | [`eddyter-mcp`](https://github.com/EddyterAI/eddyter-mcp) |

## For businesses

<!-- TODO: confirm each statement and replace links before publishing -->
- **Your content stays yours.** The editor returns HTML to your app, and you store it where you choose.
- **Privacy and data handling:** [Privacy policy](https://www.eddyter.com/privacy-policy)
- **Security:** see our [security policy](https://github.com/EddyterAI/.github/blob/main/SECURITY.md)
- **Licensing, invoices and enterprise plans:** [contact us](mailto:support@eddyter.com)

<!-- TODO: add when you have permission from customers
## Trusted by
<p>
  <img src="LOGO_URL" alt="Company" height="32">
</p>
-->

## Community and support

| | |
|---|---|
| 📘 [Documentation](https://eddyter.com/docs) | Guides and full API reference |
| 💡 [Examples](https://github.com/EddyterAI/examples) | Runnable starter projects for each framework |
| 💬 [Discussions](https://github.com/EddyterAI/eddyter/discussions) | Questions, ideas and show-and-tell |
| 🐛 [Issues](https://github.com/EddyterAI/eddyter/issues/new/choose) | Bug reports and feature requests |
| 🎮 [Discord](https://discord.com/channels/1457444571104542803/1457444572081819690) | Chat with the team and other developers |

If Eddyter saves you time, please ⭐ [star the repository](https://github.com/EddyterAI/eddyter). It helps other developers find it.

**Stay in the loop:** [X](https://x.com/EddyterAI) · [LinkedIn](https://www.linkedin.com/company/eddyter) · [YouTube](https://www.youtube.com/@AIEddyter) · [Instagram](https://www.instagram.com/eddyterai/) · [Facebook](https://www.facebook.com/profile.php?id=61587640901054)

---

<sub>Built by <a href="https://craxinno.com">Craxinno Technologies</a> for developers who would rather ship their product than build an editor.</sub>
