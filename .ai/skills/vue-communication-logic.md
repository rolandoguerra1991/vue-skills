# Skill: Component Communication Threshold

## 1. Emits (Direct Neighbor Rule)
- Use ONLY between direct Parent and Child.
- **Prohibition:** "Event Bubbling" (B emitting C's events to A) is forbidden.

## 2. Provide / Inject (Ancestry Communication)
- Use for deep trees (Grandparent to Grandchild) to avoid Prop-Drilling.
- Ideal for UI Contexts (e.g., TabGroups, Modals).

## 3. Pinia
- Use for cross-domain state or business logic that persists across views.
- **Implementation:** Follow the "Setup Store" conventions defined in `vue-state-management.md`.