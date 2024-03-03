# 프로세스 vs 스레드
|이름|의미|
|--|--|
|프로세스|컴퓨터에서 실행되고 있는 프로그램 즉, 프로그램이 메모리에 올라가 인스턴스화된 것을 의미<br>-> 별도의 메모리 공간에서 실행|
|스레드|프로세스 내 작업의 흐름의 단위<br>-> 동일한 프로세스 내의 공유 메모리 공간에서 실행|

- 프로그램이 메모리에 올라가면 프로세스가 되는 인스턴스화가 일어나고, 운영체제의 CPU 스케줄링에 따라 CPU가 프로세스 실행

<br>

# 컴파일 과정
```
컴파일: 인간이 이해할 수 있는 언어로 작성된 소스 코드(고수준 언어 : C, C++, Java 등)를 CPU가 이해할 수 있는 언어(저수준 언어 : 기계어)로 번역(변환)하는 작업
-> 컴파일 과정으로 소스코드가 0,1로 이루어진 기계어로 변환된 실행파일이 된다!
```


<img width="803" alt="image" src="https://github.com/AucSuSu/CS-study/assets/75782242/39c2601f-b9df-4f17-8767-23f2080933e0">

1. 전처리: 전처리기(preprocessor)를 통해 소스코드 파일(*.c)를 전처리된 ***소스코드 파일(*.i)로 변환**하는 과정


   - 소스 코드의 **주석 제거**: 컴퓨터는 주석의 내용을 이해할 필요가 없기 때문
   - #include 등의 **헤더 파일 병합** : 헤더 파일의 모든 내용을 복사해서 소스 코드에 삽입
   - **매크로 치환 및 적용**: #define 지시문에 정의된 매크로를 저장하고 같은 문자열을 만나면 #define된 내용으로 치환


2. 컴파일러: 컴파일러(compiler)를 통해 전처리된 소스코드 파일(*.i)를 ***어셈블리어 파일(*.s)로 변환**하는 과정


   - 오류 처리: 문법 검사
   - static한 영역(data, bss 영역)들의 메모리 할당
   - 코드 최적화 작업으로 0과 1로 이루어진 어셈블리어로 변환


3. 어셈블러: 어셈블러(assembler)를 통해 어셈블리어 파일(*.s)을 ***오브젝트 파일(*.o)로 변환** 하는 과정

4. 링킹: 링커(Linker)를 통해 라이브러리 파일과 오브젝트 파일 코드(*.o)를 결합하여 ***실행 파일(*.exe, *.out)로 변환*** 하는 과정
   - 정적 라이브러리: 프로그램 빌드 시, 라이브러리가 제공하는 모든 코드를 실행 파일에 넣는 방법
   - 동적 라이브러리: 프로그램 실행 시, 필요할 대만 DLL이라는 함수 정보를 통해 참조하여 라이브러리를 쓰는 방법


<br>


# 프로세스의 상태
<img width="628" alt="image" src="https://github.com/AucSuSu/CS-study/assets/75782242/dfc373bc-de39-4623-ab13-95da5cd7fc54">

|상태|설명|
|---|---|
|생성(create)|프로세스가 생성된 상태, fork() 또는 exec() 함수를 통해 PCB가 할당|
|준비(ready)|CPU 스케줄러로부터 CPU 소유권이 넘어오기를 기다리는 상태|
|대기 중단 상태(ready suspended)|메모리 부족으로 일시 중단된 상태|
|실행(running)|CPU 소유권과 메모리를 할당 받고 명령어를 수행 중인 상태|
|대기(waiting/blocked)|어떤 이벤트가 발생한 후, 기다리며 프로세스가 차단된 상태|
|일시 중단 상태(blocked suspended)|blocked된 상태에서 프로세스가 실행되려고 했지만, 메모리 부족으로 일시 중단된 상태|
|종료(terminated)|메모리와 CPU 소유권을 모두 놓고 가는 상태|

1. Admitted(생성 -> 준비): 준비 큐가 비어있을 때 작업 스케줄러에 의해 실행
2. Dispatch(준비 -> 실행): 스케줄러에 의해 준비 큐 맨 앞에 있는 프로세스에게 CPU 할당
3. Blocked(실행 -> 대기): CPU를 할당받은 프로세스가 입출력 작업 등으로 인해 명령을 실행할 수 없는 상태
4. Timeout(Interrupt)(실행 -> 준비): CPU를 점유 중인 프로세스가 할당된 시간을 모두 사용하여 타임아웃되거나, CPU 스케줄링 정책에 따라 우선순위가 높은 프로세스로 CPU 디스패치된 상태
5. Wake up(대기 -> 준비): block 상태의 프로세스가 입출력 작업이 끝나면 대기 상태에서 준비 상태가 됨
6. Exit(실행 -> 종료): 프로세스가 CPU를 할당받아 작업을 모두 수행한 상태


<br>


# 프로세스의 메모리 구조

<img width="666" alt="image" src="https://github.com/AucSuSu/CS-study/assets/75782242/8f802979-be4d-4d70-8781-7cf4883e2e6d">

메모리 공간(RAM)은 **코드 영역, 데이터 영역, 스택, 힙**으로 구성
- 정적 영역: 코드 영역, 데이터 영역
- 동적 영역(동적 할당:Dynamic Memory Allocation): 스택, 힙



### 1. 코드 영역
- 작성한 소스 코드가 **기계어 형태(0과 1)로 저장**되는 영역
- 실행 파일을 구성하는 **명령어들이 올라가는 메모리 영역**으로 함수, 제어문, 상수 등이 지정
- CPU는 코드 영역에 저장된 명령어들을 하나씩 실행
### 2. 데이터 영역
- code segment: 프로그램의 코드가 할당되는 영역
- BSS segment: 전역변수와 정적(static)변수가 할당되는 영역
- 메인 함수 전에 선언되어 **프로그램의 시작과 동시에 할당**되고 **프로그램이 종료되어야 메모리 소멸**
### 3. 힙 영역
- **런타임 시 크기가 결정**
- 프로그래머가 할당
- 응용 프로그램이 종료될 때까지 메모리 유지 -> **사용 후 메모리 해제**해주어야 한다(Java에서는 가비지 컬렉터가 자동 해제)
- **참조형 데이터 타입(Reference Type)을 갖는 객체(인스턴스), 배열이 저장**
- **FIFO**의 구조: 메모리의 낮은 주소부터 순서대로 할당
### 4. 스택 영역
- **컴파일 시 크기 결정**
- 프로그램이 자동으로 사용하는 임시 메모리 영역
- 함수 호출 시 생성되는 **지역 변수, 매개변수 저장**
- 함수 호출이 완료되면 저장된 메모리도 해제 -> 잠깐 사용하고 삭제하는 데이터
- **LIFO**의 구조: 메모리의 높은 주소부터 할당
- 힙 영역보다 빠른 액세스: 할당, 해제가 빠름