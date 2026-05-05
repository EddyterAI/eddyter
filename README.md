<p align="center">
  <a href="https://eddyter.com">
    <img src="https://dsckj64r0usqg.cloudfront.net/assets/Logo.svg" width="120" alt="Eddyter">
  </a>
</p>

<h1 align="center">Eddyter</h1>

<p align="center">
  <b>Write and Edit with AI — Smarter, faster, effortlessly.</b><br>
  The plug-and-play rich text editor SDK for developers.<br>
  Drop into your React (or Next.js) app in 10 minutes.<br>
  AI, storage, and infra fully managed.
</p>

<p align="center">
  <a href="https://eddyter.com"><img src="https://img.shields.io/badge/demo-eddyter.com-orange" alt="Live demo"></a>
  <a href="https://eddyter.com/docs"><img src="https://img.shields.io/badge/docs-eddyter.com-blue" alt="Docs"></a>
  <a href="https://www.npmjs.com/package/eddyter"><img src="https://img.shields.io/npm/v/eddyter" alt="npm eddyter"></a>
  <a href="https://discord.com/channels/1457444571104542803/1457444572081819690"><img src="https://img.shields.io/badge/discord-join-5865F2" alt="Discord"></a>
</p>

<p align="center">
  ▶️ <b><a href="https://youtu.be/oNHBa-DImZc">Watch: What is Eddyter? (90 seconds)</a></b> &nbsp;·&nbsp; <b><a href="https://youtu.be/5lTjRFjUWgs">Watch: Integrate Eddyter in 30 minutes</a></b>
</p>

---

## Why teams choose Eddyter

> *The editor isn't your core product. Speed is.*

- ⚡ **AI + Storage + Infra = One decision** — stop managing three vendors, get everything in one subscription.
- 🛠 **Editors aren't your core product** — why spend 100+ hours on something that should take 10 minutes?
- 💸 **Engineering time is expensive** — every hour on editor setup is an hour not spent on what matters.
- 🎛 **Sane defaults beat endless config** — most teams don't need infinite flexibility, they need it to work.

## What's inside

- 🤖 **AI-powered features** — ask anything, generate images, rewrite, corrections, change models, tone options
- ⚡ **Quick Action** — instant transformations on selected text, one click
- 🎙 **Record. Transcribe.** — voice → clean editable text, zero typing
- 💬 **Comment on Text** — real-time inline comments and feedback
- ✍️ **Sentence correction** — grammar, clarity, and flow in one click
- 🖼 **Image generation** — built-in, multi-provider (OpenAI / Grok / Gemini)
- 🔌 **BYOK or managed** — bring your own API keys, or use ours

## Quick start

### 1. Get your API key

[Grab a free API key →](https://eddyter.com/user/license-key)

### 2. Install

```bash
npm install eddyter
```

### 3. Integrate

```tsx
"use client";

import { ConfigurableEditorWithAuth, EditorProvider } from "eddyter";
import "eddyter/style.css";

export default function App() {
  return (
    <EditorProvider>
      <ConfigurableEditorWithAuth
        apiKey={process.env.NEXT_PUBLIC_EDDYTER_API_KEY}
        onChange={(html) => console.log(html)}
        initialContent="<p>Start writing...</p>"
      />
    </EditorProvider>
  );
}
```

> **Next.js note:** the `"use client"` directive at the top is required for Next.js App Router. It's harmless in plain React / Vite — leave it in.

That's the full integration. AI, storage, and infra are handled for you.

[**See the full React example →**](https://github.com/EddyterAI/examples/tree/main/react)

## Framework support

| Framework | Status |
|---|---|
| **React** | ✅ Ships today via [`eddyter`](https://www.npmjs.com/package/eddyter) |
| **Next.js** | ✅ Use `eddyter` with `next/dynamic` — [example](https://github.com/EddyterAI/examples/tree/main/react#using-in-nextjs) |
| **Vue** | 🔜 Coming soon |
| **Svelte** | 🔜 Coming soon |

## Resources

- 🎮 [**Live demo**](https://eddyter.com) — try the editor without signing up
- 📘 [**Documentation**](https://eddyter.com/docs) — full API reference and guides
- 💡 [**Examples**](https://github.com/EddyterAI/examples) — runnable starter projects
- 🐛 [**Issues & feedback**](https://github.com/EddyterAI/eddyter/issues) — report bugs or request features
- 💬 [**Discussions**](https://github.com/EddyterAI/eddyter/discussions) — community Q&A

## Stay in the loop

[Twitter / X](https://x.com/EddyterAI) · [LinkedIn](https://www.linkedin.com/company/eddyter) · [YouTube](https://www.youtube.com/@AIEddyter) · [Instagram](https://www.instagram.com/eddyterai/) · [Facebook](https://www.facebook.com/profile.php?id=61587640901054) · [Discord](https://discord.com/channels/1457444571104542803/1457444572081819690)

---

<p align="center">
  Built by <a href="https://eddyter.com">Craxinno Technologies</a> · Made for developers who'd rather ship product than build editors.
</p>
