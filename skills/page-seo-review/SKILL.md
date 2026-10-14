---
name: "page-seo-review"
description: "AI-judged SEO checklist for public pages (blog, product, documentation) based on Google Search Central docs. Use when auditing a page against Google Search guidance, when asked which Google guidance applies to a page, and for SEO how-to or what-if questions about public pages (titles, links, robots and noindex, redirects, sitemaps, images, multilingual, JavaScript, online shops), answered from the Google docs."
---

# Page SEO Review

Use this skill to judge a public page against Google Search Central guidance. Every verdict must cite a source URL and the date the source was checked. Do not judge from memory.

The skill gives verdicts and suggested fixes only. It edits files only when the user asks it to.

## Modes

Choose the mode from what the user gave you.

- **Audit mode**: a page, its HTML, or its URL is given. Follow the rules below and return the tables. Put one line before the tables that names the most important finding.
- **Advice mode**: a how-to or what-if question with no page (for example "should I use nofollow on affiliate links?"). Find the matching row in the topic map, open the page, and answer in a few sentences with one to three verbatim quotes. Name the source URL and the date checked. Do not return the source key table or the verdict table, and do not assign Pass or Fail. If the docs do not answer the question, say so. You may then add practical suggestions, but put them under a separate line starting "Not from the docs:" and keep them short. Start the answer with what the docs state, in the docs' wording; do not open with an instruction stronger than the quotes support. Mark any reading beyond the quotes with `inference:` in the body. Under "Not from the docs:" write only general practice; do not state facts about the user's site (from the conversation, handoff notes, or earlier reports) unless they were checked in this session. Otherwise write Needs verification and say what to check. The shared rules (sources, quotes, dates, no judging from memory) still apply.
- **Open questions** ("what should I fix first?", "how do I rank higher?"): do not invent a ranking recipe. Open the pages in the "Visibility and ranking" row of the topic map, quote what they say about how Search works and what is under the site owner's control, and ask for the page or URL if an audit would help.
- **Unclear mode**: if it is not clear whether the user wants an audit or advice, answer in advice mode and offer an audit.
- **Planning mode**: the user describes a page or site they are about to build. List the requirements that apply (one line each, with the source URL and the date checked) and the points that depend on decisions the user has not made. Do not assign verdicts.
- **Checklist request without a page**: list the checklist items for the page type with their source URLs and leave the verdict empty.

## Shared rules

Before judging, read `${CLAUDE_PLUGIN_ROOT}/shared/judging-core.md` and follow it. It holds the source rules, verdict meanings, report format, quote rules and dev-server rule shared with the other skill of this plugin. If that file cannot be read, say so, mark every item `Needs verification`, and do not judge.

## Page types

Apply a page type's pages only to that type (do not use the online shop pages for a blog post).

## Source rules specific to this skill

1. If a documented URL returns 404, find the current page from the left navigation, use it, and say in the report that this skill's URL is out of date.
2. E-E-A-T is not a ranking factor. You may still point to the helpful-content page for what it says about people-first content, with quotes.

## Output

Return the source key table, then one table per audit, with these columns: Group, Item, Verdict, Source, Fix.

## Judging rules specific to this skill

- **Observed evidence.** For every `Pass`, say in the Item cell what was observed in the HTML (the element or attribute).
- **Dev server examples.** Canonical URL, sitemap, robots, and crawl/render results observed on localhost or a dev server are examples of deployed-site facts the shared dev-server rule covers.
- **Structured data details.** Per-type checks belong to the structured-data-review skill. If that checklist was run in the same audit, do not repeat its verdicts in group 6: give one row per group 6 item with verdict N/A and the Item cell "judged in the structured-data table". If it was not run, mark the items Needs verification. Markup that describes content not visible on the page is judged under sd-policies, not under the spam policies, unless the spam-policies page names the practice.
- **One defect, one row (example).** If a declared hreflang URL returns 404, put the verdict on one row and mark the other hreflang rows that depend on it N/A with a pointer.

## Topic map

Use this table to find the page for a question or an item. All paths are under https://developers.google.com/search/docs/ and were found in the Search Central navigation. Open the page before you answer; do not answer from memory.

| Topic | Pages |
|---|---|
| JavaScript rendering, lazy loading, dynamic rendering | `crawling-indexing/javascript/javascript-seo-basics`, `crawling-indexing/javascript/lazy-loading`, `crawling-indexing/javascript/fix-search-javascript`, `crawling-indexing/javascript/dynamic-rendering` |
| robots.txt, blocking and noindex (read these before advising on how they combine) | `crawling-indexing/robots/intro`, `crawling-indexing/block-indexing`, `crawling-indexing/robots-meta-tag` |
| Redirects, URL changes, 404s, site moves, domain changes | `crawling-indexing/301-redirects`, `crawling-indexing/site-move-with-url-changes`, `crawling-indexing/site-move-no-url-changes`, `crawling-indexing/url-structure`, `crawling-indexing/troubleshoot-crawling-errors` |
| Pages that should not be in results (staging, removed pages) | `crawling-indexing/remove-information`, `crawling-indexing/block-indexing`, `crawling-indexing/ask-google-to-recrawl` |
| Canonical URLs, duplicates, republished content | `crawling-indexing/canonicalization`, `crawling-indexing/canonicalization-troubleshooting`, `crawling-indexing/consolidate-duplicate-urls` |
| Images (alt text, file names, formats, image sitemaps) | `appearance/google-images`, `crawling-indexing/prevent-images-on-your-page`, `crawling-indexing/sitemaps/image-sitemaps` |
| Outbound links, link attributes, link spam | `crawling-indexing/qualify-outbound-links`, `essentials/spam-policies`, `crawling-indexing/links-crawlable` |
| Dates, bylines, authorship | `appearance/publication-dates`, `fundamentals/creating-helpful-content` |
| Page experience, Core Web Vitals | `appearance/page-experience`, `appearance/core-web-vitals` |
| Pagination, infinite scroll, incremental loading | `specialty/ecommerce/pagination-and-incremental-page-loading` |
| Online shops (URL structure, variants, site structure, launching, product data, where data appears) | `specialty/ecommerce`, `specialty/ecommerce/designing-a-url-structure-for-ecommerce-sites`, `specialty/ecommerce/help-google-understand-your-ecommerce-site-structure`, `specialty/ecommerce/how-to-launch-an-ecommerce-website`, `specialty/ecommerce/share-your-product-data-with-google`, `specialty/ecommerce/where-ecommerce-data-can-appear-on-google` |
| Product reviews and reviews you collected | `specialty/ecommerce/write-high-quality-reviews`, `appearance/reviews-system` |
| Titles and snippets | `appearance/title-link`, `appearance/snippet` |
| Meta tags and page metadata | `crawling-indexing/special-tags`, `crawling-indexing/valid-page-metadata` |
| Multilingual and multi-regional sites | `specialty/international/localized-versions`, `specialty/international/managing-multi-regional-sites`, `specialty/international/locale-adaptive-pages` |
| Mobile | `crawling-indexing/mobile/mobile-sites-mobile-first-indexing` |
| Sitemaps | `crawling-indexing/sitemaps/overview`, `crawling-indexing/sitemaps/build-sitemap` |
| AI-generated content, AI features | `fundamentals/using-gen-ai-content`, `essentials/spam-policies`, `appearance/ai-features` |
| General starting point | `fundamentals/seo-starter-guide`, `fundamentals/get-started-developers` |
| Visibility and ranking, "do I need SEO", how Search works | `fundamentals/how-search-works`, `fundamentals/do-i-need-seo`, `appearance/ranking-systems-guide`, `fundamentals/seo-starter-guide` |
| Copied, scraped, or summarized content from other sites | `essentials/spam-policies`, `fundamentals/creating-helpful-content`, `fundamentals/using-gen-ai-content` |
| Content in tabs, accordions, or loaded after interaction | `crawling-indexing/javascript/javascript-seo-basics`, `crawling-indexing/javascript/lazy-loading`, `fundamentals/get-started-developers` (no page is dedicated to this; say so, quote what the pages state about content in the DOM, and mark the rest as a judgment call) |
| Out-of-stock products, sale prices, product variants | `specialty/ecommerce/include-structured-data-relevant-to-ecommerce`, `appearance/structured-data/product-snippet`, `appearance/structured-data/merchant-listing`, `appearance/structured-data/product-variants` |
| Optimizing for AI features | `fundamentals/ai-optimization-guide`, `appearance/ai-features` |
| Math notation, dark mode, framework-specific setup (Astro, Next.js, and so on) | No Google page is dedicated to these. Say so. Answer only the parts the docs cover (crawlable links, content in the DOM, mobile, page experience) and put anything else under "Not from the docs:". |
| Why a page is not indexed | Needs Search Console data, which is not available. Say so, and point to the pages above that apply. |

## Checklist

### 0. Entry
- Identify the page type: blog post, product page, or documentation page. Choose the matching items below.
- Check whether the page is intended to appear in Search at all. The site owner decides intent. Take it only from the audit request (a request that says the page is about to go live counts as intent to appear; then report a noindex or blocked state as a conflict, in the first row); otherwise mark Needs verification. Do not mark Pass only because the page's robots state looks deliberate. Source: https://developers.google.com/search/docs/crawling-indexing/control-what-you-share

### 1. Technical requirements
- Google can crawl and render the page. Source: https://developers.google.com/search/docs/fundamentals/get-started-developers
- Links on this page (outbound) are `<a>` elements with an `href` that resolves to a URL, and their anchor text is descriptive. Judge each link, and judge the href and the anchor text as separate items. Links to this page (inbound) are judged in group 3. Source: https://developers.google.com/search/docs/crawling-indexing/links-crawlable
- Content that matters is present in the DOM, not only in images or videos. Source: https://developers.google.com/search/docs/fundamentals/get-started-developers

### 2. Policy and prohibited practices
- The page does not use spam techniques described in the spam policies. Source: https://developers.google.com/search/docs/essentials/spam-policies
- The page does not host malware or unwanted software. Source: https://developers.google.com/search/docs/monitor-debug/security/malware

### 3. Discovery and crawling
- The page is reachable by a link from another findable page (inbound). This needs site-wide context, so mark Needs verification unless the audit includes it. Source: https://developers.google.com/search/docs/crawling-indexing/links-crawlable and https://developers.google.com/search/docs/fundamentals/get-started-developers
- The page is in a sitemap when the site uses one and wants the page in results. A page marked noindex should not be listed: quote the sitemap page's guidance on which URLs to include, or mark it inference. Source: https://developers.google.com/search/docs/crawling-indexing/sitemaps/build-sitemap
- robots meta tags do not block indexing unintentionally. Source: https://developers.google.com/search/docs/crawling-indexing/robots-meta-tag

### 4. Multilingual and duplicate content
- Localized versions are declared when they exist, and only for versions that exist. Check each of these as its own item: the page lists itself, reciprocal links exist, declared versions exist, and a fallback (x-default) is present. Source: https://developers.google.com/search/docs/specialty/international/localized-versions
- Duplicate URLs are consolidated. Source: https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls

### 5. How the page appears in results
- The page has a unique, descriptive title. Source: https://developers.google.com/search/docs/appearance/title-link
- The page has a meta description that describes the page. Source: https://developers.google.com/search/docs/appearance/snippet

### 6. Structured data
- Structured data follows the general guidelines. Source: https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data
- Structured data matches the visible content of the page. Source: https://developers.google.com/search/docs/appearance/structured-data/sd-policies
- If the page has no structured data, mark both items "N/A". Structured data is not a requirement for appearing in Search unless the fetched page says so.
- Mark "Needs verification" for any item that needs the feature-specific page for the markup type used.

### 7. Quality and experience
- The content is helpful and written for people first. Source: https://developers.google.com/search/docs/fundamentals/creating-helpful-content
- The page works on mobile. Source: https://developers.google.com/search/docs/crawling-indexing/mobile/mobile-sites-mobile-first-indexing

### 8. Generative AI
- Generative AI content follows the guidance on AI-generated content. Source: https://developers.google.com/search/docs/fundamentals/using-gen-ai-content (the older URL essentials/generative-ai-content returns 404)

### 9. Agents and interactions
- Forms and actions that agents may use are clear to users. Mark "Needs verification" unless a Google source for this is checked.

## Attribution

This skill contains modifications based on work created and shared by Google. Use of the underlying content is under the Creative Commons Attribution 4.0 License. Code samples are under the Apache 2.0 License. See https://developers.google.com/readme/policies. Do not copy images, logos, or trademarks.