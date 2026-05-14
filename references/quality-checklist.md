# Quality Checklist

Use this before reporting completion.

## Content

- The first screen states the proposal name, source, status, and core decision.
- The report has a clear reading spine, not a section-by-section dump.
- Each source fact can be traced to a path, URL, commit, section, or timestamp.
- Assumptions are labeled.
- Risks, tradeoffs, and reviewer questions are visible without hunting.

## Interaction

- Every interactive control changes visible content.
- Buttons, tabs, toggles, sliders, and details elements are keyboard reachable.
- Interactions explain difficult ideas: alternatives, phases, impact, scope, risk, or capacity.
- The page remains useful if JS fails; core content should still be present or obvious.

## Visual

- Desktop first viewport is nonblank and useful.
- Mobile viewport has no horizontal overflow.
- Text does not overlap or overflow controls/cards.
- Contrast is readable.
- Palette is not dominated by one hue family.
- Animations do not hide content by default.

## Browser Verification

If Playwright is available:

```bash
NODE_PATH=/path/to/node_modules node -e '
(async () => {
  const { chromium } = require("playwright");
  const browser = await chromium.launch({ headless: true, executablePath: "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" });
  const page = await browser.newPage({ viewport: { width: 1440, height: 1000 } });
  const errors = [];
  page.on("console", msg => { if (msg.type() === "error") errors.push(msg.text()); });
  page.on("pageerror", err => errors.push(err.message));
  await page.goto("http://127.0.0.1:8765/report.html", { waitUntil: "networkidle" });
  await page.screenshot({ path: "preview-desktop.png", fullPage: false });
  await page.setViewportSize({ width: 390, height: 844 });
  const overflow = await page.evaluate(() => document.documentElement.scrollWidth > document.documentElement.clientWidth);
  await page.screenshot({ path: "preview-mobile.png", fullPage: false });
  await browser.close();
  console.log(JSON.stringify({ overflow, errors }, null, 2));
})();
'
```

Adapt the executable path or use the in-app Browser plugin when it is available.
