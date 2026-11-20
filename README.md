# international-llms-txt-build

Open-source skill that **builds international and multilingual llms.txt structures for websites with multiple country or language versions**. It operationalises the methodology in [Creating a Scalable International llms.txt Structure (Step By Step)](https://www.rebelytics.com/creating-a-scalable-international-llms-txt-structure-step-by-step/): the decisions, heuristics and execution discipline that emerged across multiple real-world applications, in a form an agent can apply consistently. The north star is that llms.txt is for AI agents to understand what the business is — descriptions are factual, drafted from the live page content, never from a URL slug, and never a marketing surface. Load it into any MCP-capable AI agent (Claude, Cursor, Codex, n8n, etc.).

Part of the [Open Skill Library](https://github.com/rebelytics/one-skill-to-rule-them-all/blob/main/OPEN-SKILL-LIBRARY.md), the open-source agent skills maintained with Task Observer.

## When it triggers

When the user mentions building or maintaining an llms.txt for a multilingual or multi-country site, references hreflang-based URL mapping for llms.txt, asks "how do I scale llms.txt across languages", wants to operationalise an llms.txt workflow across many country versions, or talks about international llms.txt structure — a new file for a site with multiple country or language versions, an existing international setup to update or expand, a single-language llms.txt to convert into a multilingual structure, a newly added language version, or a content audit of an existing international llms.txt. Producing the llms.txt files themselves is in scope; monitoring how AI systems represent the brand is not.

## What this repo contains

- `SKILL.md` — the north star and its pre-flight test, when to use the skill, the three phases, Phase A in full (discovery, the two structural variants and the heuristic for picking one, the section structure, page selection with target counts and the trim pass, the live-page-content rule for descriptions, the company description, the HTTP 200 check), deployment, the pre-flight checklist, the anti-patterns, the notes for stakeholder collaboration, the skill-level self-check, and the pointer list for the reference files.
- `references/per-version-adaptation.md` — Phase B: same-language localisation as a URL-swap exercise (URL slugs are deployed reality, not localised content) and the multi-language sub-flow (URL mapping, cleanup, translation, translate-and-replace, assembly, execution mode).
- `references/quality-verification.md` — Phase C: URL accuracy, description accuracy, the per-locale register consistency sweep, and native-speaker review.
- `LICENSE` — CC BY 4.0.

## Install

Install the whole skill directory — `SKILL.md` and `references/` must travel together — into your MCP-capable agent's skills directory, following the client-specific path, and restart the client.

## Contributing

Open an issue or a PR at [github.com/rebelytics/international-llms-txt-build](https://github.com/rebelytics/international-llms-txt-build).

## Credits

- Original author: [Eoghan Henn](https://www.rebelytics.com) / [LinkedIn](https://www.linkedin.com/in/eoghanhenn)

## License

CC BY 4.0 — the licence text is in [LICENSE](./LICENSE): the full legal code, unmodified, as Creative Commons publishes it; the [human-readable summary](https://creativecommons.org/licenses/by/4.0/) says what it allows. Use it, fork it, adapt it, monetise it. Keep the attribution.

Copyright (c) 2026 Eoghan Henn / rebelytics.com
