---
name: "structured-data-review"
description: "Human-judged checklist for Google structured data on public pages (blog, product, docs). Use for any schema.org / JSON-LD / rich result / markup audit, for choosing which type fits a page, and for questions about whether or how to add markup (reviews and stars, products, prices, breadcrumbs, authors, video), answered from the Google docs."
---

# Structured Data Review

Produce a checklist a human can act on: each item has a verdict, a criterion, and a source URL with the date it was checked.

## Modes

- **Audit mode**: markup or a page is given. Follow the rules below and return the tables.
- **Advice mode**: a question about which type or property to use, or whether markup is needed, with no markup given. Find the matching routing row, open the page, and answer in a few sentences with one to three verbatim quotes, the source URL, and the date checked. Do not return the source key table or the verdict table, and do not assign verdicts. If the docs do not answer, say so; you may add short practical suggestions under a separate line starting "Not from the docs:". Start the answer with what the docs state, in the docs' wording; do not open with an instruction stronger than the quotes support. Mark any reading beyond the quotes with `inference:` in the body. Under "Not from the docs:" write only general practice; do not state facts about the user's site (from the conversation, handoff notes, or earlier reports) unless they were checked in this session. Otherwise write Needs verification and say what to check.
- **Planning mode**: the user describes a page they are about to build. List the types that apply and the requirements for each (one line, with source URL and date checked), and the points that depend on decisions the user has not made. Do not assign verdicts.

## Shared rules

Before judging, read `${CLAUDE_PLUGIN_ROOT}/shared/judging-core.md` and follow it. It holds the source rules, verdict meanings, report format, quote rules and dev-server rule shared with the other skill of this plugin. If that file cannot be read, say so, mark every item `Needs verification`, and do not judge.

## Ground rules specific to this skill

- The canonical source is `https://developers.google.com/search/docs/appearance/structured-data/<slug>`. Fetch the page for every type you judge, because requirements, beta flags and availability change.
- Do not copy documentation into files. Quote only short phrases and link to the page.

## Judging rules specific to this skill

- **Foundation pages.** The source key table includes the foundation pages (`intro-structured-data`, `sd-policies`) when a row cites them.
- **Absent markup.** If the page has no markup, items about whether markup is accurate, matches the page, or is misleading are `N/A`, not `Pass`.
- **Children of an invalid parent.** A property of a markup item that is itself invalid (for example, the parent type is not supported or its content is not visible) is not a `Pass`. Judge it under the parent item and mark the child `N/A` with a pointer.
- **Visibility is judged on the page body.** A property whose value appears only in `<meta>` tags, in the `<head>`, or in a JSON-LD block that is not shown in the body is not shown to be invisible by the docs. Judge it `Improve` (never `Fail`), quote the visibility sentence, and add `inference: value found only in head, not in the body`. Use `Fail` only when the body shows a different value for the same property, or the docs' wording is met directly by content on the page.
- **Lists of supported values.** When the docs introduce a list of supported or valid types (for example "Google also supports reviews for the following schema.org types"), a value that is not on the list is unsupported: judge it `Fail`, quote the sentence that introduces the list, and add `inference: not in list`.
- **Absence is not a mismatch.** A property whose value is not written out in the visible body (for example `itemCondition` = NewCondition with no visible "new") is not a contradiction by itself. Judge it `Improve` and quote the sentence about the property, with `inference: value not stated in body`. Use `Fail` only when visible content states a different value.
- **Counts that are not stated.** Do not add numeric conditions (for example, "at least 2 items") unless the fetched page states them. Quote the sentence or leave the item out.
- **One defect, one row (example).** An image that is not shown on the page: put the verdict on the most specific item and point to it from other rows.

## Workflow

1. Identify what the page is (blog post, product page, docs page, or something else) and what it contains (video, reviews, images, a company description, a list of items).
2. Select types with the routing table below. A page usually needs one content type plus several annotations (see Layers). Select every type that matches the visible content, not only the most common one.
3. For each selected type, fetch its canonical page and read: the opening section (what it enables and where it appears), "Guidelines" (technical and content), "Structured data type definitions" (required vs recommended), and any availability or status note at the top.
4. Check the page's markup against what you read. Check that the markup describes content visible on the page.
5. Read the three foundation pages once per audit: `intro-structured-data`, `sd-policies`, `generate-structured-data-with-javascript` (only if markup is injected by JavaScript).
6. Read companion pages named in the routing table. They live outside `structured-data/`, so the table gives the full path.
7. Report results in the output table.

## Layers (why the table is tagged, not a tree)

- `L0-foundation`: shared procedure and policies. Every type page links to them.
- `L1-entity`: who or where (Organization, LocalBusiness).
- `L2-content`: what the page is about (Article, Product, Video, Recipe, Event, JobPosting, and so on).
- `L3-annotation`: attaches to an L1 or L2 item and is not standalone (review-snippet, image-license-metadata, return-policy, shipping-policy, loyalty-program, product-variants, profile-page, paywalled-content, speakable, employer-rating). Review markup is supported on many parent types, so always ask whether an annotation applies.
- `L4-structure`: hierarchy or aggregation of other items (breadcrumb, carousel).

## Routing table

Availability flags below were read on 2026-10-12. Re-read the page; flags can change.

| Page purpose | Slugs to check | Notes |
|---|---|---|
| Any public page, site identity | `organization`, `breadcrumb` | organization: no required properties per its page. Return/shipping/loyalty policies are nested under it. breadcrumb: its page recommends a typical user path, not a copy of the URL structure |
| Blog or news article | `article`, `breadcrumb`, `organization` | article: no required properties per its page; images must represent the marked up content. Add `speakable` only if you accept beta (U.S., English) |
| Paywalled content | `paywalled-content`, `article` | Separates paywalls from cloaking |
| Product you cannot buy on the page (review, editorial) | `product`, `product-snippet` | `product` explains how to choose between snippet and merchant listing |
| Product you can buy on the page | `product`, `merchant-listing`, `product-variants`, `return-policy`, `shipping-policy`, `loyalty-program`, `organization` | Merchant Center alternatives exist for policies; some parts are beta |
| Reviews or ratings on a supported parent type | `review-snippet` | Check the page for the list of supported parent types |
| Software or app documentation | `software-app`, `article`, `breadcrumb` | |
| Page embeds video | `video` and companion `https://developers.google.com/search/docs/appearance/video` | The companion is the Video SEO best practices page (indexing, watch pages, thumbnails, previews). It is a different page from `video`; read both. LIVE badge, Clip and SeekToAction are separate sections of `video` |
| Licensed images | `image-license-metadata` | Applies to images on any content type |
| Recipe | `recipe`, `video`, `review-snippet`, `carousel` | Also an ItemList variant |
| Physical business or restaurant | `local-business`, `organization`, `review-snippet` | Restaurant carousel is limited access |
| List of items from one site | `carousel`, plus `course`, `movie`, `recipe` or `local-business` | `carousels-beta` is beta and limited to EEA, Turkiye and South Africa |
| Event | `event` | |
| Job posting | `job-posting`, `organization`, `employer-rating` | Education/experience properties are beta |
| Vacation rental | `vacation-rental` | Partner-only (Technical Account Manager, Hotel Center); English |
| Forum or community | `discussion-forum`, `profile-page`, `qapage` | Q&A-pattern forums use `qapage` |
| Learning content | `course`, `education-qa`, `math-solvers`, `qapage` | course: English |
| Dataset | `dataset`, `organization` | Dataset Search; parts are beta |
| Book feed | `book` | Feed-based submission, not page markup |
| Movie list | `movie`, `carousel`, `review-snippet` | Carousel is mobile-only |
| Fact check | `factcheck` | The page states Google is phasing out support. Report that status |
| Shop operations (out of stock, variants, URL structure, shipping and returns scope, price display) | `product`, `product-variants`, `merchant-listing`, `return-policy`, `shipping-policy`, plus `https://developers.google.com/search/docs/specialty/ecommerce/designing-a-url-structure-for-ecommerce-sites` and `https://developers.google.com/search/docs/specialty/ecommerce/help-google-understand-your-ecommerce-site-structure` | Open the pages for the decision the user asks about; do not answer from memory |
| Reviews you collected yourself (testimonials) | `review-snippet`, plus `https://developers.google.com/search/docs/specialty/ecommerce/write-high-quality-reviews` and `https://developers.google.com/search/docs/appearance/reviews-system` | Check the review-snippet page for what it says about reviews of your own entity |
| Product data without Merchant Center | `merchant-listing`, plus `https://developers.google.com/search/docs/specialty/ecommerce/share-your-product-data-with-google` and `https://developers.google.com/search/docs/specialty/ecommerce/where-ecommerce-data-can-appear-on-google` | Read what each page says about markup versus feeds |

There is no standalone Person type page. For an author, read the `author` property on the `article` page; for a profile page, read `profile-page`. Do not invent a Person page.

If the page does not fit any row, open the left navigation of the structured data section in the canonical docs and look for a type that was added after this table was written. State the gap in the report instead of guessing.

## Output format

Start with the source key table, then return one table per audit (columns as in the shared rules, with these notes):

| Group | Item | Verdict | Source | Fix |
|---|---|---|---|---|

- Group: the type slug (for example `article`) or `foundation`.
- Item: one concrete check, phrased so a human can verify it. For a `Pass`, say what was observed in the markup.
- Verdict (use exactly these five): `Pass`, `Improve`, `Fail`, `N/A`, `Needs verification`.
- Source: a defined key and the short verbatim quote, or `inference: <reason>`, as required by the judging rules. The date is in the key table.
- Fix: the smallest change that would turn a `Fail` or `Improve` into a `Pass`; leave empty for `Pass`, `N/A`, and `Needs verification`.

End with a short list of types you considered and excluded, with the reason, so the reader can disagree.

## Attribution

This checklist contains modifications based on work created and shared by Google and used according to terms described in the Creative Commons 4.0 Attribution License. Keep this notice when the checklist is redistributed.