# SimpleLiveTV · 安卓电视直播 APK

一个适用于 **Android TV** 的直播应用安装包（APK），通过 **GitHub Actions** 自动编译。

- **代码来源**：[xiaoyaocz/dart_simple_live](https://github.com/xiaoyaocz/dart_simple_live) 中的 `simple_live_tv_app`（安卓 TV 客户端）。
- **设计启发**：[pcccccc/AngelLive](https://github.com/pcccccc/AngelLive) 的 UI / 弹幕理念。
  > ⚠️ 说明：AngelLive 是 **SwiftUI** 项目，仅支持 Apple 平台（iOS / macOS / tvOS），**无法编译为 Android**。本仓库的 APK 实际由 `dart_simple_live` 的 TV 应用构建；AngelLive 仅作为产品形态与交互的参考。

---

## 自动构建（GitHub Actions）

仓库内置工作流 `.github/workflows/build.yml`，在以下情况自动运行：

- 推送到 `main` / `master` 分支
- 推送 `v*` 开头的 tag（如 `v1.0.0`，会自动生成 GitHub Release）
- 在 Actions 页面手动 **Run workflow**

构建产物为 **`app-release.apk`**（已签名，可直接安装到电视）。

### 如何获取 APK

1. 打开仓库的 **Actions** 标签页，点击对应的运行记录。
2. 在页面底部的 **Artifacts** 区域下载 `simple-live-tv-apk`（即 `app-release.apk`）。
3. 若打了 `v*` tag，也可在 **Releases** 页面直接下载。

---

## 安装到安卓电视

APK 是 **Android 安装包，在 Windows / macOS 上双击不会有任何反应**——它必须安装到安卓设备。

**方式一：ADB（推荐，需电视开启“开发者选项 / USB 调试”或网络调试）**

```bash
# 网络调试（电视与电脑同一局域网）
adb connect <电视IP>:5555
adb install app-release.apk

# 或 USB 直连
adb install app-release.apk
```

**方式二：U 盘 / 网络共享**

把 `app-release.apk` 拷到电视，用电视上的**文件管理器**打开安装。
安装前需在电视「设置 → 安全 / 应用」中开启 **“允许安装未知来源应用”**。

---

## 支持平台

取决于 `simple_live_core` 聚合源，通常包含：虎牙、斗鱼、B 站、抖音等直播平台。

---

## 关于签名

工作流默认在 CI 内 **自动生成一个临时签名密钥**，因此**无需配置任何 Secret 即可构建**。
缺点是每次构建的签名都不同，适合侧载 / 测试。

若你要发布**可增量升级**的版本（签名需固定），请在仓库 **Settings → Secrets and variables → Actions** 中配置以下仓库密钥，并参考工作流注释改用正式密钥：

| Secret | 说明 |
| --- | --- |
| `SIGNING_KEY` | 正式 `.jks` 的 **base64** 内容（`base64 -w0 keystore.jks`） |
| `KEY_ALIAS` | 密钥别名 |
| `KEY_PASSWORD` | 密钥密码 |
| `KEY_STORE_PASSWORD` | 密钥库密码 |

---

## 本地构建（可选）

如需在本机编译（Windows / macOS / Linux）：

- Flutter **3.38.3**（Dart 3.10.1）
- Android SDK：platforms 34/35、build-tools 35.0.0
- **NDK 28.2.13676358**（Flutter 3.38.3 必需）
- JDK 17

```bash
cd simple_live_tv_app
flutter pub get
flutter build apk --release
# 产物：build/app/outputs/flutter-apk/app-release.apk
```

---

## 致谢与许可

- 核心代码来自 [xiaoyaocz/dart_simple_live](https://github.com/xiaoyaocz/dart_simple_live)（遵循其 LICENSE）。
- 设计理念参考 [pcccccc/AngelLive](https://github.com/pcccccc/AngelLive)。
- 本项目沿用上游 LICENSE，详见仓库 `LICENSE` 文件。
