# Zodiac Flow

## Overview

Zodiac Flow is a real-time mood tracking mobile application built with React Native and Expo. Users select their zodiac sign, gender, and country during onboarding, then log daily moods using emoji selections. The app provides real-time mood distribution analytics across zodiac signs, compatibility views, and country-based mood breakdowns. Users build streaks, earn badges, and can share mood stickers. The application uses a profile-based identification system (no traditional login) with local persistence and backend synchronization for aggregated mood data.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework**: React Native with Expo SDK 54
- Cross-platform mobile app (iOS, Android, Web)
- New React Native architecture enabled with React Compiler experiments
- Type-safe with TypeScript

**State Management**:
- **Zustand** for local user state (profile, onboarding status, current streak)
- Persisted to AsyncStorage for offline-first experience
- **TanStack Query (React Query)** for server state (mood data, distributions, badges)

**Navigation Structure**:
- **Stack Navigation** (Root): Handles onboarding flow and modal screens
- **Tab Navigation** (Main): Four tabs - Today, Compatibility, Countries, Profile
- Profile-based flow: Users see onboarding until profile creation, then main tabs
- Modal presentation for sticker sharing screen

**UI Component Architecture**:
- Themed components (ThemedView, ThemedText) with automatic light/dark mode
- Reusable components: Button, Card with elevation levels, ErrorBoundary
- Animated components using Reanimated for smooth interactions
- Pastel color palette with zodiac-specific theming

**Key Design Patterns**:
- Custom hooks for theme (`useTheme`), screen options (`useScreenOptions`)
- Path aliases (`@/` for client, `@shared/` for shared code)
- Platform-specific rendering (iOS blur effects, Android backgrounds)
- Gesture handlers and keyboard-aware scrolling

### Backend Architecture

**Server Framework**: Express.js with TypeScript
- RESTful API endpoints
- CORS configuration for Replit deployment domains
- Environment-based configuration (development/production)

**API Endpoints**:
- `POST /api/users` - Create user profile
- `GET /api/users/:id` - Retrieve user data
- `POST /api/moods` - Submit daily mood entry
- `GET /api/moods/distribution` - Get mood percentages by zodiac/gender
- `GET /api/moods/compatibility` - Compare user's mood with other signs
- `GET /api/moods/countries` - Get country-based mood breakdown
- `GET /api/badges` - List available badges
- `GET /api/badges/user/:userId` - Get user's earned badges

**Database Layer**:
- **Drizzle ORM** for type-safe database queries
- PostgreSQL dialect configured
- Connection pooling via `pg` library
- Schema-first approach with Zod validation

**Database Schema**:
- **users**: Profile data (zodiac, gender, country, streaks, last mood date)
- **mood_entries**: Daily mood logs (user_id, mood, time_of_day, date)
- **badges**: Achievement definitions (name, icon, requirement, type)
- **user_badges**: Junction table for earned badges (user_id, badge_id, earned_at)

**Business Logic**:
- Storage abstraction layer (`IStorage` interface) for database operations
- Streak calculation: Compares last mood date to determine consecutive days
- Badge awarding: Automatic check after mood submission based on streak milestones
- Real-time aggregation: Calculates mood percentages across user segments
- Compatibility algorithm: Matches user mood with predominant moods of other zodiac signs

### Data Flow

1. **User Onboarding**: Profile created locally → Synced to backend → User ID stored
2. **Mood Selection**: Local validation → API submission → Streak update → Badge check → Cache invalidation
3. **Real-time Updates**: React Query polling/refetching on screen focus
4. **Sticker Generation**: ViewShot captures rendered component → Expo Sharing API

### Authentication & Authorization

**Profile-based System** (No traditional auth):
- Unique user IDs generated server-side (`gen_random_uuid()`)
- Local storage of user profile for app persistence
- No passwords or login credentials
- User identity tied to device installation
- Profile migration not supported (trade-off for simplicity)

**Rationale**: Reduces friction for casual mood tracking app; users can start immediately without account creation barriers.

## External Dependencies

### Third-Party Libraries

**React Native Ecosystem**:
- `@react-navigation/native` - Navigation framework (v7)
- `@react-navigation/bottom-tabs` - Tab navigation
- `@react-navigation/native-stack` - Stack navigation
- `react-native-reanimated` - Animation library
- `react-native-gesture-handler` - Touch gesture system
- `react-native-safe-area-context` - Safe area handling
- `react-native-screens` - Native screen optimization

**Expo Modules**:
- `expo-blur` - iOS blur effects for navigation
- `expo-haptics` - Tactile feedback
- `expo-sharing` - Share functionality for stickers
- `expo-file-system` - File operations
- `expo-linking` - Deep linking support
- `expo-splash-screen` - Launch screen
- `react-native-view-shot` - Screenshot/sticker generation

**State & Data**:
- `@tanstack/react-query` - Server state management
- `zustand` - Client state management
- `@react-native-async-storage/async-storage` - Local persistence

**Backend**:
- `express` - HTTP server
- `drizzle-orm` - ORM with PostgreSQL support
- `pg` - PostgreSQL client
- `drizzle-zod` - Schema validation
- `tsx` - TypeScript execution

**UI & Icons**:
- `@expo/vector-icons` - Icon library (Feather icons)
- `react-native-svg` - SVG rendering

### Infrastructure

**Deployment Platform**: Replit
- Environment variables: `REPLIT_DEV_DOMAIN`, `REPLIT_INTERNAL_APP_DOMAIN`
- Dual-process architecture: Expo dev server + Express API server
- Proxy configuration for development routing

**Database**: PostgreSQL
- Connection via `DATABASE_URL` environment variable
- Drizzle migrations in `./migrations` directory
- Schema source: `./shared/schema.ts`

### Build System

**Development**:
- Expo Metro bundler with custom domain configuration
- Hot module replacement
- Concurrent expo + server processes

**Production**:
- Static bundle generation via `expo start --no-dev --minify`
- ESBuild for server-side bundling (ESM format)
- Express serves static assets + API endpoints

### Code Quality Tools

- ESLint with Expo config
- Prettier for formatting
- TypeScript strict mode
- Module path resolution (Babel plugin)