---
description: Playwright E2E testing — test IDs, API mocking, user flow focus
applies_to: [testing, e2e]
---
# Skill: Vue 3 E2E Testing

> [!IMPORTANT]
> **Dependencies:** `playwright` (See [vue-project-setup.md](./vue-project-setup.md))


## 1. Tool Selection
- Use modern E2E testing tools like **Playwright** or **Cypress**. (Playwright is often preferred for newer Vue projects due to its speed and native async architecture).
- Ensure configuration is separated from unit tests (e.g., using a distinct `playwright.config.ts` or `cypress.config.ts`).

## 2. Robust Selectors (Test IDs)
- **CRITICAL:** Do NOT target elements using semantic HTML tags (`button`, `div`), volatile CSS classes (`.bg-red-500`, `.modal-open`), or structural hierarchy (`ul > li:nth-child(2)`).
- ALWAYS use dedicated data attributes for E2E tests, such as `data-testid` or `data-cy`.
- These attributes clearly indicate that an element is under test and should not be removed during refactoring.

```vue
<!-- Good: Using Test IDs -->
<button
  data-testid="submit-login"
  class="btn-primary"
>
  Login
</button>
```

```typescript
// Good: Playwright Selector
await page.getByTestId('submit-login').click()

// Good: Cypress Selector
cy.get('[data-testid="submit-login"]').click()
```

## 3. Mocking APIs
- Do not hit production or live staging APIs during E2E tests for continuous integration (CI).
- Use interception (e.g., `page.route` in Playwright or `cy.intercept` in Cypress) to mock network requests. E2E tests should be deterministic and isolated from external network instability.
- Keep mocking close to the test file (using fixture files).

## 4. Focus on User Flows
- E2E tests should not test minor component interactions (like "does this tooltip appear on hover if I pass a prop"). That is the realm of Unit/Component testing (Vitest).
- E2E tests evaluate critical user journeys: "Can a user log in, add an item to the cart, and checkout?"
- Keep the number of E2E tests low (high-value tests) compared to unit tests, as they are slower to execute.
