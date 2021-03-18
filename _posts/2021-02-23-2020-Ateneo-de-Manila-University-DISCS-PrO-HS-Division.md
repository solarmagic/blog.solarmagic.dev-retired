---
title:  "2021년 첫 팀 연습"
date:   2021-02-23 00:00:00
categories:
- Algorithm
tags:
- Contest
- Practice
---

![](https://i.imgur.com/qt5j6ro.png)

## 팀 결성

### 팀원 

2020년 02월 04일 작년에 같이 휴학생으로 연습한 dtc03012 한테 연락이 와서 올해 같이 PS팀을 하기로 했다. 작년 신입생으로 들어온 Lemonade255  과 셋이서 같이 UCPC, ICPC등을 나가기로 약속을 했다.

### 목표

나름 한양대학교 안에서 나올 수 있는 최상의 스쿼드가 아닐까 생각해본다. ICPC WF같은 거창한 목표를 세우고 싶은데 솔직히 요즘에 잘하는 학교가 너무 많아서 학교 순위 Top5 드는것도 열심히 안하면 힘들것 같다.

### 팀명

아직 팀 명은 못정했다. 추천 받아요. ㅎㅅㅎ

## 첫 팀 연습

- 시간: 2021-02-23 14:05 ~ 17:05
- 장소: 한양대학교 법학도서관 스터디룸 4번

5시에 문을 닫아서 ICPC 예선처럼 3시간짜리 셋을 돌기로 했다. 급하게 Codeforces Gym 2페이지에 3시간짜리로 별 3개 난이도가 있길래 그냥 그걸로 하기로 했다.

[2020 Ateneo de Manila University DISCS PrO HS Divisio](https://codeforces.com/gym/102556)

사실 무슨 대회인지 잘 모르는데 미리 요약하면 그렇게 문제 퀄이 좋진 않았지만 우리에게 많은 교훈을 주긴했다.

## 연습 타임 라인

"전체적으로 문제는 쉬웠지만 우리는 더 바보였다." 로 요약할 수 있다.


### 14:05 ~ 14:16

내가 A번을 읽고 "엥 개 쉽잖아"하고 코딩을 했다. 그러나 WA 2번을 당했다. 이렇게 쉬운걸 틀리다니하고 약간 충격을 먹었다. 근데 알고보니 구하라는 것을 잘못 생각했다. 대충 위 읽고 예제보고서 원형으로 했을 때 연속된 구간의 수를 세라는 건줄 알았는데 연속된 구간의 수로 만들기 위해 필요한 사람의 수를 구하는 거였다.

근데 우연히 예제는 잘 나와서 눈치를 전혀 못챘는데 Lemonade255 이 알려줬다.

### ~ 14:27

Lemondade255 제대로 된 문제 해석으로 A를 짜서 제출 했다. 그런데 2WA만 더 적립했다. 사실 풀이는 뭐 안 들어봐도 너무 쉬운거라 당연히 잘 짰을텐데 30분 동안 한 문제도 못풀고 -4 되어서 이게 뭐지 했다.

### ~ 14:32

난 A 손절하고 dtc03012님이 E번을 읽어보라해서 내가 E번을 읽어봤다. E번을 읽으니깐 엥 뭐지 개쉬운데 해서 하고 그냥 풀었다. 문제 풀이보다 장황한 지문 해석이 훨씬 어려운 문제다. 모든 경우 기하 평균을 구하라는데 대충 수학으로 다 상쇄되어서 그냥 입력 받은수 그대로 출력하면 된다.

E Solve

### ~ 15:06

A를 다시 내가 처음부터 짜서 계속 제출 했다. 다른 사람은 B, C, D를 읽고 있었다. 그런데 내가 짜도 A가 또 틀리더라. 페널티 생각안하고 막 제출하기 시작했고 A는 5번을 더 틀려서 -9가 되었다. 정말 아무리봐도 틀린데가 없다고 생각했다.

### ~ 15:20

- 15:07 dtc03012님이 D번을 짜서 냈다. 틀렸다. 
- 15:08 Lemonade255님이 C번을 짜서 냈다. 틀렸다.
- 15:09 dtc03012님이 D번을 수정했다. 틀렸다.
- 15:14 Lemonade255님이 B번을 제출했다. 틀렸다.
- 15:20 Lemonade255님이 B번을 제출했다. 틀렸다.

대회 시작한지 1시간 15분 되었는데 그냥 그대로 출력하는 E번 빼고 하나도 못맞췄다. 엌ㅋㅋㅋ
역대 모든 코딩중에 가장 말렸다. 나만 말린게 아니라 모두가 다같이 말린건 처음봤다.

### ~ 15:28

근데 A번에 잘못된 점을 찾았다. 예외처리를 이상하게 한거였다. 해석이 바뀐걸 알았는데도 바보같이 모두 꽉차 있을때 원펀맨이 한번 쳐야 한다 생각해서 1을 출력하게 했다. 너무 당연해서 의심을 안했는데 dtc03012님과 얘기를 하면서 알게되었다. 수정해서 제출해서 맞추었다. 중간에 코드를 잘못 제출해서 -2가 더 깎였는데 뭐 크게 중요하진 않다.

A Solve

dtc03012님도 D번의 틀린점을 알거 같아서 3번 수정 제출 했지만 틀렸다.
그후에 Lemonade255님이 B번을 한번더 수정해 제출했지만 또 틀렸다.

상황을 정리하면 다음과 같다.

- 27분 E Solve
- 81분 A Solve (10 WA 1RTE 1CE)
- C 2WA
- D 5WA
- B 3WA

### ~ 15:41

다같이 뇌절하지말고 하나하나 풀자해서 A를 끝내고 D를 봤다. D도 천천히 생각하니깐 풀이가 나왔다. 식도 천천히 했으면 금방 나오는데 오히려 급해져서 막 내다가 정리해서 맞추었다. 그 사이에 4번 더 틀리고 맞췄다.

D Solve

### ~ 15:47

Lemonade255가 알아서 실수한 부분을 찾아서 고쳤다. 

C Solve

### ~ 16:01

Lemondade255가 B번을 수정시도하나 틀렸다.
dtc03012가 H번을 2번 시도 했으나 틀렸다.

### ~ 16:12

G번을 dtc03012님과 같이 읽었는데 dtc03012님이 주변 8방향에서 시뮬레이션 하면 될것 같다고 했다. 나는 안될거같은 의구심이 들었는데 반례가 안나왔다. 노트에 대충 그리다 보니깐 8방향 말고 그냥 주변 4방향 사각형만 하면 무조건 된다라는게 나왔다. dtc03012님이 혹시모르니깐 BFS로 하라했지만 걍 수학으로 했는데 잘 되었다. 문제 끝부분에 절반 이상이면 이상한 문구 출력하는걸 빼먹어서 한 번 틀렸는데 어쨌든 내가 대충 짜서 맞추었다.

G Solve

### ~ 16:16

Lemonade255랑 B번 코드를 보면서 이상한 데를 찾았다. 잘못된걸 같은 부분이 발견 되었고 Lemonade255가 고쳐서 내니 맞았다.

### ~ 16:37

나는 F번을 읽고 풀기 시작했고 다른 사람들은 H, I번을 풀고 있었다. F번 코드 짜는 사이에 옆에서 들어보니 대충 풀이는 다 나온거 같았다. 근데 좀 코딩이 빠르게는 되지 않았지만 정확히 짜려 했다. 정말 간단한 문제이다. 지문에 소녀시대 사진이 나오면서 한국 문화에 관한 문제여서 조금 신기했다. 어쨌든 맞췄다.

F Solve

### ~ 16:43

삼각형 사이에 있는 점의 개수를 세는 문제라 한다. dtc03012님이 USACO에 나왔던 문제라고 해서 알고 있는 문제였다. 남은 시간이 부족해서 팀노트라 생각하고 과거코드를 이용해 풀었다.

I Solve

### ~ 17:05(종료)

F를 풀고 나는 할게없었다. 남은 문제 풀이가 다나왔고 H번은 Lemonade255가 짜기로 했다. 대충 들어보니 풀이는 맞는것 같았다. 17:00에 나가야해서 짐을 정리하고 알아서 잘 풀길 기도했다. 근데 뭔가 코딩이 꼬였는지 막판에 예외케이스로 1과 int를 넘어가는 실수 수정해서 냈는데도 TC 15에서 셋다 틀렸다. 뭔가 올 솔브할수 있었는데 아쉽다.

## [스코어보드](https://codeforces.com/gym/102556/standings)

![scoreboard](https://i.imgur.com/FDHvW1V.png)

8솔브/9문제 1434페널티로 8솔 꼴지 했다.

초반에 개망했는데 어떻게 그래도 거의 다 맞추기는 했다.

## 후기

- 전체적으로 문제는 엄청 쉬웠다. 근데 지문이 좀 난잡해서 읽기가 어렵고 맞왜틀을 좀 유발했다.
- 개인적인 문제점으로 터널시야가 있는지 잘 해석하다가 문제 감이 오면 중간에 해석 건너뛰고 코딩하는 안좋은 버릇이 있나보다. A번과 G번에서 그랬다.
- 근데 위에랑 별개로 3명 다 실수가 너무너무 많았다. 쉬운 문제 정확하고 빠르게 푸는 연습을 해야 한다.
- 막 셋이서 다같이 고민할만한 문제는 없었고 오늘은 디버깅 대축제였는데 다음에는 좀 어려운 셋으로 5시간 돌아봐야 겠다.
- H, I는 내일 아님 모레 풀어봐야겠다.