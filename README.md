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

# run a folder
npx playwright test tests/basics

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
│   ├── example.spec.ts     # starter test against playwright.dev
│   └── basics/             # recorded login flows
│       ├── test1.spec.ts   # app.thetestingacademy.com — email step, back to home
│       └── test2.spec.ts   # courses.thetestingacademy.com — email + password sign in
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

## The `tests/basics` specs

These were captured with the Playwright recorder rather than written by hand:

```bash
npx playwright codegen https://app.thetestingacademy.com/
```

The recorder opens a browser, follows along as you click and type, and writes the
matching `page.getByRole(...)` calls into a spec you can paste into `tests/basics`.

- **test1.spec.ts** — opens `app.thetestingacademy.com`, clicks Login, fills in an email
  address, continues, then returns via the "← Back to Home" link.
- **test2.spec.ts** — opens `courses.thetestingacademy.com`, fills the sign-in form with an
  email and password, submits, retypes the password and submits again.

Both are click-through recordings with no `expect` assertions yet, so they only fail if a
step cannot find its element. The reCAPTCHA steps the recorder captured for `test2.spec.ts`
were dropped: they addressed the challenge iframes by generated names such as
`iframe[name="a-j3t614am12xa"]` and picked tiles by index, and both change on every load.
If the site shows the challenge on a run, that test will stop at the Login button.

## Writing a test

```ts
import { test, expect } from '@playwright/test';

test('has title', async ({ page }) => {
  await page.goto('https://playwright.dev/');
  await expect(page).toHaveTitle(/Playwright/);
});
```

See the [Playwright docs](https://playwright.dev/docs/intro) for the full API.
