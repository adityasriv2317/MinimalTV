# Setup Guide — Forking MinimalTV Home

This guide covers everything you need to know when forking this project from GitHub and setting it up for your own use.

## 1. Fork the Repository

1. Click the **Fork** button on the GitHub repository page
2. Clone your fork locally:

```bash
git clone https://github.com/YOUR_USERNAME/minimaltv-home.git
cd minimaltv-home
```

## 2. Install Tizen Studio

This project is a Tizen Web App. You need Tizen Studio to build, run, and deploy it to a Samsung TV.

1. Download [Tizen Studio](https://developer.tizen.org/development/tizen-studio/download)
2. Install the Tizen TV SDK and Certificate Manager
3. Create a Tizen certificate profile for signing your `.wgt` package

## 3. Open the Project in Tizen Studio

1. Launch Tizen Studio
2. **File → Import → Tizen Project**
3. Select the cloned project folder
4. The project should appear in the Project Explorer

## 4. Configure for Your Environment

### Update `config.xml`

Edit `config.xml` with your app details:

```xml
<widget xmlns="http://www.w3.org/ns/widgets" xmlns:tizen="http://tizen.org/ns/widgets" id="YOUR_APP_ID" version="1.0.0">
  <tizen:application id="YOUR_PACKAGE_ID.YOUR_APP_ID" version="1"/>
  <content src="index.html"/>
  <access origin="*" subdomains="true"/>
</widget>
```

### Update Wallpapers

Add your own wallpapers in `assets/wallpapers/` and register them in `assets/wallpapers/catalog.js`:

```javascript
window.MinimalTVWallpapers = [
  // ... existing entries ...
  {
    id: 'myphoto',
    name: 'My Photo',
    src: 'assets/wallpapers/myphoto.jpg',   // Required: main image
    thumb: 'assets/wallpapers/myphoto-thumb.jpg', // Optional: thumbnail
    tone: 'light',                          // 'light' or 'dark'
    ut: { tr: 'mist', br: 'mist' },
    frostVeil: 0.2
  }
];
```

**Only `src` is required.** Thumbnails and panel images are optional — the app gracefully handles missing files.

### Update Apps

Edit `PRIMARY_APPS` in `main.js` to add or remove apps:

```javascript
var PRIMARY_APPS = [
  { id: 'netflix', name: 'Netflix', mark: true, appId: 'org.tizen.netflix-app' },
  // ... your apps ...
  { id: 'hdmi3', name: 'HDMI 3', mark: true, appId: 'hdmi3' }  // HDMI input switcher
];
```

For HDMI input switching, use `appId: 'hdmiX'` where X is the port number.

### Add Custom Icons

Place SVG icons in `assets/icons/` named `{app-id}.svg`. Use filled white paths on transparent background.

## 5. Build and Deploy

### Build the Package

In Tizen Studio:
1. Right-click the project → **Build Project**
2. The `.wgt` package is generated in the build output

### Deploy to TV

1. Enable **Developer Mode** on your Samsung TV
2. Note the TV's IP address
3. In Tizen Studio, use the **Remote Device Manager** to connect
4. Right-click project → **Run As → Tizen Web Application**

## 6. Customizing Appearance

The app supports several look and feel options accessible through Settings:

| Setting | Options |
|---------|---------|
| Background | Photo, Dark, Light |
| Layout | List, Dock |
| App Display | Icons, Text, Both |
| Focus Style | Type, Glass |
| Material | Soft, Frost, Tint |
| Rotation | Fixed, Every 5 min, 15 min, Hour |

## 7. Troubleshooting

**Wallpaper not loading:** Make sure `src` points to a valid image. Other images (thumb, panel, materials) are optional.

**App not launching:** Verify the `appId` in `PRIMARY_APPS` matches the actual Tizen app ID installed on your TV.

**HDMI switching not working:** Ensure HDMI 3 is connected and detected by the TV. The app uses `tizen.tvwindow.setSource()` which requires TV privileges.

**Build errors:** Check that `config.xml` has a valid app ID and version. Clean and rebuild the project.

## 8. File Reference

| File | Purpose |
|------|---------|
| `index.html` | HTML structure and UI elements |
| `main.js` | Core application logic, dock rendering, app management |
| `device.js` | Tizen API wrappers (launch apps, switch inputs, list sources) |
| `style.css` | All styles and theme variables |
| `config.xml` | Tizen widget manifest |
| `assets/wallpapers/catalog.js` | Wallpaper definitions and metadata |
| `assets/icons/*.svg` | App icon SVGs |
| `assets/materials/*` | Material effect images (blur, frost, tint) |

## 9. License

See [LICENSE](LICENSE) and [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for attribution details.
