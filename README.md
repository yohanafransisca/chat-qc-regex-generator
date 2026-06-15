# Regex Generator for Chat QC

An internal tool for Shopee chat QC. Ops paste a violating chat message, confirm the
violating keyword (auto-detected or typed), and the tool generates an **evasion-proof
regex** in the QC system's dialect — then lets them **test** it against sample text
before adding it to the QC system.

## Features

- **Keyword mode** — keyword → evasion permutations (leetspeak, spacing, repeats,
  regional-language variants) → regex (≤255 chars, auto-split into multiple patterns
  when needed) → live tester.
- **Auto-detect** — "Suggest from message" de-disguises text (leetspeak, spacing,
  repeats) and flags violating words/phrases, each tagged with its policy category
  (Abusive, Sexual, Threat, SARA/Racist, Prohibited SKU, Redirection, Competitor,
  Chat-to-Cancel). Multi-word phrases (e.g. `pesan lewat`, `transfer manual`) supported.
- **Phone-number mode** — generates a pattern for disguised Indonesian numbers
  (`O89 69O 885 151`, `823(3) 616 (4141)`, `+62…`, dashed/dotted/bracketed).
- **Exclusions** — "exclude when message also contains" wraps the regex in a negative
  lookahead (match `babi` but not `minyak babi`).
- **Live tester** — paste sample lines, see ✓ MATCH / ✗ NO MATCH with the hit highlighted.

> The in-app word list is a small, generic, editable profanity/policy seed for
> *detection only* — it is **not** the proprietary QC blacklist.

## Run locally

It's a single self-contained file — just open `index.html` in a browser
(double-click, or drag it into a browser tab). No build step, no dependencies.

If you prefer a local server (so the clipboard API works without prompts), use any
static server, e.g. VS Code "Live Server", or `npx serve` if you have Node.

## Deploy (Vercel)

1. Push this repo to GitHub (see below).
2. In [vercel.com](https://vercel.com) → **Add New… → Project** → import this GitHub repo.
3. Framework preset: **Other** (no build command, no output dir — it's static).
4. Deploy. Vercel serves `index.html` at the root URL.

No sensitive data ships (the blacklist/policy files are git-ignored), so a public
Vercel URL is acceptable.

## Push to GitHub

```bash
git init
git add .
git commit -m "Regex Generator for Chat QC"
git branch -M main
git remote add origin https://github.com/<you>/<repo>.git
git push -u origin main
```

## Regex engine note

The tester uses the **JavaScript** regex engine. Your QC system may use a different
engine — confirm syntax (inline flags, lookaheads, anchoring) with your developer.
