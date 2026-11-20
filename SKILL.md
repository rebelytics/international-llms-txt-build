---
name: international-llms-txt-build
description: Builds international and multilingual llms.txt structures for websites with multiple country or language versions. Load this skill when the user mentions building or maintaining an llms.txt for a multilingual or multi-country site, references hreflang-based URL mapping for llms.txt, asks "how do I scale llms.txt across languages", wants to operationalise an llms.txt workflow across many country versions, or talks about international llms.txt structure. Companion to AI visibility and LLM monitoring skills, but distinct in scope — this skill is about producing the llms.txt files themselves, not about monitoring how AI systems represent the brand.
license: CC-BY-4.0
metadata:
  author: Eoghan Henn and contributors
  source: https://github.com/rebelytics/international-llms-txt-build
  version: "1.0.0"
---

# International llms.txt Build

This skill operationalises the methodology Eoghan Henn published in [Creating a Scalable International llms.txt Structure (Step By Step)](https://www.rebelytics.com/creating-a-scalable-international-llms-txt-structure-step-by-step/). The article describes the approach in narrative form. This skill captures the decisions, heuristics, and execution discipline that emerged across multiple real-world applications, in a form an agent can apply consistently.

---

## Reference files in this skill — load on demand

- `references/per-version-adaptation.md` — Phase B — Per-version adaptation. **Load before Phase B, the per-version adaptation.**
- `references/quality-verification.md` — Phase C — Quality verification. **Load before Phase C, the quality verification.**

## North star: who llms.txt is for

llms.txt is for AI agents to understand what the business is. It is not a marketing surface. It does not exist to convince human users on an emotional level.

This is the foundational principle of the skill. Every drafting decision flows from it.

### What it means in practice

- **Descriptions are factual.** State what the page is, what's on it, what it covers. Do not state how good it is, how trusted the brand is, how many users love it, or how the user will feel.
- **No marketing adjectives.** "Premium", "best-in-class", "legendary", "ultimate", "trusted", "loved by thousands", "industry-leading" — all of these are out unless they appear factually on the page itself as a verifiable claim (e.g., an award-winning product has objectively won an award; "trusted by thousands" is marketing copy with no source).
- **No calls to action.** llms.txt is not selling. "Find your perfect X today!" or "Discover the world of Y" do not belong.
- **No positioning statements masquerading as facts.** "The world's leading provider of Z" is positioning, not a fact, unless there's an external source that supports the ranking.
- **The audience is the LLM, not the human user.** When choosing between two ways to phrase something, pick the one that gives the LLM more accurate, machine-parseable information about what the page contains, even if it's drier.

### Pre-flight test for any description

Before approving any description in the file, ask: "If an LLM cited this description verbatim in an answer to a user, would it sound like a factual statement about the business, or would it sound like the business marketing itself?" If the latter, rewrite.

---

## When to use this skill

Trigger this skill when:

- The user is building a new llms.txt file for a site that has multiple country or language versions
- The user is updating, expanding, or maintaining an existing international llms.txt setup
- The user is converting a single-language llms.txt into a multilingual structure
- A new language version has been added to a site and the llms.txt setup needs to scale
- The user asks for a content audit of an existing international llms.txt

For single-language llms.txt files where there's no international dimension, the article on llmstxt.org is the canonical reference and this skill adds little. Prompt monitoring — tracking how AI assistants answer about the brand — is a different job and outside this skill.

---

## Phases overview

The work runs in three phases:

- **Phase A — Template version** — Draft the llms.txt for one website version (typically the global English or main-market version). This is the editorial template that all other versions will be derived from.
- **Phase B — Per-version adaptation** — Produce all remaining versions. Same-language localisations (e.g., regional English variants) are URL-swap exercises. Other-language versions require URL mapping plus translation.
- **Phase C — Quality verification** — Two distinct verification mechanisms (URL accuracy and description accuracy), plus native-speaker review per language.

Phase A is high-judgement work that benefits from human collaboration. Phase B is mechanical at scale once the template is signed off. Phase C is mostly automated with a final human pass.

Do not start Phase B until Phase A is signed off by the relevant stakeholder. Translating against an unstable template wastes effort and produces a fragile multi-language set.

---

## Phase A — Drafting the template version

### Step 1: Discovery

Before drafting, gather the following:

- **Hreflang inventory.** Pull the hreflang annotations from the homepage of the main domain. List every language/country version, including the canonical URL pattern (subfolder vs ccTLD) and the hreflang code. This becomes the spec for the international structure section and the source for URL mapping in Phase B.
- **Verify hreflang quality.** Check that x-default is present, all entries are well-formed, and the alternates list is internally consistent (every version references the others). Major hreflang issues will block Phase B; surface them now.
- **Performance data.** A Google Search Console export of top pages is the default ranking signal. Bucket the URLs by language version (using the path or domain pattern). Rank within each bucket by clicks, with impressions as a tie-breaker for high-potential pages.
- **Site navigation and sitemap.** Identify commercially significant pages that may not rank well in GSC but are essential for understanding the business. Top-of-nav category pages, hub pages, key tools — these often need to be in the file regardless of their search performance.
- **Existing documentation.** Check whether the site has a page-type segmentation, schema implementation map, or similar artifact already in the workspace. These often catalogue URL patterns at a granularity that saves significant discovery time.

### Step 2: Choose the structural variant

Two stable structural variants exist, and the right one depends on the project type.

**Variant 1 — Article model (simpler).** A separate index file at the root of the domain that lists all language-version llms.txt files. Each language-version file then contains two main content sections: "Most important product & service pages" and "Other important pages". The index is purely a router and contains no per-page detail.

This works well when the domain has a deep folder hierarchy and the main-language version sits in a subfolder (e.g., the canonical URL is /en/ and there's no content at the domain root).

**Variant 2 — No index, integrated structure (e-commerce-friendly).** No separate index file. The main-language version lives at the root of the domain (e.g., the canonical URL is /). Each version's llms.txt is self-contained and links to all other versions in an "International [website/webshop] structure" section. The content sections are sub-divided more granularly: typically four to five sections rather than two.

This works well when the main-language version sits at the domain root and where richer content categorisation helps LLMs distinguish page types.

**Heuristic for picking:**

- Main-language version at a subfolder → Variant 1 (article model)
- Main-language version at domain root → Variant 2 (no index)
- E-commerce or content-rich site → Variant 2 (more granular section structure helps)
- B2B services or thin-content site → Variant 1 (simpler is enough)

### Step 3: Section structure for Variant 2

For e-commerce sites following Variant 2, the proven section structure is:

1. **Title** (`# Brand Name`)
2. **Tagline** (a single `>` blockquote paragraph — factual company description)
3. **About this llms.txt file** — explains what version this file is for, and that the international structure section at the bottom links to all other versions
4. **Most important category pages on the current website version** — top-level product/service categories
5. **Most important product pages on the current website version** — flagship products or named items where individual pages carry independent search authority. Skip this section if individual product pages aren't a meaningful surface (e.g., for a service business or a thin-product catalogue).
6. **Most important customer information pages on the current website version** — shipping, returns, T&Cs, privacy, about, contact, help centre
7. **Most important informational content pieces on the current website version** — the main guide hub, top educational guides, top blog posts, interactive tools (calculators, finders), the blog hub
8. **International [website/webshop] structure** — links to all language-version llms.txt files

Each section title includes a parenthetical qualifier identifying the current version (e.g., "current website version (Global English version for users outside of the US and UK)"). This matters because LLMs reading multiple version files need to be able to attribute each list of URLs to the correct version.

For Variant 1 (article model), the section structure follows the article directly: title, tagline, About, Most important product & service pages, Other important pages, Multilingual website structure.

### Step 4: Page selection

The instinct on a first pass is to be inclusive. Don't be — first drafts that include "everything that ranks in GSC plus everything in the nav" systematically over-produce. Plan a trim pass before delivery.

**Target counts** (Variant 2, e-commerce):

- Categories: 10–14
- Products: 8–12 (skip the section if not relevant)
- Customer info: 6–8
- Informational content: 12–16
- **Total page-list URLs: 40–50** (excluding the international structure section)

These targets are a starting point, not a rule. Larger or more diversified businesses can land at 60–80; tightly-focused businesses can land at 30–40. The discipline is to make every entry earn its place, not to hit a number.

**What goes in:**

- Top-level category pages (the main nav structure)
- Strategically named landing pages (Best Sellers, New Releases, Staff Picks, Award-Winning, etc. — when they exist as real pages, not as marketing constructs)
- The most iconic individual products, ranked by GSC clicks within the current version
- The standard customer information set (shipping, returns, T&Cs, privacy, about, contact, help centre)
- The flagship guide hubs and the highest-traffic educational pages
- Discovery tools (interactive finders, calculators) — file these under informational content, not categories
- The blog hub

**What stays out:**

- Feature or attribute filter pages when a parent collection page already exposes those filters
- Sub-categories that are largely covered by the parent category
- Lower-traffic individual products that are out-shadowed by the flagships
- Policy pages duplicated by the standard set (Cookies Policy is usually covered by Privacy Policy in modern sites, etc.)
- Loyalty programmes, referral pages, account pages — these are user-account surfaces, not informational content
- Blog posts that overlap heavily with the main guide hub
- Campaign and promo pages with short shelf life

**The trim pass:**

After the first draft, deliberately cut. Aim to reduce by 30–50% on the way to delivery. Ask of every entry: "Does dropping this leave a meaningful gap in what an LLM would understand about the business?" If the answer is no, drop.

### Step 5: Drafting descriptions — the live-page-content rule

This is the single most important execution rule in the skill. **Every description must be drafted from the live content of the page it describes, not inferred from the URL slug, the page name, or context from adjacent pages.**

**Why this matters.** Descriptions inferred from URL slugs and tangential context can sound plausible while being factually wrong. The model uses the URL slug, common-knowledge associations, and context from related pages to construct a coherent-seeming description. The output reads as confident and specific. It's also frequently incorrect.

**Anti-pattern (worked example):**

Imagine a category page on a generic retailer's site with a URL slug like `/123-heritage-collection`. Without reading the page, the slug could plausibly mean:

- A category of products made by long-established manufacturers
- A category of discontinued or archive products
- A category of products styled after historical designs

These are three distinct meanings. Inferring from the URL alone produces a 1-in-3 guess that sounds confident. The correct answer is whatever the live page actually says — and it could be any of the three.

**Mitigation — the drafting workflow:**

1. For each URL in the curated list, fetch the live page (or its title + meta description + on-page intro paragraph at minimum)
2. Read what the page actually says — title, meta description, H1, opening paragraph
3. Write the description from those sources
4. Avoid combining facts from unrelated pages to construct composite descriptions — that pattern is where most hallucinations originate

**Format:**

- 1–2 sentences maximum per description
- Factual, machine-parseable
- Use the page's own framing where it's accurate; reword only when the page's framing is marketing copy that the skill's north-star principle excludes
- Generic industry or category common knowledge is acceptable when the live page doesn't offer specifics — but flag this as a verification request to the brand stakeholder, since the brand may want different framing

**Cross-check before delivery:**

After drafting all descriptions, run a sample audit: pick 5–10 descriptions at random, fetch their pages, and verify the description matches the page's actual content. If any miss, audit all descriptions, not just the ones that failed. The failure mode is systemic, not isolated.

### Step 6: Company description (tagline)

The tagline (`>` block at the top of the file) is one factual paragraph describing the business.

**Source material:**

- The brand's own About Us page is the primary source
- The brand's footer or "Legal Notice" page often confirms HQ location, legal entity, and similar facts
- External factual sources (Wikipedia, regulatory filings, public press releases) supplement where helpful and verifiable
- Marketing pages, About Us pages with heavy promotional copy, and brand statements are filtered for facts only

**What to include:**

- Brand name (and acronym if widely used)
- What the business does (in one phrase)
- Founding year (if verifiable)
- Headquarters location
- Physical retail presence if relevant (locations and counts)
- Geographic shipping or service scope (if e-commerce — be explicit about which countries; "ships internationally" is too vague)
- Number of language/country versions of the website (this anchors the international structure section that follows)

**What to exclude:**

- Mission statements, vision statements, brand values
- Customer counts, review averages, satisfaction percentages
- Adjectival marketing ("the world's leading", "trusted by", "industry-pioneering")
- Product feature claims that don't have an external source
- Forward-looking statements ("we're working towards…")

**Stakeholder collaboration:** The article recommends drafting the company description with the business's PR/comms team. When the agent is drafting solo, deliver the factual draft and explicitly invite stakeholder input on positioning before deployment. Frame it as an opt-in: "the draft is strictly factual; if your team wants to layer in positioning language, that's a stakeholder call."

### Step 7: Verify all URLs in the draft return 200

Before delivery of the template, run an HTTP HEAD check against every URL in the draft (typically 40–50 calls, easily parallelised). If any return non-200, investigate before delivery — these are usually copy-paste errors, stale URLs from older crawl data, or genuine site bugs that the brand should know about.

This is the URL-accuracy verification mechanism. It catches half the hallucination space (broken or non-existent URLs). The other half — descriptions that mismatch their pages — is verified separately in Step 5's cross-check.

---

## Phase B — Per-version adaptation

This section lives in `references/per-version-adaptation.md`. **Load it before Phase B, the per-version adaptation.**

---

## Phase C — Quality verification

This section lives in `references/quality-verification.md`. **Load it before Phase C, the quality verification.**

---

## Deployment

llms.txt files are static text files that need to be served at:

- The root of the domain for the canonical version (e.g., `/llms.txt`)
- The root of each subfolder version (e.g., `/us/llms.txt`)
- The root of each ccTLD (e.g., the equivalent `/llms.txt` on each country domain)

Most CMSs and e-commerce platforms do not generate these files automatically. Deployment requires the dev team to either:

- Place static text files at the appropriate paths
- Wire up a server-side rule (Nginx config, .htaccess, edge function, etc.) to serve the content at those paths

Drafting the dev brief is part of the workflow. Include the file paths, the exact file content for each version, and a note that the file must be served as `text/plain` or `text/markdown` (not HTML).

---

## Built-in enforcement: pre-flight checklist

Before delivering any version of an llms.txt file produced via this skill, verify:

**North-star compliance**
- [ ] No marketing adjectives in any description ("premium", "best", "trusted", "leading", etc.)
- [ ] No calls to action ("discover", "find your perfect", etc.)
- [ ] No positioning claims without external source
- [ ] No customer counts, review averages, or satisfaction percentages
- [ ] No mission/vision/values content

**Description accuracy**
- [ ] Every description was drafted from live page content (title, meta, on-page intro)
- [ ] No descriptions inferred from URL slug alone
- [ ] No composite descriptions combining facts from unrelated pages
- [ ] Sample audit of 10% of descriptions verified against live pages

**URL accuracy**
- [ ] Every URL in the page-list sections returns HTTP 200
- [ ] Every URL in the international structure section points to the correct version's llms.txt path
- [ ] No accidental redirects (canonical URL used, not the redirect-source)

**Per-locale register**
- [ ] Each language version file uses spelling and vocabulary consistent with its declared locale (UK forms in UK files, US forms in US files, pt-PT forms in pt-PT files, etc.)
- [ ] URL slugs are preserved verbatim from the live site, even when the surrounding label spelling differs from the slug
- [ ] Per-locale register sweep was run on the final file, not on the source
- [ ] For mechanical UK↔US derivation, a deterministic substitution transformation is in place rather than per-word manual handling

**Structural compliance**
- [ ] Section structure matches the chosen variant (1 or 2)
- [ ] Each section title includes a parenthetical qualifier identifying the current version
- [ ] International structure section is in the agreed location (top per the integrated-pattern default, or bottom per stakeholder preference)
- [ ] Total page-list URLs in the target range (40–50 default)

**Cross-version consistency**
- [ ] International structure section is identical in structure across all versions, with each entry in the corresponding target language
- [ ] Page list scope is comparable across versions (allowing for genuine version-specific gaps)

If any check fails, fix before delivery.

---

## Anti-patterns

Avoid these failure modes — most have been observed in real applications.

**1. Drafting descriptions from URL slugs.** The single most common failure mode. See Phase A Step 5 for the mitigation.

**2. Including everything that ranks in GSC.** Produces bloated files where the signal is buried in the noise. Plan a trim pass.

**3. Including filter pages alongside their parent categories.** Filter pages (e.g., feature filters that drill into a parent collection — "high-X", "compact X", "beginner-friendly X" pages) are usually surfaced via the parent category page's filter UI. Including both creates duplication. Pick the parent.

**4. Describing tools as categories.** Interactive tools (finders, calculators, configurators) are not categories. They belong in the informational content section.

**5. Quoting the brand within its own llms.txt.** Phrasings like "Per [Brand], X is true" are awkward inside the brand's own llms.txt file. The whole file is implicitly the brand's voice. State the fact directly.

**6. Translating before the template is signed off.** Wastes effort on a multi-language set that needs to be regenerated when the template changes.

**7. Translating URLs.** URLs are not translated. The mapping prompt should explicitly instruct against this; the QA step should verify.

**8. Including the loyalty programme, referral page, or account-related pages in informational content.** These are user-account surfaces, not informational content. They don't help an LLM explain the business.

**9. Skipping the native-speaker review step.** Machine translation produces output that looks fluent but can carry subtle errors that only a native speaker catches. The cost of the review is small; the cost of deploying poor-quality translations is large.

**10. Hard-coding "X countries" or "Y languages" without verification.** The numbers shift as brands add or drop markets. Pull from the live hreflang annotations during Phase A and update the tagline before each deployment.

**11. Localising URL slugs to match the surrounding label's register.** When a UK English file contains a slug like `/colored-pencils/`, the temptation is to "fix" it to `/coloured-pencils/` to match the UK spelling of "coloured" in the label. Don't — URLs are deployed reality, not localised content. See Phase B Sub-flow B1 for the principle.

**12. Auditing translation correctness without a per-locale register sweep.** A QA pass that checks translation quality phrase-by-phrase will miss within-locale register inconsistency (UK file with US spellings, etc.). The sweep is a distinct pass that operates on the final file. See Phase C "Per-locale register consistency".

---

## Notes for stakeholder collaboration

- The first draft should always include a methodology note alongside the deliverable, explaining the structural choices, the page selection rationale, and any open questions for stakeholder review. This invites collaboration rather than presenting a finished artifact.
- The trim pass is a natural collaboration point. After the first draft, present the inventory of what's in and what was considered-but-cut, and let the stakeholder dial it back if they want pages restored.
- Tagline framing is the most stakeholder-sensitive section. Default to factual; explicitly invite the brand's PR/comms team to layer in positioning if they want.
- For agent-led drafting, surface every claim's source (the live page, the about page, an external source) when stakeholders ask. Hidden inference is the failure mode the skill exists to prevent.

---

## Pre-flight self-check (skill-level)

Before delivering output from this skill, the agent should confirm:

1. The north-star principle (factual, not marketing) was applied consistently
2. Every description was drafted from live page content
3. URL accuracy, description accuracy, and per-locale register sweeps were all run
4. URL slugs were preserved verbatim from the live site even where surrounding prose was localised
5. The chosen structural variant is documented in the methodology note
6. The trim pass happened (don't skip it because the first draft "looks reasonable")
7. The international structure section is internally consistent with the rest of the file

If any of these were skipped, do them before delivery.

## Feedback on this skill

If the user has methodology feedback, offer to draft a report for
https://github.com/rebelytics/international-llms-txt-build; before drafting, search that
repository's open and closed issues and pull requests for the same problem,
and read its CONTRIBUTING file for how the maintainer takes reports. If the
problem is the agent not following the skill's rules, acknowledge and
correct it instead.

International llms.txt Build: Eoghan Henn and contributors | CC BY 4.0
