---
applyTo: "**"
---
# Project general coding standards

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

## Error Handling
- Use try/catch blocks for async operations
- Implement proper error boundaries in React components
- Always log errors with contextual information
- Use custom error classes for specific error types
- Avoid using console.error in production code; use a logging library instead
- Handle errors gracefully and provide user-friendly messages
- Use `throw new Error('message')` for throwing errors with clear messages
- Avoid swallowing errors; always handle them appropriately


## Code Structure
- Organize code into modules and directories based on functionality
- Keep files small and focused on a single responsibility
- Use index files to re-export modules for cleaner imports
- Use consistent file extensions (.js, .ts, .jsx, .tsx)
- Use a consistent directory structure (e.g., `components`, `services`, `utils`, `hooks`)
- Group related functions and classes together
- Use a consistent order for imports: external libraries, internal modules, styles
- Use absolute imports for better readability and maintainability
- Use `export default` for main exports in modules
- Use named exports for utility functions and constants
- Avoid circular dependencies by carefully structuring imports
- Use `async`/`await` for asynchronous code to improve readability
- Use `Promise.all` for concurrent asynchronous operations

## Comments and Documentation
- Write clear and concise comments explaining the "why" behind complex logic
- Avoid redundant comments that restate the code
- Use TODO and FIXME comments to indicate areas for future improvement

## Version Control
- Commit changes frequently with clear, descriptive commit messages
- Use branches for new features or bug fixes
- Follow a consistent branching strategy (e.g., Git Flow, trunk-based development)
- Use pull requests for code reviews and discussions before merging