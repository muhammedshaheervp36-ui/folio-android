# Folio Android project

This archive contains Android source code, not an installable APK. An APK could not be compiled in the authoring environment: Android SDK and Gradle were absent, and the SDK download was not authorized. XML and archive structure were checked; Android compilation and on-device behavior have NOT been verified.

## What the app does

The Folio launcher opens the existing hosted Folio service in an Android Custom Tab (or the default browser when Custom Tabs are unsupported). The browser manages authentication, image selection/uploads, cookies, and navigation. This is a browser-backed Android wrapper, not an offline or fully native rewrite. It requires internet access and a browser. It contains no API keys or login credentials.

The hosted service is currently owner-private. Installing an APK does NOT give another person access or make the service public. Sign in with the owner account to test. Public availability must be configured separately before other designers can use it.

## Build an installable test APK using GitHub

1. Create a repository and put this folder's CONTENTS at its root, including `.github/workflows/build-apk.yml`.
2. Open Actions → Build Folio test APK → Run workflow.
3. After the run succeeds, download the `Folio-test-APK` artifact and unzip it.
4. Install the resulting `app-debug.apk` on your Android phone. The phone may ask you to allow installation from the app used to open the APK.

The workflow is provided but has not been uploaded or run on your behalf. A successful run creates a debug-signed APK for testing, not a Play Store release. Fresh CI builds may use different debug signing keys and may require uninstalling the previous test APK before installation. Do not use a debug certificate for public distribution.

## Build locally / Android Studio

Requirements: JDK 17, Gradle 8.9, Android SDK platform 35, Build Tools 34.0.0, and network access to Google/Maven/Gradle repositories.

1. Extract this folder.
2. Install Gradle 8.9 and Android SDK packages listed above; set `ANDROID_HOME` or create a `local.properties` file with `sdk.dir=/your/android/sdk`.
3. In the project root, run `gradle wrapper --gradle-version 8.9` to generate the standard wrapper. No unverified wrapper JAR is included in this archive.
4. Open the folder in Android Studio and select JDK 17 for Gradle. Allow Gradle sync to finish.
5. Run `./gradlew :app:assembleDebug :app:lintDebug` (Windows: `gradlew.bat :app:assembleDebug :app:lintDebug`).
6. Find the APK at `app/build/outputs/apk/debug/app-debug.apk`.

For release distribution, generate a signed APK through Android Studio and keep your private signing keystore secure for future updates. No release signing key has been created or bundled.

## Device checks still required

- Install and launch on Android 6 or newer.
- Confirm owner login completes and the Folio page loads.
- Upload an image; confirm it persists after closing and reopening.
- Verify private/public project controls using the hosted service's permitted accounts.
- Check file selection, copying a portfolio link, Back navigation, and no-network behavior.

## Change the service address

Edit `app/src/main/res/values/strings.xml` → `site_url`, then rebuild. The APK follows the existing website, so website changes do not normally require rebuilding the wrapper.

## Technical references

- Android Gradle Plugin 8.7 compatibility: https://developer.android.com/build/releases/agp-8-7-0-release-notes
- Chrome Custom Tabs low-level API: https://developer.chrome.com/docs/android/custom-tabs/howto-custom-tab-low-level-api
