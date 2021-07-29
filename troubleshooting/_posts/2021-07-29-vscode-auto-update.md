---
title:  "[해결법] Visual Studio Code installing c# dependencies 무한반복"
---

## 현상
매번 vscode를 켤 때마다 C# dependencies를 설치하여 버벅거리는 현상이 나타난다.

```
Installing C# dependencies...
Platform: win32, x86_64

Downloading package 'OmniSharp for Windows (.NET 4.6 / x64)' (33778 KB).................... Done!
Validating download...
Integrity Check succeeded.
Installing package 'OmniSharp for Windows (.NET 4.6 / x64)'

Downloading package '.NET Core Debugger (Windows / x64)' (47489 KB).................... Done!
Validating download...
Integrity Check succeeded.
Installing package '.NET Core Debugger (Windows / x64)'

Downloading package 'Razor Language Server (Windows / x64)' (59571 KB).................... Done!
Installing package 'Razor Language Server (Windows / x64)'

Finished
```

## 해결방법
C# dependencies 관련한 어떤 모듈이 이미 업데이트가 된 것을 인지하지 못하는 것으로 보인다.  
스택오버플로우에서 auto-updates를 꺼서 해결했다고 하여 이 방법을 택했다.  
VS Code에서 auto-updates를 끄는 방법은 아래와 같다.

1. `File > Preferences > Settings`(macOS: `Code > Preferences > Settings`)으로 진입한다.
2. `extensions.autoUpdate`를 `false`로 설정한다.

## 참고링크
- [스택오버플로우 관련 질문](https://stackoverflow.com/questions/58020169/how-to-install-c-sharp-dependencies-one-time-or-manually-or-locally-to-work-offl/58027730#comment102443873_58020169)
- [VS Code auto-updates 끄기](https://code.visualstudio.com/docs/supporting/FAQ#_how-do-i-opt-out-of-vs-code-autoupdates)
