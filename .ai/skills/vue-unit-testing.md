# Skill: Vue 3 Unit Testing Standard

## Rules
1. **Isolation:** Use `shallowMount` ONLY. Do not render child components.
2. **Mocking:** All external functions/services MUST be mocked with `vi.mock()`.
3. **Async:** Use `flushPromises()` and `await nextTick()` for async state updates.
4. **Coverage:** 100% code coverage is mandatory (branches, lines, and functions).