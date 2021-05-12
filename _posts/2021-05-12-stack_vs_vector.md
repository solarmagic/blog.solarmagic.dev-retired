---
title:  [TIL] std::vector 보다 std::stack 이 더 메모리를 많이 먹는다?
date: 2021-04-16 00:00:00
categories:
- TIL
tags:
- Data Structures
- vecor
- stack
- deque
---

## [TIL] std::vector 보다 std::stack 이 더 메모리를 많이 먹는다?

![](https://i.imgur.com/9QqgcO8.jpg)

직관적으로 생각했을 때 `std::vector`는 `std::stack`의 `pop()` `push()` `top()` 연산 등을 동일하게 $O(1)$에 계산하면서 random-access(인덱스로 접근)도 가능하니깐 `std::stack`이 구현이 더 간단하거나 해서 메모리를 더 적게 먹을것 같지만 [어떤 문제](https://www.acmicpc.net/problem/17163)를 풀어보니깐 아니었다.


![](https://i.imgur.com/9KHBkQk.png)

[cppreference](https://en.cppreference.com/w/cpp/container/stack)를 통해서 기 이유를 찾아 볼 수 있었다.

`std::stack`의 기본 Container(스택의 원소를 저장하는 역할)가 `std::deque`이기 때문이다. 설명을 보면 back(), push_back(), pop_back() 연산만 지원하면 어떠한 다른 컨테이너를 사용해도 상관 없다. 그리고 이를 만족하는 것은 `std::vector`, `std::deque`, `std::list`가 있다.

## 백준 17163번 결과

- [stack<int>](http://boj.kr/4ab6459501e9452a941a76cd802ad2f7) 

![](https://i.imgur.com/VGib4te.png)

- [stack<int, vector<int>>](http://boj.kr/0e9c2af330f4479492db90eba67b6906)

![](https://i.imgur.com/DBwIeVR.png)

- [stack<int, list<int>>](http://boj.kr/a5463e83ac934019912095b339b96776)

![](https://i.imgur.com/O2BlDgg.png)


Competitive Programming 을 할 때 `stack<int, vector<int>>`를 쓰는 것을 고려하는 게 메모리 관점에서 좋다는 것을 확인할 수 있다.

---


- Q1. 다른 C++ Adaptor는 어떻게 구현되어 있나요.
    - A1. `std::queue`는 `std::deque`가 default고 `std::list`도 사용 가능하다. `std_priority_queue`는 `std::vector`가 default고 `std::deque`도 사용 가능하다.
- Q2. 무슨 기준으로 default container가 그렇게 정해진걸까요.
    - A2. 아마도 작동 시간이 더 빠르기 때문일것으로 추측한다. [참고 자료](https://www.codeproject.com/Articles/5425/An-In-Depth-Study-of-the-STL-Deque-Container), vector는 resize를 자주하지 않고 빠르게 push_back() 지원하기 위해서 등비로 size를 바꾸는데 (amortized analysis 참고) 이게 performance가 생각보다 느린것 같다.

