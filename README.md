# easyMarketing Tracking Monitor

> Professional Chrome Extension for monitoring EATMS tracking scripts, Cless/Counter implementations, and dataLayer analysis

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-3.0.0-green.svg)](manifest.json)
[![Chrome](https://img.shields.io/badge/chrome-extension-orange.svg)](https://chrome.google.com/webstore)

A powerful Chrome extension developed by [easyMarketing](https://easy-m.de) for tracking and analyzing EATMS marketing scripts and Cless/Counter implementations.

![easyMarketing Logo](icon128.png)

## 🚀 Features

### Script Monitoring
- ✅ **EATMS Tracking**: Automatically detects all requests to `/trck/etms/eatms.js` regardless of domain
- ✅ **Cless/Counter Detection**: Recognizes scripts from `data.min-cdn.net`:
  - **Pre-Consent**: Scripts with ID 9xxxxx (6 digits)
  - **Post-Consent**: Scripts with ID 8xxxxx (6 digits, loaded via EATMS)
- ✅ **Secondary Scripts**: Tracks ALL JavaScript files loaded after EATMS (within 10 seconds)
- ✅ **Badge Counter**: Shows number of detected requests on extension icon
- ✅ **Detailed Parameters**: Lists all GET parameters clearly
- ✅ **Visual Indicators**: Distinguishes between EATMS, Cless, Counter, and loaded scripts
- ✅ **"via EATMS" Badge**: Marks all scripts loaded after EATMS

## 📸 Screenshots

Monitor all EATMS scripts, Cless/Counter implementations, and secondary scripts in real-time.

## 🔧 Installation

### From Source

1. **Download the extension**
   ```bash
   git clone https://github.com/yourusername/tracking-monitor.git
   cd tracking-monitor
   ```

2. **Load in Chrome**
   - Open Chrome and navigate to `chrome://extensions/`
   - Enable "Developer mode" (toggle in top right)
   - Click "Load unpacked"
   - Select the extension directory

3. **Start monitoring!**
   - The extension is now active and will begin monitoring all tabs
   - Click the extension icon to view tracked requests

### From Chrome Web Store
*(Coming soon)*

## 📖 Usage

### Basic Usage

1. **Visit a website** that uses EATMS tracking
2. **Observe the badge**: A badge with a number appears on the extension icon showing the count of detected requests
3. **Click the extension icon**: A popup opens showing all tracked scripts and requests

### Tracking Display

Displays all detected scripts and requests:

#### 1. EATMS (Green Border)
```
https://example.com/trck/etms/eatms.js?campaign_id=12345&referrer=google
```
**Display:**
- 🌐 Domain
- Badge: EATMS
- All GET parameters (campaign_id, referrer, etc.)

#### 2. Cless/Counter - Pre-Consent (Orange Border, Yellow Badge)
```
https://data.min-cdn.net/cless/912345.js
https://data.min-cdn.net/counter/998765.js
```
**Display:**
- 🎯 data.min-cdn.net
- Badge: CLESS or COUNTER
- Status: "vor Consent" (yellow)
- Script-ID: 9xxxxx

#### 3. Cless/Counter - Post-Consent (Orange Border, Green Badge)
```
https://data.min-cdn.net/cless/812345.js
https://data.min-cdn.net/counter/898765.js
```
**Display:**
- 🎯 data.min-cdn.net
- Badge: CLESS or COUNTER
- Status: "nach Consent" (green)
- "via EATMS" Badge (blue)
- Script-ID: 8xxxxx

#### 4. Secondary Scripts (Purple Border, "via EATMS" Badge)
```
https://analytics.example.com/tracker.js
https://cdn.partner.net/pixel.js
```
**Display:**
- 📦 Domain
- Badge: NACHGELADEN (purple)
- "via EATMS" Badge (blue)
- Filename and complete URL
- All JS files loaded within 10 seconds after EATMS

## 🎯 Cless/Counter ID Structure

- **6-digit IDs**: Format [89]xxxxx
- **9xxxxx**: Pre-Consent (loads directly)
- **8xxxxx**: Post-Consent (loaded via EATMS)

## 📊 Example Workflow

Typical flow on a website:

1. **EATMS loads**: `https://example.com/trck/etms/eatms.js?campaign_id=12345`
2. **Pre-Consent**: `https://data.min-cdn.net/cless/912345.js` (direct load)
3. **Post-Consent via EATMS**: 
   - `https://data.min-cdn.net/counter/812345.js` (Cless/Counter)
   - `https://analytics.service.com/tracking.js` (additional script)
   - `https://cdn.partner.net/pixel.js` (another script)

The extension displays all requests with corresponding badges and details:
- ✅ Green border for EATMS
- ✅ Orange border for Cless/Counter with consent status
- ✅ Purple border for all other secondary scripts
- ✅ "via EATMS" badge (blue) for all scripts loaded after EATMS

## 🛠 Technical Details

### Technology Stack
- **Manifest Version**: 3
- **Permissions**: 
  - `webRequest` - Monitor network requests
  - `storage` - Store tracked data
- **Host Permissions**: `<all_urls>` (to monitor all domains)
- **Service Worker**: background.js listens to all web requests

### Design
- **Primary Color**: #e63946 (easyMarketing Red)
- **Accent Color**: #d62828 (Dark Red)
- Modern & clean interface
- Responsive badges and statistics
- Hover effects for better UX

### Architecture

```
tracking-monitor-extension/
├── manifest.json          # Extension configuration
├── background.js          # Service worker for request monitoring
├── popup.html            # UI for popup window with easyMarketing design
├── popup.js              # Logic for data display
├── icon16.png            # Extension icon 16x16
├── icon48.png            # Extension icon 48x48
├── icon128.png           # Extension icon 128x128
└── README.md             # This file
```

### How It Works

1. **Request Monitoring**: The service worker listens to all script requests using `chrome.webRequest.onBeforeRequest`
2. **Pattern Matching**: Identifies EATMS by URL pattern `/trck/etms/eatms.js` and Cless/Counter by regex
3. **Timing Analysis**: Tracks when EATMS loads and identifies subsequent scripts (within 10-second window)
4. **Data Storage**: Stores all tracked requests per tab in memory
5. **Badge Updates**: Updates the extension badge with the count of tracked requests

## 🔒 Privacy

The extension stores tracking data only locally in the browser and only for the duration of the tab session. When a tab is closed or reloaded, the data is deleted.

**Data Handling:**
- ✅ No data is sent to external servers
- ✅ All processing happens locally
- ✅ Data is cleared on tab close/reload
- ✅ No personal information is collected

## 🐛 Troubleshooting

### Badge not showing?
- Check the Developer Console (F12) to see if requests to `/trck/etms/eatms.js` are being made
- Ensure the extension is activated

### Popup shows nothing?
- The extension only monitors the current tab
- Old data is cleared on each page reload

### Debug Logging

The extension includes comprehensive logging:

**Service Worker Console** (`chrome://extensions/` → "Service Worker"):
```
Web Request detected: https://...
✓ EATMS Request erkannt: https://...
EATMS params: {campaign_id: "...", ...}
Stored request. Total for tab X: 1
```

**Popup Console** (Right-click popup → "Inspect"):
```
loadRequests called
getRequests response: {requests: Array(1)}
Requests count: 1
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 🤝 Support & Contact

For questions or issues, please contact:
- **Website**: [https://easy-m.de](https://easy-m.de)
- **Issues**: [GitHub Issues](https://github.com/yourusername/tracking-monitor/issues)

## 📄 License

Developed by and for easyMarketing. Free to use for easyMarketing customers and partners.

---

**© 2024 easyMarketing** - Professional Digital Marketing

Powered by [easyMarketing](https://easy-m.de)
