# Distributing games

Once your game is ready to be published, it is recommended that you package it properly for consumption by players.

## Desktop games

To publish desktop games, it is recommended that you build your project as a self-contained .Net Core app. As such, your game will require absolutely no external dependencies and should run out-of-the-box as-is.

### Building and packaging for Windows

From the .Net Core CLI:

`dotnet publish -c Release -r win-x64 /p:PublishReadyToRun=false /p:TieredCompilation=false --self-contained`

You can then zip the content of the publish folder and distribute the archive as-is.

### Building and packaging for Linux

From the .Net Core CLI:

`dotnet publish -c Release -r linux-x64 /p:PublishReadyToRun=false /p:TieredCompilation=false --self-contained`

You can then zip the content of the publish folder and distribute the archive as-is.

We recommend using the .tar.gz archiving format to preserve the execution permissions.

### Build and packaging for macOS

From the .Net Core CLI:

`dotnet publish -c Release -r osx-x64 /p:PublishReadyToRun=false /p:TieredCompilation=false --self-contained`

It then required to structure the output to be a .app bundle. For that, you will have to create the following file structure:

```
YourGame.app                    (this is your root folder)
    - Contents
        - Resources
            - Content           (this is where all your content and XNB's should go)
            - YourGame.icns     (this is your app icon, in ICNS format)
        - MacOS                 (this is where your game belongs, except for content files)
    - Info.plist                (the metadata of your app)
```

The Info.plist file is a standard macOS file containing metadata about your game. An example file would go like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>CFBundleDevelopmentRegion</key>
    <string>en</string>
    <key>CFBundleExecutable</key>
    <string>YourGame</string>
    <key>CFBundleIconFile</key>
    <string>YourGame</string>
    <key>CFBundleIdentifier</key>
    <string>com.my-domain.YourGame</string>
    <key>CFBundleInfoDictionaryVersion</key>
    <string>6.0</string>
    <key>CFBundleName</key>
    <string>YourGame</string>
    <key>CFBundlePackageType</key>
    <string>APPL</string>
    <key>CFBundleShortVersionString</key>
    <string>1.0</string>
    <key>CFBundleSignature</key>
    <string>FONV</string>
    <key>CFBundleVersion</key>
    <string>1</string>
    <key>LSApplicationCategoryType</key>
    <string>public.app-category.games</string>
    <key>LSMinimumSystemVersion</key>
    <string>10.13</string>
    <key>NSHumanReadableCopyright</key>
    <string>Copyright © 2020</string>
    <key>NSPrincipalClass</key>
    <string>NSApplication</string>
</dict>
</plist>
```

After completing these steps, your .app folder should appears as an executable application on macOS.

For archiving, we recommend using the .tar.gz format for preserve the execution permissions.

### Special notes about .Net Core parameters

.Net Core proposes several parameters when publishing apps that may sound helpful, but have many issues when it comes to games (because they were never meant for games in the first place, but for small lightweight applications).

**Ready2Run**

You might be tempted to use Ready2Run assemblies for games. We recommended to not using it because it produces a lot a micro stutters throughout games.

This is due to the fact that Ready2Run code is of low quality and makes the Just-In-Time compiler to trigger very regularly to promote the code to a higher quality. Whenever the JIT runs, it produces potentially very visible stutters that players don't like at all.

Disabling Ready2Run solves this issue (at the cost of a slightly longer startup time, but typically very negligible).

**TieredCompilation**

Tiered compilation is a companion system to Ready2Run and works on the same principle to enhance startup time. We suggest disabling it to avoid any stutter while playing.

**SingleFilePublish**

SingleFilePublish packages your game into a single executable file with all dependencies and content integrated.

While it sounds convenient, be aware that it's not magical and is in fact an hidden self-extracting zip archive. As such, it may make app startup extremly longer if your game is large, and may fail to launch on systems where user permissions don't allow to extract files (or if there is not enough storage space available).

We recommend to not use it for a greater compatibility throughout all systems.

## Windows Store games

Please refer to the [Windows Store documentation]([Publish Windows apps - UWP applications | Microsoft Docs](https://docs.microsoft.com/en-us/windows/uwp/publish/).

## Mobile games

Please refer to the Xamarin documentation:

- [Android]([Publishing an Application - Xamarin | Microsoft Docs](https://docs.microsoft.com/en-us/xamarin/android/deploy-test/publishing/))

- [iOS](https://docs.microsoft.com/en-us/xamarin/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store?tabs=windows)

