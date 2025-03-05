```markdown
# Farcaster Media Gallery Frame Specification

## 1. OVERVIEW

### Core Functionality
- Displays carousel of 5 most recent Farcaster media shares (images/videos)
- Navigation controls for previous/next items
- Dynamic media loading with smooth transitions
- Context-aware UI with toggle functionality
- Wallet integration for user verification

### UX Flow
1. User opens frame → displays latest media item
2. Navigation buttons (←/→) update displayed content
3. Toggle button expands/collapses technical context
4. Auto-detect media type (image/video) with appropriate player
5. Mobile-responsive layout adapts to screen size
6. Wallet connection status persists across sessions

## 2. TECHNICAL REQUIREMENTS

### Frontend Components
```html
<div className="gallery-container">
  <div className="media-wrapper">
    <button className="nav-btn prev" onClick={handlePrev}>←</button>
    <div className="media-content">
      {isVideo ? (
        <video controls src={currentMedia.url} />
      ) : (
        <img src={currentMedia.url} alt="User content" />
      )}
    </div>
    <button className="nav-btn next" onClick={handleNext}>→</button>
  </div>
  <div className="media-meta">
    <span>{currentIndex + 1}/5</span>
    <ContextDisplay context={frameContext} />
  </div>
</div>
```

### API Integrations
1. **Neynar Cast Search API**
```typescript
async function fetchRecentMedia(fid: string) {
  const res = await fetch(
    `https://api.neynar.com/v2/farcaster/cast/search?author_fid=${fid}&limit=5`,
    { headers: { 'x-api-key': process.env.NEYNAR_API_KEY } }
  );
  const data = await res.json();
  return data.result.casts
    .flatMap(cast => cast.embeds)
    .filter(embed => embed.url.match(/\.(jpg|jpeg|png|gif|mp4|mov)$/i));
}
```

### Client-Side State
```typescript
const [mediaItems, setMediaItems] = useState<Embed[]>([]);
const [currentIndex, setCurrentIndex] = useState(0);
const [isVideo, setIsVideo] = useState(false);

useEffect(() => {
  const fetchMedia = async () => {
    const items = await fetchRecentMedia(userFid);
    setMediaItems(items.slice(0, 5));
    setIsVideo(items[0]?.url.match(/\.(mp4|mov)$/i));
  };
  fetchMedia();
}, [userFid]);
```

### Mobile Responsiveness
- CSS Grid layout with fractional units
- Media queries for aspect ratio adjustment
- Touch event handlers for swipe navigation:
```javascript
const handleTouchStart = (e) => {
  touchStart.current = e.touches[0].clientX;
};

const handleTouchEnd = (e) => {
  const delta = e.changedTouches[0].clientX - touchStart.current;
  if (Math.abs(delta) > 50) {
    delta > 0 ? handlePrev() : handleNext();
  }
};
```

## 3. FRAMES v2 IMPLEMENTATION

### Interactive Elements
- Canvas-based progress indicator
- Gesture-controlled navigation (swipe left/right)
- Keyboard shortcuts (arrow keys)
- Hover effects for desktop:
```css
.nav-btn:hover {
  transform: scale(1.2);
  filter: drop-shadow(0 0 8px rgba(255,255,255,0.5));
}
```

### Animations
```css
.media-content {
  transition: opacity 0.3s ease-in-out;
}

.media-enter {
  opacity: 0;
  transform: translateX(100%);
}
.media-exit {
  opacity: 0;
  transform: translateX(-100%);
}
```

### Input Handling
```typescript
useEffect(() => {
  const handleKeyPress = (e: KeyboardEvent) => {
    if (e.key === 'ArrowLeft') handlePrev();
    if (e.key === 'ArrowRight') handleNext();
  };

  window.addEventListener('keydown', handleKeyPress);
  return () => window.removeEventListener('keydown', handleKeyPress);
}, [currentIndex]);
```

## 4. MOBILE CONSIDERATIONS

### Touch Optimization
- 50px minimum touch target size
- 300ms click delay prevention:
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

### Responsive Techniques
- Viewport-relative units (vw/vh)
- Flexible grid layouts
- Conditional video controls:
```css
@media (pointer: coarse) {
  video::-webkit-media-controls {
    display: none;
  }
}
```

### Performance
- Lazy loading for off-screen media
- Video preload="metadata"
- WebP image format fallbacks
- Intersection Observer API for resource management

## 5. CONSTRAINTS COMPLIANCE

1. **No Database Requirements**  
   - All state managed client-side
   - Media cached in IndexedDB

2. **No Smart Contracts**  
   - Wallet integration only for authentication
   - No transaction capabilities required

3. **Approved Integrations Only**  
   - Neynar API (cast search)
   - Wagmi (wallet management)

4. **Complexity Control**  
   - Single-purpose gallery component
   - No social features beyond display
   - Minimal dependencies
```