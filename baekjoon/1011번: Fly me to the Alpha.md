# 1011번: Fly me to the Alpha Centauri

## 문제

x에서 y까지의 거리를 dis라 할때, k-1/k/k+1 중 하나를 선택해서 이동할 수 있다. 이때 최소 이동 횟수를 구하시오. (단, k는 이전 번에 움직인 거리이며, 처음과 마지막에는 1만큼 움직인다.)

## 입력

입력의 첫 줄에는 테스트케이스의 개수 T가 주어진다. 각각의 테스트 케이스에 대해 현재 위치 x 와 목표 위치 y 가 정수로 주어지며, x는 항상 y보다 작은 값을 갖는다. (0 ≤ x < y < 2<sup>31</sup>)

## 출력

각 테스트 케이스에 대해 x지점으로부터 y지점까지 정확히 도달하는데 필요한 최소한의 공간이동 장치 작동 횟수를 출력한다.

## 제한

시간 제한 : 2초 <br>
메모리 제한 : 512 MB

## 풀이

dis = 1 + 2 + 3 + ... + n + ... + 1로 표현해야하며 더해진 숫자의 개수가 최소가 되어야한다.
dis를 3부터 시작하여 순차적으로 늘려가면 다음과 같다.

3 = 1 + 1 + 1

**4 = 1 + 2 + 1 ⇒ 3번(2\*2 - 1)**

5 = 1 + 2 + 1 + 1 (remain: 1 → 1번)

6 = 1 + 2 + 2 + 1 (remain: 2 → 1번)

7 = 1 + 2 + 2 + 1 + 1 (remain: 3 → 2번)

8 = 1 + 2 + 2 + 2+ 1 (remain: 4 → 2번)

**9 = 1 + 2 + 3 + 2 + 1 ⇒ 5번(2\*3 - 1)**

…

**16 = 1 + 2 + 3 + 4 + 3 + 2 + 1 ⇒ 7번(2\*4 - 1)**

즉, dis가 제곱수일때를 기준으로 remain(이전 제곱수보다 얼마나 더 거리가 남았는가)의 이동횟수를 파악하면된다.

a <sup>2</sup> -> 2 \* a - 1 번 이동

a <sup>2</sup> + remain -> 2 \* a - 1 + ɑ 번 이동
(ɑ = (remain / a)의 올림)

```c++
#include <iostream>
#include <cmath>
using namespace std;
int main() {
    int t;
    long long x, y;
    cin >> t;
    while(t > 0){
        cin >> x >> y;
        long long ans = 0, a = 0;
        while(a*a <= y - x) a++;
        a--;
        ans = 2 * a - 1;
        long long remain  = y - x - a*a;
        ans += (long long)ceil((double)remain / (double)a);
        cout << ans << "\n";
        t--;
    }
}

```

#math #cpp
