# GUI for EasyTier

this is a GUI implementation for EasyTier, based on Tauri2.

## Compile

### Install prerequisites

```
apt install npm
npm install -g pnpm
```

### For Desktop (Win/Mac/Linux)

```
cd ../tauri-plugin-vpnservice
pnpm install
pnpm build

cd ../easytier-web/frontend-lib
pnpm install
pnpm build

cd ../../easytier-gui
pnpm install
pnpm tauri build
```

### For Android

Need to install android SDK / emulator / NDK / Java (easy with android studio)

```
# For ArchLinux
sudo pacman -Sy sdkmanager
sudo sdkmanager --install platform-tools platforms\;android-34 ndk\;r26 build-tools
export PATH=/opt/android-sdk/platform-tools:$PATH
export ANDROID_HOME=/opt/android-sdk/
export NDK_HOME=/opt/android-sdk/ndk/26.0.10792818/
rustup target add aarch64-linux-android

install java 20
```

Java version depend on gradle version specified in (easytier-gui\src-tauri\gen\android\build.gradle.kts)

See [Gradle compatibility matrix](https://docs.gradle.org/current/userguide/compatibility.html) for detail .

```
pnpm install
pnpm tauri android init
pnpm tauri android build
```

### For iOS (iPhone/iPad)

> ℹ️ 目前仅支持在 macOS 上打包 iOS 版本，因为需要 Xcode 及苹果开发者工具链。

1. 安装依赖
   ```bash
   # 安装 Tauri 移动端依赖
   brew install cocoapods ios-deploy

   # 安装 Rust 的 iOS 目标
   rustup target add aarch64-apple-ios x86_64-apple-ios
   ```

2. 在 `easytier-gui` 目录下初始化 iOS 项目（只需执行一次，生成 `src-tauri/gen/apple` 目录）
   ```bash
   pnpm install
   pnpm tauri ios init
   ```

3. 构建 `.ipa` 安装包（默认会在 `src-tauri/target/universal/release/bundle/ios/` 下生成）
   ```bash
   pnpm tauri ios build --release
   ```

4. 如果需要发布到实际设备或 TestFlight，请使用 Xcode 的 `Organizer` 或 `xcodebuild -exportArchive` 完成签名与导出，`pnpm tauri ios build --release` 生成的 Xcode archive 位于
   ```
   src-tauri/target/universal/release/bundle/ios/*.xcarchive
   ```

上述流程生成的 `.ipa` 文件可直接安装在 iPad 上。
