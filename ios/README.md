# iOS
At present I use Unity and Xcode to make iOS builds and releases. Here are some notes on building for iOS using Unity and Xcode.

## Deploy to iOS device
1. Create an application identifier on the Apple Developer account with bundle identifier of type com.organization.appname. We only need to do this step once.
2. Create certificate (set the type to Apple Development). To do so, we must create a Certificate Signing Request (CSR) file on a Mac computer, upload the CSR to the Apple Developer account, and then download the newly created certificate and add it to the Mac computer keychain (double click the downloaded certificate, add under login). We only need to do this step once.
3. Register test devices on the Apple Developer account. We need the device UDID to register a device for testing. This can be obtained using Xcode. Connect your device, open Xcode, open Window > Devices and Simulators. The device UDID is shown in the window.
4. Create a provisioning profile for iOS App Development. Select the application identifier created above. Select the certificate created above. Give the provisioning profile a name and download to the build system. This will be used in the build process.
5. Build the Xcode project in Unity. Make sure the application identifier in the build settings matches the application identifier registered in the Apple Developer account.
6. Open the Xcode project. Connect your iOS device. Under Signing & Capabilities, select the provisioning profile created above. Press the build/play button to deploy to the connected device.

## Deploy a build to Apple Connect
1. Create an application identifier on the Apple Developer account with bundle identifier of type com.organization.appname. We only need to do this step once. If this was already done for deploying a build to an iOS device, then we do not need to do it again.
2. Create certificate (set the type to Apple Distribution). To do so, we must create a Certificate Signing Request (CSR) file on a Mac computer, upload the CSR to the Apple Developer account, and then download the newly created certificate and add it to the Mac computer keychain (double click the downloaded certificate, add under login). We only need to do this step once.
3. Register test devices on the Apple Developer account. We need the device UDID to register a device for testing. This can be obtained using Xcode. Connect your device, open Xcode, open Window > Devices and Simulators. The device UDID is shown in the window.
4. Create a provisioning profile for App Store Distribution. Select the application identifier created above. Select the certificate created above. Give the provisioning profile a name and download to the build system. This will be used in the build process.
5. Build the Xcode project in Unity. Make sure the application identifier in the build settings matches the application identifier registered in the Apple Developer account. Be sure to bump the version number before building.
6. Open the Xcode project. Make sure you are logged into an Apple account that has admin privileges for the Apple Developer account.
7. Under Build Settings > Signing, set the Code Signing Identity to Apple Distribution for ALL entries.
8. Under Signing & Capabilities, select the provisioning profile created above.
9. In `info.plist` change Bundle Display Name to "DIY Sun Sci" otherwise spaces will be removed.
10. Create an archive by selecting Product > Archive.
11. Upload the archive. Under Archives, select the newly created archive and press Distribute App. Set the method of distribution to App Store Connect. Select Upload. Make sure the proper certificate and provisioning profile are selected.


An example of this process can be found here:
https://www.youtube.com/watch?v=9xQw0p0w9Ac

## Preview Videos on Apple Connect
Preview videos can be created and uploaded to Apple Connect.  Videos must meet the following requirements:
- 30 seconds or less in length
- 30 FPS or less
- resolution must match one of the screen display sizes

The supported display sizes and require video resolutions are as follows:
- iPhone 6.5" display: 886x1920, 1920x886
- iPhone 5.5" display: 1080x1920, 1920x1080
- iPad Pro (3rd Gen) 12.9" display: 1600x1200, 1200x1600
- iPad Pro (2nd Gen) 12.9" display: 1600x1200, 1200x1600

FFmpeg can be used to convert video files to required sizes as follows:
`ffmpeg -i input.mov -acodec copy -crf 12 -vf scale=886:1920,setsar=1:1 output.mp4`

## Build Size and Compression
Images make up the bulk of the application build size. Image compression is used to reduce build size. Use the following compression and build settings rules to reduce build size.
- override the platform compression for iOS/Android and set a Max Size suitable for the image. Avoid anything over 1024 and reduce to the nearest power of 2 when possible.
- use RGB(A) Compressed ASTC HDR 4x4 block for best results. Previously I used RGB Compressed ETC 4 bits for images with no transparency and RGB + 1 bit Alpha Compressed ETC2 4 bits for images with transparency. However, testing shows that ASTC gets better results.
- In build settings, set compression method to LZ4HC.
- In build settings, turn off Development Build.