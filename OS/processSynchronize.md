## 프로세스 동기화가 필요한 이유
- 프로세스 동기화(Process Synchronization) : 여러 프로세스가 공유하는 자원의 일관성을 유지하는 것
- 여러 프로세스가 서로 협력해 공유자원을 사용하는 상황에서 경쟁조건(race condition)이 발생하면 공유자원의 신뢰성이 떨어진다. 이를 방지하기 위해 프로세스들이 공유자원을 사용할 때 특별한 규칙을 만드는 것

## race condition이란?
- 여러 프로세스(또는 스레드)가 공유자원에 동시에 접근할 때 공유자원에 대한 접근 순서에 따라 실행 결과가 달라질 수 있는 상황. 동시에 접근할 때 자료의 일관성을 해치는 결과가 나올 수 있다.
- 임계구역(critical section) : 여러 프로세스(또는 스레드)가 자원을 공유하는 상황에서 하나의 프로세스(스레드)만 접근할 수 있도록 제한해둔 영역 -> race condition을 막을 수 있는 해결책이 될 수 있다.


## CSP(Critical Section Problem)란?
- enter section(entry section) : 각 프로세스가 임계구역(critical section)에 들어가기 위해 진입허가 요청을 하는 코드가 있는 부분
- critical section : 한 프로세스가 자신의 임계구역에서 작업을 수행하는 동안에는 다른 프로세스는 그들의 임계구역에 접근할수 없다
- exit section : 임계구역을 빠져나오는 코드가 있는 부분
- remainder section : 나머지 구역


## CSP 문제 해결방안 3가지 
critical section problem에 대한 해결안은 아래 세가지 요구조건을 충족해야한다
<br>
- mutual exclusion(상호배제) : 프로세스 P_i가 자기의 임계구역에서 실행된다면, 다른 프로세스들은 그들 자신의 임계구역에서 실행될수 없다.
- progress(deadlock 회피) (진행) : 임계구역을 사용하고 있지 않다면 다른 프로세스가 접근할 수 있도록 한다
  - 자신의 임계구역에서 실행되는 프로세스가 없고, 그들 자신의 임계구역으로 진입하려고 하는 프로세스들이 있다면, 나머지 구역에서 실행 중이지 않은 프로세스들만 다음에 누가 그 임계구역으로 진입할 수 있는지를 결정하는데 참여할 수 있으며, 이 선택은 무기한 연장될수 있다.
- bounded waiting(starvation 회피) (한정 대기) : 프로세스가 자기의 임계구역에 진입하려는 요청을 한 후부터 그 요청이 허용될 때까지 다른 프로세스들이 그들 자신의 임계구역에 진입하도록 허용되는 횟수에 한계가 있어야 한다.
  - 임계구역 진입횟수에 한계를 두어 같은 프로세스가 계속해서 독점해서 사용하지 못하게 한다. -> 다른 프로세스들이 기아상태에 빠지지 않게 한다

## monitor란?
- 세마포어의 경우 세마포어를 이용해 임계구역 문제를 해결할 때 프로그래머가 세마포어를 잘못 사용하면 다양한 유형의 오류가 난다. 이를 막기 위한 고급 언어 구조물 중 하나
- 하나의 lock과 여러 condition variable로 구성된다. lock을 이용하여 여러개의 쓰레드가 동시에 critical section에 접근하지 못하도록 제어역할 수행

## syncronized의 의미
- Thread 사이의 동기화를 맞추는 기법
- 각 일반 Instance안에 존재하는 Monitor를 이용하여 Thread 사이의 동기화를 수행

## wait()와 notify(), notifyAll() 메소드의 의미
- wait() : Instance의 Monitor의 Condition Variable을 이용하여 구현. Condition Variable에서 대기중인 Thread는 notify(), notifyAll() 함수를 통해서 깨울수 있다.
- notify() : Condition Variable에서 대기중인 임의의 하나의 Thread만을 깨우는 동작을 수행
- notifyAll() : Condition Variable에서 대기중인 모든 Thread를 깨운다. 모든 Thread가 깨어나도 하나의 Thread만 Lock을 획득하고 나머지 Thread들은 다시 대기한다.
