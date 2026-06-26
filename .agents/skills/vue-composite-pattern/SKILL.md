---
description: Composite Pattern — Object composition in hierarchical structures, only for complexity in recursive groupings
applies_to: [components, utilities, ui]
---
# Skill: Composite Pattern for Vue/TypeScript

> [!IMPORTANT]
> This pattern should NOT be applied always. Use it only for complex hierarchical structures (e.g., trees or nested menus). Keep it simple: for flat lists, use simple arrays.

## 1. When to Apply It
- **Complexity Context**: When individual objects and groups must be treated equally (e.g., recursion in UI or data). Avoid if there's no hierarchy.
- **Examples in Vue**:
  - Nested menu or navigation structures.
  - UI component composition (e.g., panels with subpanels).
  - Data trees (e.g., product categories).
- **Rule**: If you can use an array or flat object, do so. Apply Composite only if you need uniformity in recursive operations.

## 2. Implementation in TypeScript
Define a common interface and classes for leaves and composites.

```typescript
// Example: Composite for menu structure in Vue
interface MenuItem {
  render(): string;
}

class MenuLeaf implements MenuItem {
  constructor(private label: string) {}
  render(): string {
    return `<li>${this.label}</li>`;
  }
}

class MenuComposite implements MenuItem {
  private children: MenuItem[] = [];

  constructor(private label: string) {}

  add(child: MenuItem): void {
    this.children.push(child);
  }

  render(): string {
    const childrenHtml = this.children.map(c => c.render()).join('');
    return `<ul><li>${this.label}${childrenHtml}</li></ul>`;
  }
}

// Usage in a Vue component (integrated with vue-atomic-design.md)
export function useMenuStructure() {
  const root = new MenuComposite('Main');
  root.add(new MenuLeaf('Home'));
  const sub = new MenuComposite('Products');
  sub.add(new MenuLeaf('Electronics'));
  root.add(sub);
  return { render: () => root.render() };
}
```

## 3. Integration with Your Skills
- Combine with [vue-atomic-design](../vue-atomic-design/SKILL.md) for organism hierarchies.
- Avoid in simple lists; use direct v-for.
- Testing: Facilitates recursive tests in [vue-unit-testing](../vue-unit-testing/SKILL.md).

## 4. Advantages and Disadvantages
- **Advantages**: Uniformity in operations, recursive extensibility.
- **Disadvantages**: Extra complexity for flat structures. Only justified in hierarchies.</content>
<parameter name="filePath">c:\Users\rolan\Desarrollos\vue-skills\.ai\skills\vue-composite-pattern.md