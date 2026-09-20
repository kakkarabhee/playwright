# Playwright Test Automation

End-to-end browser tests written with [Playwright Test](https://playwright.dev/) and TypeScript.

## Requirements

- Node.js 18 or newer
- npm

## Setup

```bash
npm install
npx playwright install
```

The second command downloads the browser binaries Playwright drives.

## Running tests

```bash
# run every test
npx playwright test

# run a single file
npx playwright test tests/example.spec.ts

# run in headed mode (a visible browser window)
npx playwright test --headed

# open the interactive UI mode
npx playwright test --ui

# debug step by step
npx playwright test --debug
```

## Viewing the report

After a run, open the generated HTML report:

```bash
npx playwright show-report
```

## Project layout

```
.
├── tests/                  # test specs
│   └── example.spec.ts
├── playwright.config.ts    # Playwright configuration
└── package.json
```

## Configuration

Settings live in [playwright.config.ts](playwright.config.ts):

- `testDir` — tests are read from `./tests`
- `fullyParallel` — test files run in parallel
- `retries` — failed tests are retried twice on CI, not at all locally
- `reporter` — HTML report
- `trace: 'on-first-retry'` — a trace is recorded when a test is retried, viewable with `npx playwright show-trace`
- `headless: false` — a browser window is shown during local runs
- `projects` — Chromium (Desktop Chrome) is enabled; Firefox, WebKit and mobile viewports are available as commented-out entries

## Writing a test

```ts
import { test, expect } from '@playwright/test';

test('has title', async ({ page }) => {
  await page.goto('https://playwright.dev/');
  await expect(page).toHaveTitle(/Playwright/);
});
```

See the [Playwright docs](https://playwright.dev/docs/intro) for the full API.
