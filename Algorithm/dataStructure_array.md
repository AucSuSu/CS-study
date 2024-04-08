# 배열

## 0. 배열의 정의

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F992BD84F5B230A4425)

-   같은 타입의 변수들로 이루어진 집합
-   순서를 가진다
-   메모리상의 고정된 크기의 연속된 공간을 갖기 때문에 <U>한번 생성된 배열의 크기는 변경할 수 없음</U>

### 정적 배열 (우리는 정적배열에 대해 얘기함)

```
int[] staticArray = new int[5]; // 크기가 5인 정적 배열 생성
staticArray[0] = 1;
staticArray[1] = 2;
```

-   일반적으로 우리가 말하는 배열
-   미리 할당된 크기만큼 저장하는 자료구조
-   [장점] : 메모리 관리 간편 (한번 만들면 변경이 없으므로)
-   [단점] : 크기 변경 불가

### 동적 배열

```
ArrayList<Integer> dynamicArray = new ArrayList<>();
dynamicArray.add(1);
dynamicArray.add(2);
```

-   대표적으로, 파이썬의 리스트, Java는 ArrayList
-   저장 공간이 가득 차는 경우 자동적으로 사이즈를 늘려 데이터를 추가/저장 할 수 있는 유동적인 자료구조
-   [장점] : 크기 조정 가능
-   [단점] : 크기 조정을 위해 배열을 재할당하고 요소를 복사하는 작업이 필요하기 때문에 메모리 관리에 추가적인 오버헤드가 발생

### 정적 배열 특징

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FkBcPP%2FbtqUZumg5KK%2FLUdautoyWuykvke1UxtZ50%2Fimg.jpg)
![](https://velog.velcdn.com/images/warmsy/post/a29b2925-4072-446e-8774-aaa9e95add21/image.png)

-   접근 : **O(1)**
-   검색 : **O(n)**
-   추가, 삭제
    -   추가, 삭제해야하는 요소의 위치(index)를 알고 있다면 **O(1)**
    -   추가, 삭제해야하는 요소의 위치를 모른다면 찾아야 하므로 **O(n)**

### 객체 배열의 저장 방식

> 객체 배열의 경우

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FumSd3%2FbtrFJmhG2lR%2Fcka72xnJjGlUASpyrO3hGK%2Fimg.png)

## 1. 정적 배열의 장,단점

### 장점

-   인덱스를 통해서 빠른 접근 가능
-   기록 밀도가 1이다
    -   포인터 등 부가 정보를 가지는게 아니라 데이터 그 자체만 저장하므로 1

> -   기록 밀도 : 데이터 저장 장치(예: 하드 드라이브, 플래시 드라이브)에 저장된 데이터 양을 나타내는 측정 단위

### 단점

-   동일한 데이터 유형을 가짐
-   크기를 생성할 때 무조건 정해야함
-   데이터 삽입, 제거를 할 수 없음 (변경한다는 개념)
-   원소값을 중복 없이 관리할 수 없음

## 2. 정적 배열 활용

-   순차적 데이터 저장하는 경우 (순서가 중요할 때)
-   다차원 데이터를 다룰 때
-   데이터 사이즈가 자주 바뀌지 않을때

## 3. 궁금증

-   :first: 다른 구조의 기록 밀도는 어떻게 되는가

> -   Q. linkedList의 기록 밀도는 1보다 작아?
> -   A. 기록 밀도는 "단위 면적 또는 체적당 저장된 비트의 수" 이다. 링크드 리스트의 각 노드가 다른 노드를 가리키는 포인터를 포함하기 때문에, 메모리에서 요소가 연속적으로 저장되지 않음 =>
>     **단순히 "1보다 작다"고 표현하기 어렵다.**
