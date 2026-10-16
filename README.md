# SEO Page Structure Checklist

Unofficial. Not affiliated with or endorsed by Google.

A developer checklist for building a site without structural mistakes, covering:

- HTML
- links
- robots and noindex
- sitemaps
- canonical
- metadata
- structured data

It does not judge visual design or copywriting.

Two skills judge public pages against the English Google Search Central docs.

Source: https://developers.google.com/search/docs

## Skills

| Skill | Use for |
|---|---|
| `page-seo-review` | Single-page audits, SEO how-to questions, planning a new page |
| `structured-data-review` | JSON-LD / schema.org audits, choosing a markup type, markup questions |

To install, see [Install](#install) below. The sections that follow explain how to use the skills, their modes and limits.

---

## Before you use it

1. It provides assessments and suggested revisions only
2. When you give it a page, it checks the page as it is at that time, against the "Google Search Central" guidelines
3. Each review cites the source page URL and the date checked
4. Assessments are made by an AI. Make the final decision after you check the official pages yourself
5. Each audit covers one page. Several URLs in one request are supported, and each one is audited separately. Three URLs have been checked in one request; no upper limit is stated
6. If a page cannot be fetched or has moved, the related item is marked "Needs verification"
7. The AI model may return wrong information when it searches for the current page. Check the cited source page
8. It edits files only when you ask it to. Before you apply any change, keep a copy of the page, or use Git version control, because the workspace may not be under Git
9. It does not guarantee an improvement in search rankings, and it does not treat E-E-A-T as a ranking factor

---

## Install

### Repository

Source code and issues: [1abcdefggs/seo-page-structure-checklist](https://github.com/1abcdefggs/seo-page-structure-checklist)

### Use it locally without a marketplace

This works now, without `marketplace.json`. Clone the repository, then load it with `--plugin-dir`:

```bash
git clone https://github.com/1abcdefggs/seo-page-structure-checklist.git
claude --plugin-dir ./seo-page-structure-checklist
```

The plugin loads for that session. A local copy does not update by itself. Pull the repository again to get a new version.

### Install from a marketplace

The commands below do not work yet. They need a `.claude-plugin/marketplace.json` in this repository that names the marketplace `seo-page-structure-checklist`, and that file is not added yet. After that file is added, run these in Claude Code:

```text
/plugin marketplace add 1abcdefggs/seo-page-structure-checklist
/plugin install seo-page-structure-checklist@seo-page-structure-checklist
```

---

## page-seo-review (single-page SEO checks)

### Quick start

1. Install the plugin (see [Install](#install) above)
2. In a Claude Code session, give it a page. Replace the URL with your page:

   ```text
   /seo-page-structure-checklist:page-seo-review https://example.com/post/
   ```

3. Read the verdict tables. Each row cites a source page and the date checked

### Input

Give the page URL or file path, plus:

- Page type: blog post, product page, or documentation page
- Whether the page should appear in Search. A noindex or block on a page meant for Search is reported as a conflict
- Whether the page is live or only running locally. Local results do not prove how the live site is crawled
- Whether to also run `structured-data-review`. Otherwise the structured data items stay Needs verification
- Language: for a multilingual page, also give the URL of each version
- Goal: an audit (verdict tables) or advice (an answer to a question)
- Report language and save location, if you want a saved report

### Scope

- One audit checks one page. A root URL is one page, not the whole site
- Items that need site-wide data are marked Needs verification: inbound links, sitemap as a whole, multilingual versions, and duplicate URLs across the site
- To review more pages, give several URLs. Each one is audited separately

### Review modes and output

The skill picks the mode from your input. You do not need to name it. Saying "audit" or "answer my question" helps the skill choose correctly.

| # | Mode | When | Verdict? |
|---|---|---|---|
| 1 | Audit | A page, its HTML, or its URL is given | Yes |
| 2 | Advice | A how-to or what-if question with no page | No |
| 3 | Open questions | "What should I fix first?" or "How do I rank higher?" | No |
| 4 | Unclear | The intent is not clear | No |
| 5 | Planning | You describe a page you are about to build | No |
| 6 | Checklist without a page | You want the checklist items only | Empty |

The output of each mode:

- **Audit**: One line naming the most important finding, the source key table, then one table per audit (Group, Item, Verdict, Source, Fix)
- **Advice**: A few sentences, one to three verbatim quotes, source URL and date checked. Practical tips go under "Not from the docs:"
- **Open questions**: Quotes on how Search works and what the site owner controls. The skill asks for a page or URL
- **Unclear**: Answered as advice, with an offer to audit
- **Planning**: One line per requirement (source URL and date checked), and the decisions you still need to make
- **Checklist without a page**: Items for the page type, with source URLs

"Page types" (blog post, product page, documentation page) is a rule, not an output. It decides which items apply.

#### Example

This is a layout example only. It is not a real audit. The URLs, verdicts, and quotes are placeholders. A real audit quotes only text taken from the fetched page.

Most important finding: *[one-line summary of the top issue]*

##### Source key

| Key | URL | Date checked |
|---|---|---|
| S1 | https://developers.google.com/search/docs/appearance/title-link | 2026-10-14 |
| S2 | https://developers.google.com/search/docs/appearance/snippet | 2026-10-14 |

##### Audit: `/example/post` (blog post)

| Group | Item | Verdict | Source | Fix |
|---|---|---|---|---|
| 5. Results | Unique, descriptive title. Observed: one `<title>` element in `<head>` | Pass | S1: "[verbatim sentence from fetched page]" | |
| 5. Results | Meta description describes the page | Improve | S2: "[verbatim sentence from fetched page]" | Rewrite the meta description to summarize the post in one sentence |
| 1. Technical | Outbound link has descriptive anchor text | Needs verification | inference: link text not yet checked | |

- For a `Pass`: the Item cell says what was observed in the HTML
- For a `Fail`: the Source cell needs the required wording, and the Item cell needs the observed fact in the page body

---

## structured-data-review (JSON-LD and schema.org)

### Modes and output

This skill checks JSON-LD and schema.org markup. It has three modes:

- **Audit** (judge a page or markup)
  - When: markup or a page is given
  - Output:
    - a source key table
    - one table with the columns `Group | Item | Verdict | Source | Fix`
    - a short list of the types considered and excluded, with the reason
  - Group: the type slug (for example `article`) or `foundation`
- **Advice** (answer a question, no page needed)
  - When: a question about which type or property to use, with no markup given
  - Output:
    - a few sentences
    - one to three verbatim quotes
    - the source URL and the date checked
  - Tables: none. Verdicts: none
- **Planning** (plan a page you have not built yet)
  - When: a page you are about to build
  - Output:
    - the types that apply, one line each with its source URL and date checked
    - the decisions you still need to make
  - Verdicts: none

When you run `page-seo-review` and `structured-data-review` on the same audit:

- The group 6 items in `page-seo-review` are marked N/A with the pointer "judged in the structured-data table"
- Run both to get a complete result

---

## Behavior

- Verdicts: Pass, Improve, Fail, N/A, Needs verification
- Every verdict cites a source URL and the date checked; quotes are verbatim
- Docs are not copied into the plugin; the skills point to the Google Search Central docs listed at the top of this README
- No Search Console data. Items that need it are marked Needs verification

---

## Known limits

- A quoted sentence is word for word, but its source URL can occasionally point to a neighboring page that covers the same topic. Before relying on a quote, open the URL in the Source cell and confirm that the sentence is on that page
- The verdicts are a developer checklist, not a ranking or indexing prediction

---

## Not included

- Search Console help (support.google.com/webmasters) is not covered yet

---

## How this was made (sources and method)

- The skills were written from the English Google Search Central documentation, published by Google under CC BY 4.0
- The pages were downloaded as Markdown, stored in a working folder, and restructured into checklist items with judging rules, modes, and an output format
- The documentation is not copied into this plugin. Each skill points to the canonical pages at https://developers.google.com/search/docs
- Each skill asks the reviewer to open those pages when judging, and to record the URL and the date checked

The checklist reflects the documentation at the time of writing. Google may change its pages, so always check the source page before relying on a verdict.

---

## What it contains

The plugin has two skills, and they contain instructions only. It has no hooks, MCP or LSP servers, monitors or `bin/` directory, so the plugin itself runs no code.

The skills instruct Claude to fetch web pages when judging a page. For shell fetches (used only when a shell is available), the rules are:

- **Who**: Claude, while judging a page the user asked to audit
- **What**: only `https` URLs. Any URL that is not `https` is rejected
- **Where**: only two hosts: `developers.google.com` (the Google Search Central docs, fixed in this plugin) and the host of the page the user asked to audit (the user supplies this URL). Any other host is rejected
- **When**: only when a shell can reach the page. Otherwise the web fetch tool is used
- **Why**: to confirm that each quoted sentence appears in the raw page text, because the web fetch tool returns a model summary whose wording can vary
- **How**: the URL is passed in double quotes as `curl -sL "<url>"`. On Windows, `Invoke-WebRequest -Uri '<url>' -UseBasicParsing` is used, because curl cannot complete the certificate revocation check there. There is no `eval`, no unquoted variables, no pipes built from the URL, and no disabled certificate or revocation checks

Other notes:

- These are Claude's own tool calls, which your permission rules still govern
- Fetched content is read as data and is never executed or piped into a shell
- To check the plugin, run `claude plugin details seo-page-structure-checklist`

---

## License and attribution

- Original parts: MIT License (see `LICENSE`)
- Material derived from Google Search Central: Creative Commons Attribution 4.0 for text, Apache 2.0 for code samples. Contains modifications based on work created and shared by Google. See `THIRD_PARTY_NOTICES.md` and https://developers.google.com/readme/policies
- Images, logos and trademarks of Google are not included
