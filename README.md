<p align="center">
  <a href="https://eddyter.com">
    <img src="https://dsckj64r0usqg.cloudfront.net/assets/Logo.svg" width="120" alt="Eddyter">
  </a>
</p>

<h1 align="center">Eddyter</h1>

<p align="center">
  <b>Write and Edit with AI — Smarter, faster, effortlessly.</b><br>
  The plug-and-play rich text editor SDK for developers.
</p>

<p align="center">
  <a href="https://eddyter.com">Live demo</a> ·
  <a href="https://eddyter.com/docs">Documentation</a> ·
  <a href="https://github.com/EddyterAI/examples">Examples</a> ·
  <a href="https://www.npmjs.com/package/eddyter">npm</a> ·
  <a href="https://discord.com/channels/1457444571104542803/1457444572081819690">Discord</a>
</p>

---

## What is this repo?

This is the **public issue tracker and discussion home** for Eddyter. The editor itself ships as the [`eddyter`](https://www.npmjs.com/package/eddyter) npm package — **this repo doesn't host the source.** Use it to:

- 🐛 [**Report a bug**](https://github.com/EddyterAI/eddyter/issues/new?template=bug_report.yml)
- 💡 [**Request a feature**](https://github.com/EddyterAI/eddyter/issues/new?template=feature_request.yml)
- ❓ [**Ask a question**](https://github.com/EddyterAI/eddyter/discussions)

## Get started

```bash
npm install eddyter
```

```tsx
import {
  ConfigurableEditorWithAuth,
  EditorProvider,
  defaultEditorConfig,
} from "eddyter";
import "eddyter/style.css";

<EditorProvider
  defaultFontFamilies={defaultEditorConfig.defaultFontFamilies}
  currentUser={{ id: "u1", name: "Akash", email: "akash@example.com" }}
>
  <ConfigurableEditorWithAuth
    apiKey={process.env.NEXT_PUBLIC_EDDYTER_API_KEY!}
    onChange={(html) => console.log(html)}
  />
</EditorProvider>;
```

That's it. AI, storage, and infra are handled. Get a free API key at [eddyter.com](https://eddyter.com).

[**Full React example →**](https://github.com/EddyterAI/examples/tree/main/react)

## Framework support

- ✅ **React** — ships today
- ✅ **Next.js** — use `eddyter` with `next/dynamic` ([guide](https://github.com/EddyterAI/examples/tree/main/react#using-in-nextjs))
- 🔜 **Vue** — coming soon
- 🔜 **Svelte** — coming soon

## Why Eddyter

- ⚡ **AI + Storage + Infra = One decision** — one subscription, three vendors gone
- 🛠 **Editors aren't your core product** — 100+ hours → 10 minutes
- 💸 **Engineering time is expensive** — every hour on editor setup is one not spent on what matters
- 🎛 **Sane defaults beat endless config** — most teams need it to work, not infinite knobs

## Communication

Pick the right channel for what you need:

| What | Where |
|---|---|
| Bug in the editor | [Bug report issue](https://github.com/EddyterAI/eddyter/issues/new?template=bug_report.yml) |
| Feature idea | [Feature request issue](https://github.com/EddyterAI/eddyter/issues/new?template=feature_request.yml) |
| Question / how-to | [GitHub Discussions](https://github.com/EddyterAI/eddyter/discussions) |
| Real-time chat | [Discord](https://discord.com/channels/1457444571104542803/1457444572081819690) |
| Account / billing | [eddyter.com support](https://eddyter.com) |

## Stay in the loop

[Twitter / X](https://x.com/EddyterAI) · [LinkedIn](https://www.linkedin.com/company/eddyter) · [YouTube](https://www.youtube.com/@AIEddyter) · [Instagram](https://www.instagram.com/eddyterai/) · [Facebook](https://www.facebook.com/profile.php?id=61587640901054)

---

<p align="center">
  Built by <a href="https://eddyter.com">Craxinno Technologies</a>.
</p>
