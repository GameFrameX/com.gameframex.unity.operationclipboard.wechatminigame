<div align="center">
<img src="https://download.alianblank.com/gameframex/gameframex_logo_320.png" alt="Game Frame X Logo" width="160" />

# GameFrameX OperationClipboard (微信小遊戲)

[![License](https://img.shields.io/github/license/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame)](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/blob/main/LICENSE.md)
[![Version](https://img.shields.io/github/v/release/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame)](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/releases)
[![Unity Version](https://img.shields.io/badge/Unity-2019.4-black?logo=unity)](https://unity.com/)
[![Documentation](https://img.shields.io/badge/Documentation-docs-blue)](https://gameframex.doc.alianblank.com)

獨立遊戲前後端一體化解決方案 · 獨立遊戲開發者的圓夢大使

<br />

[文檔](https://gameframex.doc.alianblank.com) · [快速開始](#快速開始) · QQ群: 467608841 / 233840761

<br />

[English](README.md) | [简体中文](README.zh-CN.md) | **繁體中文** | [日本語](README.ja.md) | [한국어](README.ko.md)

</div>

## 目錄導航

- [項目簡介](#項目簡介)
- [快速開始](#快速開始)
- [使用範例](#使用範例)
- [文檔與資源](#文檔與資源)
- [社區與支援](#社區與支援)
- [開源協議](#開源協議)

---

## 項目簡介

GameFrameX OperationClipboard（微信小遊戲）是面向微信小遊戲平台的粘貼板適配渠道包。本包實現 `IOperationClipboardManager`（`WechatMiniGameOperationClipboardManager`），並通過 `WechatMiniGameOperationClipboardBootstrap` 在 `BeforeSceneLoad` 階段自註冊進主包 `OperationClipboardRegistry`（宏 `UNITY_WEBGL && ENABLE_WECHAT_MINI_GAME && ENABLE_WECHAT_MINI_GAME_OPERATIONCLIPBOARD` 未啟用時代碼被裁剪，註冊不會發生；引用即註冊）。粘貼板 API 僅收文本，無需任何啟動期配置。

### 核心特性

- **微信小遊戲粘貼板** - 原生 SDK 集成，回調式讀寫直通平台
- **自註冊** - `BeforeSceneLoad` 階段自註冊進 `OperationClipboardRegistry`（引用即註冊）
- **零耦合** - 主包不反向引用渠道 SDK；SDK 由目標工程自帶

### 系統需求

- Unity 2019.4 或更高版本

---

## 快速開始

### 安裝

選擇以下任一方式：

1. 編輯 Unity 專案的 `Packages/manifest.json`，添加 `scopedRegistries` 部分：
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

   `scopes` 控制哪些包通過此註冊表解析。只有以 `com.gameframex` 開頭的包才會從這個註冊表獲取。

2. 直接在 `manifest.json` 的 `dependencies` 節點下添加以下內容：
   ```json
   {
      "com.gameframex.unity.operationclipboard.wechatminigame": "https://github.com/gameframex/com.gameframex.unity.operationclipboard.wechatminigame.git"
   }
   ```
3. 在 Unity 的 `Package Manager` 中使用 `Git URL` 的方式添加庫，地址為： `https://github.com/gameframex/com.gameframex.unity.operationclipboard.wechatminigame.git`
4. 直接下載倉庫放置到 Unity 專案的 `Packages` 目錄下，會自動加載識別。

### 渠道註冊（Bootstrap 自註冊）

無需任何啟動期配置。本包與微信小遊戲 SDK（`com.qq.weixin.minigame`）一併安裝後渠道自動註冊，遊戲側照常使用靜態門面 `OperationClipboard` 即可。

```csharp
using GameFrameX.OperationClipboard.Runtime;

// 遊戲側無需平台分支 —— 門面自動轉發已註冊渠道。
OperationClipboard.SetValue(text, success => { /* 結果 */ });
OperationClipboard.GetValue((success, content) => { /* 內容 */ });
```

渠道 `ChannelId` 常量值為 `"wechatminigame"`；`OperationClipboardRegistry.ResolveManager()` 按註冊序解析渠道，也可通過 `GetManager("wechatminigame")` 顯式獲取本渠道。
## 使用範例

### 項目結構

```
Runtime/
├── WechatMiniGame/
│   ├── WechatMiniGameOperationClipboardManager.cs  # 微信小遊戲 clipboard manager
│   └── WechatMiniGameOperationClipboardBootstrap.cs  # channel bootstrap (self-registration)
└── GameFrameXOperationClipboardWechatMiniGameCroppingHelper.cs  # code cropping helper
```

### 主要類

- **`WechatMiniGameOperationClipboardManager`**：實現了微信小遊戲粘貼板 SDK 的 `IOperationClipboardManager` 介面（`public const string ChannelId`）。
- **`WechatMiniGameOperationClipboardBootstrap`**：渠道啟動註冊，`BeforeSceneLoad` 階段自註冊進主包 `OperationClipboardRegistry`。
- **`GameFrameXOperationClipboardWechatMiniGameCroppingHelper`**：用於構建優化的代碼裁剪輔助類。

### 注意事項

- 微信端寫入成功後平台會彈出「內容已複製」系統提示。

---

## 文檔與資源

- **完整文檔**: [https://gameframex.doc.alianblank.com](https://gameframex.doc.alianblank.com)
- **問題反饋**: [GitHub Issues](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/issues)

---

## 社區與支援

- **QQ 討論群**: [467608841](https://qm.qq.com/cgi-bin/qm/qr?k=sYFd1nv6m2KZIWFLorZ5pBR0AE5ZhbuL&jump_from=webapi&authKey=oCu+uoL3n35fT5SEt7iLgGtROPxh31n/rHUxRlp0w1f+j38W4tKBuWyRH3KEdwHN)
- **功能建議**: [GitHub Discussions](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/discussions)

---

## 依賴

| 包 | 說明 |
|----|------|
| com.gameframex.unity.operationclipboard | GameFrameX OperationClipboard 主包 |
| com.gameframex.unity | GameFrameX 核心 |
| com.qq.weixin.minigame | 目標工程自帶（不在本包聲明） |


## 更新日誌

查看 [Releases](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/releases) 了解更新日誌。

## 開源協議

詳見 [LICENSE.md](LICENSE.md) 檔案。
