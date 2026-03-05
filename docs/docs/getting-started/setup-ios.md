---
sidebar_position: 2
---

import IapKitBanner from '@site/src/uis/IapKitBanner';

# iOS Setup

<IapKitBanner />

## Godot Export

1. **Project → Export → Add → iOS**
2. Configure your export settings (Bundle Identifier, Team ID, etc.)
3. Export the project to a folder

## Xcode Configuration (Required)

After exporting from Godot, you **must** configure the frameworks in Xcode:

1. Open the exported `.xcodeproj` in Xcode
2. Select your target → **General** tab
3. Scroll to **Frameworks, Libraries, and Embedded Content**
4. Click **+** and add the following frameworks:
   - `GodotIap.framework`
   - `SwiftGodotRuntime.framework`
5. Set both to **"Embed & Sign"**

The frameworks are located at:
```
[exported_project]/addons/godot-iap/bin/ios/GodotIap.framework
[exported_project]/addons/godot-iap/bin/ios/SwiftGodotRuntime.framework
```

6. Go to **Build Settings** tab
7. Search for **"Runpath Search Paths"** (`LD_RUNPATH_SEARCH_PATHS`)
8. Add `@executable_path/Frameworks` if it is not already present

:::warning Important
If you skip embedding the frameworks, the app will crash on launch with:
```
Library not loaded: @rpath/GodotIap.framework/GodotIap
```
If you skip setting the Runpath Search Paths, the app will crash even with embedded frameworks:
```
Library not loaded: @rpath/SwiftGodotRuntime.framework/SwiftGodotRuntime
```
:::

## Fix Missing Info.plist (Required)

Due to a [Godot export bug](https://github.com/godotengine/godot/issues/109075), the `Info.plist` files inside frameworks may not be copied during export. This causes the build to fail with:

```
Framework did not contain an Info.plist
```

### Solution: Add Build Phase Script

1. In Xcode, select your target
2. Go to **Build Phases** tab
3. Click **+** → **New Run Script Phase**
4. Name it "Copy Framework Info.plist"
5. Paste the following script:

```bash
# Copy missing Info.plist files for GodotIap frameworks
ADDONS_DIR="${PROJECT_DIR}"
FRAMEWORKS_DIR="${BUILT_PRODUCTS_DIR}/${FRAMEWORKS_FOLDER_PATH}"

if [ -f "${ADDONS_DIR}/addons/godot-iap/bin/ios/GodotIap.framework/Info.plist" ]; then
    cp "${ADDONS_DIR}/addons/godot-iap/bin/ios/GodotIap.framework/Info.plist" \
       "${FRAMEWORKS_DIR}/GodotIap.framework/" 2>/dev/null || true
fi

if [ -f "${ADDONS_DIR}/addons/godot-iap/bin/ios/SwiftGodotRuntime.framework/Info.plist" ]; then
    cp "${ADDONS_DIR}/addons/godot-iap/bin/ios/SwiftGodotRuntime.framework/Info.plist" \
       "${FRAMEWORKS_DIR}/SwiftGodotRuntime.framework/" 2>/dev/null || true
fi
```

6. Drag this script phase **before** "Embed Frameworks" phase

:::tip
This script automatically copies the missing `Info.plist` files from the source frameworks to the build output. You only need to set this up once per project.
:::

## App Store Connect

For complete App Store Connect configuration including product setup and sandbox testing, visit:

**👉 [iOS Setup Guide - openiap.dev](https://openiap.dev/docs/ios-setup)**

The guide covers:

- App Store Connect configuration
- In-App Purchase product setup
- Sandbox testing
- Common troubleshooting steps
