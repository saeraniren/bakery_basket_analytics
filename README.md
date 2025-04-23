![image.png](attachment:4528619f-d1fd-4df0-9500-0f311719b5b8:image.png)

# 미션 목표

---

> 베이커리의 사장이라고 가정을 해보고 고객의 구매 패턴을 분석하여 상품간의 상호 작용이나 구매 행동의 흥미로운 패턴을 파악해보자. 데이터 EDA와 장바구니 분석을 적용하여 분석 결과를 도출하여, 베이커리의 매출을 증대시키고, 고객 만족도를 높이는 방법을 찾는 것이 목표!

# 데이터 설명

---

Kaggle에 존재하는 데이터를 토대로 분석을 진행하였기 때문에 원문과 변역본을 함께 올려두었다.

- 원문
    
    ## About Dataset
    
    ### Context
    
    We live in the era of e-commerce and digital marketing. We have even small scale businesses going online as the opportunities are endless. Since a huge chunk of the people who have access to internet is switching to online shopping, large retailers are actively searching for ways to increase their profit. Market Basket analysis is one such key techniques used by large retailers to to increase sales by understanding the customers' purchasing behavior & patterns. Market basket analysis examines collections of items to find relationships between items that go together within the business context.
    
    ### Content
    
    The dataset belongs to "The Bread Basket" a bakery located in Edinburgh. The dataset provide the transaction details of customers who ordered different items from this bakery online during the time period from 30-10-2016 to 09-04-2017. The dataset has 20507 entries, over 9000 transactions, and 4 columns.
    
    ### Variables
    
    - `TransactionNo` : unique identifier for every single transaction
    - `Items` : items purchased
    - `DateTime` : date and time stamp of the transactions
    - `Daypart` : part of the day when a transaction is made (morning, afternoon, evening, night)
    - `DayType` : classifies whether a transaction has been made in weekend or weekdays
    
    ### Inspiration
    
    The dataset is ideal for anyone looking to practice association rule mining and understand the business context of data mining for better understanding of the buying pattern of customers.
    
- 번역본
    
    ### 배경
    
    우리는 전자상거래와 디지털 마케팅의 시대에 살고 있습니다. 온라인의 기회가 무궁무진하기 때문에 소규모 사업체조차 온라인으로 진출하고 있습니다. 인터넷 접근성이 높은 사람들의 상당 부분이 온라인 쇼핑으로 전환하면서 대형 소매업체들은 수익 증대를 위한 방법을 적극적으로 모색하고 있습니다. 장바구니 분석은 고객의 구매 행동 및 패턴을 이해하여 매출을 늘리는 데 사용되는 핵심 기술 중 하나입니다. 장바구니 분석은 품목의 모음을 조사하여 비즈니스 맥락 내에서 함께 구매되는 품목 간의 관계를 파악합니다.
    
    ### 내용
    
    이 데이터셋은 에든버러에 위치한 "The Bread Basket"이라는 빵집의 것입니다. 이 데이터셋은 2016년 10월 30일부터 2017년 4월 9일까지 온라인으로 이 빵집에서 다양한 품목을 주문한 고객들의 거래 내역을 제공합니다. 이 데이터셋은 20507개의 항목, 9000건 이상의 거래, 그리고 4개의 열로 구성되어 있습니다.
    
    ### 변수
    
    - `TransactionNo`: 각 거래에 대한 고유 식별자
    - `Items`: 구매한 품목
    - `DateTime`: 거래의 날짜 및 시간 스탬프
    - `Daypart`: 거래가 이루어진 시간대 (아침, 오후, 저녁, 밤)
    - `DayType`: 거래가 주말에 이루어졌는지 평일에 이루어졌는지 분류
    
    ### 영감
    
    이 데이터셋은 연관 규칙 마이닝을 연습하고 고객의 구매 패턴에 대한 더 나은 이해를 위해 데이터 마이닝의 비즈니스 맥락을 이해하고자 하는 모든 사람에게 이상적입니다.
    

현재 코드잇에서 제공해준 데이터와는 상이한 차이점이 있는데, 이는 이 데이터의 제공자가 각 컬럼들의 내용을 업데이트 하면서 달라진 것으로 추측된다. 하지만 데이터 컬럼들의 특징이 달라진 것이 없어, 다음과 같이 정리할 수 있다.

| 현재 제공받은 데이터 컬럼 | Kaggle에서 업데이트된 데이터 컬럼 | 컬럼 설명 |
| --- | --- | --- |
| Transaction | TransactionNo | 거래내역 |
| Item | Items | 구입한 아이템 |
| datei_time | DateTime | 구입한 날짜 |
| period_day | Daypart | 구입한 시간대 |
| weekday_weekend | DayType | 구입한 날이 평일인지 주말인지의 여부 |

# 데이터의 내용 확인

---

데이터를 전처리하기 전 데이터의 각 컬럼들의 dtype, 결측치, 기술통계량을 한꺼번에 출력하는 함수를 만들어 다음과 같이 출력하여 확인하였다.

```
data shape : (20507, 5)
```

|  | **dtypes** | **missing** | **missing_rate (%)** | **nunique** | **mean** | **std** | **min** | **25%** | **50%** | **75%** | **max** | **mode** | **mode_counts** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Transaction | int64 | 0 | 0.0 | 9465 | 4976.20237 | 2796.203001 | 1.0 | 2552.0 | 5137.0 | 7357.0 | 9684.0 | 6279 | 11 |
| Item | object | 0 | 0.0 | 94 | NaN | NaN | NaN | NaN | NaN | NaN | NaN | Coffee | 5471 |
| date_time | object | 0 | 0.0 | 9182 | NaN | NaN | NaN | NaN | NaN | NaN | NaN | 2/5/2017 11:58 | 12 |
| period_day | object | 0 | 0.0 | 4 | NaN | NaN | NaN | NaN | NaN | NaN | NaN | afternoon | 11569 |
| weekday_weekend | object | 0 | 0.0 | 2 | NaN | NaN | NaN | NaN | NaN | NaN | NaN | weekday | 12807 |

다음과 같은 내용을 확인할 수 있었다.

> 💡
> - 결측치는 나타나지 않았다.
> - 가장 많이 팔린 아이템은 Coffe이며 총 5471건이 팔렸다.
> - 가장 많이 팔린 날은 2017년 5월 2일 11시 58분에 많이 팔렸다.
> - 가장 많이 팔린 시간대는 오후 시간대인 afternoon 시간대에 많이 팔렸다.
> - 평일날에 많은 판매건을 보유하고 있다.

# 데이터 중복값 처리

---

현재 이 데이터가 kaggle에서 장바구니 분석을 통한 데이터 분석을 많이 하기에 장바구니 분석을 실시하고자 하였다.
그리고, 이 데이터를 활용한 여러 개의 kaggle notebook를 보면서 다음과 같은 궁금증을 갖게 되었다.

## ***"Q.장바구니 분석을 할 때 중복값을 처리하는 이유가 어떻게 되는가?"***

> ### 1. 아이템 간 관계의 왜곡 방지
> - 예 : 고객 A가 같은 상품을 한 장바구니에 5개 담았다고 해서, 그 상품이 다른 상품과 "더 많이 연관되어 있다" 고 판단하는 건 과장된 해석이 될 수 있다.
> - 대부분의 연관 규칙 분석은 아이템의 **존재 여부**만 따지기 때문에, 중복은 의미 없는 노이즈가 될 수 있다.
> ### 2. 정확한 규칙 도출
> - Apriori나 FP-Growth같은 알고리즘은 support(지지도), confidence(신뢰도), lift(향상도) 등을 기준으로 규칙을 추출.
> - 중복값이 포함되면 지지도 등이 왜곡되어 잘못된 규칙이 나올 수 있음.
> ### 3. 데이터의 일관성 유지
> - 대부분의 장바구니 분석은 한 거래(트랜잭션)에서 어떤 제품이 포함되었는지에만 관심이 있다.
> - 따라스 같은 제품이 여러 번 등장하면 중복 제거를 통해 트랜잭션을 "집합(set)"으로 변환하는 것이 일반적.
> ### 4. 분석 성능 향상
> - 중복값이 많으면 데이터의 크기가 커지고, 연산량도 늘어나서 알고리즘 성능이 저하될 수 있다.
> - 불필요한 데이터 제거는 처리 시간을 절약하고 결과 해석에 용이하다.

따라서, 중복값을 제거하고 통계량을 다시 살펴본 결과 다음과 같은 결과를 알 수 있었다.

```
data shape : (18887, 5)
```

|  | **dtypes** | **missing** | **missing_rate (%)** | **nunique** | **mean** | **std** | **min** | **25%** | **50%** | **75%** | **max** | **mode** | **mode_counts** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Transaction | int64 | 0 | 0.0 | 9465 | 4951.051517 | 2811.619306 | 1.0 | 2496.5 | 5082.0 | 7378.5 | 9684.0 | 9447 | 10 |
| Item | object | 0 | 0.0 | 94 | NaN | NaN | NaN | NaN | NaN | NaN | NaN | Coffee | 4528 |
| date_time | object | 0 | 0.0 | 9182 | NaN | NaN | NaN | NaN | NaN | NaN | NaN | 4/5/2017 17:22 | 10 |
| period_day | object | 0 | 0.0 | 4 | NaN | NaN | NaN | NaN | NaN | NaN | NaN | afternoon | 10687 |
| weekday_weekend | object | 0 | 0.0 | 2 | NaN | NaN | NaN | NaN | NaN | NaN | NaN | weekday | 11830 |

<aside>
💡

- 중복값을 삭제하였더니 기존에 있던 20507건에서 18887건이 삭제되었다.
- 수량 체크를 통해서 알아내는 것은 원본 데이터에서 알아내는 것이므로 중복값 처리된 데이터에서는 하지 않았다.
- 중복값을 처리하고 난 뒤의 데이터를 살펴본 결과 다음과 같은 내용을 확인할 수 있었다.
    - 중복되지 않은 아이템을 많이 산 거래내역은 9447번이고, 거래시간은 2017년 4월 5일 17시 22분이다.
    - 하지만 이 시간이 afternoon인지 weekday인지는 알 수 없다.
</aside>

# 데이터 EDA

---

판매 수량에 대한 EDA 분석이기 때문에, 중복값 처리를 하지 않은 데이터를 토대로 작업을 진행하였다.

## 가장 많이 팔린 아이템 상위 20개

![download.png](attachment:e8243e1c-2458-40db-a1eb-f32637c22354:download.png)

| **Item** | **count** |
| --- | --- |
| Coffee | 5471 |
| Bread | 3325 |
| Tea | 1435 |
| Cake | 1025 |
| Pastry | 856 |
| Sandwich | 771 |
| Medialuna | 616 |
| Hot chocolate | 590 |
| Cookies | 540 |
| Brownie | 379 |
| Farm House | 374 |
| Muffin | 370 |
| Juice | 369 |
| Alfajores | 369 |
| Soup | 342 |
| Scone | 327 |
| Toast | 318 |
| Scandinavian | 277 |
| Truffles | 193 |
| Coke | 185 |

> 💡
> - Coffee와 Bread 판매량이 전체 20507 판매량 건 중 5471, 3325건으로 전체 판매량의 42.89% 를 차지하고 있다.

## 월별 가장 많이 팔린 아이템 상위 5개 시각화

![download.png](attachment:92e61dcf-b815-4614-a8ee-ce1a7ea1357e:download.png)

> 💡
> - 해당 데이터가 2016년부터 2017년 사이의 데이터를 가지고 있고, 이를 통합한 월별 가장 많이 팔린 데이터를 확인하고자 하였다.
> - 하지만 5, 6, 7, 8, 9월의 데이터가 존재하지 않았고, 판매량이 적은 데이터는 2016, 2017년 중 적어도 한 년도의 데이터로 추측이 된다.
> - 따라서 월별에 따른 물건의 트렌드를 추측하기에는 파악하기에는 분석하기 힘든 점이 있었다.

## 시간대별 판매량 수

|  | **count** |
| --- | --- |
| afternoon | 11569 |
| morning | 8404 |
| evening | 520 |
| night | 14 |

![download.png](attachment:f21e12db-48ce-4a74-83ce-d68f1f4e1291:download.png)

> 💡
> - 기술 통계량을 통해서 최빈값이 afternoon이고 그 값이 11569인 값을 확인하였다.
> - morning의 판매량 수가 8404이고, evening, night의 데이터는 전체 데이터에 비해 너무 적은 판매량을 보유하고 있다.

## 요일별 판매량 수

|  | **count** |
| --- | --- |
| Mon | 2324 |
| Tue | 2392 |
| Wen | 2321 |
| Thu | 2646 |
| Fri | 3124 |
| Sat | 4605 |
| Sun | 3095 |

![download.png](attachment:5b337e66-6abb-4cd6-b0ef-152c7aa93354:download.png)


> 💡
> - 기술 통계량에서 평일일 때 많은 판매량을 보였다.
> - 하지만 요일별로 살펴본 결과 토요일에 가장 많은 판매량을 보유하고 있다.
> - 또한 금요일을 제외한 나머지 평일에 해당하는 요일은 일요일보다 판매량이 적게 나타나는 것을 알 수 있다.

## 시간대별 연도별 판매량

![download.png](attachment:4809ec7c-d57e-48f1-ab3a-b803909085b1:download.png)

> 💡
> - 2016년 2017년 두 년도 공통점이 있다면 11~13시 사이의 판매량이 압도적으로 높게 나타나는 것으로 알 수 있다.
> - 2017년도가 2016년도보다 전제척인 판매량에서 많은 양을 웃도는 것을 알 수 있다.

## 시간대별 요일별 판매량

![download.png](attachment:c8aaafdd-44e6-4fea-8fc8-d27ac16059a4:download.png)

> 💡
> - 평일보다 주말간의 판매 데이터가 높은 것이 시각적으로도 매우 잘 보임.
> - 모든 요일별 브런치 시간대의 시작인 11시에 대부분 많은 판매량을 보이고 있음.

## Item과 날짜 데이터 간의 연관분석

---

![download.png](attachment:cabbb1dd-978f-49a4-8c1e-6ea609a07047:download.png)

| **Categorical** | **Numerical** | **Eta Squared** | **p-value** |
| --- | --- | --- | --- |
| Item | day_of_week | 0.642295 | 0.000115 |
| Item | year | 0.002357 | 0.000115 |
| Item | month | 0.002338 | 0.000115 |
| Item | Transaction | 0.001666 | 0.000115 |
| Item | minute | 0.000208 | 0.000115 |
| Item | day | 0.000113 | 0.000115 |
| Item | hour | 0.000012 | 0.000115 |

> 💡
> - 모든 p-value가 매우 작으므로, 해당 수치형 변수들의 평균값이 통계적으로 유의미하게 다르다는 것을 알 수 있다.
> - 특히 요일별 판매 패턴이 크게 다르게 나타나는 것을 알 수 있다. (Eta-Squared 값이 매우 큼.)
> - 다른 변수들 관련해서도 유의미한 차이는 있으나, 실제 영향력은 크게 나타나지 않음.

## EDA 종합 분석 결론

---

> 💡
> - 연도에 따른 팔린 제품의 시계열적인 트렌드를 찾을 수 없었다.
> - 하지만 요일별로 팔린 데이터를 통해 기술 통계량에서 확인한 평일과 주말간의 판매량과는 생각보다 큰 차이점이 있다는 것을 확인하였다.
> - 이후 장바구니 분석과 결합하여 요일별에 따른 판매에 대한 조언을 찾을 수 있을 것이라 판단된다.

# 장바구니 분석 파트

---

## 장바구니 분석이란?

---

장바구니 분석이란 고객의 구매 데이터에서 특정 상품들이 함께 구매되는 패턴을 찾아내는 연관성 분석 기법이다. 이를 통해 어떤 상품이 함꼐 구매될 가능성이 높은지 파악하여 매장 진열(제품 배치 최적화), 마케팅 전략, 고객 관계 관리(CRM) 등에 활용할 수 있다.

예시를 들면 다음과 같은 분야에 적용된다.

> 💡
> - **OTT 서비스** : 사용자의 시청 패턴을 분석하여 맞춤 콘텐츠를 추천
> - **금융 기관** : 거래 패턴을 통해 사기 행위 탐지 및 맞춤 금융 상품 기획
> - **의료 분야** : 환자의 증상 및 질병 패턴을 분석하여 예방 조치 마련
> - **요식업계** : 인기 메뉴 조합을 찾아 세트 메뉴 개발

이에 맞게 각 거래 내역에서 중복 거래된 것들을 모두 중복 처리하고, 구입한 물건들의 내역들을 groupby하여 데이터프레임으로 출력한 결과 다음과 같이 출력이 되었다.

| Transaction | Item |
| --- | --- |
| 1 | [Bread] |
| 2 | [Scandinavian] |
| 3 | [Hot chocolate, Jam, Cookies] |
| 4 | [Muffin] |
| 5 | [Coffee, Pastry, Bread] |
| ... | ... |
| 9680 | [Bread] |
| 9681 | [Truffles, Tea, Spanish Brunch, Christmas common] |
| 9682 | [Muffin, Tacos-Fajita, Coffee, Tea] |
| 9683 | [Coffee, Pastry] |
| 9684 | [Smoothies] |

이 데이터프레임을 pivot_table로 변환하여 다음과 같이 출력되도록 하였다.

| **Adjustment** | **Afternoon with the baker** | **Alfajores** | **Argentina Night** | **Art Tray** | **Bacon** | … |
| --- | --- | --- | --- | --- | --- | --- |
| **0** | False | False | False | False | False | … |
| **1** | False | False | False | False | False | … |
| **2** | False | False | False | False | False | … |
| **3** | False | False | False | False | False | … |
| **4** | False | False | False | False | False | … |
| **...** | ... | ... | ... | ... | ... | … |
| **9460** | False | False | False | False | False | … |
| **9461** | False | False | False | False | False | … |
| **9462** | False | False | False | False | False | … |
| **9463** | False | False | False | False | False | … |
| **9464** | False | False | False | False | False | … |

이렇게 pivot_table로 변환된 데이터프레임을 연관규칙 마이닝을 적용시키기로 하였다.

## 연관 규칙 마이닝이란?

---

연관 규칙 마이닝(Association Rule Mining)은 데이터에서 아이템 간의 상호 관련성을 분석하여 유의미한 정보를 추출하는 방법이다. 이 방법은 주로 장바구니 분석에서 사용되며, 규칙은 보통 “If (조건) Then (결과)” 의 형식으로 표현된다.

연관 규칙 마이닝에서 좋은 규칙을 찾기 위해서는 규칙의 가치를 평가하는 정량적인 지표가 필요한데, 다음과 같은 지표를 사용할 수 있다.

### 지지도(Support)

지지도는 특정 규칙이 얼마나 자주 발생하는지를 나타내는 지표로, 규칙의 일반성을 평가한다.

지지도가 높을수록 규칙이 데이터 내에서 빈벅하게 나타나며, 범용성이 높다고 해석할 수 있다.

지지도의 수식은 다음과 같다.

$$
Support(X \Rightarrow Y) = \frac{N(X \cap Y)}{N}
$$

### 신뢰도(Confidence)

신뢰도는 규칙의 신뢰성을 평가하는 지표로, X를 구매한 사람 중 Y도 함께 구매한 비율을 나타낸다.

신뢰도가 높을수록 규칙이 더 믿을만하며, 추천의 방향성도 결정할 수 있게 된다.

신뢰도의 수식은 다음과 같다.

$$
Confidence(X \Rightarrow Y) = \frac{N(X \cap Y)}{N(X)}
$$

### 향상도(Lift)

두 항목의 관계가 정말로 유의미한지, 또는 단순히 우연에 의한 것인지 평가하는데 사용하는 지표이다.

향상도는 X를 구매하는 것이 Y를 구매할 확률에 얼마나 영향을 미치는지 측정한다.

향상도의 수식은 다음과 같다.

$$
Lift(X \Rightarrow Y) = \frac{N(X \cap Y) \times N}{N(X) \times N(Y)}
$$

연관 규칙 마이닝에서 가장 중요하게 해석되는 지표는 **향상도**이며, 왜 향상도가 중요하게 활용되는 이유는 다음과 같다.

> 💡
> 1. **실질적인 연관성 파악**: 지지도나 신뢰도만으로는 우연히 함께 자주 나타나는 품목들을 실제 연관성이 높은 품목으로 오해할 수 있다. 향상도는 이러한 우연적인 동시 발생을 보정하여 **의미 있는 연관성**을 파악하는 데 도움을 준다.
> 2. **불필요한 규칙 제거**: 향상도가 1에 가까운 규칙은 품목 간에 연관성이 없다는 것을 의미한다. 따라서 향상도 값이 낮은 규칙들은 마케팅 전략 수립에 실질적인 도움이 되지 않으므로 분석 대상에서 제외할 수 있다.
> 3. **교차판매 및 추천 전략** : 향상도가 높은 품목들을 함께 진열하거나, 특정 품목을 구매한 고객에게 다른 품목을 추천하는 등의 전략을 수립하는 데 유용한 정보를 제공한다. 예를 들어, ‘빵’을 구매한 고객에게 ‘잼’의 향상도가 높다면 함께 추천하는 것이 효과적일 수 있다.
> 4. **데이터의 희소성 문제 완화** : 특정 품목들의 동시 구매 빈도가 낮더라도, 각 품목의 개별 구매 빈도를 고려하여 의미 있는 연관성을 포착할 수 있다.

물론 향상도만으로 모든 것을 판단할 수는 없다. 그러므로 지지도와 신뢰도와 함께 고려하여 비즈니스 상황에 맞는 최적의 규칙을 찾아내는 것이 중요하다.

## FP-Growth를 통한 빈발항목 출력

---

`min_support` 수치는 데이터 내에서 자동으로 추천을 받기 위해 다음과 같은 함수를 만들었다.

```python
def recommend_min_support(frequent_itemsets, total_transactions, min_count_threshold=50, quantile_threshold=0.75):
    """
    frequent_itemsets: fpgrowth 결과 (support 포함된 DataFrame)
    total_transactions: 전체 거래 수
    min_count_threshold: 최소 등장 횟수 기준 (예: 50건 이상)
    quantile_threshold: 등장 횟수의 상위 퍼센트 기준 (예: 상위 25% 이상만 분석하고 싶을 때)

    반환값:
        - 추천 min_support (지지도 값)
        - 기준에 맞는 itemsets 수
    """
    # 등장 건수 계산
    frequent_itemsets = frequent_itemsets.copy()
    frequent_itemsets['support_count'] = frequent_itemsets['support'] * total_transactions

    # 등장 횟수 기준 필터링
    above_min_count = frequent_itemsets[frequent_itemsets['support_count'] >= min_count_threshold]
    min_support_from_count = min_count_threshold / total_transactions

    # 상위 퍼센트 기준 필터링
    threshold_count = frequent_itemsets['support_count'].quantile(quantile_threshold)
    min_support_from_quantile = threshold_count / total_transactions

    print(f"기준1: 최소 {min_count_threshold}건 이상 등장하려면 min_support ≥ {min_support_from_count:.4f}")
    print(f"기준2: 상위 {int((1 - quantile_threshold) * 100)}% 아이템셋 기준 min_support ≥ {min_support_from_quantile:.4f}")

    return {
        'min_support_by_count': round(min_support_from_count, 4),
        'min_support_by_quantile': round(min_support_from_quantile, 4),
        'frequent_itemsets_over_count': len(above_min_count),
        'total_itemsets': len(frequent_itemsets)
    }
```

이후 `min_support` 값을 0.001인 작은 값부터 시작하여 자동으로 추천받는 값을 반환하려고 한 결과, 다음과 같이 출력되었다.

```
기준1: 최소 50건 이상 등장하려면 min_support ≥ 0.0052
기준2: 상위 25% 아이템셋 기준 min_support ≥ 0.0049
{'min_support_by_count': 0.0052,
 'min_support_by_quantile': 0.0049,
 'frequent_itemsets_over_count': 112,
 'total_itemsets': 471}
```

전체 거래건수인 9683건 중, 상위 25% 아이템셋을 뽑아내어 규칙을 찾아보기 위해 `min_support` 값을 0.0049로 지정하여 나온 빈발항목을 상위 20개만 따로 뽑아내어 시각화 하였다.

![download.png](attachment:45b3d9eb-c4e4-42e3-8a11-b10473922b44:download.png)

| **support** | **itemsets** |
| --- | --- |
| 0.478394 | (Coffee) |
| 0.327205 | (Bread) |
| 0.142631 | (Tea) |
| 0.103856 | (Cake) |
| 0.090016 | (Coffee, Bread) |
| 0.086107 | (Pastry) |
| 0.071844 | (Sandwich) |
| 0.061807 | (Medialuna) |
| 0.058320 | (Hot chocolate) |
| 0.054728 | (Coffee, Cake) |
| 0.054411 | (Cookies) |
| 0.049868 | (Coffee, Tea) |
| 0.047544 | (Pastry, Coffee) |
| 0.040042 | (Brownie) |
| 0.039197 | (Farm House) |
| 0.038563 | (Juice) |
| 0.038457 | (Muffin) |
| 0.038246 | (Sandwich, Coffee) |
| 0.036344 | (Alfajores) |
| 0.035182 | (Coffee, Medialuna) |

## 연관규칙 마이닝을 통한 규칙 도출

---

전체 연관규칙 마이닝을 lift(향상도) 기준으로 최소 임계값을 1로 지정한 뒤, 각 지표별로 내림차순으로 정렬하여 살펴본 결과 다음과 같이 출력되었다.

각 표는 주요 지표들만 가져와 편집한 내용이다.

- 지지도(support) 기준 Top 10개 규칙
    
    
    | **ntecedents** | **consequents** | **support** | **confidence** | **lift** |
    | --- | --- | --- | --- | --- |
    | (Cake) | (Coffee) | 0.054728 | 0.526958 | 1.101515 |
    | (Coffee) | (Cake) | 0.054728 | 0.114399 | 1.101515 |
    | (Coffee) | (Pastry) | 0.047544 | 0.099382 | 1.154168 |
    | (Pastry) | (Coffee) | 0.047544 | 0.552147 | 1.154168 |
    | (Sandwich) | (Coffee) | 0.038246 | 0.532353 | 1.112792 |
    | (Coffee) | (Sandwich) | 0.038246 | 0.079947 | 1.112792 |
    | (Coffee) | (Medialuna) | 0.035182 | 0.073542 | 1.189878 |
    | (Medialuna) |  | 0.035182 | 0.569231 | 1.189878 |
    | (Coffee) | (Hot chocolate) | 0.029583 | 0.061837 | 1.060311 |
    | (Hot chocolate) | (Coffee) | 0.029583 | 0.507246 | 1.060311 |

> 💡
> 가장 자주 함께 팔리는 항목이지만, 새로운 인사이트보다 패턴 빈도가 강조된 규칙을 보여줌.

- 신뢰도(confidence) 기준 Top 10개 규칙
    
    
    | **antecedents** | **consequents** | **support** | **confidence** | **lift** |
    | --- | --- | --- | --- | --- |
    | (Salad, Extra Salami or Feta) | (Coffee) | 0.001479 | 0.875000 | 1.829036 |
    | (Pastry, Toast) | (Coffee) | 0.001373 | 0.866667 | 1.811617 |
    | (Sandwich, Hearty & Seasonal) | (Coffee) | 0.001268 | 0.857143 | 1.791709 |
    | (Vegan mincepie, Cake) | (Coffee) | 0.001057 | 0.833333 | 1.741939 |
    | (Salad, Sandwich) | (Coffee) | 0.001585 | 0.833333 | 1.741939 |
    | (Extra Salami or Feta) | (Coffee) | 0.003275 | 0.815789 | 1.705267 |
    | (Keeping It Local) | (Coffee) | 0.005388 | 0.809524 | 1.692169 |
    | (Cookies, Scone) | (Coffee) | 0.001585 | 0.789474 | 1.650258 |
    | (Pastry, Juice) | (Coffee) | 0.001796 | 0.772727 | 1.615253 |
    | (Salad, Cake) | (Coffee) | 0.001057 | 0.769231 | 1.607944 |


> 💡
> 이 조합이 있으면 Coffee는 거의 필수로 따라온다는 강한 연관성을 띄워줌.

- 향상도(lift) 기준 Top 10개 규칙
    
    
    | **antecedents** | **consequents** | **support** | **confidence** | **lift** |
    | --- | --- | --- | --- | --- |
    | (Extra Salami or Feta) | (Salad, Coffee) | 0.001479 | 0.368421 | 56.243633 |
    | (Salad, Coffee) | (Extra Salami or Feta) | 0.001479 | 0.225806 | 56.243633 |
    | (Salad) | (Coffee, Extra Salami or Feta) | 0.001479 | 0.141414 | 43.176931 |
    | (Coffee, Extra Salami or Feta) | (Salad) | 0.001479 | 0.451613 | 43.176931 |
    | (Salad) | (Extra Salami or Feta) | 0.001690 | 0.161616 | 40.255183 |
    | (Extra Salami or Feta) | (Salad) | 0.001690 | 0.421053 | 40.255183 |
    | (Cookies, Alfajores) | (Juice) | 0.001057 | 0.434783 | 11.274568 |
    | (Juice) | (Cookies, Alfajores) | 0.001057 | 0.027397 | 11.274568 |
    | (Jam) | (Fudge) | 0.002536 | 0.169014 | 11.265622 |
    | (Fudge) | (Jam) | 0.002536 | 0.169014 | 11.265622 |

> 💡
> 매우 희귀한 조합이지만 등장하면 강한 연관성이 존재.

# 분석결론

---

EDA와 장바구니 분석을 통해 다음과 같은 분석 결론을 이야기할 수 있었다.

## 시간대별 추천 세트 메뉴 신설

- 시간대 별로 고객군/니즈가 다르므로 다음과 같은 세트 메뉴를 지정할 수 있음.
    - 아침 : 간단하고 빠른 식사를 위한 세트 메뉴, 비교적 간단한 식사에 맞는 음료를 제공하면 효과적.
    - 브런치 : 든든한 식사를 위한 세트 메뉴, 식사 메뉴와 맞는 음료를 제공하면 매우 효과적.
    - 오후 : 디저트 타임, 간단하게 먹을 수 있으면서 디저트와 맞는 음료를 제공하면 매우 효과적.

## 주말 간 특별 세트 판매 유도

- 주말에 많은 판매량을 보이는 경우가 잦음.
- 주말에 방문하는 사람들을 위한 특별한 추천 세트 신설을 통해 매출 증대 효과를 볼 수 있음.
- 특히 Brunch 시간대에 많이 사는 디저트 메뉴와 커피를 같이 세트 메뉴로 신설한다면 좋은 기대 효과를 볼 수 있을 것으로 보임.
- 많은 주문이 몰릴 것으로 예상될 수 있는 시간대이므로, 간편하게 소비할 수 있는 세트 구성이 유리.

## 커피 연관 구매 품목 기반 묶음 전략

- 연관 분석에 따르면, 커피는 다양한 메뉴와 함꼐 자주 구매됨.
- 특히 Pastry, Cake, Sandwich, Medialuna와의 높은 지지도와 향상도가 확인됨.
- 이에 따라, 커피를 중심으로 한 “기본 세트” 시리즈를 도입하여 선택 폭을 넓히는 방식 유도.

## 소수 마니아 고객들을 위한 특별 마케팅

- Cookies, Alfajores → Juice와 같은 빈도가 잦지 않지만 향상도가 가장 높은 조합을 통해, 소수 마니아층을 위한 마케팅을 시도해볼 수 있음.
- 희귀한 조합을 묶은 “시그니처 세트” 메뉴 신설을 통해 **충성 고객 유치 전략**에 적합.
