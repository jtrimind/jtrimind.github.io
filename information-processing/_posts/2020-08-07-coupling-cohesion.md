---

title: "[정보처리기사] 결합도(Coupling), 응집도(Cohesion), 모듈의 기능적 독립성"
image: /assets/information-processing/2020-08-07-coupling-cohesion.png
modified_date: 2021-07-13
---

## 요약도

![결합도 응집도](/assets/information-processing/2020-08-07-coupling-cohesion.png)

## 모듈의 기능적 독립성
- 모듈의 기능적 독립성은 소프트웨어를 구성하는 각 모듈의 기능이 서로 독립됨을 의미
- 모듈이 하나의 기능만을 수행하고 다른 모듈과의 과도한 상호작용을 배제
- 결합도는 약하게, 응집도는 강하게 만들어야 함

## 결합도(Coupling)
- 두 모듈간의 상호작용, 또는 의존도 정도
- 모듈 간의 결합도를 약하게 하면 모듈 독립성이 향상
- 결합도의 종료(약->강)
  1. 자료(Data)
  2. 스탬프(Stamp)
    - 두 모듈이 매개변수로 자료를 전달할 때, 자료구조 형태로 전달
  3. 제어(Control)
    - 어떤 모듈이 다른 모듈의 내부 논리 조직을 제어하기 위한 목적으로 제어신호를 이용하여 통신
    - 하위 모듈에서 상위 모듈로 제어 신호가 이동하여 상위 모듈에게 처리 명령을 부여하는 권리 전도 현상이 발생
  4. 외부(Extern)
  5. 공통(Common)
    - 두 모듈이 동일한 전역 데이터를 접근
  6. 내용(Content)
    - 하나의 모듈이 직접적으로 다른 모듈의 내용을 참조

## 응집도(Cohesion)
- 좋은 모듈은 응집도가 높음
- 응집도의 종류(약->강)
  1. 우연적(Coincidental)
    - 서로 간에 어떠한 의미 있는 연관관계도 지니지 않은 기능 요소로 구성
    - 서로 다른 상위 모듈에 의해 호출되어 처리상의 연관성이 없는 서로 다른 기능을 수행
  2. 논리적(Logical)
  3. 시간적(Temporal)
  4. 절차적(Procedural)
    - 모듈이 다수의 관련 기능을 가질 떄 모듈안의 구성 요소들이 그 기능을 순차적으로 수행
  5. 교환적(Communication)
  6. 순서적(Sequential)
  7. 기능적(Functional)

## 중간 광고
<iframe src="https://coupa.ng/bT5WRy" width="120" height="240" frameborder="0" scrolling="no" referrerpolicy="unsafe-url"></iframe>

`파트너스 활동을 통해 일정액의 수수료를 제공받을 수 있음`

```bash
# 20년 4회 필기
73. 결합도(Coupling)에 대한 설명으로 틀린 것은?
① 데이터 결합도(Data Coupling)는 두 모듈이 매개변수로 자료를 전달할 때, 자료구조 형태로 전달되어 이용 될 때 데이터가 결합되어 있다고 한다.
② 내용 결합도(Content Coupling)는 하나의 모듈이 직접적으로 다른 모듈의 내용을 참조할 때 두 모듈은 내용적으로 결합되어 있다고 한다.
③ 공통 결합도(Common Coupling)는 두 모듈이 동일한 전역 데이터를 접근한다면 공통결합 되어 있다고 한다.
④ 결합도(Coupling)는 두 모듈간의 상호작용, 또는 의존도 정도를 나타내는 것이다.
```

```bash
# 20년 4회 필기
74. 응집도의 종류 중 서로 간에 어떠한 의미 있는 연관관계도 지니지 않은 기능 요소로 구성되는 경우이며,
 서로 다른 상위 모듈에 의해 호출되어 처리상의 연관성이 없는 서로 다른 기능을 수행하는 경우의 응집도는?
① Functional Cohesion
② Sequential Cohesion
③ Logical Cohesion
④ Coincidental Cohesion
```

```bash
# 20년 3회 필기
70. 다음이 설명하는 응집도의 유형은?
모듈이 다수의 관련 기능을 가질 떄 모듈안의 구성 요소들이 그 기능을 순차적으로 수행할 경우의 응집도
① 기능적 응집도 ② 우연적 응집도
③ 논리적 응집도 ④ 절차적 응집도
```

```bash
# 20년 3회 필기
72. 다음 중 가장 결합도가 강한 것은?
① data coupling   ② stamp coupling
③ common coupling ④ control coupling
```

```bash
# 20년 3회 필기
77. 어떤 모듈이 다른 모듈의 내부 논리 조직을 제어하기 위한 목적으로 제어신호를 이용하여 통신하는 경우이며,
하위 모듈에서 상위 모듈로 제어 신호가 이동하여 상위 모듈에게 처리 명령을 부여하는 권리 전도 현상이 발생하게 되는 결합도는?
① data coupling   ② stamp coupling
③ common coupling ④ control coupling
```

```bash
# 20년 2회 실기
14. (가), (나)에 들어갈 단어를 각각 적으시오.
모듈의 기능적 독립성은 소프트웨어를 구성하는 각 모듈의 기능이 서로 독립됨을 의미하는 것으로,
모듈이 하나의 기능만을 수행하고 다른 모듈과의 과도한 상호작용을 배제함으로써 이루어진다.
모듈의 독립성을 높이기 위해서는 (가)는 약하게, (나)는 강하게 만들어야 한다.
```

```bash
# 20년 2회 필기
64. 시스템에서 모듈 사이의 결합도(Coupling)에 대한 설명으로 옳은 것은?
① 한 모듈 내에 있는 처리요소들 사이의 기능적인 연관 정도를 나타낸다.
② 결합도가 높으면 시스템 구현 및 유지보수 작업이 쉽다.
③ 모듈 간의 결합도를 약하게 하면 모듈 독립성이 향상된다.
④ 자료결합도는 내용결합도보다 결합도가 높다
```

```bash
# 20년 2회 필기
77. 응집도가 가장 낮은 것은?
① 기능적 응집도 ② 시간적 응집도
③ 절차적 응집도 ④ 우연적 응집도
```

```bash
# 04년 1회 필기
x. 좋은 모듈이 되기 위한 응집도와 결합도에 대한 설명으로 옳은 것은?
① 모듈의 응집도와 결합도 모두가 높아야 한다.
② 모듈의 응집도는 높아야 하고 결합도는 낮아야 한다.
③ 모듈의 응집도는 낮아야 하고 결합도는 높아야 한다.
④ 모듈의 응집도와 결합도 모두가 낮아야 한다.
```