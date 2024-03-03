# 멀티스레드
멀티스레드란?
한 프로세스가 하나의 스레드를 이용하여 한 번에 한 작업만 수행하는 것은 싱글 스레드(Single thread),
한 프로세스가 여러 스레드로 동시에 여러 작업을 수행하는 것을 멀티 스레드(Multi thread) 라고 한다.

# 멀티 스레드 사용이유
- **프로세스를 생성하거나 컨텍스트 스위칭 하는 작업**은 너무 무겁고 잦으면 성능 저하가 발생하지만, 스레드를 생성하거나 스위칭 하는것은 그에 비해 **가볍다**.
- 두 프로세스가 하나의 데이터를 공유하려면 메시지 패싱이나 공유 메모리,파이프를 사용해야 하는데 이건 효율도 떨어지고 개발자가 구현, 관리하기 번거롭다.

# 멀티 스레딩

한 프로세스 내에 멀티 스레딩 방법을 사용하면 아래가 가능하다.

1. 프로세서(CPU 내의 코어)가 여러 개인 경우 멀티 스레드를 통해 병렬성(Parallelism)을 높일 수 있다. -> 동시에 여러 작업이 가능

그래서 프로세스의 스레드들이 각각 다른 프로세서(CPU 내의 코어)에서 병렬적으로 수행된다.

2. 프로세서가 하나이면 동시성(Concurrency)을 높일 수 있다. 실제로는 각각의 시간에 한 작업만 수행되지만, 병렬적으로 수행되는 것처럼 보인다.

![alt text](/img/image.png)

## 멀티 스레딩의 장점
1. 응답성(Responsiveness)
   - 싱글 스레드인 경우, 작업이 끝나기 전까지 사용자에게 응답하지 않는다. 반면 멀티스레드인 경우 작업을 분리해서 수행하므로 실시간으로 사용자에게 응답할 수 있다.

2. 자원 공유(Resource sharing)
    - 프로세스끼리는 오직 공유 메모리나 메시지 패싱을 이용해서 자원을 공유할 수 있지만, 스레드는 자신이 속한 프로세스 내의 스레드들과 메모리나 자원을 공유하여 효율적으로 사용할 수 있다.

3. 경제성(Economy)
    - 프로세스를 새로 생성하는 비용보다 스레드를 새로 생성하는 게 훨씬 싸다. Context switching의 오버헤드 또한 스레드가 더 경제적이다.

4. 확장성(Scalability)

    - 싱글 스레드인 경우 한 프로세스는 오직 한 프로세서(CPU)에서만 수행 가능하다. 반면 멀티 스레드인 경우 한 프로세스를 여러 프로세서에서 수행할 수 있으므로 훨씬 효율적이다.

## 멀티 스레딩의 단점
1. 스레드들이 프로세스의 자원을 공유하기 때문에 한 스레드의 영향이 다른 모든 스레드에 영향을 준다.
2. 하나의 스레드로 구현하는 프로그램보다 훨씬 더 난이도가 높다.


# 스레드 우선 순위
만약 스레드의 개수가 코어의 수보다 많을 경우, 스레드를 어떤 순서에 의해 동시성으로 실행할 것인가를 결정해야 하는데 이것을 **스레드 스케줄링** 이라고 한다.

## 스레드 스케줄링 방식
- 우선 순위 방식
  - 우선 순위가 높은 스레드가 실행 상태를 더 많이 가지도록 스케줄링
  - 개발자가 setPriority() 메소드를 사용하여 우선순위를 설정할 수 있습니다.


- 순환 할당 방식 (Round-Robin)
  - 시간 할당량을 정해서 하나의 스레드를 정해진 시간만큼 실행하고 다시 다른 스레드를 실행하는 방식. JVM에 의해 결정되기 때문에 개발자가 임의로 수정 불가능!