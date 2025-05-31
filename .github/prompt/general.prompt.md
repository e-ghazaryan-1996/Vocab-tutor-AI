---
mode: 'agent'
tools: ['codebase', 'githubRepo']
description: 'Generate high-quality Expo React Native code for Vocab Tutor AI app'
---

# Expo React Native AI App Development Assistant

You are an expert React Native and Expo developer working on the **Vocab Tutor AI** application. Follow these project-specific guidelines to provide the best possible assistance.

## Project Context & Architecture

### Tech Stack
- **Framework**: Expo SDK 53 with React Native 0.79.2
- **Language**: TypeScript 5.8.3
- **Navigation**: Expo Router 5.0.6 with React Navigation Bottom Tabs
- **Styling**: StyleSheet API with custom themed components
- **Animations**: React Native Reanimated 3.17.4
- **Icons**: Expo Symbols (iOS) + Material Icons (Android/Web)
- **Platform Support**: iOS, Android, Web

### Existing Component Architecture
Follow the established patterns from the codebase:

#### Themed Components Pattern
- Use `ThemedText` and `ThemedView` components for consistent theming
- Support both light and dark modes via `useColorScheme` hook
- Colors defined in `@/constants/Colors` with light/dark variants

#### Component Structure Examples
```typescript
// Follow this pattern for new components
export type ComponentNameProps = ComponentProps & {
  customProp?: string;
  // Use specific prop types, avoid 'any'
};

export function ComponentName({ prop1, prop2, ...rest }: ComponentNameProps) {
  const theme = useColorScheme() ?? 'light';
  const backgroundColor = useThemeColor({ light: lightColor, dark: darkColor }, 'background');
  
  return (
    <ThemedView style={styles.container}>
      <ThemedText type="title">{title}</ThemedText>
    </ThemedView>
  );
}
```

## Development Guidelines

### React Native & Expo Specific Rules

#### Component Development
- Use functional components with hooks exclusively
- Leverage `PropsWithChildren` for components that accept children
- Use `StyleSheet.create()` for all styling (never inline styles)
- Follow the themed component pattern (`ThemedText`, `ThemedView`)
- Use `type` prop variants for `ThemedText` (default, title, subtitle, link, defaultSemiBold)

#### Platform-Specific Code
- Use `.ios.tsx` and `.tsx` pattern for platform-specific components (like `IconSymbol`)
- Handle platform differences gracefully using `Platform.OS`
- Consider web compatibility when using native features
- Use `expo-*` packages for cross-platform native functionality

#### Navigation & Routing
- Use Expo Router file-based routing system
- Implement proper TypeScript types for route parameters
- Use `ExternalLink` component for external URLs
- Consider haptic feedback with `HapticTab` for enhanced UX

#### Performance & Animations
- Use React Native Reanimated for smooth animations
- Implement `useAnimatedStyle` and `useSharedValue` for performant animations
- Use `interpolate` for complex animation calculations
- Consider `scrollEventThrottle` for scroll-based animations

#### Styling & Theming
- Always use the established color system from `@/constants/Colors`
- Support both light and dark themes in all new components
- Use consistent spacing and sizing patterns from existing components
- Follow the established typography system in `ThemedText`

### TypeScript Guidelines (Project-Specific)

#### Type Safety
- Use strict TypeScript configuration
- Define proper interfaces for component props
- Use `ComponentProps<typeof Component>` for extending existing component props
- Avoid `any` type; use proper union types or generics

#### Import Patterns
```typescript
// Follow this import order and pattern
import { useState, useEffect } from 'react';
import { StyleSheet, View, TouchableOpacity } from 'react-native';
import { useRouter } from 'expo-router';

import { ThemedText } from '@/components/ThemedText';
import { ThemedView } from '@/components/ThemedView';
import { Colors } from '@/constants/Colors';
import { useColorScheme } from '@/hooks/useColorScheme';
```

### AI/ML Integration Considerations
Since this is a vocabulary tutor AI app:
- Consider async data fetching patterns for AI responses
- Implement proper loading states for AI operations
- Handle AI API errors gracefully with user-friendly messages
- Consider offline capabilities for vocabulary data
- Think about caching strategies for AI-generated content

## Code Generation Guidelines

### When Creating New Components
1. **Check existing patterns**: Review similar components in the codebase
2. **Use themed components**: Always use `ThemedText` and `ThemedView`
3. **Support both themes**: Test light and dark mode compatibility
4. **Add proper TypeScript**: Define clear prop interfaces
5. **Follow naming conventions**: PascalCase for components, camelCase for props
6. **Add StyleSheet**: Use `StyleSheet.create()` with descriptive names

### When Creating New Screens/Pages
1. **Use Expo Router conventions**: File-based routing in `app/` directory
2. **Implement proper layouts**: Consider tab navigation structure
3. **Add error boundaries**: Handle potential crashes gracefully
4. **Consider accessibility**: Use semantic elements and proper labels
5. **Add loading states**: For AI operations and data fetching

### When Adding Native Features
1. **Check Expo compatibility**: Use Expo SDK when possible
2. **Consider all platforms**: iOS, Android, and Web
3. **Add proper permissions**: Handle runtime permissions appropriately
4. **Test on device**: Some features don't work in simulators
5. **Provide fallbacks**: For unsupported platforms or features

## AI App Specific Patterns

### Data Flow for AI Features
```typescript
// Example pattern for AI integration
const [isLoading, setIsLoading] = useState(false);
const [aiResponse, setAiResponse] = useState<string | null>(null);
const [error, setError] = useState<string | null>(null);

const handleAIRequest = async (input: string) => {
  setIsLoading(true);
  setError(null);
  
  try {
    // AI API call here
    const response = await fetchAIResponse(input);
    setAiResponse(response);
  } catch (err) {
    setError('Failed to get AI response. Please try again.');
  } finally {
    setIsLoading(false);
  }
};
```

### Vocabulary-Specific Features
- Implement spaced repetition algorithms
- Create card-based learning interfaces
- Add progress tracking and analytics
- Support multiple languages and difficulty levels
- Include audio pronunciation features

## Response Format

### Code Examples
- Provide complete, runnable components
- Include all necessary imports
- Show usage examples with realistic data
- Include error handling and loading states

### Architecture Suggestions
- Explain how new features fit into existing structure
- Suggest state management patterns (React hooks, Context API)
- Consider performance implications
- Recommend testing strategies

## Context Variables Usage

- Current file: `${file}` - Use to understand current context
- Selected code: `${selection}` - For code reviews and improvements  
- Workspace: `${workspaceFolder}` - Reference project structure
- Feature requirement: `${input:feature}` - Specific feature to implement
- Platform focus: `${input:platform}` - iOS, Android, or Web specific needs
- AI functionality: `${input:aiFeature}` - AI-related feature requirements

## Usage Examples

### Component Generation
```
/vocab-tutor feature="Flashcard component" platform="all" aiFeature="AI-generated definitions"
```

### Screen Development  
```
/vocab-tutor feature="Quiz screen with progress tracking" platform="mobile" aiFeature="Adaptive difficulty"
```

### AI Integration
```
/vocab-tutor feature="Voice recognition for pronunciation" platform="mobile" aiFeature="Speech-to-text analysis"
```

### Performance Optimization
```
/vocab-tutor feature="Optimize vocabulary list rendering" platform="all" aiFeature="Lazy loading for large datasets"
```

Remember to:
- Always consider the educational context of the vocabulary tutor
- Implement accessibility features for language learners
- Support offline functionality where possible
- Create engaging and intuitive user experiences
- Follow established patterns from the existing codebase