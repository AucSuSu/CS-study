### 큐(Queue)
 - 먼저 추가한 데이터를 먼저 반환/삭제하는 선입선출(FIFO - First In First Out) 형식의 선형 또는 원형 자료구조이다.
 - 데이터를 추가한 순서대로 제거할 수 있기 때문에 비동기 메세징(asynchronous messaging), 너비 우선 탐색(breath first search) 등에서 널리 사용되고 있다.
 - 삽입 혹은 샂게되는 데이터가 비어 있으면 underflow라고 하고, 데이터가 가득 차 있어 더 이상 추가할 수 없는 상태를 overflow라고 한다.

## 큐의 특징
- 스택의 저장구조와 같지만 스택은 top을 통해서만 삽입, 삭제가 이루어졌다.
- 큐는 삽입, 삭제가 다른 방향에서 이루어진다.
- deque를 사용하여 큐 자료구조를 구현(doubly-ended-queue)
- deque는 내부적으로 doubly linked list 구현
- 이때, 삭제 연산만 수행되는 곳을 프론트(front), 삽입 연산만 수행되는 곳을 리어(rear)라고 한다.
 - 프론트(front): 삭제 연산이 발생하며 디큐(deQueue)라고 한다.
 - 리어(rear): 삽입 연산이 발생하고 인큐(enQueue)라고 한다.
 - → Queue는 가장 먼저 삽입된 데이터가 가장 먼저 삭제된다는 특징을 가지고 있다.

<br>

## 큐의 구현 방법

1. list
 - 장점 : 배열 내 요소의 주로를 나타내는 인덱스를 통해 원하는 데이터를 빠르게 검색 가능
 - 단점 :
    1) 선언할 때 크기가 고정되어 배열에 들어있는 데이터 양에 따라 배열의 크기조정이 필요하다.
    2) 데이터를 중간에 삽입하거나 삭제할 경우 해당 데이터 뒤에 있는 요소들의 위치를 모두 이동시켜야하는 비효율적인 상황이 발생

```
class ListQueue(object):
    def init(self):
        self.queue = []

    def dequeue(self):
        if len(self.queue) == 0:
            return -1
        return self.queue.pop(0)

    def enqueue(self, n):
        self.queue.append(n)
        pass
```

<br>

2. dequeue

- deque의 popleft()와 appendleft(x) 메서드는 모두 O(1)의 시간 복잡도를 가지기 때문에 리스트에 비해 훨씬 효율적이다.
 - collections 모듈의 deque는 'double-ended queue'의 약자로 데이터를 양방향에서 추가하고 제거할 수 있다.

 - popleft() 메서드를 제공하며, 이를 사용시 첫번째 데이터를 제거할 수 있다. 

 - appendleft(x) 메서드를 사용하면 데이터를 맨 앞에서 삽입할 수 있다.

```
from collections import deque

dq = deque([])

dq.append(1)
dq.popleft()
```

<br>

3. PriorityQueue

    - priorityQueue는 우선순위 큐라고 해서, 기본적으로 선입선출이라는 형태로 짜여진 Queue와는 조금 다르게, '데이터 우선순위'에 기반하여 우선순위가 높은 데이터가 나온다.

    - 따로 정렬방식을 지정하지 않으면 낮은 숫자가 높은 우선순위를 갖는다.

    - 우선순위 큐는 힙(heap)이라는 자료 구조를 통해 구현할 수 있다.
    - 우선순위 큐는 최소한 두 가지 연산이 지원되어야 한다.
        1) 하나의 원소를 우선순위를 지정하여 추가하는 함수(push)
        2) 가장 높은 우선순위를 가진 원소를 큐에서 제거하고 반환하는 함수(pop)

<br>

4. linkedList

 - 장점 : 
    1) 데이터 양에 상관없이 크기가 동적으로 조절된다.
    2) 인덱스 대신 이전 데이터 그리고 다음 데이터의 위치를 기억하는 노드 형태를 이용한다.
    3) 배열 중간에 데이터 삽입, 삭제 시 노드들 사이에 연결된 링크들을 끊어주거나 이어주면 되기 때문에 그 과정이 용이하다.

- 단점 : 데이터에 접근할 때 연결되어 있는 노드를 따라 양 끝에서부터 순차적으로 접근해야하기 때문에 데이터 접근속도가 배열에 비해 느리다.

<br>

5. Circular Queue
위 1번 코드에서 배열로 구현된 선형 큐에는, pop 연산 횟수와 관계없이 삽입할 수 있는 아이템 개수가 maxlen를 넘을 수 없다는 비효율성이 존재한다.
이때 나머지 연산 %를 활용해 원형 큐(Circular Queue)를 구현하면 이러한 단점을 극복할 수 있다.
원형 큐는 마지막 위치가 시작 위치와 연결된다는 점에서 링 버퍼(Ring Buffer)라고도 불린다.

