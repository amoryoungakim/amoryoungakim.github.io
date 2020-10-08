---
title: "빅쿼리(BigQuery)로 e-commerce data 분석하기 Part1"
date: 2020-10-09 00:00:00 -0400
permalink: '/bigquery_part1/'
categories: Bigquery
---

연말 캠페인을 준비할 때, 작년 동기간의 판매 데이터와 고객 정보를 분석해 활용한다면 한층 더 높은 성과를 기대할 수 있을 것이다. 구글 애널리틱스(Google Analytics) 데이터를 빅쿼리(BigQuery)를 이용해서 추출하는 팁을 소개한다.

### 특정 기간 가장 많이 팔린 제품 Top 5

```sql
SELECT
    prod.v2ProductName AS product_name,
    COUNT(*) AS qty
FROM 
    `bigquery-public-data.google_analytics_sample.ga_sessions_*`  AS t1
    LEFT JOIN UNNEST(hits) AS hits
    LEFT JOIN UNNEST(hits.product) AS prod
WHERE _TABLE_SUFFIX BETWEEN '20161216' AND '20161231'
    AND hits.eCommerceAction.action_type = '6'
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

#### 결과

|  row  |       product_name                                      | qty|
|-------|---------------------------------------------------------|----|
| 1     | Google Sunglasses                                       | 86 |
| 2     | Google Men's 100% Cotton Short Sleeve Hero Tee Black    | 86 |
| 3     | Red Shine 15 oz Mug                                     | 78 |
| 4     | Google Men's 100% Cotton Short Sleeve Hero Tee Navy     | 76 |
| 5     | Engraved Ceramic Google Mug                             | 72 |


작년 12/16 ~ 12/31 사이에 가장 많이 팔렸던 제품은 바로 Google Sunglass였다.(한 겨울에?) '판매'가 이루어진 action은 `eCommerceAction`에 `action_type`으로 정리되어 있다. `action_type = 6`이면 결재가 끝난 것이고, `action_type = 3`이면 cart에 담은 것이다. 더 상세한 기준은 여기https://storage.googleapis.com/e-nor/visualizations/bigquery/ga360-schema.html#section-collapsible-tree를 참고하면 된다.

`hits` 정보는 array 형식으로 한 셀에 묶여 있어서, array 안에 있는 정보를 꺼내오기 위해서는 반드시 `UNNEST`를 해주어야 한다.

```sql
FROM 
    `bigquery-public-data.google_analytics_sample.ga_sessions_*`,
    UNNEST(hits) AS hits,
    UNNEST(hits.product) AS prod
```

위와 같이 TABLE NAME과 UNNEST 사이에 `,`를 쓰면 CROSS JOIN을 한다는 뜻이다. 만약 UNNEST 할 셀 값이 NULL이면 CROSS JOIN을 했을 때 해당 Session은 누락되어 버린다. 따라서, 그럴 의도가 아니라면 첫 예시와 같이 LEFT JOIN을 해주는 것이 좋다. `hits` 셀은 값이 NULL인 경우가 없겠지만 (값이 NULL이라면 어차피 필요 없는 Session이다) `hits.product`은 값이 NULL인 경우가 종종 있으므로 유의해야 한다.

### Sunglass를 구매한 ID, 구매한 시간

```sql
SELECT
    t1.fullVisitorId,
    MIN(CASE
        WHEN prod.v2ProductName = 'Google Sunglasses' AND hits.eCommerceAction.action_type = '6'
        THEN visitStartTime
        ELSE UNIX_SECONDS(TIMESTAMP("2017-01-01 00:00:00", "America/Los_Angeles"))
    END) AS convStartTime,
    MAX(CASE
        WHEN prod.v2ProductName = 'Google Sunglasses' AND hits.eCommerceAction.action_type = '3'
        THEN 'Y'
        ELSE 'N'
    END) AS add_to_cart,
FROM 
    `bigquery-public-data.google_analytics_sample.ga_sessions_*`  AS t1
    LEFT JOIN UNNEST(hits) AS hits
    LEFT JOIN UNNEST(hits.product) AS prod
WHERE _TABLE_SUFFIX BETWEEN '20161216' AND '20161231'
GROUP BY 1
```

결과
|  row  |    fullVisitorId    | convStartTime | add_to_cart|
|-------|---------------------|----|----|
| 1     | 9998768158040586927 | 1483257600 | N |
| 2     | 979215407457047348 | 1483257600 | N |
| 3     | 6790396104201220080 | 1483257600 | N |
| 4     | 7681066101584919753  | 1483257600 | N |
| 5     | 8921276772339987363 | 1483257600 | N |

한겨울에 Sunglass를 구매한 사람들은 어떤 사람들일까? 고객 정보를 분석하기에 앞서 구매한 ID와 그렇지 않은 ID를 추려보자.

`convStartTime`이라는 열을 만들어 구매한 경우 그 Session의 visitStartTime을 넣어주고, 구매 하지 않은 경우 데이터 추출 기간의 끝 시점인 '17년 1월 1일 0시를 UNIX_SECONDS로 변환해 넣어주었다. 구매 여부를 Y/N으로 출력하면 더 간단하지만 나중에 visitStartTime을 기준으로 삼아 다른 데이터를 추출하기 위해 일부러 남겨두었다. `add_to_cart`는 간단하게 Y/N으로 출력했다.

빅쿼리(BigQuery)에서 날짜를 다루는 스킬이 꽤 어렵고 재미가 없지만(...) 한 번은 꼭 정리해 둘 필요가 있다. 자세한 내용은 BigQuery 시간 함수 다루기 post를 참고하자.
