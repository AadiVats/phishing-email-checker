# 🛡️ PhishGuard AI - Android APK Generation Guide

## 📱 Methods to Create APK

### Method 1: PWABuilder (Recommended - Easiest)
**Best for: Quick deployment without coding**

1. **Host Your PWA Online**
   - Upload all files (index.html, style.css, script.js, app.js, manifest.json, sw.js) to a web server
   - GitHub Pages (free): https://pages.github.com
   - Netlify (free): https://www.netlify.com
   - Vercel (free): https://vercel.com

2. **Generate APK via PWABuilder**
   - Go to: https://www.pwabuilder.com
   - Enter your hosted PWA URL
   - Click "Start" and wait for analysis
   - Click "Package for Stores" > Select "Android"
   - Download the APK/AAB bundle (signed & ready for Play Store)

**Advantages:**
- No Android Studio required
- Automatic signing
- Generates both APK (testing) and AAB (Play Store)
- Trusted Web Activity (TWA) implementation
- ~5 minutes total time

---

### Method 2: Bubblewrap CLI (For Developers)
**Best for: Command-line users and automation**

#### Prerequisites:
```bash
# Install Node.js (v14+)
# Install Java JDK 11+
# Install Android SDK
```

#### Steps:
```bash
# 1. Install Bubblewrap
npm install -g @bubblewrap/cli

# 2. Initialize your project
bubblewrap init --manifest=https://your-domain.com/manifest.json

# 3. Build the APK
bubblewrap build

# 4. Your APK will be in: ./app-release-signed.apk
```

**Configuration will be saved in twa-manifest.json**

---

### Method 3: Web2APK Pro
**Best for: Quick testing without hosting**

1. Go to: https://web2apkpro.com
2. Paste your PWA URL (or use localhost with ngrok)
3. Configure:
   - App Name: PhishGuard AI
   - Package Name: com.phishguard.ai
   - Icon: Upload icon file
4. Generate and download APK

---

### Method 4: AppsGeyser (No Coding Required)
**Best for: Complete beginners**

1. Visit: https://appsgeyser.com
2. Choose "Progressive Web App"
3. Enter your hosted PWA URL
4. Customize appearance
5. Download APK instantly

---

### Method 5: Android Studio with TWA (Full Control)
**Best for: Professional deployment with custom features**

See separate guide: ANDROID_STUDIO_GUIDE.md

---

## 📋 Pre-Requirements for APK Generation

### Your PWA Must Have:
✅ **manifest.json** - Already included  
✅ **Service Worker (sw.js)** - Already included  
✅ **HTTPS hosting** - Required for production  
✅ **Valid icons** - 192x192 and 512x512 PNG  

### For Play Store Submission:
- Signed AAB file (not APK)
- Privacy Policy URL
- App description and screenshots
- Google Play Console account ($25 one-time fee)

---

## 🎨 Creating App Icons

You need icons in multiple sizes. Use this online tool:
https://www.pwabuilder.com/imageGenerator

Upload a single 1024x1024 PNG and it generates all required sizes.

**Required Sizes:**
- 192x192 (manifest)
- 512x512 (manifest)
- 48x48, 72x72, 96x96, 144x144 (Android)

---

## 🚀 Quick Start - GitHub Pages Hosting (FREE)

```bash
# 1. Create a new repository on GitHub
# 2. Upload all your files
# 3. Go to Settings > Pages
# 4. Select "main" branch
# 5. Your site will be live at: https://username.github.io/repo-name
# 6. Use PWABuilder to convert to APK
```

---

## 🔐 Digital Asset Links (Remove Address Bar)

After generating APK, you'll need to verify ownership.

1. **Get your app's fingerprint:**
   - Download "Asset Links Tool" from Play Store
   - Select your installed APK
   - Copy the generated assetlinks.json

2. **Host assetlinks.json:**
   ```
   https://your-domain.com/.well-known/assetlinks.json
   ```

3. **Content structure:**
   ```json
   [{
     "relation": ["delegate_permission/common.handle_all_urls"],
     "target": {
       "namespace": "android_app",
       "package_name": "com.phishguard.ai",
       "sha256_cert_fingerprints": ["YOUR_FINGERPRINT_HERE"]
     }
   }]
   ```

---

## 📦 What's Included in This Package

```
phishguard-ai/
├── index.html          # Main app structure
├── style.css           # Modern styling
├── script.js           # Phishing detection logic
├── app.js              # PWA functionality
├── manifest.json       # PWA manifest
├── sw.js               # Service worker
├── README_APK.md       # This guide
├── bubblewrap-config.json  # Bubblewrap configuration
└── ANDROID_STUDIO_GUIDE.md # Detailed Android Studio guide
```

---

## ⚡ Fastest Method (5 Minutes)

1. **Create GitHub account** (if you don't have)
2. **Upload files to new repository**
3. **Enable GitHub Pages** in settings
4. **Visit PWABuilder.com**
5. **Enter your GitHub Pages URL**
6. **Download APK** 

DONE! 🎉

---

## 🆘 Troubleshooting

**Problem:** PWABuilder shows errors  
**Solution:** Ensure manifest.json and sw.js are accessible at root

**Problem:** Address bar still showing in APK  
**Solution:** Add assetlinks.json to your server

**Problem:** APK won't install  
**Solution:** Enable "Install from Unknown Sources" in Android settings

**Problem:** Service worker not working  
**Solution:** PWA must be served over HTTPS (not http)

---

## 📱 Testing Your APK

```bash
# Install via ADB (Android Debug Bridge)
adb install app-release.apk

# Or transfer APK to phone and install manually
```

---

## 🎯 Play Store Publishing Checklist

- [ ] Create Google Play Console account
- [ ] Generate signed AAB (not APK)
- [ ] Create app listing (title, description, screenshots)
- [ ] Add privacy policy URL
- [ ] Upload AAB to Play Console
- [ ] Complete content rating questionnaire
- [ ] Set pricing (free/paid)
- [ ] Submit for review

**Review time:** Usually 1-3 days

---

## 💡 Tips

- Test APK on multiple Android devices
- Use Android 8.0+ for best TWA support
- Keep PWA and APK in sync (update both)
- Monitor Play Console for crash reports
- Enable Firebase for analytics and crash reporting

---

## 🔗 Useful Resources

- PWABuilder: https://www.pwabuilder.com
- Bubblewrap Docs: https://github.com/GoogleChromeLabs/bubblewrap
- TWA Guide: https://developer.android.com/codelabs/pwa-in-play
- Play Console: https://play.google.com/console

---

## 📞 Support

For issues, create a GitHub issue or contact support.

Built with ❤️ for Android | © 2025 PhishGuard AI
