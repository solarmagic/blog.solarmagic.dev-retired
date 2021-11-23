---
title: "백준[BOJ] 1577번 제곱 ㄴㄴ"
date: 2021-11-23 00:00:00
categories:
- BOJ
tags:
- Inculsion-Exclusion
- Math
- Mobius
---

![SQUAREFREE](https://i.imgur.com/Vyayxqp.png)

# Square-free integers

## 서론

오늘 기준(2021/11/23) 가장 많이 풀린 다이아몬드 이상의 문제가 [1557번: 제곱 ㄴㄴ](https://www.acmicpc.net/problem/1557)이다. 그만큼 어려우면서 쉬운(?) 문제가 바로 이 문제인데 이 문제의 풀이는 정말 짧지만 그것을 제대로 이해하는 건 매우 어렵다고 생각한다. 실제로 나도 완벽히 이해하지 못했었고 다른 블로그의 글 설명 또한 부족한 부분이 있다고 생각하여 이 글을 작성하게 된다. 이 글이 1557번 풀이의 레퍼런스가 되었으면 한다.

글의 양이 상당히 긴데 차근 차근 따라오면 문제가 풀리도록 설명하도록 노력했다.

사실 jh05013님이 설명한 걸 다시 한 번 정리하는 것이긴 하다.

## 1557번 문제

어떤수 $N$이 $1$이 아닌 제곱수로 나누어지지 않을 때, 이 수를 제곱ㄴㄴ수라고 한다. 제곱수는 $4, 9, 16, 25$와 같은 것이고, 제곱ㄴㄴ수는 $1, 2, 3, 5, 6, 7, 10, 11, 13, ...$과 같은 수이다.

이 때 $1 \leq K \leq 10^9$ 의 $K$가 주어졌을 때 $K$ 번째 제곱 ㄴㄴ수(Square-free integers)를 구하는 문제이다.

- 시간 제한: 2초
- 메모리 제한: 128MB

## 풀이

### 브루트 포스

```python
def is_square_free(n):
    '''
    function to check if n is square free
    '''
    for i in range(2, int(n**0.5)+1):
        if n % (i * i) == 0: return False
    return True

def kth_square_free(k):
    '''
    function to find kth square free numnber
    '''
    cnt, i = 0, 1
    while True:
        if is_square_free(i):
            cnt += 1
            if cnt == k:
                return i
        i += 1


print(kth_square_free(100))
```

하나씩 세면서 제곱 ㄴㄴ수인지 확인 하는 방법이다. 코드는 쉽지만 매우 느리다.


### 포함 배제의 원리(17436번 풀기)

이 문제 풀이의 가장 핵심은 바로 포함 배제의 원리이다.

[17436번: 소수의 배수](https://www.acmicpc.net/problem/17436)는 포함 배제의 가장 기초적인 문제이다. (~~푼 사람 수는 [1557번](https://www.acmicpc.net/problem/155)이 포함 배제 태그 중에 가장 많다.~~)

$N(1 \leq N \leq 10)$개의 소수가 주어지고 $M(1 \leq M \leq 10^{12})$ 이하의 자연수 중 $N$개의 소수 중 적어도 하나라도 나누어 떨어지는 개수를 세는 문제이다.

이 문제의 풀이는 다음과 같다. 하나 하나 해보면서 규칙을 파악해보자.

1) 소수($p$)가 한 개 주어 졌을 경우

$[M/p]$를 출력하면 된다. 예를 들어 $p = 3$이고 $M = 100$이면 100보다 작은 수에 3으로 나누어 떨어지는 수는 33개다. 여기서 $[x]$는 $x$ 이하의 최대 정수를 구하는 함수이다.

2) 소수가 두 개 주어졌을 경우  

$[M/p_1] + [M/p_2] - [M/(p_1p_2)]$를 계산하면 된다.
예를 들어 $p1 = 2, p2 = 3, M = 100$인 경우
$[100/2] + [100/3] - [100/6] = 50 + 33 - 16 = 67$이다.

$2$로 나누어 떨어지는 개수를 세고 $3$으로 나누어 떨어지는 개수를 세는데 이 때 중복으로 센 개수는 다시 빼주는 데 이는 $[100/(2*3)]$으로 쉽게 구할 수가 있다.

3) 소수가 세 개 주어졌을 경우

![](https://i.imgur.com/Af6ZdNL.png)
> Wikipedia(포함 배제의 원리)

$[M/p_1] + [M/p_2] + [M/p_3] - [M/(p_1p_2)] - [M/(p_1p_3)] - [M/(p_2p_3)] + [M/(p_1p_2p_3)]$을 더하면 된다.

소수가 두 개일 경우와 비슷하게 하는데 이 때는 다시 3개가 겹쳐진 영역은 너무 많이 빼져서 다시 더해 줘야한다. 

일반화하면 모든 소수들의 부분집합의 곱을 보는데 하면 홀수 개의 소수가 곱해졌을 때는 더하고 짝수 개의 소수를 곱한 것은 빼주면 된다.

![](https://i.imgur.com/xT26JD0.png)


매우 자주 쓰이는 테크닉이니 알아두면 좋다.
[17436](https://www.acmicpc.net/problem/17436)번 풀이 소스

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int N; cin >> N;
    ll M, ans = 0; cin >> M;
    vector<int> primes(N);

    for(auto& x: primes) cin >> x;
    for (int i = 1; i < (1<<N); i++) { // 2^N - 1번 반복하면서 
        int popcnt = __builtin_popcount(i); // 켜진(1) 비트의 개수를 센다
        ll x = 1;
        for (int j = 0; j < N; j++) {
            if (i & (1<<j)) x *= primes[j]; // 켜진 비트에 해당하는 소수 곱하기
        }
        if (popcnt % 2 == 1) ans += (M / x); // 켜진 비트가 홀수면 더하고
        else ans -= (M / x); // 켜진 비트가 짝수면 뺀다
    }
    cout << ans;
}
```

비트마스크를 이용하면 정말 간단한게 코딩할 수 있다.

### 1557번에 적용하고 시간복잡도 줄이기

이 문제도 위의 문제와 매우 비슷하다. 각 소수의 제곱수로 떨어지는 수의 개수를 구하는데 포함 배제를 사용해서 중복 처리를 잘 해주면 된다.

좀 더 수학으로 정리하면 집합 $P = \{p \mid p$는 소수$\}$
의 공집합이 아닌 모든 부분집합 $S$에 대해 $-1^{\mid S \mid}[N/q^2](q = \prod_{p\in S} p)$를 계산하면 $N$ 이하의 제곱 ㄴㄴ 수의 개수를 알 수 있다.

$K$번째 제곱 ㄴㄴ 수를 구하는 것이므로 $N$을 이분 탐색해서 답을 찾을 수가 있다. 최댓값은 $2K$ 정도면 충분하다. 그러나 문제는 $2K = 2 \times 10^9$ 이하의 소수가 몇 천만개는 있다는 것이고([소수 정리](https://namu.wiki/w/%EC%86%8C%EC%88%98%20%EC%A0%95%EB%A6%AC)를 사용하면 개수를 추측할 수 있다.) 이러면 시간 복잡도가 2의 5천만제곱 이라는 무지막지한 시간 복잡도가 나온다.

여기서 5분 정도 생각을 해보자. 

---

이 때 필요한 관찰은 $-1^{\mid S \mid}[N/q^2]$에서 **$q$가 $\sqrt N$보다 크다면 저 값은 항상 0이 된다는 것**이다. 원래 포함 배제 문제를 풀 때에는 모든 부분 집합 $S$를 찾고 그것에 해당하는 $q$를 만들어서 저 식을 계산했다면 이 문제에서는 반대로 $q$에 대해서 반복하면서 그에 해당하는 $S$가 있는지를 판단하는 것이다!

여기서 필요한 또 다른 관찰은 그에 해당하는 $S$의 개수는 항상 $\sqrt N$개 이하라는 것이다. 왜냐하면 서로 다른 집합 **$S$의 원소의 곱은 항상 다르기** 때문이다.

이분 탐색을 하면서 모든 $q$에 대해 값을 계산하면 $O(\sqrt K \log K)$이 걸린다. 

여기서 이제 남은 것은 모든 $q$에 대해서 이것이 제곱ㅇㅇ(squareful number)인지 파악하고 제곱 ㄴㄴ수라면 소수가 짝수번 곱해졌는지 홀수번 곱해졌는지만 파악하면 된다. 이는 미리 전처리하기만 하면 된다. 각 수에 대해서 $O(\sqrt x)$의 시간이 걸리므로 전체 시간 복잡도는 $O(K^{3/4})$가 된다. 이정도만 되어도 이 문제를 통과하기 충분한 시간 복잡도이다. 


### 1557 최종 풀이

최종적으로 정리하면 풀이는 다음과 같다.

1. 집합 $P = \{p \mid p$는 소수$\}$의 공집합이 아닌 모든 부분집합 $S$에 대해 $-1^{\mid S \mid}[N/q^2]$($q = \prod_{p\in S} p$)를 계산하면 $N$ 이하의 제곱 ㄴㄴ 수의 개수를 알아낼 수 있다. 이를 $Q(N)$이라 하자.
2. 이 문제의 답이 될수 있는 최댓값을 $R = 2*10^{9}$ 라고 하자 최솟값은 $1$이다. $1$부터 $R$사이에서 $Q(x) = K$를 만족하는 최소의 $x$를 이분 탐색으로 알아낸다. 
3. $Q(x)$를 계산할 때 다 계산하면 양이 많으니 $q$의 후보가 최대 $\sqrt R$ 까지임을 이용해 $q$를 정했을때 $q$가 가능한 $q$인지 판단 및 $-1^{ \mid S \mid}$의 값을 $q$를 소인수 분해해서 알아 낸다. 이를 전처리해서 알아내자.


소스 코드

```cpp
#include <bits/stdc++.h>
using namespace std;
const int KMAX = 1'000'000'000;
const int NMAX = 2*KMAX;
const int MAX = 50000;

int is_composite[MAX];
int sign[MAX];
vector<int> primes;

void get_sieve(int n) {
    for (int i = 2; i < n; i++) {
        if (!is_composite[i]) 
            primes.push_back(i);
        for (int j = i+i; j < n; j+=i) {
            is_composite[j] = 1;
        }
    }
}

void sign_precompute(int n) {
    memset(sign, -1, sizeof(sign));
    sign[1] = 1;
    for (int i = 2; i < n; i++) {
        for (int j = 2; j * j <= i; j++) {
            if (i % (j * j) == 0) {
                sign[i] = 0;
                break;
            }
        }
        if (sign[i] == 0) continue;
        int cnt = 0;
        int x = i;
        for (int j = 0; j < primes.size() && primes[j] <= x; j++) {
            if (x % primes[j] == 0) {
                cnt++;
                x /= primes[j];
            }
        }
        if (cnt % 2 == 0) sign[i] = 1;
    }
}

int Q(int x) {
    int ans = 0;
    for (int i = 1; i * i <= x; i++) {
        ans += sign[i] * (x / (i * i));
    }
    return ans;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    get_sieve(MAX);
    sign_precompute(MAX);
    int K; cin >> K;
    int s = 1, e = K*2, ans = e;
    while (s <= e) {
        int m = s + (e - s) / 2;
        if (Q(m) < K) s = m + 1;
        else e = m - 1, ans = m;
    }
    cout << ans;
}
```

백준 기준으로 100ms 가 나왔다.
![](https://i.imgur.com/77Ogw7w.png)

정답 코드를 가지고 확인 해보면 $K$가 $10^9$일때 답은 $1644934081$이고 이를 $R$로 할때 MAX는 $40558$까지만 계산하면 된다. 이러면 72ms로 조금 더 시간 단축을 할 수 있다.

### 더 발전하기

현재 시간 복잡도의 큰 축을 차지하는 것은 sign을 계산하는 것이다. 

이는 에라토스테네스의 체를 살짝 변형시켜서 발전할 수 있다. 에라토스트네스의 체에서 $i$가 소수면 소수 목록에서 $i$를 추가한다. 소수 목록에 있는 $j$에 대해서, $ij$는 합성수이고, $i$가 $j$의 배수면 멈춘다. 이를 Linear Sieve라 하여 기존의 에라토스테네스의 시간 복잡도인 $O(n \log \log n)$ 보다 빠른 $O(n)$에 돌아가는 소수 체를 구현 할 수 있다. 뿐만 아니라 가장 작은 소인수를 담고 있어 소인수 분해도 훨씬 빨리 할 수 있다.

```cpp
int smallest_prime_factor[MAX];
vector<int> primes;
void get_linear_sieve(int n) {
    for (int i = 2; i < n; i++) {
        if (smallest_prime_factor[i] == 0) 
            primes.push_back(i);
        for (auto j : primes) {
            if (i * j >= n) break;
            smallest_prime_factor[i * j] = j;
            if (i % j == 0) break;
        }
    }
}

// return -1 when squareful number
// return number of primes 
int prime_factorization(int n) {
    int pre = -1, cnt = 1;
    if (n == 1) return 2; // exception
    while (smallest_prime_factor[n]) {
        if (smallest_prime_factor[n] == pre) return -1;
        pre = smallest_prime_factor[n];
        n /= smallest_prime_factor[n];
        ++cnt;
    }
    if (pre == n) return -1;
    return cnt;
}

void sign_precompute() {
    for (int i = 1; i < MAX; i++) {
        int num_of_primes = prime_factorization(i);
        sign[i] = (num_of_primes == -1) ? 0 : (num_of_primes % 2 ? -1 : 1); 
    }
}
```

이 때 `smallest_prime_factor[x]` 에는 $x$가 소수면 0, 합성수면 가장 작은 소인수가 들어가 있다. Linear Sieve에 대한 자세한 설명은 [여기](https://ahgus89.github.io/algorithm/Linear-sieve/)에서 볼 수 있다.

이를 활용한 코드는 4ms로 매우 빠른 속도를 보여주었다. 
![](https://i.imgur.com/LzIun9g.png)

### 뫼비우스 함수 

여태까지 위에서 구한 `sign[x]`는 사실 $\mu(x)$ 와 같이 쓰며 [뫼비우스 함수](https://ko.wikipedia.org/wiki/%EB%AB%BC%EB%B9%84%EC%9A%B0%EC%8A%A4_%ED%95%A8%EC%88%98)라는 이름이 있다. 

```cpp
void sign_precompute() {
    sign[1] = 1;
    for(int i = 1; i < MAX; i++) {
        for(int j = 2 * i; j < MAX; j += i) {
            sign[j] -= sign[i];
        }
    }
}
```

위키를 통해 알 수 있듯이 정의는 우리가 구하려던것과 완전히 동일하고 이를 구하는 방법은 위와 같이 잘 알려져 있는데, 에라토스테네스의 체처럼 자신의 배수의 홀짝성이 바뀌는 것을 이용해 계산한다.

위를 활용한 최종 코드는 다음과 같다.

```cpp
#include <bits/stdc++.h>
using namespace std;
const int R = 1644934081;
const int MAX = 40558;
int sign[MAX];

void sign_precompute() {
    sign[1] = 1;
    for(int i = 1; i < MAX; i++) {
        for(int j = 2 * i; j < MAX; j += i) {
            sign[j] -= sign[i];
        }
    }
}

int Q(int x) {
    int ans = 0;
    for (int i = 1; i * i <= x; i++) {
        ans += sign[i] * (x / (i * i));
    }
    return ans;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    sign_precompute();
    int K; cin >> K;
    int s = 1, e = R, ans = e;
    while (s <= e) {
        int m = s + (e - s) / 2;
        if (Q(m) < K) s = m + 1;
        else e = m - 1, ans = m;
    }
    cout << ans;
}
```

역시 4ms 에 배열을 조금 써서 메모리가 조금 줄었다.

![](https://i.imgur.com/c6Jrmzj.png)


혹시 다른 블로그 등에서 이 문제의 풀이를 보면 뫼비우스 함수 얘기를 먼저 꺼내곤 한다. 하지만 나도 그렇고 뫼비우스 함수를 갑자기 얘기하면 왜 이 함수가 나오는지 이해하기 힘들고 문제의 중요한 부분을 놓치게 된다. 이 문제의 핵심 풀이는 포함 배제에서 집합 $S$를 구하고 $q$를 구하는게 아니라 $q$를 구하고 집합을 구하는 발상이 가장 핵심 발상이라 할 수 있는데(개인적인 생각이다.) 그 부분의 주목도가 조금 떨어지는 것 같아서 아쉽다. 또한 뫼비우스를 모르고 있었어도 충분히 풀 수있다는 것을 이 블로그 글을 통해서 알리고 싶었다.

### 정답 예측하기

이 문제를 브루트 포스를 응용해서 풀 수 없을까 혹은 더 발전되어서 수학으로 한번에 푸는 방법이 없을까를 생각한다.

| K | K번째 제곱 ㄴㄴ수 |
| -------- | -------- |
| 1     | 1     |
| 10 | 14|
| 100 | 163 |
| 1000 | 1637 |
| 10000 | 16446 |
| 100000 | 164498 |
| 1000000 | 1644918 |
| 10000000 | 16449369|
| 100000000 | 164493390|
| 1000000000 | 1644934081|

무언가 규칙이 보이지 않는가? K번째 제곱 ㄴㄴ 수는 완벽하진 않더라도 약 1.64493...를 곱한 수에 매우 가깝다.

정확히 이 수는 $\pi^2/6$ (역수로 보면 $6/\pi^2$)에 수렴한다. 갑자기 뜬금없이 $\pi$가 나왔다. 원과는 전혀 관련이 없어 보이는 문제임에도 말이다.

왜냐하면 Q(x)를 위에서 정의한 함수라고 하고 x가 매우 큰수라고 하자. 그러면 3/4는 4로 나누어떨어지지 않을 것이고, 8/9는 9로 나누어 떨어지지 않을 것이다. 그런식으로 가다보면 multiplicative property를 만족하기 때문에

$\begin{align}Q(x) &\approx x\prod_{p\ \text{prime}} \left(1-\frac{1}{p^2}\right) = x\prod_{p\ \text{prime}} \frac{1}{(1-\frac{1}{p^2})^{-1}}\\
&\approx x\prod_{p\ \text{prime}} \frac{1}{1+\frac{1}{p^2}+\frac{1}{p^4}+\cdots} = \frac{x}{\sum_{k=1}^\infty \frac{1}{k^2}} = \frac{x}{\zeta(2)} = \frac{6x}{\pi^2}.\end{align}$

임을 알 수 있다.

리만 제타 함수 $\zeta(2)$ 는 Bessel Problem 이라는 문제로도 유명하며 이 값에 [왜 $\pi$가 나오는지는 아주 잘 설명해주는 영상](https://youtu.be/d-o3eB9sfls)이 있다. 

또한 $Q(x) =  \frac{6x}{\pi^2} + O\left(\sqrt{x}\right)$ 라는것이 알려져 있으며 [2015년 논문](https://www.sciencedirect.com/science/article/pii/S0022314X15002462)에서는 $O(x^{11/35+\epsilon})$ 이라고 오차항이 더 작음을 증명했다. 어쨋든 간에 $Q(x)$와 $x$ 사이의 관계가 $6/\pi^2$에서 크게 벗어나지 않음을 알 수 있다. 이를 이용하면 이분 탐색의 L~R범위의 후보를 줄일 수가 있다.

심지어 이를 활용해 리니어 서치로 하는 코드도 통과가 된 것을 확인할 수 있다.

### 더더 발전하기

아직 끝이 아니라 더 시간 복잡도를 줄인 논문이 존재한다. 바이너리 서치를 빼고 현재 $O(\sqrt N)$ 인 알고리즘을 $O(N^{2/5})$로 더 최적화 시켰다. 이것까지 소개하는 건 안그래도 글이 너무 긴데 더 길어질것 같아서 이만 줄인다. 관심 있는 사람은 [링크](https://www.researchgate.net/publication/51918452_Counting_Square-Free_Numbers) 확인


## 비슷한 문제


### [프로젝트 오일러 193번: 제곱청정 수](https://projecteuler.net/problem=193)

$2^{50}$미만의 제곱청정 수는 몇 개입니까?

### [해커랭크: 프로젝트 오일러 193번](https://www.hackerrank.com/contests/projecteuler/challenges/euler193/problem)

$K$의 제한이 $10^{18}$로 증가한 BOJ 1557번

### [백준 8464번: Non-Squarefree Numbers](https://www.acmicpc.net/problem/8464)

사실 이 문제가 많이 풀린 이유중 하나가 한 문제 풀면 다이아 2개를 풀 수 있기 때문이다.

### [백준 2814번: 최소인수](https://www.acmicpc.net/problem/2814)

소수와 포함 배제 등에 재미를 느꼈으면 또 풀만한 문제

### [백준 1792번: 공약수](https://www.acmicpc.net/problem/1792)

뫼비우스 함수를 넘어, 뫼비우스 반전 공식의 튜토리얼(?) 급 문제
