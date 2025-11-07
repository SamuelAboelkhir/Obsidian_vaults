---
tags:
- Programming-Language/JS-TS
- Framework
- PG
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG JS-TS index|Back to index]]
# PMM Playwright Testing Guide

Welcome to the Playwright testing suite for the PMM project! This guide will help you set up, run, and write your own end-to-end (E2E) tests using Playwright.

---

## Overview

- **Location:** All Playwright tests are in this `test/` folder.
- **Purpose:** Ensure the PMM app works as expected from a user's perspective by simulating real browser interactions.
- **Framework:** [Playwright](https://playwright.dev/)

---

## Setup

1. **Install dependencies**
   - Make sure you have [Node.js](https://nodejs.org/) and [pnpm](https://pnpm.io/) installed.
   - In the `frontend/` directory, run:
     ```sh
     pnpm install
     ```

2. **Install Playwright browsers**
   - Still in `frontend/`, run:
     ```sh
     pnpm exec playwright install
     ```

3. **Start the PMM app**
   - The app should be running locally (default: `http://localhost:5173`).
   - In a separate terminal, start the backend if needed.

---

## Running Tests

- **All tests:**
  ```sh
  pnpm exec playwright test
  ```
- **A specific test file:**
  ```sh
  pnpm exec playwright test test/YourTestFile.spec.ts
  ```
- **With UI (debug mode):**
  ```sh
  pnpm exec playwright test --ui
  ```
- **View HTML report:**
  After running tests:
  ```sh
  pnpm exec playwright show-report
  ```

---

## Writing New Tests

1. **Create a new file:**
   - Place it in `frontend/test/`.
   - Name it descriptively, e.g., `FeatureName.spec.ts`.

2. **Basic test structure:**
   ```ts
   import { test, expect } from '@playwright/test';

   test('describe your scenario', async ({ page }) => {
     await page.goto('http://localhost:5173/');
     // Interact with the page
     // Assert expected outcomes
   });

   

   
   ```

3. **Common Playwright commands:**
   - `page.goto(url)` — Navigate to a page
   - `page.locator(selector)` — Select an element
   - `page.getByRole(...)` — Select by ARIA role (preferred for accessibility)
   - `locator.click()`, `locator.fill()`, `locator.check()` — Interact with elements
   - `expect(locator).toBeVisible()` — Assert element is visible
   - `page.waitForResponse()` — Wait for and assert on API calls

4. **Selectors:**
   - Prefer `[data-path="..."]` attributes or accessible roles/labels for robust selectors.

5. **API Interception Example:**
   ```ts
   const [response] = await Promise.all([
     page.waitForResponse(res =>
       res.url().includes('/api/your-endpoint') && res.request().method().toLowerCase() === 'post'
     ),
     page.getByRole('button', { name: 'Save', exact: true }).click(),
   ]);
   expect([200, 201]).toContain(response.status());
   ```

---

## Best Practices

- Use `test.setTimeout(ms)` for long flows.
- Use `await` for all Playwright actions.
- Use `try/catch` for optional UI elements.
- Use `page.evaluate()` to interact with hidden or custom elements if needed.
- Assert on both UI and API responses for reliability.
- Keep tests independent—avoid relying on state from previous tests.
- Clean up test data if possible.

---

## Troubleshooting

- **Timeouts:**
  - Increase test timeout with `test.setTimeout(60000)`.
  - Ensure the app and backend are running.
- **Selectors not found:**
  - Double-check the selector or use `page.pause()` to debug.
- **Devtools overlays blocking UI:**
  - Hide overlays in your test with:
    ```ts
    await page.evaluate(() => {
      const devtools = document.querySelector('[aria-label="Open Tanstack query devtools"]');
      if (devtools) (devtools as HTMLElement).style.display = 'none';
    });
    ```
- **API assertions failing:**
  - Log network requests with:
    ```ts
    page.on('request', req => console.log(req.method(), req.url()));
    ```

---

## Resources

- [Playwright Docs](https://playwright.dev/docs/intro)
- [Playwright Test Examples](https://playwright.dev/docs/test-examples)
- [PMM Source Code](../src/)

---

Happy testing! 🎉 