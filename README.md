# SEO Page Structure Checklist

Unofficial. Not affiliated with or endorsed by Google. It is a developer checklist for building a site without structural mistakes (HTML, links, robots and noindex, sitemaps, canonical, metadata, structured data). It does not judge visual design or copywriting.

Two skills that judge public pages against the English Google Search Central docs.

| Skill | Use for |
|---|---|
| `page-seo-review` | Single-page audits, SEO how-to questions, planning a new page |
| `structured-data-review` | JSON-LD / schema.org audits, choosing a markup type, markup questions |

## Before you use it

- This skill checks a page you are building against the Google Search Central guidelines. It saves time spent reading and matching the guidelines by hand
- Verdicts are made by an AI. Each one cites a Google page and the date checked, but the sources do not cover everything. Make the final decision after you check the official pages yourself
- If a page cannot be fetched or has moved, the related item is marked Needs verification. When the skill searches for the current page, the model may return wrong information. Check the cited page
- Passing a check does not guarantee a higher ranking. E-E-A-T is not treated as a ranking factor
- The skill gives verdicts and suggested fixes only. It edits files only when you ask it to. Keep a copy of the page before you fix it, because the workspace may not be under Git

### What to tell the skill

Give the page URL or file path, plus:

- Page type: blog post, product page, or documentation page
- Whether the page should appear in Search. A noindex or block on a page meant for Search is reported as a conflict
- Whether the page is live or only running locally. Local results do not prove how the live site is crawled
- Whether to also run `structured-data-review`. Otherwise the structured data items stay Needs verification
- Language: for a multilingual page, also give the URL of each version
- Goal: an audit (verdict tables) or advice (an answer to a question)
- Report language and save location, if you want a saved report

## structured-data-review output

This skill checks JSON-LD and schema.org markup. It has three modes:

- **Audit**: markup or a page is given. The output is a source key table, then one table with the columns `Group | Item | Verdict | Source | Fix`, then a short list of the types that were considered and excluded, with the reason. Group is the type slug (for example `article`) or `foundation`.
- **Advice**: a question about which type or property to use, with no markup given. The output is a few sentences with one to three verbatim quotes, the source URL, and the date checked. There are no tables and no verdicts.
- **Planning**: a page you are about to build. The output lists the types that apply, one line each with its source URL and date checked, and the decisions you still need to make. There are no verdicts.

When `page-seo-review` and `structured-data-review` run on the same audit, the group 6 items in `page-seo-review` are marked N/A with the pointer "judged in the structured-data table". Run both to get a complete result.

## Scope

- One audit checks one page. A root URL is one page, not the whole site
- Items that need site-wide data are marked Needs verification: inbound links, sitemap as a whole, multilingual versions, and duplicate URLs across the site
- To review more pages, give several URLs. Each one is audited separately

## Modes

The skill picks the mode from your input. You do not need to name it. Saying "audit" or "answer my question" helps the skill choose correctly.

| # | Mode | When | Output | Verdicts |
|---|---|---|---|---|
| 1 | Audit | A page, its HTML, or its URL is given | One line naming the most important finding, the source key table, then one table per audit (Group, Item, Verdict, Source, Fix) | Yes |
| 2 | Advice | A how-to or what-if question with no page | A few sentences, one to three verbatim quotes, source URL and date checked. Practical tips go under "Not from the docs:" | No |
| 3 | Open questions | "What should I fix first?" or "How do I rank higher?" | Quotes on how Search works and what the site owner controls. The skill asks for a page or URL | No |
| 4 | Unclear | The intent is not clear | Answered as advice, with an offer to audit | No |
| 5 | Planning | You describe a page you are about to build | One line per requirement (source URL and date checked), and the decisions you still need to make | No |
| 6 | Checklist without a page | You want the checklist items only | Items for the page type, with source URLs | Empty |

"Page types" (blog post, product page, documentation page) is a rule, not an output. It decides which items apply.

## Example output

This is a layout example only. It is not a real audit. The URLs, verdicts, and quotes are placeholders. A real audit quotes only text taken from the fetched page.

Most important finding: *[one-line summary of the top issue]*

### Source key

| Key | URL | Date checked |
|---|---|---|
| S1 | https://developers.google.com/search/docs/appearance/title-link | 2026-10-14 |
| S2 | https://developers.google.com/search/docs/appearance/snippet | 2026-10-14 |

### Audit: `/example/post` (blog post)

| Group | Item | Verdict | Source | Fix |
|---|---|---|---|---|
| 5. Results | Unique, descriptive title. Observed: one `<title>` element in `<head>` | Pass | S1: "[verbatim sentence from fetched page]" | |
| 5. Results | Meta description describes the page | Improve | S2: "[verbatim sentence from fetched page]" | Rewrite the meta description to summarize the post in one sentence |
| 1. Technical | Outbound link has descriptive anchor text | Needs verification | inference: link text not yet checked | |

For a `Pass`, the Item cell says what was observed in the HTML. For a `Fail`, the Source cell needs the required wording and the Item cell needs the observed fact in the page body.

## Behavior

- Verdicts: Pass, Improve, Fail, N/A, Needs verification
- Every verdict cites a source URL and the date checked; quotes are verbatim
- Docs are not copied into the plugin; the skills point to https://developers.google.com/search/docs
- No Search Console data. Items that need it are marked Needs verification

## Known limits

- A quoted sentence is word for word, but its source URL can occasionally point to a neighboring page that covers the same topic. Before relying on a quote, open the URL in the Source cell and confirm that the sentence is on that page
- The verdicts are a developer checklist, not a ranking or indexing prediction

## Not included

- Search Console help (support.google.com/webmasters) is not covered yet

## How this was made

The skills were written from the English Google Search Central documentation (published by Google under CC BY 4.0). The pages were downloaded as Markdown, stored in a working folder, and restructured into checklist items with judging rules, modes and an output format. The documentation is not copied into this plugin. Each skill points to the canonical pages at https://developers.google.com/search/docs and asks the reviewer to open them when judging and to record the URL and the date checked.

The checklist reflects the documentation at the time of writing. Google may change its pages later, so always check the source page before relying on a verdict.

## What it contains

Two skills (instructions only). It has no hooks, MCP or LSP servers, monitors or `bin/` directory, so the plugin itself runs no code. The skills instruct Claude to fetch Google documentation pages and, when a shell is available, to read the raw page with `curl` (or PowerShell `Invoke-WebRequest` on Windows). Fetches are limited to `developers.google.com` and the page under audit. Those are Claude's own tool calls, which your permission rules still govern. To check, run `claude plugin details seo-page-structure-checklist`.

## License and attribution

- Original parts: MIT License (see `LICENSE`)
- Material derived from Google Search Central: Creative Commons Attribution 4.0 for text, Apache 2.0 for code samples. Contains modifications based on work created and shared by Google. See `THIRD_PARTY_NOTICES.md` and https://developers.google.com/readme/policies
- Images, logos and trademarks of Google are not included
