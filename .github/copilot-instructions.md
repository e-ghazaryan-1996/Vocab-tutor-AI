## TypeScript 5 Guidelines
- Use TypeScript for all new code with strict mode enabled
- Leverage TypeScript 5.8.3's enhanced conditional return type checking for better error detection
- Use satisfies operator for type validation while preserving literal types
- Prefer const assertions for immutable data structures
- Use optional chaining (?.) and nullish coalescing (??) operators
- Use template literal types for precise string typing
- Use Utility Types and Mapped Types


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

## Naming Conventions
- Use PascalCase for component names, interfaces, and type aliases
- Use camelCase for variables, functions, and methods
- Prefix private class members with underscore (_)
- Use ALL_CAPS for constants
- Use descriptive names that convey purpose and intent
- Avoid abbreviations unless they are widely recognized
- Use singular nouns for functions and methods that perform actions (e.g., `getUser`, `createPost`)
- Use plural nouns for collections (e.g., `users`, `posts`)
- Use prefixes for boolean variables (e.g., `isActive`, `hasPermission`)
- Use suffixes for event handlers (e.g., `onClick`, `onChange`)