# 1005번: CM Craft

## 문제

서기 2012년! 드디어 2년간 수많은 국민들을 기다리게 한 게임 ACM Craft (Association of Construction Manager Craft)가 발매되었다.

이 게임은 지금까지 나온 게임들과는 다르게 ACM크래프트는 다이나믹한 게임 진행을 위해 건물을 짓는 순서가 정해져 있지 않다. 즉, 첫 번째 게임과 두 번째 게임이 건물을 짓는 순서가 다를 수도 있다. 매 게임시작 시 건물을 짓는 순서가 주어진다. 또한 모든 건물은 각각 건설을 시작하여 완성이 될 때까지 Delay가 존재한다.

![star](https://github.com/king-raccoon/king-raccoon/assets/78426205/7524f6ab-f1c2-4e90-835c-334ec605bbf9)

위의 예시를 보자.

이번 게임에서는 다음과 같이 건설 순서 규칙이 주어졌다. 1번 건물의 건설이 완료된다면 2번과 3번의 건설을 시작할수 있다. (동시에 진행이 가능하다) 그리고 4번 건물을 짓기 위해서는 2번과 3번 건물이 모두 건설 완료되어야지만 4번건물의 건설을 시작할수 있다.

따라서 4번건물의 건설을 완료하기 위해서는 우선 처음 1번 건물을 건설하는데 10초가 소요된다. 그리고 2번 건물과 3번 건물을 동시에 건설하기 시작하면 2번은 1초뒤에 건설이 완료되지만 아직 3번 건물이 완료되지 않았으므로 4번 건물을 건설할 수 없다. 3번 건물이 완성되고 나면 그때 4번 건물을 지을수 있으므로 4번 건물이 완성되기까지는 총 120초가 소요된다.

프로게이머 최백준은 애인과의 데이트 비용을 마련하기 위해 서강대학교배 ACM크래프트 대회에 참가했다! 최백준은 화려한 컨트롤 실력을 가지고 있기 때문에 모든 경기에서 특정 건물만 짓는다면 무조건 게임에서 이길 수 있다. 그러나 매 게임마다 특정건물을 짓기 위한 순서가 달라지므로 최백준은 좌절하고 있었다. 백준이를 위해 특정건물을 가장 빨리 지을 때까지 걸리는 최소시간을 알아내는 프로그램을 작성해주자.

## 입력

첫째 줄에는 테스트케이스의 개수 T가 주어진다. 각 테스트 케이스는 다음과 같이 주어진다. 첫째 줄에 건물의 개수 N과 건물간의 건설순서 규칙의 총 개수 K이 주어진다. (건물의 번호는 1번부터 N번까지 존재한다)

둘째 줄에는 각 건물당 건설에 걸리는 시간 D1, D2, ..., DN이 공백을 사이로 주어진다. 셋째 줄부터 K+2줄까지 건설순서 X Y가 주어진다. (이는 건물 X를 지은 다음에 건물 Y를 짓는 것이 가능하다는 의미이다)

마지막 줄에는 백준이가 승리하기 위해 건설해야 할 건물의 번호 W가 주어진다.

## 출력

건물 W를 건설완료 하는데 드는 최소 시간을 출력한다. 편의상 건물을 짓는 명령을 내리는 데는 시간이 소요되지 않는다고 가정한다.

건설순서는 모든 건물이 건설 가능하도록 주어진다.

## 제한

- 2 ≤ N ≤ 1000
- 1 ≤ K ≤ 100,000
- 1 ≤ X, Y, W ≤ N
- 0 ≤ Di ≤ 100,000, Di는 정수

## 풀이

우선 방향이 있는 그래프로, N은 노드의 개수, K는 엣지의 개수를 의미한다.

근데 그래프에 사이클이 존재하면 시작 지점을 알 수가 없기 때문에 **위상정렬**로 생각해야할 것 같다

> 위상정렬
>
> 1. 모든 노드의 진입 차수를 계산한 배열 생성
> 2. 진입 차수가 0인 노드들을 큐에 삽입
> 3. 큐에서 노드를 빼고, 연결된 정점들의 진입 차수 배열 1 감소
>    1. 연결된 노드의 진입 차수가 0이 됐다면 큐에 해당 노드 삽입
>    2. 다시 3으로 돌아가 큐가 빌 때까지 반복
> 4. 종료
>
> [출처](https://my-coding-notes.tistory.com/306)

```python
n, k = map(int, input().split())

# 1
cntLink = [-1] + [0] * n
link = [[] for _ in range(n+1)]
for _ in range(k):
    f, b = map(int, input().split())
    cntLink[b] += 1
    link[f].append(b)

# 2
q = deque
for i in range(1, n+1):
    if not cntLink(i):
        q.append(i)

# 3
result = []
while q:
    v = q.popleft()
    result.append(v)
    for i in link[v]:
        cntLink[i] -= 1
        if not cntLink[i]:
            q.append(i)
```

해당 코드는 순환하는 그래프에선 작동하지 않지만 해당 문제에서는 순환이 없기 때문에 사용가능하다

또한 해당 문제에서는 어떤 작업이 끝나야 이후 작업이 수행 가능하므로 dynamic programming(dp)를 활용해야한다. dp의 주요 특징은 **메모이제이젼**이기 때문에 이를 사용하여 해결한다.

> dp 적용하기 위해 충족되어야하는 조건
>
> 1.  최적 부분 구조 : 큰 문제의 최적 해결 방법이 구성하는 작은 문제들의 최적 해결 방법으로부터 구해질 수 있다
> 2.  중복되는 부분 문제 : 동일한 작은 문제를 반복적으로 해결해야 하는 문제

즉, 위상정렬을 통해 작업 순서를 정하고, dp를 통해 소요되는 시간을 최적화하면 된다

```python
from collections import deque
import sys
input = sys.stdin.readline

for _ in range(int(input())):  # testcase
    n, k = map(int, input().split())
    delay = [0] + list(map(int, input().split()))
    cntLink = [-1] + [0] * n  # 현재 연결된 노드 개수
    link = [[] for _ in range(n+1)]  # 진입 차수 계산한 리스트
    for _ in range(k):
        f, b = map(int, input().split())
        cntLink[b] += 1
        link[f].append(b)
    w = int(input())

    q = deque()
    for i in range(1, n+1):
        if not cntLink[i]:
            q.append(i)

    dp = [0] * (n+1)
    q = deque()
    for i in range(1, n+1):
        if cntLink[i] == 0:
            q.append(i)
            dp[i] = delay[i]

    while q:
        v = q.popleft()
        for i in link[v]:
            cntLink[i] -= 1
            dp[i] = max(dp[i], dp[v] + delay[i])
            if not cntLink[i]:
                q.append(i)

        if cntLink[w] == 0:
            print(dp[w])
            break

```

#python #dynamic_program #graph #topological_sort #directed_acyclic_graph #memoization
