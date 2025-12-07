# ZODIAC FLOW - Design Guidelines

## Architecture Decisions

### Authentication
**Auth Required:**
- Users create profiles with zodiac sign, gender, and country selection
- Data syncs to backend for real-time mood aggregation
- Local storage for profile persistence
- No traditional login required - profile-based identification using unique user_id

### Navigation
**Root Navigation:** Tab Navigation (4 tabs)
- **Today** - Core mood selection and daily distribution
- **Compatibility** - Compare moods across zodiac signs
- **Countries** - Global mood breakdown by country
- **Profile** - Streaks, badges, timeline, and settings

Additional Screens:
- Onboarding (Stack-only, shown on first launch)
- Sticker Preview (Modal)

## Screen Specifications

### 1. Onboarding Flow (Stack Navigation)
**Purpose:** Initial profile setup
- Step 1: Zodiac sign selection (12 pastel icons in 3x4 grid)
- Step 2: Gender selection (simplified cards)
- Step 3: Country selection (searchable dropdown or list)
- Submit button at bottom, soft pastel accent color
- Progress indicator at top
- No header, fullscreen experience

### 2. Today Screen (Tab: Today)
**Layout:**
- Transparent header with app name "Zodiac Flow" centered
- Top inset: headerHeight + Spacing.xl
- Bottom inset: tabBarHeight + Spacing.xl
- Scrollable root view

**Components:**
- Welcome message with zodiac sign icon
- Emoji mood selection grid (6-8 moods: Happy 😊, Sad 😢, Angry 😠, Thoughtful 🤔, Excited 🤩, Calm 😌, Anxious 😰, Loved 🥰)
- After selection: Real-time distribution card showing percentages with animated progress bars
- Floating "Share Sticker" button (bottom right, soft drop shadow)
- Streak indicator badge (top right of header)

### 3. Compatibility Screen (Tab: Compatibility)
**Layout:**
- Header: "Mood Compatibility" title, refresh icon (right)
- Scrollable list of cards
- Auto-refresh every 30 seconds

**Components:**
- 12 zodiac sign cards, each showing:
  - Zodiac icon (pastel)
  - Sign name
  - Percentage of same mood
  - Small emoji indicator
- Rounded cards with subtle pastel backgrounds alternating between palette colors

### 4. Countries Screen (Tab: Countries)
**Layout:**
- Header: "Global Moods" title, filter icon (right)
- Searchable/filterable list
- Scrollable root view

**Components:**
- Country cards showing:
  - Country name
  - Dominant mood emoji (large)
  - Percentage
  - "for [Zodiac] [Gender]" subtitle
- Pastel background per card, rounded corners

### 5. Profile Screen (Tab: Profile)
**Layout:**
- Custom header with large zodiac icon and user's sign
- Scrollable content
- Bottom inset: tabBarHeight + Spacing.xl

**Components:**
- Streak counter (large, prominent, with fire emoji 🔥)
- Badges grid (earned badges highlighted, locked badges grayed)
- 30-day mood timeline (line/bar chart with pastel colors)
- Daily breakdown: Morning/Noon/Evening/Night mood pills
- Theme selector (different pastel variations)
- Settings button

### 6. Sticker Screen (Modal)
**Layout:**
- Modal presentation
- Close button (top left)
- Preview of generated sticker (centered)
- Download & Share buttons at bottom

**Sticker Components:**
- Pastel gradient background
- Zodiac icon (top)
- Gender icon (small, corner)
- Large mood emoji (center)
- Text: "X% of [Sign] [Gender] chose [Mood] today"
- Date stamp (bottom)
- Rounded corners, soft aesthetic

## Design System

### Color Palette (Pastel Theme)
**Primary Colors:**
- Pink: #FCE4EC
- Light Blue: #E3F2FD
- Mint: #E8F5E9
- Cream: #FFF8E1
- Lavender: #F3E5F5

**Usage:**
- Use these colors for card backgrounds, buttons, and accents
- Rotate colors across lists to create visual rhythm
- White (#FFFFFF) for main backgrounds
- Soft grays (#F5F5F5, #EEEEEE) for subtle dividers
- Text: Dark gray (#333333) for primary, medium gray (#666666) for secondary

### Typography
- **Header:** SF Pro Display / Roboto, 28-32pt, Medium weight
- **Subheader:** 20-24pt, Regular
- **Body:** 16-18pt, Regular
- **Caption:** 14pt, Regular
- **Numbers/Stats:** 24-28pt, Bold

### Component Design
**Cards:**
- Border radius: 16-20px
- Padding: 20-24px
- Subtle shadow: shadowOffset (0, 2), shadowOpacity 0.08, shadowRadius 4
- Background: Pastel colors from palette

**Buttons:**
- Border radius: 12-16px
- Padding: 14-18px vertical, 24-32px horizontal
- Filled style with pastel background
- Text: 16pt, Medium weight

**Floating Action Button (Share Sticker):**
- Circular or rounded square
- Position: Bottom right, 20px from edges
- Drop shadow: shadowOffset (0, 2), shadowOpacity 0.10, shadowRadius 2
- Pastel accent color

**Icons:**
- Zodiac icons: 48-64px for selection grids, 32-40px in lists
- Gender icons: 24-32px
- System icons: 24px (Feather icons from @expo/vector-icons)

**Emoji Mood Selection:**
- 56-64px emoji size
- Grid layout with even spacing
- Subtle pastel border when selected
- Scale animation on press

**Progress Bars/Stats:**
- Rounded pill shape
- Pastel fill colors matching mood categories
- Animated transitions
- Height: 8-12px for small, 20-24px for large

### Visual Feedback
- All touchables: Scale down to 0.95 on press
- Mood selection: Scale up to 1.1, then scale down with pastel background
- Buttons: Slight opacity change (0.8) on press
- Tab bar: Active tab has pastel underline/icon tint

## Assets Required

### Critical Assets:
1. **12 Pastel Zodiac Icons** (Aries, Taurus, Gemini, Cancer, Leo, Virgo, Libra, Scorpio, Sagittarius, Capricorn, Aquarius, Pisces)
   - Style: Simple line art with pastel fills
   - Format: SVG or PNG with transparency
   - Size: 512x512px minimum

2. **Gender Icons** (2-3 variations)
   - Minimal, inclusive designs
   - Pastel colors
   - 256x256px

3. **Badge Icons** (at least 4)
   - Streak milestones: 3-day, 7-day, 30-day, 100-day
   - Star/trophy style with pastel colors
   - 256x256px

### System Icons (use Feather from @expo/vector-icons):
- Navigation: home, bar-chart-2, globe, user
- Actions: share-2, download, settings, refresh-cw
- UI: search, filter, x, check

## Accessibility
- Minimum touch target: 44x44px
- Color contrast ratio: 4.5:1 for text
- Emoji-based moods should have text labels for screen readers
- All interactive elements must have accessible labels
- Support dynamic text sizing

## Animations
- Mood selection: Spring animation (tension: 300, friction: 20)
- Distribution bars: Animated fill over 800ms
- Screen transitions: Gentle fade + slide
- Auto-refresh: Subtle pulse indicator before data updates
- Streak counter: Celebration animation on new milestone