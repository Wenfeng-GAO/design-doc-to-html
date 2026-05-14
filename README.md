# Design Doc To HTML

Codex skill for turning dense design documents, proposals, PRDs, RFCs, KEPs, technical plans, Markdown files, and DOCX/PDF-derived text into polished interactive single-file HTML reports.

The skill is intended for reviewers who need to understand a document quickly: the generated report should lead with the decision, preserve source traceability, and use interaction only when it clarifies alternatives, phases, risks, or implementation details.

## Contents

- `SKILL.md` - the skill instructions.
- `assets/single-file-report-template.html` - a minimal single-file HTML starter template.
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

## Output Expectations

The default output is a self-contained `.html` file with inline CSS and JavaScript, no build step, and no external runtime dependency. The report should include:

- a first-screen executive summary;
- a clear problem-to-decision reading spine;
- source traceability and labeled assumptions;
- risks, tradeoffs, and reviewer questions;
- browser verification for desktop, mobile, console errors, interactions, and overflow when tooling is available.

## License

MIT
