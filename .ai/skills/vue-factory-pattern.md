---
description: Factory Pattern — Centralized object creation, only when polymorphism or abstraction is needed in object families
applies_to: [utilities, services, components]
---
# Skill: Factory Pattern for Vue/TypeScript

> [!IMPORTANT]
> This pattern should NOT be applied always. Use it only for abstraction in complex creations (e.g., multiple variants of an object based on types). Keep it simple: for direct creations, use `new` or simple functions.

## 1. When to Apply It
- **Complexity Context**: When you decide the object type at runtime (e.g., based on configuration or user), or to encapsulate creation logic. Avoid if there's only one variant.
- **Examples in Vue**:
  - Creation of form validators (e.g., different validation strategies).
  - Factories for dynamic components (e.g., conditional rendering of inputs).
  - API services based on environment (development vs. production).
- **Rule**: If you can instantiate directly, do so. Apply Factory only if it reduces dependencies or facilitates testing/extension.

## 2. Implementation in TypeScript
Use a static function or class to create instances based on parameters.

```typescript
// Example: Factory for validation strategies in forms
interface Validator {
  validate(value: string): boolean;
}

class EmailValidator implements Validator {
  validate(value: string): boolean {
    return value.includes('@');
  }
}

class PhoneValidator implements Validator {
  validate(value: string): boolean {
    return /^\d{10}$/.test(value);
  }
}

class ValidatorFactory {
  static create(type: 'email' | 'phone'): Validator {
    switch (type) {
      case 'email': return new EmailValidator();
      case 'phone': return new PhoneValidator();
      default: throw new Error('Unknown validator type');
    }
  }
}

// Usage in a Vue composable (integrated with vue-form-validation.md)
export function useFormValidator(type: 'email' | 'phone') {
  const validator = ValidatorFactory.create(type);
  return { validate: (value: string) => validator.validate(value) };
}
```

## 3. Integration with Your Skills
- Combine with [vue-form-validation.md](./vue-form-validation.md) for dynamic validators.
- Avoid in basic components; use direct imports.
- Testing: Simplifies mocks in [vue-unit-testing.md](./vue-unit-testing.md).

## 4. Advantages and Disadvantages
- **Advantages**: Abstraction, ease of adding variants, type-safety.
- **Disadvantages**: Overhead for simple cases. Only justified in polymorphism.</content>
<parameter name="filePath">c:\Users\rolan\Desarrollos\vue-skills\.ai\skills\vue-factory-pattern.md