# Wedding Website - AI Coding Agent Instructions

## Project Overview
A single-page wedding website for Jason Decker & Ashton Belenchia (October 17, 2026 wedding). The site is a self-contained HTML file with embedded CSS and JavaScript, featuring an elegant dark theme with gold accents. No build tools, frameworks, or backend infrastructure—this is a static site deployed directly.

## Architecture & Key Patterns

### Design Philosophy
- **Single HTML file** (`index.html`) with all styling and scripts embedded
- **Dark theme** (#0f0f0f background) with gold accents (#d4af37)
- **Two serif fonts**: `Cinzel` (headings, formal elements), `Crimson Text` (body content)
- **No hard dependencies**: relies only on Google Maps API and Spotify embed iframes
- **Responsive mobile-first** approach with media queries at 768px and 480px breakpoints

### Navigation Architecture
- Fixed header with horizontal navigation
- Sections use `.hidden` class (display: none) for SPA-like behavior
- `showSection(id)` function toggles visibility, handles smooth scroll, and closes mobile menu
- Mobile hamburger menu at ≤768px with overlay backdrop and slide-in animation

### Section Structure (Single-Page App Pattern)
```
#home (hero banner) → #schedule → #travel → #registry → #rsvp → #gallery → #details → #faqs
```
Each section:
1. Uses `.section` class for consistent styling (min-height: 100vh, fadeInUp animation)
2. Contains a container (`.form-container`, `.info-container`, or `.hero`)
3. Is hidden by default except #home with `class="hidden"` selector

## Critical Implementation Details

### RSVP Form Workflow
- Form submits to Google Apps Script endpoint via `fetch()` POST
- **API URL**: `https://script.google.com/macros/s/AKfycbzXaFLpnIOmV3HDkahEgtuT7lXH0TpC8mGRu1tCNFDWiAx_CzdNtlyVMEUfbL0xh7-1bg/exec`
- Conditional display: dietary fields only shown if `isAttending === 'yes'`
- Success response triggers: message display, form reset, auto-hide after 5 seconds
- **Validation**: email regex check, required field check before submit
- Button state management: disabled during submission with loading text

### Gallery System
- **Images stored in**: `./IMAGES/` directory
- **Hardcoded array** in JavaScript: `galleryImages` array with URL and title
- Images loaded via `forEach()` to populate grid dynamically
- **Lightbox**: click image → `openLightbox(index)` sets `#lightboxImage.src` and adds `.active` class
- Click outside image closes lightbox (event delegation on parent)
- **Missing images handled gracefully**: browser's default broken image behavior (no error handling)

### Countdown Timer
- Hardcoded wedding date: `'October 17, 2026 16:00:00'`
- Updates every 1000ms with `setInterval()`
- Calculates: days, hours, minutes, seconds from current time
- DOM elements: `#days`, `#hours`, `#minutes`, `#seconds` span IDs

### External Integrations
- **Google Maps**: 
  - API key placeholder: `YOUR_API_KEY_HERE` (unused; no actual map displays without key)
  - Custom dark-themed map styling applied
  - Marker at Sussex County coordinates (41.1976, -74.4838)
  - `initMap()` called on page load and delayed 500ms in DOMContentLoaded
- **Spotify Embed**: embedded iframe (track ID: `7aZDcmt34eouhqw29aMR91`)
- **Registry Links**: hardcoded external links (Target, Amazon, Zola, etc.)

## Common Modifications

### Update Wedding Details
- Headline names: `<h1>Jason Decker</h1>` and `<h1>Ashton Belenchia</h1>`
- Date/location: `.date` span and `.location` div in hero
- API key for maps: search/replace `YOUR_API_KEY_HERE`

### Add/Remove Gallery Images
- Edit `galleryImages` array (JavaScript section, ~line 630)
- Add object: `{ url: './IMAGES/FileName.jpg', title: 'Description' }`
- Must match actual files in `./IMAGES/` folder (case-sensitive)

### Change Color Scheme
- Primary gold: `#d4af37` (text, borders, accents)
- Dark background: `#0f0f0f` (body)
- Secondary burgundy: `#8b2345` (rgba overlays)
- Change via CSS variables or search/replace (no CSS variables currently used)

### Modify RSVP Endpoints
- Search for Google Apps Script URL in `fetch()` call (line ~688)
- Update form fields by adding to FormData and data object
- Keep timestamp and field names synchronized with backend sheet

## Development Notes

### Known Issues/Quirks
- Google Maps API key is hardcoded as placeholder—must be replaced before map displays
- Gallery image URLs are hardcoded, not configurable from UI
- No error handling for missing gallery images (broken image state)
- Registry links are placeholder URLs (not actual registries)
- Hamburger menu doesn't auto-close on link click from mobile (must click overlay or a link)

### Performance Considerations
- All CSS/JS inline (no HTTP requests for assets aside from fonts, maps, Spotify)
- Lazy loading on gallery images (`loading="lazy"`)
- Backdrop filter blur on header may impact older devices
- setInterval for countdown runs continuously (consider cleanup on section hide)

### Testing Checklist
- Mobile responsiveness: 480px, 768px, desktop viewports
- Navigation: all section transitions work, hamburger toggles correctly
- Countdown: updates in real time, correctly calculates days/hours
- RSVP: form validates, submits correctly, dietary fields toggle
- Gallery: images load, lightbox opens/closes, overlay click works
- Maps: displays (requires valid API key)
- Spotify embed: loads and plays
