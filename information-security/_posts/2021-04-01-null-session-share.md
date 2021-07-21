---

title: "[정보보안기사] Null Session Share 취약점"
---

## 공부한 책
### 이기적 정보보안기사 필기+실기 환상의 콤비

<iframe src="https://coupa.ng/bTZNkr" width="120" height="240" frameborder="0" scrolling="no" referrerpolicy="unsafe-url"></iframe>

`파트너스 활동을 통해 일정액의 수수료를 제공받을 수 있음`  

## Null Session Share 취약점
- Windows에서는 C$, D$, IPC$, ADMIN$이 기본적으로 공유되어 있다.
- Null Session Share 취약점은 IPC$를 사용해서 원격접속을 할 때 패스워드를 NULL로 설정하여 접속할 수 있는 보안 취약점이다.

```
>net share

공유 이름   리소스                        설명

-------------------------------------------------------------------------------
C$           C:\                             기본 공유
D$           D:\                             기본 공유
IPC$                                         원격 IPC
ADMIN$       C:\WINDOWS                      원격 관리
명령을 잘 실행했습니다.
```
