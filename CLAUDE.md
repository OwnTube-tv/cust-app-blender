# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What is This Repository?

This is the **"Blender Tube"** branded app repository - one of multiple production branded apps built on OwnTube.tv. It contains only:

1. Brand-specific customizations (`.customizations` file)
2. Visual assets (`/assets/` directory)
3. GitHub Actions workflows (pulls from upstream)
4. Documentation

**No application source code lives here.** All code, architecture, and development happens in the upstream repository.

## Upstream Documentation

**For all OwnTube architecture, development commands, testing, and general knowledge:**

👉 **Read the upstream CLAUDE.md:** [OwnTube-tv/web-client/CLAUDE.md](https://github.com/OwnTube-tv/web-client/blob/main/CLAUDE.md)

The upstream documentation covers:
- Complete architecture and technical stack
- Development/build/test commands
- Code style guidelines and contribution process
- Customization system details
- CI/CD pipeline documentation
- Multi-instance architecture
- Platform-specific development (iOS, Android, tvOS, Android TV, Web)

## Blender Tube Brand Configuration

### Primary Backend
- **PeerTube Instance:** `video.blender.org`
- **Content Focus:** Blender 3D creation software - tutorials, evolutions, animated films supported by Blender Foundation

### App Identity
```bash
EXPO_PUBLIC_APP_NAME='Blender Tube'
EXPO_PUBLIC_APP_SLUG=com.owntubetv.blendertube
EXPO_PUBLIC_IOS_BUNDLE_IDENTIFIER=com.owntubetv.blendertube
EXPO_PUBLIC_ANDROID_PACKAGE=com.owntubetv.blendertube
```

### Legal Entity
```bash
EXPO_PUBLIC_PROVIDER_LEGAL_ENTITY='OwnTube Nordic AB'
EXPO_PUBLIC_PROVIDER_LEGAL_EMAIL=legal@owntube.tv
EXPO_PUBLIC_PROVIDER_CONTACT_EMAIL=hello@owntube.tv
```

### Deployment Targets
- **Web:** https://cust-app-blender.owntube.tv/
- **iOS/tvOS TestFlight:** https://testflight.apple.com/join/cEDD3KeK
- **Android/Android TV Google Play:** https://play.google.com/store/apps/details?id=com.owntubetv.blendertube

### Analytics
- **PostHog API Key:** `phc_GQKgtk89daHe6WEkJGw2iQHzT4czc4yl51vYFntA9LG`
- Users can opt out in Settings

## Assets Inventory

All visual assets in `/assets/` directory follow naming convention: `{assetType}_{width}x{height}.{ext}`

- `favicon_32x32.png` - Web favicon
- `icon_192x192.png`, `icon_512x512.png` - PWA icons
- `splashScreen_1152x1152.png` - Mobile splash screen (transparent, background color: `#335683`)
- `androidTvBanner_320x180.png` - Android TV banner
- `AppleTvIcon_1280x768.png` - Apple TV icon
- `AppleTvIconSmall_400x240.png`, `AppleTvIconSmall2x800x480.png` - Apple TV small icons
- `AppleTvTopShelf_1920x720.png`, `AppleTvTopShelf2x_3840x1440.png` - Apple TV top shelf
- `AppleTvTopShelfWide_2320x720.png`, `AppleTvTopShelfWide_4640x1440.png` - Apple TV top shelf wide

**Asset Path Pattern in `.customizations`:** `../customizations/assets/{filename}`

## How to Work with This Repository

### Making Changes
1. **Update branding:** Edit `.customizations` file
2. **Update assets:** Replace files in `/assets/` directory (maintain naming convention)
3. **Test changes:** No local testing available - use GitHub Actions pipeline (see below)

### Developing New Features

**For customization-only changes** (`.customizations` or `/assets/`): Work directly in this repo.

**For changes requiring upstream code modifications:**
1. **Fork the upstream repo:** Create a fork of [OwnTube-tv/web-client](https://github.com/OwnTube-tv/web-client)
2. **Update workflow source:** In `.github/workflows/*.yml`, change `owntube_source: OwnTube-tv/web-client` to point to your fork (e.g., `owntube_source: YourUsername/web-client`)
3. **Develop features:** Make code changes in your fork
4. **Test via this branded app:** Trigger builds from this repo to test your fork's changes
5. **Contribute upstream:** Once tested, submit PR to upstream `OwnTube-tv/web-client`
6. **Restore workflow source:** After upstream merge, revert workflows to use `OwnTube-tv/web-client`

### Testing Changes

**No local testing is supported for branded apps.** To test changes:

1. **Commit changes** to `.customizations`, `/assets/`, or workflow files (if using fork)
2. **Trigger build workflow** manually from GitHub Actions tab (see below)
3. **Test on devices** using platform-specific testing tools:
   - **iOS/tvOS:** Apple TestFlight (beta testing)
   - **Android/Android TV:** Google Play Internal Testing track
   - **Web:** Direct access via GitHub Pages URL

### Triggering Builds

This repo uses **manual workflow_dispatch** (push-button releases) - not automatic CI/CD.

**Main Build Workflow:** `.github/workflows/build_branded_main.yml`
- Trigger from Actions tab with options:
  - `upload_ios_to_testflight` - Upload iOS build to TestFlight for device testing
  - `upload_tvos_to_testflight` - Upload tvOS build to TestFlight for device testing
  - `google_play_track` - Upload Android to selected track (none/internal/alpha/beta/production)
  - `google_play_upload_tv` - Include Android TV in Google Play upload

**Initial Bundle Workflow:** `.github/workflows/google_play_initial_bundle.yml`
- Only needed for first-time Google Play submission
- Generates signed bundles for manual upload to Google Play Console

### Required GitHub Secrets
Configured in `owntube` and `github-pages` GitHub environments (see upstream docs/pipeline.md for details):

- Apple: Certificates, provisioning profiles, API keys
- Android: Keystore, signing credentials, service account JSON
- Custom domain: `CUSTOM_DEPLOYMENT_URL` (optional)

## Key Differences from Upstream

- **No source code** - This repo is configuration-only
- **Manual deployments** - Workflows triggered manually (not on push)
- **Single instance focus** - Locked to `video.blender.org` backend
- **Brand-specific assets** - Custom icons, splash screens, colors for Blender Tube

## Reference Documentation

For detailed customization options and CI/CD setup:
- **Upstream CLAUDE.md:** [OwnTube-tv/web-client/CLAUDE.md](https://github.com/OwnTube-tv/web-client/blob/main/CLAUDE.md)
- **Customizations Guide:** [OwnTube-tv/web-client/docs/customizations.md](https://github.com/OwnTube-tv/web-client/blob/main/docs/customizations.md)
- **Pipeline Setup:** [OwnTube-tv/web-client/docs/pipeline.md](https://github.com/OwnTube-tv/web-client/blob/main/docs/pipeline.md)
- **Template Repository:** [OwnTube-tv/cust-app-template](https://github.com/OwnTube-tv/cust-app-template)
