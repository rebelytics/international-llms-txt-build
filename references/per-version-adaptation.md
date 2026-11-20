# Phase B — Per-version adaptation

Reference file for the `international-llms-txt-build` skill (CC BY 4.0 — Eoghan Henn and contributors). Load before Phase B, the per-version adaptation.

## Phase B — Per-version adaptation

Phase B has two sub-flows depending on whether the version is a same-language localisation or a different-language version.

### Sub-flow B1 — Same-language localisation

When a site has multiple versions in the same language (e.g., a global English version, a regional English version, a market-specific English version), the localisations are URL-swap exercises plus copy adjustments:

1. Take the template Phase A file
2. For every URL, swap the path/domain to the localised equivalent (verified via hreflang annotations on the source pages)
3. Drop any URLs that don't exist on the localised version (e.g., a feature page that's only on the global version)
4. Adjust the "About this llms.txt file" block to describe the localised version instead of the template
5. Adjust any descriptions where the localised page genuinely differs from the template page (rare for true localisations, common for regional variants)
6. Keep the international structure section identical

No translation is required because the language is the same. Most of the work is mechanical URL substitution.

#### URL slugs are deployed reality, not localised content

When localising for a target market — whether that means translating prose, switching English spelling register, or adjusting tone — URLs are not part of what gets localised. Preserve URL slugs verbatim from the live site, even when the surrounding label or description uses a different register.

The common temptation is to "fix" a URL slug that contains language-marked vocabulary inconsistent with the surrounding prose. Don't. If the site uses the same slug across all markets (e.g., `/colored-pencils/` on both `/uk/` and `/us/`), the llms.txt files for those markets use that same slug, even when the visible labels differ in spelling.

**Worked example.** A UK English file might contain:

`[Coloured Pencils](https://www.example.com/uk/33-colored-pencils)`

UK spelling in the visible label ("Coloured"), US-form slug in the URL ("colored") because that's the site's deployed path. Both correct, both required. Localising the URL to `/coloured-pencils/` would produce a 404.

**The principle.** An llms.txt is a faithful index of what's actually deployed, not an idealised representation of what each market's content should look like. URLs are constants determined by the deployment system; the surrounding prose is a variable determined by the locale. Conflating them — by localising URLs to match locale spelling, or by mirroring URL spelling into prose — produces either broken links or inconsistent prose. Keep them on separate tracks during localisation work.

The same principle holds for any deliverable that lists URLs alongside translated text: sitemaps with descriptive metadata, schema.org BreadcrumbList markup with translated labels, internal link audits with localised anchor text. URL = deployed reality. Prose = locale-appropriate.

### Sub-flow B2 — Multi-language version

For the more substantial workflow — adapting the template into the remaining language versions — the key insight from the article is that you cannot simply translate the file. URLs are language-specific (path slugs differ, some pages exist only on certain versions), so the work combines translation with URL replacement and filtering.

**Step 1: Build the URL mapping**

The recommended source is a Screaming Frog crawl of the URLs from the template file, in list mode, with hreflang annotations exported. This produces a spreadsheet where each row contains the template-version URL and all hreflang alternates.

Alternative sources if hreflang isn't reliable: the brand's URL routing logic (from the dev team) or a platform-specific URL export.

**Step 2: Per-language URL mapping cleanup**

For each target language, extract a clean two-column mapping: template URL → target-language URL. Handle missing alternates explicitly (mark them as "not available on this version" so they can be filtered out in Step 3).

A simple prompt to the agent works:

> "From the URL mapping below, return only the rows where the target-language URL exists. For each, output: Template URL | Target-Language URL. If no target-language URL exists for a template URL, output: Template URL | NO TARGET URL AVAILABLE."

**Step 3: Translate non-URL content**

The header, tagline, "About this llms.txt file" block, and the descriptions in each section are all candidates for translation. URLs themselves are never translated.

A simple translation prompt suffices for this step. Standard rule: keep brand names, product names, and proper nouns in their original form unless the brand uses a localised name in the target market.

**Translation consistency — use sibling versions as the terminology corpus.** When expanding or repairing a description that some language versions already carry in full (e.g., a shortened description on four versions where the full taxonomy exists on four others), do not translate from the source-language template alone. Read how the already-complete sibling versions rendered each term, and base the new translation on those established renderings — otherwise the repaired line invents terminology that diverges from the brand's existing translations. Also confirm the target file's own dominant term choices (grep the file) before writing, so the new line matches that file's established voice (e.g., one locale may use one synonym in body copy and another in labels). In a multi-version set, the other versions are a terminology corpus; reuse it so a repaired line reads as part of the same family rather than a fresh translation.

**Step 4: Translate-and-replace**

The most complex step. The template's page-list sections need to be translated AND have their URLs replaced AND have non-existent URLs filtered out, all in one pass. The article's prompt template handles all three:

> "Please translate the content provided at the end of this prompt to [target language]. The content contains URLs which should not be translated, but replaced. Use the URL mapping above to find the corresponding [target language] URL for each template URL. Do not translate URLs. If you cannot find a matching target-language URL, omit the entire line from the output. Return the result in markdown format."

**Step 5: Assemble the per-language file**

Combine the translated header, sections, and the international structure section (which stays consistent across all versions, listing all 19/27/etc. language versions in the same order with each link in the corresponding language).

The international structure section is the one part that's NOT machine-translated — each entry in it appears in its target language across all files (e.g., the line "[llms.txt for Italian language version]" appears as "[llms.txt for Italian language version]" in the English files but as "[llms.txt for Italienische Sprachversion]" in the German file). Generate this section freshly per language rather than running it through the URL-mapping prompt.

### Execution mode

The article describes a PromptMate-based workflow that batches the per-language steps. PromptMate is one execution path; it is not the only one. A conversational mode — running the cleanup, translation, and translate-and-replace prompts directly through the agent's chat interface, language by language — works equally well and is often more efficient for small numbers of languages or when iterating with stakeholder feedback.

The key requirement, regardless of execution mode, is that each per-language file is generated end-to-end before moving to the next, so that stakeholder feedback on language A can inform language B's drafting.
