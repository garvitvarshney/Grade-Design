# Grade-Design

**Cinematic photo grading, text overlays, and Pinterest-style collage maker with EXIF location grouping.**

A professional, feature-rich progressive web app (PWA) for creative photo editing and design directly in your browser.

## Features

### 🎬 **Cinematic Grading**
- **13 professional color grades**: No Grade, Teal & Orange, Kashmir, Golden Hour, Kodak 500T, Nordic Cold, Vintage Print, Silver Bypass, Morning Mist, Blue Dusk, Ember, Fuji Velvia, Black & White
- **9 aspect ratios**: Original, 2.39:1, 2.35:1, 1.85:1, 16:9, 4:3, 1:1, 4:5, 9:16
- **Image reframing**: Pan and reposition photos within aspect ratios
- **Lightbox preview**: Full-screen view of each graded look
- **Instant compare**: Hold-to-compare original vs. grade in real-time
- **Grain & vignette**: Professional film-like texture applied to all grades

### ✍️ **Text Overlays**
- Add custom text to any graded photo
- **Font options**: Inter, DM Serif, Georgia, Courier, Impact
- **Full control**: Size, weight, color (7 curated swatches), opacity
- **9-point positioning**: Corners, edges, center placement
- **Persistent layers**: Text baked into saved images

### 📱 **Pinterest-Style Collages**
- **12 creative layouts**:
  - Polaroid Scatter (rotated cards on cream background)
  - Film Strip (35mm frames with sprocket holes)
  - Magazine Editorial (hero + overlapping cards)
  - Moodboard (chaotic overlapping with rotation)
  - Photo Dump (tight grid)
  - Triptych (3 portrait strips)
  - Cinescope (2.39:1 hero + detail strip)
  - Golden Ratio (Fibonacci asymmetric)
  - Polaroid Wall (rotated grid on linen)
  - Arch Diptych (elegant arch frames)
  - Broken Grid (intentionally off-grid editorial)
  - IG Carousel (panoramic + bottom strip)
- **13 cinematic grades** applied to collages
- **Randomise button**: Auto-shuffle photos, style, and grade for instant variations
- **1080×1350px output** (Instagram 4:5 portrait)

### 📍 **EXIF Location Grouping**
- **Reads GPS from photo metadata**: No API key needed, uses browser-native EXIF parsing
- **Reverse geocodes**: Automatically finds city/region name for each photo
- **Clusters by proximity**: Groups photos taken within 30km of each other
- **One-tap selection**: Tap a location to float those photos to the top for collage work
- **Perfect for travelers**: Auto-organizes photos by where you were

### ⚙️ **Progressive Web App (PWA)**
- **Install on home screen**: Works like native app (no browser chrome)
- **Offline support**: Cached assets work without internet
- **Live updates**: Redeploy and changes appear automatically on next open
- **Fast**: Service worker + optimized rendering

## Installation & Setup

### For Users (iPhone, Android, Mac)

1. **Open in Safari** (or your mobile browser)
2. Tap **Share** → **Add to Home Screen**
3. Name it `Grade-Design` → **Add**
4. It's now a full app on your home screen!

### For Developers

**Option 1: GitHub Pages** (free hosting)
```
1. Clone or download this repo
2. Push to a GitHub repository named 'gradeandcollage' (or any name)
3. Go to Settings → Pages → select 'main' branch
4. Get your live URL: https://yourusername.github.io/gradeandcollage
5. Open in Safari → Add to Home Screen
```

**Option 2: Netlify Drop** (no Git needed)
```
1. Go to app.netlify.com/drop
2. Drag all 5 files in
3. Get instant live URL
4. Open in Safari → Add to Home Screen
```

**Option 3: Cloudflare Pages** (what Garvit uses)
```
1. Upload to Cloudflare Pages dashboard
2. Deploy automatically
3. Open URL in Safari → Add to Home Screen
```

## File Structure

```
gradeandcollage/
├── index.html           # Main app (1,234 lines)
├── manifest.json        # PWA metadata
├── sw.js               # Service worker (offline support)
├── icon-192.png        # App icon (192×192)
├── icon-512.png        # App icon (512×512)
└── README.md           # This file
```

## Technical Details

### Architecture
- **Vanilla JS** (no dependencies)
- **Canvas API** for image processing
- **Service Worker** for offline caching
- **EXIF parser** (custom, no library)
- **Nominatim API** (free, no key needed)

### Grade Pipeline
Each pixel processes through:
1. S-curve contrast boost (1.06)
2. Grade function (style-specific color transform)
3. Clamp to [0, 1]
4. Convert to 8-bit (0-255)
5. Apply vignette (radial gradient)
6. Add film grain (overlay blend)

### Collage Rendering
- **1080×1350px canvas** (Instagram portrait)
- **Cover-fit layout**: Photos bias to top-third for faces
- **Grades applied per-cell**: Each photo gets the selected grade
- **Grain overlay**: Unified film texture across all cells
- **Gutters**: 6px spacing between cells
- **Background**: #0A0A0A (dark)

### EXIF GPS Parsing
- Reads JPEG byte-by-byte
- Finds APP1 marker (0xFFE1)
- Extracts TIFF directory
- Parses GPS IFD (Image File Directory)
- Converts DMS (degrees/minutes/seconds) to decimal
- Works on all modern iOS/Android photos

## Keyboard Shortcuts (Desktop)
- **Escape** → Close lightbox
- **Arrow keys** → Navigate graded slides (in carousel)

## Mobile Tips
- **Swipe left/right** in Grade tab to browse looks
- **Pinch to zoom** photo in lightbox
- **Long-press text button** to see text controls
- **iOS**: Use Files app to access photos

## Customization

### Adding a New Grade
Edit `STYLES[]` array in `index.html`:
```javascript
{
  id: 'my-grade',
  name: 'My Grade',
  desc: 'Description of the look',
  fn(r, g, b, L) {
    // L = luminance
    // Modify r, g, b in [0, 1] range
    return [r, g, b];
  }
}
```

### Adding a Collage Layout
Edit `COL_STYLES[]` array:
```javascript
{
  id: 'my-layout',
  name: 'My Layout',
  icon: 'my-icon',
  minPh: 2,  // minimum photos
  render(ctx, photos, grade) {
    // ctx = canvas context
    // photos = array of canvas objects
    // grade = selected grade
    ctx.fillRect(0, 0, 1080, 1350);
    // ... draw your layout
  }
}
```

## Color Palette
- **Dark Charcoal**: #0A0A0A
- **Surface**: #141414
- **Border**: #222222
- **Text**: #F2EFE9 (warm cream)
- **Secondary Text**: #8A8680 (muted brown)
- **Accent Gold**: #D4A654
- **Accent Teal**: #7BAFC4

## Browser Support
- **iOS Safari** (14+) ✅ — Full support, installable
- **Chrome Android** (90+) ✅
- **Firefox** (88+) ✅
- **Edge** (90+) ✅
- **Desktop Safari** ✅

## Performance
- **App size**: ~104 KB total (including icons)
- **Load time**: < 2 seconds (on 4G)
- **Rendering**: Real-time on iPhone 12+
- **No external scripts** (except fonts from Google)

## Known Limitations
- **EXIF GPS**: iOS Safari cannot access photo library GPS directly; users must use file picker
- **Video**: Grabs first 10% of video; doesn't preserve audio in exports
- **Canvas taint**: Uses data URLs (not blob URLs) to avoid cross-origin issues on iOS
- **Offline collages**: Collage generation requires all photos in memory

## Future Ideas
- Animated overlays (fade/slide text effects)
- Batch grade application
- Instagram direct upload
- Custom preset saving
- Watermark templates
- Before/after slider
- Color curves editor
- Social media metadata (hashtags, captions)
- Export to Figma design
- MCP server for automation

## License
MIT — free to use, modify, and distribute

## Credits
- **Grades** inspired by CineMatch, DaVinci Resolve
- **Collage layouts** researched on Pinterest
- **EXIF parser** custom implementation
- **Icons** generated with Pinterest aesthetic inspiration
- **Typography** DM Serif Display + Inter (Google Fonts)

---

**Made with ❤️ for creative professionals and travelers**

Questions? Open an issue or fork and improve!
