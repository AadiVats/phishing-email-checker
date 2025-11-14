# 🤖 Android Studio TWA Implementation Guide

## Complete Guide to Building APK with Android Studio

### Prerequisites
- Android Studio (latest version)
- Java JDK 11 or higher
- Android SDK (API 28+)
- Basic knowledge of Android development

---

## Step 1: Install Android Studio

Download from: https://developer.android.com/studio

---

## Step 2: Create New Android Project

1. Open Android Studio
2. Click "New Project"
3. Select "Empty Activity"
4. Configure:
   - Name: PhishGuard AI
   - Package: com.phishguard.ai
   - Language: Java
   - Minimum SDK: API 28 (Android 9.0)
5. Click "Finish"

---

## Step 3: Add Dependencies

Open `app/build.gradle` and add:

```gradle
dependencies {
    implementation 'androidx.browser:browser:1.7.0'
    implementation 'com.google.androidbrowserhelper:androidbrowserhelper:2.5.0'
}
```

Sync project with Gradle files.

---

## Step 4: Update AndroidManifest.xml

Replace content with:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.phishguard.ai">

    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.PhishGuardAI">

        <activity
            android:name="com.google.androidbrowserhelper.trusted.LauncherActivity"
            android:label="@string/app_name"
            android:exported="true">

            <meta-data
                android:name="android.support.customtabs.trusted.DEFAULT_URL"
                android:value="https://your-domain.com" />

            <meta-data
                android:name="android.support.customtabs.trusted.STATUS_BAR_COLOR"
                android:resource="@color/colorPrimary" />

            <meta-data
                android:name="android.support.customtabs.trusted.NAVIGATION_BAR_COLOR"
                android:resource="@color/colorPrimary" />

            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

            <intent-filter android:autoVerify="true">
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data
                    android:scheme="https"
                    android:host="your-domain.com" />
            </intent-filter>
        </activity>

        <activity android:name="com.google.androidbrowserhelper.trusted.FallbackActivity" />

    </application>

</manifest>
```

---

## Step 5: Add Colors

Open `res/values/colors.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="colorPrimary">#667eea</color>
    <color name="colorPrimaryDark">#4f46e5</color>
    <color name="colorAccent">#8b5cf6</color>
</resources>
```

---

## Step 6: Update strings.xml

Open `res/values/strings.xml`:

```xml
<resources>
    <string name="app_name">PhishGuard AI</string>
    <string name="asset_statements">
        [{
            "relation": ["delegate_permission/common.handle_all_urls"],
            "target": {
                "namespace": "web",
                "site": "https://your-domain.com"
            }
        }]
    </string>
</resources>
```

---

## Step 7: Create App Icon

1. Right-click `res` folder
2. New > Image Asset
3. Choose icon type: Launcher Icons
4. Upload your 512x512 icon
5. Click "Next" and "Finish"

---

## Step 8: Generate Signing Key

```bash
# In terminal or command prompt
keytool -genkey -v -keystore phishguard.keystore -alias phishguard -keyalg RSA -keysize 2048 -validity 10000

# Follow prompts to set password and details
```

---

## Step 9: Configure Signing in build.gradle

Add to `app/build.gradle`:

```gradle
android {
    signingConfigs {
        release {
            storeFile file("phishguard.keystore")
            storePassword "your_keystore_password"
            keyAlias "phishguard"
            keyPassword "your_key_password"
        }
    }

    buildTypes {
        release {
            signingConfig signingConfigs.release
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}
```

---

## Step 10: Build APK

### For Testing (Debug APK):
1. Build > Build Bundle(s) / APK(s) > Build APK(s)
2. APK location: `app/build/outputs/apk/debug/app-debug.apk`

### For Release (Signed APK):
1. Build > Generate Signed Bundle / APK
2. Select APK
3. Choose keystore file
4. Enter passwords
5. Select "release" build variant
6. Click "Finish"
7. APK location: `app/release/app-release.apk`

### For Play Store (AAB):
1. Build > Generate Signed Bundle / APK
2. Select Android App Bundle
3. Follow same signing steps
4. AAB location: `app/release/app-release.aab`

---

## Step 11: Test APK

```bash
# Install via USB debugging
adb install app-release.apk

# Or transfer to phone and install manually
```

---

## Step 12: Setup Digital Asset Links

1. Get SHA-256 fingerprint:
```bash
keytool -list -v -keystore phishguard.keystore -alias phishguard
```

2. Create `assetlinks.json`:
```json
[{
  "relation": ["delegate_permission/common.handle_all_urls"],
  "target": {
    "namespace": "android_app",
    "package_name": "com.phishguard.ai",
    "sha256_cert_fingerprints": [
      "YOUR_SHA256_FINGERPRINT_HERE"
    ]
  }
}]
```

3. Upload to:
```
https://your-domain.com/.well-known/assetlinks.json
```

4. Verify at:
```
https://developers.google.com/digital-asset-links/tools/generator
```

---

## Troubleshooting

**Issue:** App shows address bar  
**Fix:** Verify assetlinks.json is accessible and correct

**Issue:** App won't open  
**Fix:** Check if PWA URL is accessible from phone

**Issue:** Build fails  
**Fix:** Ensure all dependencies are synced, clean and rebuild

**Issue:** Signing error  
**Fix:** Verify keystore path and passwords are correct

---

## Advanced: Add Splash Screen

Create `res/drawable/splash_screen.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@color/colorPrimary"/>
    <item>
        <bitmap
            android:src="@drawable/splash_icon"
            android:gravity="center"/>
    </item>
</layer-list>
```

---

## Project Structure

```
PhishGuardAI/
├── app/
│   ├── src/
│   │   └── main/
│   │       ├── AndroidManifest.xml
│   │       ├── res/
│   │       │   ├── values/
│   │       │   │   ├── colors.xml
│   │       │   │   └── strings.xml
│   │       │   └── mipmap/
│   │       │       └── (app icons)
│   │       └── java/com/phishguard/ai/
│   └── build.gradle
├── phishguard.keystore
└── build.gradle
```

---

## Publishing to Google Play Store

1. Create Developer Account ($25)
2. Go to Play Console
3. Create New App
4. Upload AAB file
5. Complete store listing
6. Add screenshots (phone, tablet)
7. Set content rating
8. Submit for review

**Review time:** 1-7 days

---

## Tips

- Test on multiple Android versions
- Use Android Emulator for quick testing
- Enable proguard for production
- Monitor crash reports in Play Console
- Update regularly with security patches

---

## Additional Resources

- Android Developers: https://developer.android.com
- TWA Documentation: https://developers.google.com/web/android/trusted-web-activity
- Digital Asset Links: https://developers.google.com/digital-asset-links

---

Built for PhishGuard AI | © 2025
