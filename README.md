<div align="center">
<img src="https://download.alianblank.com/gameframex/gameframex_logo_320.png" alt="Game Frame X Logo" width="160" />

# GameFrameX OperationClipboard (WeChat Mini Game)

[![License](https://img.shields.io/github/license/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame)](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/blob/main/LICENSE.md)
[![Version](https://img.shields.io/github/v/release/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame)](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/releases)
[![Unity Version](https://img.shields.io/badge/Unity-2019.4-black?logo=unity)](https://unity.com/)
[![Documentation](https://img.shields.io/badge/Documentation-docs-blue)](https://gameframex.doc.alianblank.com)

All-in-One Solution for Indie Game Development · Empowering Indie Developers' Dreams

<br />

[Documentation](https://gameframex.doc.alianblank.com) · [Quick Start](#quick-start) · QQ Group: 467608841 / 233840761

<br />

**English** | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | [한국어](README.ko.md)

</div>

## Table of Contents

- [Project Overview](#project-overview)
- [Quick Start](#quick-start)
- [Usage Examples](#usage-examples)
- [Documentation & Resources](#documentation--resources)
- [Community & Support](#community--support)
- [License](#license)

---

## Project Overview

GameFrameX OperationClipboard (WeChat Mini Game) is the clipboard adapter channel package for the WeChat Mini Game platform. It implements `IOperationClipboardManager` (`WechatMiniGameOperationClipboardManager`) and self-registers into the main package's `OperationClipboardRegistry` via `WechatMiniGameOperationClipboardBootstrap` at the `BeforeSceneLoad` stage (when the macros `UNITY_WEBGL && ENABLE_WECHAT_MINI_GAME && ENABLE_WECHAT_MINI_GAME_OPERATIONCLIPBOARD` are not enabled the code is stripped and registration does not happen; referencing the package is registering). No configuration is needed: clipboard APIs only take text.

### Key Features

- **WeChat Mini Game Clipboard** - native SDK integration with callback-based read/write
- **Self-Registration** - registers into `OperationClipboardRegistry` at `BeforeSceneLoad` (referencing means registering)
- **Zero Coupling** - the main package never references the channel SDK; the SDK is provided by the target project

### System Requirements

- Unity 2019.4+

---

## Quick Start

### Installation

Choose one of the following methods:

1. Edit your Unity project's `Packages/manifest.json` and add the `scopedRegistries` section:
   ```json
   {
     "scopedRegistries": [
       {
         "name": "GameFrameX",
         "url": "https://gameframex.upm.alianblank.uk",
         "scopes": [
           "com.gameframex"
         ]
       }
     ],
     "dependencies": {
       "com.gameframex.unity.operationclipboard.wechatminigame": "1.0.0"
     }
   }
   ```

   `scopes` controls which packages are resolved through this registry. Only packages whose names start with `com.gameframex` will be fetched from it.

2. Add to `manifest.json` dependencies:
   ```json
   {
      "com.gameframex.unity.operationclipboard.wechatminigame": "https://github.com/gameframex/com.gameframex.unity.operationclipboard.wechatminigame.git"
   }
   ```
3. Use **Package Manager** in Unity with **Git URL**: `https://github.com/gameframex/com.gameframex.unity.operationclipboard.wechatminigame.git`
4. Clone the repository into your Unity project's `Packages` directory. It will be loaded automatically.

### Channel Registration (Bootstrap Self-Registration)

No startup configuration is required. After the package is installed together with the WeChat Mini Game SDK (`com.qq.weixin.minigame`), the channel registers itself automatically; use the static `OperationClipboard` facade as usual.

```csharp
using GameFrameX.OperationClipboard.Runtime;

// No platform branches needed in game code - the facade forwards to the registered channel.
OperationClipboard.SetValue(text, success => { /* result */ });
OperationClipboard.GetValue((success, content) => { /* content */ });
```

The channel `ChannelId` constant is `"wechatminigame"`; `OperationClipboardRegistry.ResolveManager()` resolves channels in registration order, or use `GetManager("wechatminigame")` to fetch this channel explicitly.
## Usage Examples

### Project Structure

```
Runtime/
├── WechatMiniGame/
│   ├── WechatMiniGameOperationClipboardManager.cs  # WeChat Mini Game clipboard manager
│   └── WechatMiniGameOperationClipboardBootstrap.cs  # channel bootstrap (self-registration)
└── GameFrameXOperationClipboardWechatMiniGameCroppingHelper.cs  # code cropping helper
```

### Main Classes

- **`WechatMiniGameOperationClipboardManager`**: implements `IOperationClipboardManager` for the WeChat Mini Game clipboard SDK (`public const string ChannelId`).
- **`WechatMiniGameOperationClipboardBootstrap`**: channel startup registration; self-registers into the main package's `OperationClipboardRegistry` at `BeforeSceneLoad`.
- **`GameFrameXOperationClipboardWechatMiniGameCroppingHelper`**: code cropping helper for build optimization.

### Notes

- After a successful write on WeChat, the platform shows a "content copied" system toast.

---

## Documentation & Resources

- **Documentation**: [https://gameframex.doc.alianblank.com](https://gameframex.doc.alianblank.com)
- **Issue Tracker**: [GitHub Issues](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/issues)

---

## Community & Support

- **QQ Group**: [467608841](https://qm.qq.com/cgi-bin/qm/qr?k=sYFd1nv6m2KZIWFLorZ5pBR0AE5ZhbuL&jump_from=webapi&authKey=oCu+uoL3n35fT5SEt7iLgGtROPxh31n/rHUxRlp0w1f+j38W4tKBuWyRH3KEdwHN)
- **Feature Requests**: [GitHub Discussions](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/discussions)

---

## Dependencies

| Package | Description |
|----|------|
| com.gameframex.unity.operationclipboard | GameFrameX OperationClipboard core |
| com.gameframex.unity | GameFrameX core |
| com.qq.weixin.minigame | provided by the target project |


## Changelog

See [Releases](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/releases) for changelog.

## License

See [LICENSE.md](LICENSE.md) for license information.
