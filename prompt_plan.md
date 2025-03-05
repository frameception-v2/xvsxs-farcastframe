Here's the optimized step-by-step implementation plan:

```markdown
# Phase 1: Core Infrastructure Setup

## Step 1.1: Initialize Next.js Application
```text
Create a new Next.js 14 app with TypeScript template.
Install required dependencies:
- @neynar/nodejs-sdk
- wagmi v2 + viem
- framer-motion
- react-icons
Configure base layout with responsive viewport meta tags.
Set up environment variables for Neynar API key.
Create initial page structure with empty GalleryContainer component.
```

## Step 1.2: API Route Setup
```text
Create /api/media route handler:
- Implement server-side Neynar API call
- Add error handling for API limits
- Set cache headers for 5-minute TTL
- Validate FID parameter
- Return filtered media embeds in standardized format
Test with Postman/curl for valid response structure.
```

# Phase 2: Core Gallery Functionality

## Step 2.1: Media Fetcher Hook
```text
Create useMediaLoader hook:
- Uses SWR for data fetching
- Handles Neynar API pagination
- TypeScript interfaces for MediaEmbed
- Error boundary integration
- Loading state management
Connect to existing API route.
Add basic console logging for validation.
```

## Step 2.2: Media Display Component
```text
Create MediaContent component:
- Props: MediaEmbed object
- Auto-detect media type from URL
- Next/Image for images
- Video element with preload=metadata
- Fallback UI for unsupported types
- Aspect ratio preservation
Style with CSS Grid layout.
Add basic keyboard event listeners.
```

# Phase 3: Navigation System

## Step 3.1: Carousel Controls
```text
Implement GalleryNavigation:
- Prev/Next buttons with touch targets
- Disabled states for boundaries
- Keyboard arrow support
- SWR mutate on navigation
- Index display (1/5)
Use framer-motion for hover effects.
Integrate with useMediaLoader context.
```

## Step 3.2: State Management
```text
Create galleryStore with Zustand:
- Current index tracking
- Media items cache
- Navigation actions
- Derived isVideo state
- Session persistence
Connect to MediaContent and Navigation.
Add localStorage sync middleware.
```

# Phase 4: Mobile Optimization

## Step 4.1: Touch Gestures
```text
Add useTouchNavigation hook:
- Track touch start/end coordinates
- Swipe threshold detection
- Prevent scroll interference
- Haptic feedback
- Velocity-based animation
Integrate with GalleryNavigation.
Add touch event listeners to media container.
```

## Step 4.2: Responsive Layout
```text
Implement mobile-first CSS:
- Viewport units scaling
- Dynamic aspect ratios
- Conditional video controls
- CSS transitions
- Safe area insets
Test on Chrome DevTools device emulator.
Add media queries for desktop hover states.
```

# Phase 5: Enhanced Features

## Step 5.1: Wallet Integration
```text
Add WalletBadge component:
- Wagmi connector setup
- Farcaster auth validation
- SIWE authentication
- Connection status in header
- Session persistence
Integrate FID into media fetching.
Add error handling for disconnected state.
```

## Step 5.2: Performance Optimizations
```text
Implement lazy loading:
- Intersection Observer API
- Blur placeholders
- Video poster images
- IndexedDB caching
- Prefetch next media
Add loading states with skeletons.
Configure Next.js image optimization.
```

# Phase 6: Final Integration

## Step 6.1: Animation System
```text
Add enter/exit transitions:
- Framer Motion layoutId
- Opacity crossfades
- Directional sliding
- Spring physics
- GPU-accelerated transforms
Coordinate with navigation actions.
Add reduced motion preference check.
```

## Step 6.2: Comprehensive Testing
```text
Create test suite:
- Jest DOM interactions
- Cypress E2E flows
- Lighthouse audit
- Device lab testing
- Error scenario validation
Set up CI/CD pipeline.
Add monitoring for API errors.
```

Each step builds on previous implementations while maintaining isolated testability. The final integration phase ensures all components work together with proper type safety and performance characteristics.
```