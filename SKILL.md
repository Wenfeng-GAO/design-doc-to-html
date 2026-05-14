---
name: design-doc-to-html
description: Use when transforming a design document, proposal, PRD, RFC, KEP, technical plan, Markdown file, DOCX/PDF-derived text, or ordinary document into a polished interactive single-file HTML report for faster reviewer understanding.
---

# Design Doc To HTML

## Overview

Turn dense design/proposal documents into a self-contained interactive HTML report that helps readers understand the problem, tradeoffs, design, risks, and review focus quickly. The output should be an artifact, not a markdown dump in a browser.

## Workflow

1. **Collect source truth**
   - Read the provided document first. If the input is a URL, local path, repo directory, DOCX, PDF, or Markdown file, extract the primary source content before summarizing.
   - Preserve traceability: record source paths/URLs, commit or timestamp when available, and any assumptions.
   - If the user asks for "latest" or a live URL/source, verify freshness before writing.

2. **Choose the story**
   - Identify the document type: proposal/RFC/KEP, product PRD, architecture plan, review packet, or status memo.
   - Pick one reading spine: usually `Problem -> Decision -> Design -> Tradeoffs -> Risks -> Reviewer checklist`.
   - Prefer structured interpretation over long excerpts. Quote sparingly; paraphrase and cite source sections.

3. **Design the HTML report**
   - Build a first-screen executive summary with title, one-sentence thesis, status metadata, and 2-4 key facts.
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

5. **Verify before completion**
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
| Unverified polish | Run desktop/mobile/browser checks before claiming done. |

## Optional References

- `references/quality-checklist.md`: final QA checklist and Playwright/browser snippets.
- `assets/single-file-report-template.html`: minimal single-file scaffold for reports.
