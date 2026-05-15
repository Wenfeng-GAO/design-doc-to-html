---
name: design-doc-to-html
description: Use when transforming a design document, proposal, PRD, RFC, KEP, technical plan, Markdown file, DOCX/PDF-derived text, or ordinary document into a polished interactive single-file HTML report, either as a concise summary or as a full-detail interactive replacement for the original document.
---

# Design Doc To HTML

## Overview

Turn dense design/proposal documents into a self-contained interactive HTML report. The default `summary` mode helps readers understand the problem, tradeoffs, design, risks, and review focus quickly. The `detail` mode preserves the full information content of the source document while making it easier to navigate, compare, inspect, and review interactively. The output should be an artifact, not a markdown dump in a browser.

## Output Modes

Pick the mode explicitly when the user names one. If no mode is specified, use `summary` unless the user asks to replace the original Markdown/design doc, preserve all details, produce full content, or avoid information loss; in those cases use `detail`.

| Mode | Use when | Output contract |
| --- | --- | --- |
| `summary` | The reader needs a fast briefing, review prep, or decision-oriented overview. | One polished HTML report that compresses the source into an executive summary, reading spine, risks, decisions, and reviewer checklist. It may omit low-level detail if the omitted content is not needed for the summary. |
| `detail` | The HTML should be usable instead of the original Markdown/design doc, or the user asks for full details. | One polished HTML report that includes a summary plus full-detail sections. It must preserve every substantive source heading, requirement, decision, option, workflow, API/contract, data model, table, example, risk, open question, appendix, and cited source. Information density must be at least as high as the original Markdown; interaction may reorganize content but must not remove it. |

In `detail` mode, do not hide essential content behind JS-only state. Collapsed details are acceptable only when their headings make the hidden content discoverable and the content exists in the HTML source.

## Workflow

1. **Collect source truth**
   - Read the provided document first. If the input is a URL, local path, repo directory, DOCX, PDF, or Markdown file, extract the primary source content before summarizing.
   - Preserve traceability: record source paths/URLs, commit or timestamp when available, and any assumptions.
   - If the user asks for "latest" or a live URL/source, verify freshness before writing.
   - For `detail` mode, build a source outline before writing HTML: all top-level and nested headings, tables, code/example blocks, diagrams, requirements, decisions, risks, open questions, appendices, and links. Use this as a coverage checklist.

2. **Choose the story and mode**
   - Identify the document type: proposal/RFC/KEP, product PRD, architecture plan, review packet, or status memo.
   - Pick one reading spine: usually `Problem -> Decision -> Design -> Tradeoffs -> Risks -> Reviewer checklist`.
   - In `summary` mode, prefer structured interpretation over long excerpts. Quote sparingly; paraphrase and cite source sections.
   - In `detail` mode, keep the reading spine but add a full-content layer. Preserve source granularity by turning each substantive source section into a navigable HTML section, tab, table, callout, code block, checklist, timeline item, or expandable evidence block.

3. **Design the HTML report**
   - Build a first-screen executive summary with title, one-sentence thesis, status metadata, and 2-4 key facts.
   - For `detail` mode, add a persistent table of contents, source coverage indicator, and per-section anchors so the report can replace the Markdown for reference work.
   - Add interactive elements only where they improve understanding:
     - Toggle: alternatives, modes, policies, personas, rollout choices.
     - Timeline: phases, migration steps, release plan, decision history.
     - Calculator/simulator: capacity, cost, latency, quota, blast radius, sizing.
     - Tabs: API/design/testing/operations/risk sections.
     - Expanders: reviewer questions, detailed evidence, caveats.
   - Use a restrained editorial/dashboard hybrid style: strong typography, clear hierarchy, few colors, no generic card soup.

4. **Implement as a single file**
   - Default output: one `.html` file with inline CSS and JS, no build step, no external runtime dependency.
   - Use semantic HTML: `header`, `main`, `section`, `article`, `nav`, `button`, `details`.
   - Include an inline favicon data URI to avoid browser 404 noise.
   - Keep animations progressive. Content must be visible by default; never make first paint depend on IntersectionObserver or JS-only reveal state.
   - Use the starter in `assets/single-file-report-template.html` when speed matters.
   - For `detail` mode, use or adapt `assets/detail-report-template.html` when speed matters.

5. **Verify before completion**
   - For `detail` mode, run a coverage check against the source outline before visual QA. Every source outline item should map to an HTML anchor, table row, details block, card, code block, or source note. If anything is intentionally condensed, the target section must still preserve the facts and cite the source.
   - Run a local static server or open the file directly.
   - Use browser automation when available to check: console errors, desktop viewport, mobile viewport, key interactions, and horizontal overflow.
   - Save preview screenshots when helpful.
   - Report what was verified and what was not.

## Information Architecture Patterns

| Source document | Recommended report shape |
| --- | --- |
| KEP/RFC/architecture proposal | Executive summary, problem, strategy/option switcher, design timeline, API/contracts, risks, observability, sources |
| PRD/product design | User/job summary, workflow map, scope boundaries, decision matrix, success metrics, open questions, reviewer checklist |
| Technical plan | System map, implementation phases, dependencies, migration/rollback, test plan, risk register |
| Review packet | What changed, why it matters, reviewer routes, hotspots, evidence, unresolved questions |

## Detail Mode Information Architecture

Use this shape when `detail` mode is selected:

1. **Executive summary**: keep the fast first-screen summary.
2. **Reading routes**: provide shortcuts for reviewers, implementers, operators, product owners, or other relevant audiences.
3. **Full source map**: list the original document outline and link each item to its rendered HTML section.
4. **Detailed content sections**: preserve each substantive source section with headings, body facts, tables, examples, requirements, and source references.
5. **Decision and tradeoff explorer**: expose alternatives, accepted/rejected options, constraints, and rationale.
6. **Implementation/reference appendix**: include API contracts, data models, config, migration details, test plans, rollout steps, risks, open questions, and appendices from the source.
7. **Evidence and traceability**: include source paths/URLs, document section names, timestamps/commits when known, and assumptions.

Acceptable transformations in `detail` mode:

- Markdown tables become responsive HTML tables or comparison matrices.
- Bullet lists become checklists, grouped rows, timelines, or nested lists.
- Code fences remain complete code blocks unless the user explicitly asks to shorten them.
- Long paragraphs can be split into scannable paragraphs/callouts, but the factual content must remain.
- Repeated text can be deduplicated only if a nearby note states where it was merged.

## Design Rules

- Start with conclusion, then support it.
- Make the first viewport useful without scrolling.
- Use interaction to explain choices, not to decorate.
- Prefer dense but readable panels for operational/technical content.
- Use stable dimensions for controls, charts, cards, and button groups to avoid layout shift.
- Avoid one-note palettes, decorative blobs, large gradients, and marketing-style filler.
- Keep text inside buttons/cards compact; verify mobile wrapping.
- Include "How to read this" only if it saves time; do not explain obvious UI controls.

## Source Handling

- For Markdown, parse headings and tables structurally where possible.
- In `detail` mode, preserve Markdown document structure first, then improve it visually. Do not collapse multiple source sections into one unlabeled summary unless all original headings are represented in the full source map.
- For repo proposals, inspect nearby metadata files such as `kep.yaml`, `owners`, `README`, changelog, or issue links.
- For DOCX/PDF, use available document tools or extraction libraries; keep source page/section references if possible.
- Separate facts from interpretation. Label assumptions explicitly.

## Common Mistakes

| Mistake | Fix |
| --- | --- |
| Turning the document into a pretty long page | Reframe around the reader's decisions and review questions. |
| Adding many interactions | Use 2-4 meaningful interactions tied to hard concepts. |
| First screen appears blank during reveal animation | Content visible by default; animation only enhances. |
| Browser console has favicon 404 | Add inline favicon. |
| No traceability | Add source section with paths, URLs, commit/timestamp. |
| Detail mode loses source content | Build a source outline and coverage map; add missing sections before visual polish. |
| Detail mode becomes raw Markdown in HTML | Convert Markdown structures into semantic HTML components while preserving full content. |
| Unverified polish | Run desktop/mobile/browser checks before claiming done. |

## Optional References

- `references/quality-checklist.md`: final QA checklist and Playwright/browser snippets.
- `assets/single-file-report-template.html`: minimal single-file scaffold for reports.
- `assets/detail-report-template.html`: single-file scaffold for full-detail reports that preserve source coverage.
