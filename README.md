# LinguaMind – AI Language Coach

> Personalised language coaching built by composing four MeDo plugins. Learn vocabulary from text you actually read, not from a generic frequency list.

## What it does

Most language apps drill you on the same 2,000 generic words regardless of what you read, watch, or write in your target language. LinguaMind flips this. You paste in a paragraph — an article, a song lyric, a Slack message from a friend abroad, a paragraph from a book — and LinguaMind extracts the vocabulary you don't yet know, ranks it by usefulness for your level, and generates a short role-play conversation that forces you to use the new words in context.

Under the hood it's a small orchestration: an LLM does the extraction and conversational tutoring, a persistent store remembers which words you've already seen across sessions so the difficulty curve adapts, a URL/web plugin lets you point at a webpage instead of pasting, and an audio plugin reads the role-play aloud so you can shadow the pronunciation. The whole thing runs on MeDo's plugin platform without a traditional backend.

## Architecture

LinguaMind is composed from four MeDo plugins:

1. LLM plugin — vocabulary extraction, level-conditioned ranking, role-play tutor, grammar gap analysis.
2. Persistent storage plugin — per-user vocab graph (word, lemma, source sentence, mastery state), SM-2 scheduling state, and source library.
3. URL / web plugin — fetches and cleans article text when the user gives a link instead of pasting.
4. Audio plugin — text-to-speech for the role-play dialogue and individual word pronunciation.

See [ARCHITECTURE.md](./ARCHITECTURE.md) for the call graph and [PROMPTS.md](./PROMPTS.md) for the actual prompts.

## How to try it

- Live demo: https://app-boxm5fa7smip.appmedo.com/
- Devpost: https://devpost.com/software/linguamind-ai-language-coach

There is nothing to install locally. The app runs entirely on MeDo.

## How it's built

This repository is not a source tree. LinguaMind was built end-to-end on MeDo's no-code/low-code plugin platform, so there are no Node modules, no Dockerfiles, no servers. What this repo does contain is the design: which plugins are wired to which, the prompts that do the heavy lifting, and the trade-offs made along the way.

## What's in this repo

- [README.md](./README.md) — this file.
- [ARCHITECTURE.md](./ARCHITECTURE.md) — the 4-plugin composition in technical detail.
- [PROMPTS.md](./PROMPTS.md) — extraction, role-play, and grammar-gap prompts.
- [LICENSE](./LICENSE) — Apache 2.0.

## License

Apache License 2.0 — see [LICENSE](./LICENSE).

Author: mick-jae-johnson
