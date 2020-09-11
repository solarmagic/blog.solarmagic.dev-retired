---
title:  "한양대 소프트웨어인재(특기자) 기출 분석 및 모의 기출"
date:   2020-09-11 00:00:00
categories:
- Algorithm
tags:
- DP
- Sorting
- Selection
- Greedy
- Dijkstra
---

![hanyang](https://i.imgur.com/iOGR0Td.png)

한양대학교는 2018학년도부터 소프트웨어인재 전형을 통해 13명씩 컴퓨터소프트웨어학부로 모집하고 있다.

[학교 홈페이지](http://site.hanyang.ac.kr/web/go/10)를 통해 기출 문제, 출제 의도, 예시 답안등을 다 확인할 수 있지만 간략하게 분석, 요약을 해 보았다.

기본적으로 30분동안 문제를 이해하고 5분동안 발표, 8분동안 Q&A를 한 구조이다.

21학번을 노리는 사람들이 혹시나 본다면 조금이나마 도움될 수 있으면 좋겠다.

~~필자는 17학번이라 이런걸 못해봣다. ㅠ~~

---

*경고* 아래에는 문제와 풀이가 같이 있으므로 제대로 풀어보려면 내리지말고 아래 링크로 문제를 읽고 풀어보세요.

## 2018학년도

- [18학년도 문제 보기](https://site.hanyang.ac.kr/web/go/10?p_p_id=board_WAR_bbsportlet&p_p_lifecycle=2&p_p_state=normal&p_p_mode=view&p_p_cacheability=cacheLevelPage&p_p_col_id=column-1&p_p_col_count=1&_board_WAR_bbsportlet_fileId=517762&_board_WAR_bbsportlet_cmd=download&_board_WAR_bbsportlet_messageId=518665&_board_WAR_bbsportlet_messageId=518665&_board_WAR_bbsportlet_sCategoryId=0&_board_WAR_bbsportlet_sDisplayType=1&_board_WAR_bbsportlet_action=view_message&_board_WAR_bbsportlet_sCurPage=2)
- [18학년도 출제의도](https://site.hanyang.ac.kr/web/go/10?p_p_id=board_WAR_bbsportlet&p_p_lifecycle=2&p_p_state=normal&p_p_mode=view&p_p_cacheability=cacheLevelPage&p_p_col_id=column-1&p_p_col_count=1&_board_WAR_bbsportlet_fileId=517763&_board_WAR_bbsportlet_cmd=download&_board_WAR_bbsportlet_messageId=518666&_board_WAR_bbsportlet_messageId=518666&_board_WAR_bbsportlet_sCategoryId=0&_board_WAR_bbsportlet_sDisplayType=1&_board_WAR_bbsportlet_action=view_message&_board_WAR_bbsportlet_sCurPage=1)
- [18학년도 예시답안](https://site.hanyang.ac.kr/web/go/10?p_p_id=board_WAR_bbsportlet&p_p_lifecycle=2&p_p_state=normal&p_p_mode=view&p_p_cacheability=cacheLevelPage&p_p_col_id=column-1&p_p_col_count=1&_board_WAR_bbsportlet_fileId=517764&_board_WAR_bbsportlet_cmd=download&_board_WAR_bbsportlet_messageId=518667&_board_WAR_bbsportlet_messageId=518667&_board_WAR_bbsportlet_sCategoryId=0&_board_WAR_bbsportlet_sDisplayType=1&_board_WAR_bbsportlet_action=view_message&_board_WAR_bbsportlet_sCurPage=1)

### 18-1번 문제

- 정렬

![18-1](https://i.imgur.com/dtltaHf.png)

(30점) 같은값이 들어있는 두 배열이 있다. 같은 배열 원소끼리는 비교할 수 없고 다른 배열의 원소끼리 비교 가능하다. 두 배열을 정렬하시오.

퀵소트를 활용해서 반대쪽 배열을 피벗으로 삼고 반복해서 양쪽을 큰 그룹, 작은 그룹으로 나누면 된다. 이 문제는 교내 알고리즘 중간고사 문제로 다시 나왔다.

### 18-2번 문제

- 그리디

![18-2](https://i.imgur.com/1hW5KeO.png)

(40점) W, B 개수가 같은 문자열(길이가 4이상)에서 다시 W, B개수가 같은 부분문자열로 쪼갤수 있는지 물어보는 문제이다.

답은 Yes이고, W를 +1, B를 -1로 했을때 연속합에서 0이될때마다 자르면 된다. 그리고 0이 항상있을수 밖에 없다는 것을 증명하면 된다.

### 18-3번 문제

- 문제 해결 능력

> (30점) 본인의 소프트웨어 활동 중 대표적인 문제와 그 문제를 효율적으로 해결하기 위해 사용한 문제 해결 전략을 설명하시오.

그러나 실제로 채점 항목을 보면 15점이 이 문제(문제분석/해결방법)에대한 배점이고, 15점은 추가질문으로 버그가 난 코드의 위치를 찾는법(7점), 버그가 났을때 원인을 찾는 방법(8점)으로 실제 개발에 대한 지식/경험도 같이 물어보는 문제였다.

## 2019학년도

- [19학년도 문제](https://site.hanyang.ac.kr/web/go/10?p_p_id=board_WAR_bbsportlet&p_p_lifecycle=2&p_p_state=normal&p_p_mode=view&p_p_cacheability=cacheLevelPage&p_p_col_id=column-1&p_p_col_count=1&_board_WAR_bbsportlet_fileId=521152&_board_WAR_bbsportlet_cmd=download&_board_WAR_bbsportlet_messageId=521970&_board_WAR_bbsportlet_messageId=521970&_board_WAR_bbsportlet_sCategoryId=0&_board_WAR_bbsportlet_sDisplayType=1&_board_WAR_bbsportlet_action=view_message&_board_WAR_bbsportlet_sCurPage=1)
- [19학년도 출제의도](https://site.hanyang.ac.kr/web/go/10?p_p_id=board_WAR_bbsportlet&p_p_lifecycle=2&p_p_state=normal&p_p_mode=view&p_p_cacheability=cacheLevelPage&p_p_col_id=column-1&p_p_col_count=1&_board_WAR_bbsportlet_fileId=521154&_board_WAR_bbsportlet_cmd=download&_board_WAR_bbsportlet_messageId=521972&_board_WAR_bbsportlet_messageId=521972&_board_WAR_bbsportlet_sCategoryId=0&_board_WAR_bbsportlet_sDisplayType=1&_board_WAR_bbsportlet_action=view_message&_board_WAR_bbsportlet_sCurPage=1)
- [19학년도 예시답안](https://site.hanyang.ac.kr/web/go/10?p_p_id=board_WAR_bbsportlet&p_p_lifecycle=0&p_p_state=normal&p_p_mode=view&p_p_col_id=column-1&p_p_col_count=1&_board_WAR_bbsportlet_sCategoryId=0&_board_WAR_bbsportlet_sDisplayType=1&_board_WAR_bbsportlet_sCurPage=1&_board_WAR_bbsportlet_action=view_message&_board_WAR_bbsportlet_messageId=521973)

### 19-1번 문제

- 탐색

(40점) 길이 N인 두 배열이 주어졌을 때 N번째로 작은 원소를 찾는 방법을 설명하시오.

![19-1](https://i.imgur.com/pitdPcW.png)

꽤 어려운 문제로 양쪽에서 이분탐색을 돌리면 $O(log^2N)$이다.

$O(logN)$의 풀이가 존재하는데 이분탐색과 비슷하게하면서 두배열의 pivot의 대소를 이용해서 두 배열의 절반씩을 날릴수 있다.

다만 이 문제는 풀이보다 풀이를 잘 설명하는게 꽤 어려운(홀, 짝)문제라 논리적으로 설명하는 연습을 키워야할 것 같다. 참고로 이 문제도 교내 알고리즘 수업 기말고사 문제로 출제되었다.

### 19-2번 문제

- 최단경로

(60점) 다익스트라 설명이 주어진다. (힙을 활용하지 않는 방식이다.)

![19-2](https://i.imgur.com/pOf3C4V.png)

- 2-1. (15점) 주어진 그래프의 다익스트라 결과
- 2-2-1. (5점) X->Y->Z 최단 경로 찾는 방법
- 2-2-2. (10점) X->Z에서 Y를 거치지 않는 최단경로 찾는 방법
- 2-3. (20점)임의의 N개의 정점 배열이 주어진다. X->Z 최단경로 중 적어도 N에 있는 정점 중 하나의 중간정점을 지나는 최단경로
- 2-4. (15점) 다익스트라를 돌릴 때, 정점 거리값을 정렬 했을때와 안했을때의 시간복잡도

간략한 풀이는

- 2-2-1: X->Y 와 Y->Z 두번 다익스트라의 합이 답이다.
- 2-2-2: Y와 관련된 간선을 제거하고 다익스트라를 돌리면된다.
- 2-3: X에서 다익을 돌리고, Z에서는 간선을 거꾸로 하고 다익을 돌린다음에 N개의 정점에서 양쪽에서 왔을 때 합의 최솟값을 구하면 된다.
- 2-4: $O(V^2)$과 $O(VE)$ 실제 다익과는 좀 복잡도가 다른데 주어진 알고리즘이 살짝 다르고 시간복잡도가 중요한게 아니라 왜 이런지 설명하는게 중요하다.

## 2020학년도

- [20학년도 문제](https://site.hanyang.ac.kr/web/go/10?p_p_id=board_WAR_bbsportlet&p_p_lifecycle=2&p_p_state=normal&p_p_mode=view&p_p_cacheability=cacheLevelPage&p_p_col_id=column-1&p_p_col_count=1&_board_WAR_bbsportlet_fileId=524112&_board_WAR_bbsportlet_cmd=download&_board_WAR_bbsportlet_messageId=525864&_board_WAR_bbsportlet_messageId=525864&_board_WAR_bbsportlet_sCategoryId=0&_board_WAR_bbsportlet_sDisplayType=1&_board_WAR_bbsportlet_action=view_message&_board_WAR_bbsportlet_sCurPage=1)
- [20학년도 출제의도](https://site.hanyang.ac.kr/web/go/10?p_p_id=board_WAR_bbsportlet&p_p_lifecycle=2&p_p_state=normal&p_p_mode=view&p_p_cacheability=cacheLevelPage&p_p_col_id=column-1&p_p_col_count=1&_board_WAR_bbsportlet_fileId=524009&_board_WAR_bbsportlet_cmd=download&_board_WAR_bbsportlet_messageId=525865&_board_WAR_bbsportlet_messageId=525865&_board_WAR_bbsportlet_sCategoryId=0&_board_WAR_bbsportlet_sDisplayType=1&_board_WAR_bbsportlet_action=view_message&_board_WAR_bbsportlet_sCurPage=1)
- [20학년도 예시답안](https://site.hanyang.ac.kr/web/go/10?p_p_id=board_WAR_bbsportlet&p_p_lifecycle=2&p_p_state=normal&p_p_mode=view&p_p_cacheability=cacheLevelPage&p_p_col_id=column-1&p_p_col_count=1&_board_WAR_bbsportlet_fileId=524010&_board_WAR_bbsportlet_cmd=download&_board_WAR_bbsportlet_messageId=525866&_board_WAR_bbsportlet_messageId=525866&_board_WAR_bbsportlet_sCategoryId=0&_board_WAR_bbsportlet_sDisplayType=1&_board_WAR_bbsportlet_action=view_message&_board_WAR_bbsportlet_sCurPage=1)

### 20-1번 문제

- 탐색

(40점)
![20-1-1](https://i.imgur.com/FUF3U50.png)
![20-1-2](https://i.imgur.com/wli64RZ.png)

- 1-1. (10점) 이진탐색 설명, $n=2^k$ 배열 이진탐색 비교횟수 질문
- 1-2. (15점) 삼진탐색 설명, $n=3^k$ 배열 삼진탐색 비교횟수
- 1-3. (15점) 이진탐색과 삼진탐색 중 최악의 경우 비교를 적게 하는것은?

간략한 풀이는

- 1-1. $log_{2}n +1$ (+1 없어도 됨)
- 1-2. $2log_{3}n +1$ (+1 없어도 됨)
- 1-3. 이진탐색, 이유는 $2log_{3}n = log_{\sqrt{3}}n = log_{1.732...}n > log_{2}n$

### 20-2번 문제

- DP

(60점)

![20-2](https://i.imgur.com/9EPi3ll.png)

- 2-1. (20점) 6개의 수업 시작 시간과 종료시간 수강인원이 주어졌을때 겹치지 않게 최대 수강인원 수업하려 한다. 그때 하는 수업과 수강인원의 합은? 단 모든 수업은 전 수업과 다음수업과만 시간이 겹친다.
- 2-2. (20점) 2-1과 똑같은데 n개로 바뀌었다.
- 2-3. (20점) 2-2와 똑같은데 모든 수업은 모든 수업과 겹칠수 있다.

간략한 풀이는

- 2-2: $dp[i] = max(dp[i-1], dp[i-2]+a[i])$, 그떄하는 수업 역시 같은방법으로 구하면 된다.
- 2-3: $dp[i] = max(dp[i-1], dp[k[i]]+a[i])$ ($k[i]$는 $i$와 수업이 겹치지 않는 수업중 가장 늦게 끝나는 수업 번호)

## 기출 분석

제너럴한것을 물어본 18학년도 3번을 제외하면 항상 2문제씩 나왔고 분류는 다음과 같다.

| 년도 | 1번문제 | 2번 문제 |
| -------- | -------- | -------- |
| 18학년 | 정렬 | 그리디, DP |
| 19학년 | 탐색 | 최단경로 |
| 20학년 | 탐색 | DP |

교과서를 기반으로 하다보니 문제범위가 한정적이다. 대신에 그 알고리즘을 설명하고 문제에 내는 방식을 택하고 있긴 하다.

정렬과 탐색 알고리즘의 기본 원리를 잘 이해해야한다.
그리고 그리디, 최단경로, DP, 기초자료구조 백준 골드 급 문제는 무난히 풀 실력을 가지면 충분할것 같다. 문제의 난이도가 엄청 높기보다는 얼마나 조리있게 잘 말하냐가 중요할것 같다.

### 솔라매직 모의고사: 올해(2021학년도) 문제 예측

- 1번(40점)

$M(10^5 \leq M \leq 10^7)$개의 수가 정렬된 배열이 $N(N = 300)$개 있다.

1. (20점) 주어진 모든 수를 고려했을떄 가장 작은 $K(1 \leq K \leq 10^5)$개 수를 찾는 적절한 방법을 제시하시오.
2. (20점) 주어진 모든 수를 고려했을때 $K(10^5 \leq K \leq N*M)$번째마다 오는 수를 찾는 적절한 방법을 제시하시오.

- 2번(60점)

정수로 채워진 $N \times N$ 행렬이 있다.

1. (20점) 최대 합을 가지는 영역을 선택하는 방법을 제시하시오.
2. (20점) 최대 합을 가지는 직사각형 영역을 선택하는 방법을 제시하시오.
3. (20잠) 행렬이 $-\infty$과 $1$로만 이루어졌을때 최대 합 직사각형 영역을 구하는 방법을 제시하시오.

## 여담

사실 심심해서 그냥 보고 쓴건데 혹시 미래에 한양대 지원자한테 도움이 되었으면 하네요. 합격하시거나 궁금한점 있으면 편하게 연락주세요.
