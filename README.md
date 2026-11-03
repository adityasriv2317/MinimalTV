# MinimalTV Home

A clean, modern Tizen web application home screen for Samsung TVs. Features a dock-based app launcher, wallpaper rotation, settings panel, and input switching.

## Features

- **App Dock** — Quick-launch bar for streaming apps (Netflix, Disney+, YouTube, Apple TV, Prime Video, Stremio, ABC iview, SBS On Demand, Apple Music, Samsung TV Plus)
- **HDMI Input Switcher** — Built-in shortcut to switch to HDMI 3 input
- **Wallpaper System** — Photo backgrounds with rotation, adaptive ink, and material effects
- **Settings Panel** — Background type, layout, typography, focus style, materials, and rotation
- **App Management** — Add/remove apps from Home, rearrange order
- **Input Selection** — Choose HDMI inputs via the Settings > Samsung > Inputs menu

## Project Structure

```
.
├── index.html          # Main HTML entry point
├── main.js             # Application logic (dock, settings, wallpapers, app launching)
├── device.js           # Tizen device API wrappers (app launch, input switching, wallpaper)
├── style.css           # Application styles
├── config.xml          # Tizen widget configuration
├── icon.png            # App icon
├── .gitignore          # Git ignore rules
├── LICENSE             # Project license
├── THIRD_PARTY_NOTICES.md  # Third-party attribution
├── setup.md            # Forking and setup guide
└── assets/
    ├── icons/          # App icons (SVG)
    ├── wallpapers/     # Wallpaper images and catalog
    ├── fonts/          # Font files
    ├── materials/      # Material effect images (blur, frost, tint)
    └── *.css           # Font and typeface stylesheets
```

## Getting Started

### Prerequisites

- A Samsung Tizen TV (2020 or later recommended)
- Tizen Studio installed for building and deploying
- Or a Tizen TV emulator for testing

### Building

The project is a standard Tizen Web App. Open the folder in Tizen Studio and build the `.wgt` package for deployment to your TV.

### Running Locally

Open `index.html` in a modern browser to preview the UI. Note that TV-specific features (app launching, input switching, wallpaper loading via Tizen APIs) require the Tizen runtime and will not work in a regular browser.

## Configuration

### Adding Custom Wallpapers

Wallpapers are defined in `assets/wallpapers/catalog.js`. Each entry supports:

```javascript
{
  id: 'mywall',           // Unique identifier
  name: 'My Wallpaper',   // Display name
  src: 'path/to/image.jpg', // Main wallpaper image (required)
  thumb: 'path/to/thumb.jpg', // Thumbnail for picker (optional, falls back to src)
  tone: 'light',          // 'light' or 'dark' — affects UI contrast
  ut: { tr: 'mist', br: 'mist' }, // Underlay tones for adaptive ink
  frostVeil: 0.2,         // Frost material opacity (0-1)
  listColumn: 0,          // Column position in list view
  dockQuiet: false,       // Reduce dock visual noise
  clockMist: false        // Clock mist effect
}
```

The wallpaper loader is modular: only the main `src` image is required. Panel images (`-panel.jpg`), thumbnails (`-thumb.jpg`), and material images are optional — the app will work without them.

### Adding Custom Apps

Apps are defined in `main.js` in the `PRIMARY_APPS` array:

```javascript
{ id: 'myapp', name: 'My App', mark: true, appId: 'com.example.myapp' }
```

- `id` — Internal identifier used for dock and storage
- `name` — Display name on the dock
- `mark` — Whether to use a brand icon SVG
- `appId` — Tizen application ID for launching

For HDMI input switching, create an entry with `appId: 'hdmiX'` (where X is the port number) — the launch handler will switch to that HDMI input instead of launching an app.

### Icons

App icons are SVG files in `assets/icons/`. Name them `{id}.svg` to match the app `id` in `PRIMARY_APPS`. Use filled white paths on a transparent background for best results with the mask-based rendering.

## License

See [LICENSE](LICENSE) for details.
