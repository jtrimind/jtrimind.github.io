---

title:  "Underwater Period란?"
---

## Underwater Period란?
under(아래) + water(물)이므로 수중이라는 뜻이다.  
일반적으로 자산을 구매하였을 때의 비용보다 자산의 가치가 낮아진 상태를 의미한다.  

- 부동산 담보대출이 부동산의 가치보다 커지면 underwater이다.
- 옵션거래에서 옵션의 이득이 없는 상황, 즉 행사가격이 기초자산보다 높으면 underwater이다.

Underwater Period는 underwater인 상태의 기간으로, 전고점에서 하락했다가 다시 전고점까지의 가치를 회복하는 기간이다.  

## Underwater Period 계산식

$$Underwater Period = Recovered Date - Peak Date$$

## 예시
KOSPI 지수는 IMF로 인해 -75.41%의 MDD를 기록하였다. 이 때의 underwater period는 얼마였을까?  
[stooq의 KOSPI 데이터](https://stooq.com/q/?s=^kospi)를 가지고 분석해보았다.  

- Peak Date: 1994년 11월 8일(1138.75)
- Recovered Date: 2005년 9월 7일(1142.99)

$$Underwater Period = 2005년 9월 7일 - 1994년 11월 8일 = 3955일$$

즉 1994년 11월 8일에 KOSPI 인덱스 펀드를 샀으면 3955일동안 수익률이 마이너스였다는 것을 의미한다.  

## 의미
Underwater Period도 포트폴리오의 유지에 중요한 역할을 한다.  
11년동안 마이너스 수익률을 감내하는 것은 굉장히 어려운 일이다. 대부분이 그 전에 포트폴리오를 포기하고 말 것이다.  
Underwater Period와 MDD는 연관이 깊은데, 보통 MDD가 크면 전고점을 회복하는데 오래 걸리므로 Underwater Period도 길어지게 된다.  
따라서 투자자는 Underwater Period가 짧은 포트폴리오를 선호하게 된다.  

## 연관용어
- [Max Drawdown]({% post_url 2020-08-30-mdd %})
- [Compound Annual Growth Rate]({% post_url 2020-08-17-cagr %})
