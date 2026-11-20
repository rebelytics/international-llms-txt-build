# Phase C — Quality verification

Reference file for the `international-llms-txt-build` skill (CC BY 4.0 — Eoghan Henn and contributors). Load before Phase C, the quality verification.

## Phase C — Quality verification

Three distinct verification mechanisms, plus a fourth human review pass.

### URL accuracy

Re-crawl every URL across every version's file. Confirm 200 responses. Surface any 404s, redirects, or soft-404s for fixing before deployment.

This catches URL hallucinations (URLs that don't exist or have changed since drafting). The article's Screaming Frog re-crawl approach is the canonical method.

**URL-error remediation is a coordinated loop, not a file edit.** Because llms.txt must mirror live URLs exactly, a flagged URL typo or wrong slug on the live site forces a decision: fix the URL on the live site, or keep mirroring the error. If the site fix is chosen, do NOT regenerate the affected file until the web team confirms the live fix has actually landed — regenerating against an unfixed site re-introduces the error. When flagging a URL issue to a web team, always carry exact page identity: the full URL, the page's H1, and the page type. Web teams routinely check a *same-keyword sibling page* (e.g., the category page instead of the flagged blog post) and report "no issue found"; precise identity prevents that false-negative loop.

**Geo-gated responses (HTTP 451 and similar) may be intended, not broken.** International e-commerce sites can return 451 (or other blocks) to human visitors in markets where a product is illegal, while returning 200 to crawlers and to visitors in permitted markets. Two consequences for verification: (a) record 451s and other geo-gated statuses for triage rather than treating them as defects — the block can be fully intended; (b) a 200 from the verification egress does not prove human reachability in every market, since reachability is per-market, not global. Note the verification egress location when reporting status codes.

### Description accuracy

This verification mechanism is **not in the article** and was added based on real-world failures: descriptions that read as plausible but mismatch the live page content. URL re-crawls don't catch this — the URLs are valid; the problem is descriptive drift.

**Process:**

For the template version (Phase A output), this should already have happened in Step 5 of Phase A. For the per-language versions (Phase B output), re-verify only when:

- The description was rewritten beyond simple translation (not common; flag in the prompt design)
- The target-language page genuinely diverges from the template page in content scope (rare for true localisations)

For a standard translation pass where descriptions are direct translations of the template, description accuracy carries forward from Phase A's verification — translation doesn't introduce semantic drift if the translation prompt is clean.

**Sampling depth:**

A 10–15% spot-check across the file is usually sufficient if Phase A's audit was thorough. If spot-checks find descriptions that mismatch, audit the full file.

### Per-locale register consistency

A QA pass focused on translation correctness, marketing tone, and content claims will miss within-locale register inconsistency — the case where a UK English file contains US spellings (or vice versa), or where a pt-PT file contains pt-BR vocabulary. This class of error is structural: it comes from sharing source dicts across regional variants, or from translators handling one locale at a time without seeing the others. Each phrase is "translated correctly", but the file as a whole reads inconsistently to a native reader of the target locale.

Run a distinct audit pass per locale, on the final file, after translation review is complete. This is locale-internal — it doesn't need a reference corpus, just a list of pair-tells per language.

**English UK/US register sweep.** Grep each English variant's file for forms inconsistent with the declared locale:

```bash
grep -iE 'vigour|vigor|flavour|flavor|organis|organiz|catalogue|catalog|centre|center|mould|mold|colour|color|behaviour|behavior|realise|realize|analyse|analyze' file.md
```

For UK files, flag any US forms found; for US files, flag any UK forms found. The list above is a starter — extend per project as new pair-tells surface.

**Other regional variants.** Apply the same approach to:

- **pt-PT vs pt-BR.** Flag Brazilian-marked forms in pt-PT files (e.g., progressive "está fazendo" vs European "está a fazer"; lexical items like "ônibus" vs "autocarro"; verb forms with explicit subject pronouns where European Portuguese omits them).
- **fr-FR vs fr-CA.** Flag Canadian-marked vocabulary or anglicisms in French (France) files.
- **es-ES vs es-LATAM.** Flag LATAM-marked vocabulary or grammar in Spanish (Spain) files (e.g., "ustedes" instead of "vosotros"; "computadora" vs "ordenador").

**When the same source dicts back multiple regional variants.** If one version is intended to track another with mechanical substitutions (e.g., UK files derived from US source via spelling swaps), build the substitution as a deterministic transformation (`_swap_uk_to_us()` or equivalent) rather than relying on translators or stakeholders to remember per-word. The deterministic version is also easier to audit and to extend when new pair-tells surface.

**When the sweep finds something.** Don't just fix the single mismatch — audit the full file for related forms. If "colored" slipped through, "organized", "flavored", and "realized" probably did too. Mismatch is structural, not isolated.

**Important: run the sweep on the final file, not on the source.** Within-locale inconsistency comes from sharing source dicts across variants; auditing the shared source can't see the problem. The sweep has to read what each locale's deployed file actually contains.

### Native-speaker review

Each per-language file should be reviewed by a native or fluent speaker on the brand's team before deployment. This catches:

- Translation quality issues (machine translation artifacts, awkward phrasing, terminology mismatches)
- Cultural or regional nuances missed by the translation pass
- Brand voice issues (where the literal translation is correct but doesn't sound like how the brand speaks in that market)
- Factual issues specific to the regional market that the agent couldn't catch

This is a brand stakeholder responsibility, not an agent responsibility. Surface it as a required step and don't proceed to deployment without it.
