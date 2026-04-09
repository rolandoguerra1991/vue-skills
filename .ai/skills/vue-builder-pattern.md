---
description: Builder Pattern — Step-by-step construction of complex objects, only when necessary to avoid overloaded constructors
applies_to: [utilities, components, state]
---
# Skill: Builder Pattern for Vue/TypeScript

> [!IMPORTANT]
> This pattern should NOT be applied always. Use it only when object construction becomes complex (e.g., many optional parameters or nested configurations). Keep it simple: for simple objects, use direct constructors or object literals.

## 1. When to Apply It
- **Complexity Context**: When an object has more than 3-4 optional properties, or requires validations during construction. Avoid constructors with long parameter lists.
- **Examples in Vue**:
  - Configuration of complex components (e.g., forms with dynamic validations).
  - Construction of API queries or initial states in Pinia stores.
  - Creation of configuration objects for libraries (e.g., Axios interceptors).
- **Rule**: If you can use an object literal or simple constructor, do so. Apply Builder only if it reduces errors or improves readability in repetitive code.

## 2. Implementation in TypeScript
Use a Builder class with chained methods. Return `this` for fluency.

```typescript
// Example: Builder for an API configuration object
interface ApiConfig {
  baseUrl: string;
  timeout?: number;
  retries?: number;
  headers?: Record<string, string>;
}

class ApiConfigBuilder {
  private config: Partial<ApiConfig> = {};

  setBaseUrl(url: string): this {
    this.config.baseUrl = url;
    return this;
  }

  setTimeout(timeout: number): this {
    this.config.timeout = timeout;
    return this;
  }

  setRetries(retries: number): this {
    this.config.retries = retries;
    return this;
  }

  setHeaders(headers: Record<string, string>): this {
    this.config.headers = headers;
    return this;
  }

  build(): ApiConfig {
    if (!this.config.baseUrl) throw new Error('baseUrl is required');
    return this.config as ApiConfig;
  }
}

// Usage in a Vue composable
export function useApiConfig() {
  const config = new ApiConfigBuilder()
    .setBaseUrl('https://api.example.com')
    .setTimeout(5000)
    .setRetries(3)
    .build();
  return config;
}
```

## 3. Integration with Your Skills
- Combine with [vue-api-integration.md](./vue-api-integration.md) for service configurations.
- Avoid in simple components; use direct props.
- Testing: Facilitates mocks in [vue-unit-testing.md](./vue-unit-testing.md).

## 4. Advantages and Disadvantages
- **Advantages**: Readability, type-safety, extensible.
- **Disadvantages**: Extra code for simple cases. Only justified in high complexity.</content>
<parameter name="filePath">c:\Users\rolan\Desarrollos\vue-skills\.ai\skills\vue-builder-pattern.md