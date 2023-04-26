# Android
At present I use Unity to make Android builds and releases. Here are some notes on building for Android using Unity.

## Android Build Settings
Under Project Settings > Player > Other Settings, set the following:
- set Target API Level to API Level 31 or higher (as of August 2022)
- set Scripting Backend to IL2CPP
- check ARM64 in Project Architecture
- create a new keystore, save locally, and add to publish settings (keep track of the keystore password)
- Ensure the bundle identifier is set to the bundle identifier configured in the Google Play Developer Console
- If there is a warning in Google Play about sensitive permissions and APIs, then we likely need to remove the REQUEST_INSTALL_PACKAGES permission from the manifest file. In the manifest file created in Plugin > Android add the following: `<uses-permission android:name="android.permission.REQUEST_INSTALL_PACKAGES" tools:node="remove"/>`

## Build Size and Compression
Images make up the bulk of the application build size. Image compression is used to reduce build size. Use the following compression and build settings rules to reduce build size.
- override the platform compression for iOS/Android and set a Max Size suitable for the image. Avoid anything over 1024 and reduce to the nearest power of 2 when possible.
- use RGB(A) Compressed ASTC HDR 4x4 block for best results. Previously I used RGB Compressed ETC 4 bits for images with no transparency and RGB + 1 bit Alpha Compressed ETC2 4 bits for images with transparency. However, testing shows that ASTC gets better results.
- In build settings, set compression method to LZ4HC.
- In build settings, turn off Development Build.