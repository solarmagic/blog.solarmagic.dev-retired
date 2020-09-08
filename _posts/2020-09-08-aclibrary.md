---
title:  "AtCoder Library(ACL) 소개"
date:   2020-09-08 00:00:00
categories:
- Algorithm
tags:
- AtCoder
- Library
---

어제 밤(2020년 9월 7일) AtCoder에서 프로그래밍 대회에서 유용하게 쓸 수 있는 알고리즘 구현의 모음을 라이브러리 형식으로 배포했다. 이를 활용하면 적은 수의 타이핑만으로 알고리즘 문제를 해결 할 수 있다.

![atcoder](https://i.imgur.com/crM8jHb.png)

[원본 글 링크](https://atcoder.jp/posts/518)
[라이브러리 다운로드 링크](https://img.atcoder.jp/practice2/ac-library.zip)

다운로드를 받고 압축을 풀면 영어와 일본어 문서(`document_en/`, `document_ja/`), 라이브러리(`atcoder/`), 온라인 저지에 제출하기 위한 도구(`expander.py`)가 존재한다.

## 내장 알고리즘

- 전부 인클루드 하기 <atcoder/all>

### 자료구조

- 펜윅 트리 <atcoder/fenwicktree>
- 세그먼트 트리 <atcoder/segtree>
- 느리게 갱신되는 세그먼트 트리 <atcoder/lazysegtree>
- 문자열 <atcoder/string>
  - 접미사 배열
  - LCP 배열
  - Z 알고리즘

### 수학

- 기초 수학 <atcoder/math>
  - 제곱 모듈러
  - 인버스 모듈러
  - 중국인의 나머지 정리
- 컨볼루션 <atcoder/convolution>
- 소수 유한체 <atcoder/modint>

### 그래프

- 분리 집합 <atcoder/dsu>
- 최대 유량 <atcoder/maxflow>
- 최소 비용 최대 유량 <atcoder/mincostflow>
- 강한 연결 요소 <atcoder/scc>
- 2-SAT <atcoder/twosat>

## 사용법

구체적인 사용법은 `document_en/` 을 읽는 것을 추천한다. 한국어 번역이 없기 때문에 간단한 사용법만 한국어로 적어본다.

가장 간단하게 사용하는 방법은 ac-library 폴더 안에 main.cc(또는 main.cpp) 파일을 생성하고 그곳에 코드를 작성하는 것이다.

![dir](https://i.imgur.com/Czmr09Z.png)

### 1717번 예시

백준 1717번은 대표적인 Disjoint Set Union(DSU) 문제이다.

```cpp
#include <atcoder/dsu>
#include <bits/stdc++.h>
using namespace std;
using namespace atcoder;

int main() {
    int n, m;
    scanf("%d %d", &n, &m);
    dsu d(n+2);
    while (m--) {
        int t, a, b;
        scanf("%d %d %d", &t, &a, &b);
        if (t == 0) d.merge(a, b);
        else printf("%s\n", d.same(a, b) ? "YES" : "NO");
    }
}
```

매우 간단하게 코드를 작성할 수 있다. 단 이대로 제출하면 백준 온라인 저지에는 ``<atcoder/dsu>`` 가 없으므로 컴파일 에러가 난다.
이를 해결하기 위해 제공해준 스크립트를 사용한다.

```sh
python3 expander.py main.cc
```

`expander.py`는 `combined.cpp` 라는 제출 가능한 파일을 라이브러리 함수를 끌어와서 작성해준다. 이 코드를 복사 & 붙여넣기를 하면 된다.

[제출 코드 보기](http://boj.kr/50d67cdff17c4ba88796d9588e6ca1d1)

구체적인 사용방법은 `ac-library/document_en/dsu.html`을 통해 확인할 수 있다.

## 여담

AC 라이브러리는 아직 테스팅을 덜 해보았지만 AtCoder에서 만든 만큼 믿고 써도 되는 라이브러리로 보인다. 속도나 안정성이 괜찮아 보인다.

최근 많은 대회들이 펜데믹 상황으로 인해 온라인으로 치루는 추세이다. 대부분의 온라인 대회들은 라이브러리를 이용하여 대회를 치루는 것을 허용한다. 그러므로 이렇게 자주 쓰이는 알고리즘은 네임스페이스 단위로 가지고 있는 것은 시간을 단축시킬 뿐만 아니라 맞왜틀을 방지한다.

그러나 템플릿에 너무 의존하게 되면 실제 그 자료구조나 알고리즘을 짜는 법을 까먹거나 내부원리를 이용한 응용을 하기 힘들어질 수 있다. 알고리즘을 공부할때는 힘들고 귀찮더라도 직접 짜는 연습을 하고 시간이 촉박한 대회(그러면서 라이브러리 사용을 허용하는 대회)에서 라이브러리를 사용하는 것이 좋을거로 생각 된다.

또한 시간이 남고 AtCoder측에서 허락을 해준다면 `document_kr` 만들 의향이 있다.
