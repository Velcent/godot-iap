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

:::warning Important
If you skip this step, the app will crash on launch with:
```
Library not loaded: @rpath/GodotIap.framework/GodotIap
```
:::

## App Store Connect

For complete App Store Connect configuration including product setup and sandbox testing, visit:

**👉 [iOS Setup Guide - openiap.dev](https://openiap.dev/docs/ios-setup)**

The guide covers:

- App Store Connect configuration
- In-App Purchase product setup
- Sandbox testing
- Common troubleshooting steps
