---
applyTo: "**/*.test.ts,**/*.test.tsx,**/*.spec.ts,**/*.spec.tsx"
---
# Jest Testing Guidelines for Expo React Native App

## Test Organization and Structure

### File Naming and Location
- Use `.test.tsx` for React component tests
- Use `.test.ts` for utility function and hook tests
- Use `.spec.ts` for integration and API tests
- Place test files adjacent to the code they test or in a `__tests__` directory
- Use descriptive test file names that match the component/module being tested
- Follow Expo's naming convention: `ComponentName-test.tsx` for components

### Test Directory Structure
Follow Expo's recommended patterns for organizing tests:

**Option 1: Single `__tests__` directory (recommended for smaller projects)**
```
__tests__/
  HomeScreen-test.tsx
  CustomText-test.tsx
  utils-test.ts
components/
  ThemedText.tsx
  ThemedView.tsx
```

**Option 2: Multiple `__tests__` subdirectories (recommended for larger projects)**
```
components/
  ThemedText.tsx
  __tests__/
    ThemedText-test.tsx
utils/
  index.ts
  __tests__/
    index-test.ts
services/
  api.ts
  __tests__/
    api-test.ts
```

### Test Structure
- Follow AAA pattern: Arrange, Act, Assert
- Use `describe` blocks to group related tests
- Use descriptive test names that explain what is being tested and expected outcome
- Keep tests focused on a single behavior or scenario
- Use `beforeEach` and `afterEach` for test setup and cleanup

```typescript
// Generated by Copilot
describe('<ComponentName />', () => {
  beforeEach(() => {
    // Setup code
  });

  afterEach(() => {
    // Cleanup code
  });

  test('should render correctly with default props', () => {
    // Test implementation
  });

  test('should handle user interaction properly', () => {
    // Test implementation
  });
});
```

## Jest Configuration for Expo

### Basic Configuration
Add the following configuration to your package.json:

```json
// Generated by Copilot
{
  "scripts": {
    "test": "jest --watchAll",
    "test:ci": "jest --ci --coverage --watchAll=false",
    "test:debug": "jest -o --watch --coverage=false",
    "test:final": "jest",
    "test:update-snapshots": "jest -u --coverage=false"
  },
  "jest": {
    "preset": "jest-expo",
    "collectCoverage": true,
    "collectCoverageFrom": [
      "**/*.{ts,tsx,js,jsx}",
      "!**/coverage/**",
      "!**/node_modules/**",
      "!**/babel.config.js",
      "!**/expo-env.d.ts",
      "!**/.expo/**"
    ]
  }
}
```

### Transform Ignore Patterns
Configure `transformIgnorePatterns` based on your package manager:

**For npm/Yarn:**
```json
// Generated by Copilot
{
  "jest": {
    "preset": "jest-expo",
    "transformIgnorePatterns": [
      "node_modules/(?!((jest-)?react-native|@react-native(-community)?)|expo(nent)?|@expo(nent)?/.*|@expo-google-fonts/.*|react-navigation|@react-navigation/.*|@sentry/react-native|native-base|react-native-svg)"
    ]
  }
}
```

**For pnpm:**
```json
// Generated by Copilot
{
  "jest": {
    "preset": "jest-expo",
    "transformIgnorePatterns": [
      "node_modules/(?!(?:.pnpm/)?((jest-)?react-native|@react-native(-community)?|expo(nent)?|@expo(nent)?/.*|@expo-google-fonts/.*|react-navigation|@react-navigation/.*|@sentry/react-native|native-base|react-native-svg))"
    ]
  }
}
```

## React Native Testing with Jest and React Native Testing Library

### Component Testing
- Use `@testing-library/react-native` for component testing (replaces deprecated `react-test-renderer`)
- Test user interactions, not implementation details
- Use `render` from testing library
- Test accessibility features with `getByRole` and `getByLabelText`
- Mock native modules and Expo APIs properly

```typescript
// Generated by Copilot
import { render, fireEvent, screen } from '@testing-library/react-native';
import { ComponentName } from '../ComponentName';

describe('<ComponentName />', () => {
  test('should handle press events correctly', () => {
    const mockOnPress = jest.fn();
    
    render(<ComponentName onPress={mockOnPress} />);
    
    const button = screen.getByRole('button');
    fireEvent.press(button);
    
    expect(mockOnPress).toHaveBeenCalledTimes(1);
  });

  test('should render text content correctly', () => {
    const { getByText } = render(<ComponentName text="Welcome!" />);
    
    getByText('Welcome!');
  });
});
```

### Hook Testing
- Use `@testing-library/react-hooks` for testing custom hooks
- Test hook return values and side effects
- Test hook state changes and async operations
- Mock dependencies and external services

```typescript
// Generated by Copilot
import { renderHook, act } from '@testing-library/react-hooks';
import { useCustomHook } from '../useCustomHook';

describe('useCustomHook', () => {
  test('should return initial state correctly', () => {
    const { result } = renderHook(() => useCustomHook());
    
    expect(result.current.isLoading).toBe(false);
    expect(result.current.data).toBeNull();
  });

  test('should handle async operations', async () => {
    const { result } = renderHook(() => useCustomHook());
    
    await act(async () => {
      await result.current.fetchData();
    });
    
    expect(result.current.isLoading).toBe(false);
    expect(result.current.data).toBeDefined();
  });
});
```

## Mocking Guidelines

### Expo and React Native Mocks
- Mock Expo modules using `jest.mock` at the top of test files
- Use `expo-constants` mock for app configuration
- Mock navigation with `@react-navigation/native` testing utilities
- Mock async storage and other native dependencies

```typescript
// Generated by Copilot
jest.mock('expo-font', () => ({
  useFonts: jest.fn(() => [true]),
}));

jest.mock('@react-navigation/native', () => ({
  useNavigation: () => ({
    navigate: jest.fn(),
    goBack: jest.fn(),
  }),
  useRoute: () => ({
    params: {},
  }),
}));

jest.mock('expo-haptics', () => ({
  impactAsync: jest.fn(),
  ImpactFeedbackStyle: {
    Light: 'light',
  },
}));
```

### API and Service Mocks
- Mock API calls and external services
- Use `jest.spyOn` for partial mocking
- Create mock data that matches TypeScript interfaces
- Use `MSW` (Mock Service Worker) for complex API mocking

```typescript
// Generated by Copilot
const mockApiResponse = {
  data: {
    id: 1,
    name: 'Test User',
    email: 'test@example.com',
  },
} satisfies ApiResponse;

jest.mock('../services/apiService', () => ({
  fetchUser: jest.fn(() => Promise.resolve(mockApiResponse)),
  updateUser: jest.fn(() => Promise.resolve(mockApiResponse)),
}));
```

## Snapshot Testing

### When to Use Snapshot Tests
- Use snapshot tests sparingly for critical UI components
- Prefer end-to-end tests for UI testing when possible
- Focus on components with complex styling or layout
- Ensure snapshots are reviewed during code reviews

```typescript
// Generated by Copilot
import { render } from '@testing-library/react-native';
import { CustomText } from '../CustomText';

describe('<CustomText />', () => {
  test('should render correctly', () => {
    const tree = render(<CustomText>Some text</CustomText>).toJSON();
    
    expect(tree).toMatchSnapshot();
  });

  test('should render with custom styles', () => {
    const tree = render(
      <CustomText style={{ color: 'red' }}>Styled text</CustomText>
    ).toJSON();
    
    expect(tree).toMatchSnapshot();
  });
});
```

## Testing Themed Components

### Theme Context Testing
- Provide theme context in tests using custom render function
- Test both light and dark theme variations
- Test color scheme changes and theme switching

```typescript
// Generated by Copilot
import { render } from '@testing-library/react-native';
import { ThemeProvider } from '@react-navigation/native';
import { ThemedComponent } from '../ThemedComponent';

const renderWithTheme = (component: React.ReactElement, theme: 'light' | 'dark' = 'light') => {
  const themeConfig = theme === 'light' ? LightTheme : DarkTheme;
  
  return render(
    <ThemeProvider value={themeConfig}>
      {component}
    </ThemeProvider>
  );
};

describe('<ThemedComponent />', () => {
  test('should render with light theme colors', () => {
    const { getByTestId } = renderWithTheme(<ThemedComponent testID="component" />);
    
    const component = getByTestId('component');
    expect(component.props.style).toMatchObject({
      backgroundColor: expect.stringMatching(/#fff|white/),
    });
  });

  test('should render with dark theme colors', () => {
    const { getByTestId } = renderWithTheme(<ThemedComponent testID="component" />, 'dark');
    
    const component = getByTestId('component');
    expect(component.props.style).toMatchObject({
      backgroundColor: expect.stringMatching(/#151718/),
    });
  });
});
```

## Async Testing and Error Handling

### Async Operations
- Use `waitFor` for testing async state changes
- Test loading states, success states, and error states
- Use fake timers for testing time-dependent code
- Test cleanup and unmounting scenarios

```typescript
// Generated by Copilot
import { render, waitFor, screen } from '@testing-library/react-native';
import { AsyncComponent } from '../AsyncComponent';

describe('<AsyncComponent />', () => {
  test('should show loading state initially', () => {
    render(<AsyncComponent />);
    
    expect(screen.getByText('Loading...')).toBeTruthy();
  });

  test('should show data after successful fetch', async () => {
    render(<AsyncComponent />);
    
    await waitFor(() => {
      expect(screen.getByText('Data loaded successfully')).toBeTruthy();
    });
  });

  test('should handle error states gracefully', async () => {
    // Mock API to throw error
    jest.spyOn(console, 'error').mockImplementation(() => {});
    
    render(<AsyncComponent />);
    
    await waitFor(() => {
      expect(screen.getByText('Error loading data')).toBeTruthy();
    });
  });
});
```

### Error Boundary Testing
- Test error boundaries with components that throw errors
- Test error recovery and fallback UI
- Use `console.error` mocking for error boundary tests

```typescript
// Generated by Copilot
import { render, Text } from '@testing-library/react-native';
import { ErrorBoundary } from '../ErrorBoundary';

interface ThrowErrorProps {
  shouldThrow: boolean;
}

const ThrowError: React.FC<ThrowErrorProps> = ({ shouldThrow }) => {
  if (shouldThrow) {
    throw new Error('Test error');
  }
  return <Text>No error</Text>;
};

describe('<ErrorBoundary />', () => {
  beforeEach(() => {
    jest.spyOn(console, 'error').mockImplementation(() => {});
  });

  afterEach(() => {
    jest.restoreAllMocks();
  });

  test('should catch and display error', () => {
    const { getByText } = render(
      <ErrorBoundary>
        <ThrowError shouldThrow={true} />
      </ErrorBoundary>
    );
    
    expect(getByText('Something went wrong')).toBeTruthy();
  });
});
```

## Performance and Animation Testing

### Animation Testing
- Mock `react-native-reanimated` for consistent test results
- Test animation completion and intermediate states
- Use `jest.useFakeTimers()` for time-based animations

```typescript
// Generated by Copilot
jest.mock('react-native-reanimated', () => {
  const Reanimated = require('react-native-reanimated/mock');
  Reanimated.default.call = () => {};
  return Reanimated;
});

describe('<AnimatedComponent />', () => {
  beforeEach(() => {
    jest.useFakeTimers();
  });

  afterEach(() => {
    jest.useRealTimers();
  });

  test('should complete animation after specified duration', () => {
    const { getByTestId } = render(<AnimatedComponent />);
    
    jest.advanceTimersByTime(1000);
    
    const animatedElement = getByTestId('animated-element');
    expect(animatedElement.props.style.opacity).toBe(1);
  });
});
```

## AI Feature Testing

### AI Response Mocking
- Mock AI API responses with realistic data
- Test loading states during AI processing
- Test error handling for AI service failures
- Test offline behavior and fallbacks

```typescript
// Generated by Copilot
interface AiResponse {
  definition: string;
  examples: string[];
  difficulty: 'beginner' | 'intermediate' | 'advanced';
}

const mockAiResponse: AiResponse = {
  definition: 'A word or phrase used to describe something',
  examples: ['This is an example sentence'],
  difficulty: 'intermediate',
} satisfies AiResponse;

jest.mock('../services/aiService', () => ({
  generateDefinition: jest.fn(() => Promise.resolve(mockAiResponse)),
  analyzePronunciation: jest.fn(() => Promise.resolve({ accuracy: 85 })),
}));

describe('<VocabularyCard /> with AI features', () => {
  test('should display AI-generated definition', async () => {
    const { getByText } = render(<VocabularyCard word="example" />);
    
    await waitFor(() => {
      expect(getByText(mockAiResponse.definition)).toBeTruthy();
    });
  });

  test('should handle AI service errors gracefully', async () => {
    const aiService = require('../services/aiService');
    aiService.generateDefinition.mockRejectedValueOnce(new Error('AI service unavailable'));
    
    const { getByText } = render(<VocabularyCard word="example" />);
    
    await waitFor(() => {
      expect(getByText('Definition unavailable')).toBeTruthy();
    });
  });
});
```

## Test Coverage and Quality

### Coverage Guidelines
- Aim for 80%+ code coverage on critical business logic
- Prioritize testing user-facing features and error paths
- Use `--coverage` flag to generate coverage reports
- Focus on meaningful tests over coverage percentage
- Add `coverage/**/*` to `.gitignore` to prevent tracking coverage reports

### Test Quality Checks
- Tests should be deterministic and not flaky
- Tests should be independent and not rely on other tests
- Use meaningful assertions that test actual behavior
- Avoid testing implementation details (private methods, internal state)
- Write tests that would catch real bugs, not just increase coverage

### CI/CD Integration
- Run tests on every pull request
- Fail builds on test failures
- Use different test commands for unit vs integration tests
- Consider running tests in parallel for faster feedback

```json
// Generated by Copilot
// package.json scripts for different test flows
{
  "scripts": {
    "test": "jest --watch --coverage=false --changedSince=origin/main",
    "test:debug": "jest -o --watch --coverage=false",
    "test:final": "jest",
    "test:ci": "jest --ci --coverage --watchAll=false",
    "test:update-snapshots": "jest -u --coverage=false"
  }
}
```

## Best Practices Summary

1. **Follow Expo's conventions** for file naming and directory structure
2. **Use React Native Testing Library** instead of deprecated react-test-renderer
3. **Write tests first** for critical features (TDD approach)
4. **Test behavior, not implementation** - focus on what users experience
5. **Use descriptive test names** that explain the scenario and expected outcome
6. **Mock external dependencies** to keep tests fast and reliable
7. **Test edge cases and error conditions** not just happy paths
8. **Keep tests simple and focused** - one assertion per test when possible
9. **Use proper cleanup** to prevent test interference
10. **Test accessibility features** to ensure inclusive design
11. **Mock native modules properly** for React Native specifics
12. **Test both theme variations** for themed components
13. **Use snapshot tests sparingly** - prefer E2E tests for UI testing
14. **Configure coverage properly** and add to .gitignore
15. **Use TypeScript strictly** with proper interface definitions for mocks