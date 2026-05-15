# Design Doc To HTML

Codex skill for turning dense design documents, proposals, PRDs, RFCs, KEPs, technical plans, Markdown files, and DOCX/PDF-derived text into polished interactive single-file HTML reports.

The skill supports two modes:

- `summary`: a concise, decision-oriented report for reviewers who need to understand a document quickly.
- `detail`: a full-fidelity interactive report that can replace the original Markdown/design document without losing substantive content.

Both modes should lead with the decision, preserve source traceability, and use interaction only when it clarifies alternatives, phases, risks, or implementation details.

## Contents

- `SKILL.md` - the skill instructions.
- `assets/single-file-report-template.html` - a minimal single-file HTML starter template.
- `assets/detail-report-template.html` - a full-detail starter template with source coverage, navigation, and reference sections.
- `references/quality-checklist.md` - final content, interaction, visual, and browser QA checklist.
- `agents/openai.yaml` - display metadata for OpenAI/Codex-style agent interfaces.

## Install

Clone this repository into your Codex skills directory:

```bash
git clone https://github.com/Wenfeng-GAO/design-doc-to-html.git ~/.codex/skills/design-doc-to-html
```

Then invoke it as:

```text
Use $design-doc-to-html to transform this design document into an interactive HTML report.
```

For full-detail output that preserves all markdown content:

```text
Use $design-doc-to-html in detail mode to transform this design document into an interactive HTML report that can replace the original Markdown without information loss.
```

## Output Expectations

The default output is a self-contained `.html` file with inline CSS and JavaScript, no build step, and no external runtime dependency.

Summary mode should include:

- a first-screen executive summary;
- a clear problem-to-decision reading spine;
- source traceability and labeled assumptions;
- risks, tradeoffs, and reviewer questions;
- browser verification for desktop, mobile, console errors, interactions, and overflow when tooling is available.

Detail mode should include everything above plus:

- a source outline and coverage map;
- every substantive original heading, requirement, decision, option, workflow, API/contract, data model, table, code block, diagram/image/embed, example, risk, open question, appendix, and cited source;
- full-content detail sections with anchors so readers can use the HTML instead of the original Markdown;
- embedded asset handling for Obsidian `![[...]]` embeds and Markdown images, including visible incomplete-coverage warnings when assets cannot be resolved or rendered;
- a final coverage check confirming source content was preserved, not just summarized.

## License

MIT
