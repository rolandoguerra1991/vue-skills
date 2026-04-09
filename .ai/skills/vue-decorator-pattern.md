---
description: Decorator Pattern — Dynamic extension of objects, only to add responsibilities without modifying base classes
applies_to: [utilities, components, composables]
---
# Skill: Decorator Pattern for Vue/TypeScript

> [!IMPORTANT]
> This pattern should NOT be applied always. Use it only for complex dynamic extensions (e.g., adding logging or caching without changing base code). Keep it simple: for static extensions, use inheritance or simple composition.

## 1. When to Apply It
- **Complexity Context**: When you need to add functionalities to objects at runtime without modifying the original class (e.g., cross-cutting concerns). Avoid if you can use mixins or inheritance.
- **Examples in Vue**:
  - Decorate composables with logging or error handling.
  - Extend components with dynamic themes or permissions.
  - Add caching to API services.
- **Rule**: If you can modify the class directly, do so. Apply Decorator only if you preserve the original interface and need flexibility.

## 2. Implementation in TypeScript
Wrap objects with decorators that implement the same interface.

```typescript
// Example: Decorator to add logging to a service
interface DataService {
  fetchData(): Promise<string>;
}

class BaseDataService implements DataService {
  async fetchData(): Promise<string> {
    return 'Data';
  }
}

class LoggingDecorator implements DataService {
  constructor(private service: DataService) {}

  async fetchData(): Promise<string> {
    console.log('Fetching data...');
    const result = await this.service.fetchData();
    console.log('Data fetched:', result);
    return result;
  }
}

// Usage in a Vue composable (integrated with vue-api-integration.md)
export function useLoggedDataService() {
  const baseService = new BaseDataService();
  const loggedService = new LoggingDecorator(baseService);
  return { fetch: () => loggedService.fetchData() };
}
```

## 3. Integration with Your Skills
- Combine with [vue-composables-standard.md](./vue-composables-standard.md) to extend logic.
- Avoid in simple components; use computed or watchers.
- Testing: Allows isolating decorators in [vue-unit-testing.md](./vue-unit-testing.md).

## 4. Advantages and Disadvantages
- **Advantages**: Extensibility without modification, flexible composition.
- **Disadvantages**: Can increase complexity if overused. Only justified in cross-cutting concerns.</content>
<parameter name="filePath">c:\Users\rolan\Desarrollos\vue-skills\.ai\skills\vue-decorator-pattern.md