---

title: "[정보보안기사] 레이스 컨디션 공격(Race Condition Attack)"
---

## 공부한 책
### 이기적 정보보안기사 필기+실기 환상의 콤비

<iframe src="https://coupa.ng/bTZNkr" width="120" height="240" frameborder="0" scrolling="no" referrerpolicy="unsafe-url"></iframe>

`파트너스 활동을 통해 일정액의 수수료를 제공받을 수 있음`  

## 레이스 컨디션(Race Condition)
공유 자원을 여러 프로세스가 동시에 이용하고자 경쟁을 벌이는 상태

## 레이스 컨디션 공격(Race Condition Attack)
- 공격대상은
  - 소유자가 root이고
  - setuid가 설정되었고
  - 임시파일을 생성하는 프로그램이다.
- 공격자는 임시파일의 이름으로 심볼링 링크를 만들어 공격한다.

## 참고 링크
[Race Condition](https://d4m0n.tistory.com/3)
