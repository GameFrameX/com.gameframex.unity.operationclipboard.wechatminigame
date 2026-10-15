<div align="center">
<img src="https://download.alianblank.com/gameframex/gameframex_logo_320.png" alt="Game Frame X Logo" width="160" />

# GameFrameX OperationClipboard (WeChat 미니게임)

[![License](https://img.shields.io/github/license/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame)](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/blob/main/LICENSE.md)
[![Version](https://img.shields.io/github/v/release/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame)](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/releases)
[![Unity Version](https://img.shields.io/badge/Unity-2019.4-black?logo=unity)](https://unity.com/)
[![Documentation](https://img.shields.io/badge/Documentation-docs-blue)](https://gameframex.doc.alianblank.com)

인디 게임 개발자를 위한 올인원 솔루션 · 인디 개발자의 꿈을 실현

<br />

[문서](https://gameframex.doc.alianblank.com) · [빠른 시작](#빠른-시작) · QQ 그룹: 467608841 / 233840761

<br />

[English](README.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [日本語](README.ja.md) | **한국어**

</div>

## 목차

- [프로젝트 개요](#프로젝트-개요)
- [빠른 시작](#빠른-시작)
- [사용 예시](#사용-예시)
- [문서 및 자료](#문서-및-자료)
- [커뮤니티 및 지원](#커뮤니티-및-지원)
- [라이선스](#라이선스)

---

## 프로젝트 개요

GameFrameX OperationClipboard(WeChat 미니게임)는 WeChat 미니게임 플랫폼용 클립보드 적응 채널 패키지입니다. `IOperationClipboardManager`(`WechatMiniGameOperationClipboardManager`)를 구현하며, `WechatMiniGameOperationClipboardBootstrap`가 `BeforeSceneLoad` 단계에서 메인 패키지의 `OperationClipboardRegistry`에 자가 등록합니다(매크로 `UNITY_WEBGL && ENABLE_WECHAT_MINI_GAME && ENABLE_WECHAT_MINI_GAME_OPERATIONCLIPBOARD`가 활성화되지 않으면 코드가 제거되어 등록이 일어나지 않습니다. 참조 곧 등록). 클립보드 API는 텍스트만 받으므로 시작 시 설정이 필요 없습니다.

### 주요 기능

- **WeChat 미니게임 클립보드** - 네이티브 SDK 통합, 콜백 방식 읽기/쓰기 플랫폼 직통
- **자가 등록** - `BeforeSceneLoad` 단계에서 `OperationClipboardRegistry`에 자가 등록(참조 곧 등록)
- **제로 결합** - 메인 패키지는 채널 SDK를 역참조하지 않습니다. SDK는 대상 프로젝트가 제공합니다

### 시스템 요구사항

- Unity 2019.4 이상

---

## 빠른 시작

### 설치

다음 방법 중 하나를 선택하세요:

1. Unity 프로젝트의 `Packages/manifest.json`을 편집하여 `scopedRegistries` 섹션을 추가하세요:
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

   `scopes`는 이 레지스트리를 통해 해석되는 패키지를 제어합니다. `com.gameframex`로 시작하는 패키지만 이 레지스트리에서 가져옵니다.

2. `manifest.json`의 `dependencies`에 다음을 직접 추가하세요:
   ```json
   {
      "com.gameframex.unity.operationclipboard.wechatminigame": "https://github.com/gameframex/com.gameframex.unity.operationclipboard.wechatminigame.git"
   }
   ```
3. Unity의 `Package Manager`에서 **Git URL**로 추가하세요. 주소: `https://github.com/gameframex/com.gameframex.unity.operationclipboard.wechatminigame.git`
4. 저장소를 Unity 프로젝트의 `Packages` 디렉토리에 복사하면 자동으로 로드됩니다.

### 채널 등록 (Bootstrap 자가 등록)

시작 시 설정이 필요 없습니다. 이 패키지를 WeChat 미니게임 SDK(`com.qq.weixin.minigame`)와 함께 설치하면 채널이 자동 등록되며, 게임 측은 정적 퍼사드 `OperationClipboard`를 그대로 사용하면 됩니다.

```csharp
using GameFrameX.OperationClipboard.Runtime;

// 게임 측에 플랫폼 분기 불필요 —— 퍼사드가 등록된 채널로 자동 전달합니다.
OperationClipboard.SetValue(text, success => { /* 결과 */ });
OperationClipboard.GetValue((success, content) => { /* 내용 */ });
```

채널 `ChannelId` 상수는 `"wechatminigame"`입니다. `OperationClipboardRegistry.ResolveManager()`는 등록 순으로 채널을 해석하며, `GetManager("wechatminigame")`로 명시적으로 가져올 수도 있습니다.
## 사용 예시

### 프로젝트 구조

```
Runtime/
├── WechatMiniGame/
│   ├── WechatMiniGameOperationClipboardManager.cs  # WeChat 미니게임 clipboard manager
│   └── WechatMiniGameOperationClipboardBootstrap.cs  # channel bootstrap (self-registration)
└── GameFrameXOperationClipboardWechatMiniGameCroppingHelper.cs  # code cropping helper
```

### 주요 클래스

- **`WechatMiniGameOperationClipboardManager`**: WeChat 미니게임 클립보드 SDK용 `IOperationClipboardManager` 구현(`public const string ChannelId`).
- **`WechatMiniGameOperationClipboardBootstrap`**: 채널 시작 등록. `BeforeSceneLoad` 단계에서 메인 패키지의 `OperationClipboardRegistry`에 자가 등록합니다.
- **`GameFrameXOperationClipboardWechatMiniGameCroppingHelper`**: 빌드 최적화용 코드 크로핑 헬퍼.

### 주의 사항

- WeChat에서 쓰기 성공 후 플랫폼이 "복사됨" 시스템 토스트를 표시합니다.

---

## 문서 및 자료

- **문서**: [https://gameframex.doc.alianblank.com](https://gameframex.doc.alianblank.com)
- **이슈 트래커**: [GitHub Issues](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/issues)

---

## 커뮤니티 및 지원

- **QQ 그룹**: [467608841](https://qm.qq.com/cgi-bin/qm/qr?k=sYFd1nv6m2KZIWFLorZ5pBR0AE5ZhbuL&jump_from=webapi&authKey=oCu+uoL3n35fT5SEt7iLgGtROPxh31n/rHUxRlp0w1f+j38W4tKBuWyRH3KEdwHN)
- **기능 요청**: [GitHub Discussions](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/discussions)

---

## 의존성

| 패키지 | 설명 |
|----|------|
| com.gameframex.unity.operationclipboard | GameFrameX OperationClipboard 코어 |
| com.gameframex.unity | GameFrameX 코어 |
| com.qq.weixin.minigame | 대상 프로젝트 제공 |


## 변경 로그

변경 로그는 [Releases](https://github.com/GameFrameX/com.gameframex.unity.operationclipboard.wechatminigame/releases)를 참조하세요.

## 라이선스

자세한 내용은 [LICENSE.md](LICENSE.md) 파일을 참조하세요.
