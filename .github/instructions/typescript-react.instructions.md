---
applyTo: "**/*.ts,**/*.tsx"
---
# Project coding standards for TypeScript and React

Apply the [general coding guidelines](./general-coding.instructions.md) to all code.

## TypeScript 5 Guidelines
- Use TypeScript for all new code with strict mode enabled
- Leverage TypeScript 5.8.3's enhanced conditional return type checking for better error detection in navigation and state logic
- Use satisfies operator for Expo configuration, theme objects, and style definitions while preserving literal types
- Prefer const assertions for theme constants, color palettes, and configuration objects
- Use optional chaining (?.) and nullish coalescing (??) operators for safe property access
- Use template literal types for route names, event types, and string unions
- Use Utility Types (Partial, Pick, Omit) and Mapped Types for component props and API responses
- Leverage discriminated unions for state management and API response handling
- Use generic constraints with React Native component types
- Implement proper error handling with Result types and safe async patterns
- Use type guards for runtime type validation
- Prefer interfaces over type aliases for object shapes and component props
- Use branded types for IDs and specific string types
- Implement exhaustive checking with never type for switch statements


## React Guidelines
- Use functional components with hooks
- Follow the React hooks rules (no conditional hooks)
- Use React.FC type for components with children
- Keep components small and focused
- Use useReducer with discriminated union actions for complex state
- Handle async state with proper loading, error, and success states
- Implement optimistic updates with proper rollback typing
- Use StyleSheet.create with proper TypeScript style object typing
- Create typed theme systems with consistent color and spacing scales
- Use conditional styling with proper TypeScript union types
- Implement responsive styling with proper screen dimension typing
- Use styled-system approach with TypeScript for consistent component styling
- Handle platform-specific styles with proper TypeScript conditionals
