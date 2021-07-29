---

title: "[정보처리기사] 팬 인(Fan-in), 팬 아웃(Fan-out)"
image: /assets/images/information-processing/2021-07-14-fan-in-fan-out.png
---

## 팬 인(Fan-in)
모듈에 필요한 상위 모듈의 개수

## 팬 아웃(Fan-out)
모듈을 사용하는 하위 모듈의 개수

## 에시
![팬 인 팬 아웃 예시](/assets/images/information-processing/2021-07-14-fan-in-fan-out.png)

| 모듈 | 팬 인 | 팬 아웃 |
|-|-|-|
| A | 0 | 3 |
| B | 1 | 2 |
| C | 1 | 1 |
| D | 1 | 1 |
| E | 1 | 0 |
| F | 3 | 2 |
| G | 1 | 0 |
| H | 1 | 0 |

예를 들어 모듈 F의 경우,  
상위 모듈로 B, C, D를 가지고 있어 Fan-in이 3,  
하위 모듈로 G, H를 가지고 있어 Fan-out이 2이다.

## 정보처리기사 기출문제

```bash
# 21년 1회
8. 다음은 어떤 프로그램 구조를 나타낸다. 모듈 F에서의 fan-in과 fan-out의 수는 얼마인가?
① fan-in：2, fan-out：3
② fan-in：3, fan-out：2
③ fan-in：1, fan-out：2
④ fan-in：2, fan-out：1
```
![팬 인 팬 아웃 예시](/assets/images/information-processing/2021-07-14-fan-in-fan-out.png)

