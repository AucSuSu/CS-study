# 캐시
> 캐시 - 데이터나 값을 미리 복사해 놓는 임시 장소

![캐시](https://github.com/AucSuSu/CS-study/assets/139415941/c029836b-6563-494a-87c0-29273661dd79)

**캐시의 필요성**
 - 프로그램을 실행하거나 데이터에 접근할 때, 속도가 빠른 CPU와 상대적으로 느린 RAM간의 속도 차이에 의한 병목 현상이 발생한다.
 - 이렇게 병목 현상에 의해 낭비되는 시간을 줄이기 위해 RAM과 CPU사이에 보다 훨씬 빠른 캐시 메모리를 사용한다.

<br>

**캐시 히트 & 캐시 미스**
- CPU가 RAM 까지 갈 필요 없이 캐시 메모리에서 필요한 데이터를 많이 찾을 수록 캐시의 성능이 좋은 것이다.
- 이렇듯 캐시 메모리에서 필요한 데이터를 찾을 수 있는 경우를 캐시 히트라고 한다.
- 반대로 필요한 데이터를 찾지 못한 경우를 캐시 미스라고 하며, 이 경우 RAM에서 필요한 데이터를 찾아 온다.
- 즉, 캐시 히트를 극대화 시켜야 하며 이를 위해 필요한 개념이 캐시의 지역성이다.

<br>
<br>

# 캐시의 지역성
> 지역성(Locality) - 프로그램이나 데이터 엑세스 패턴에서 나타나는 특성

**캐시의 지역성**

 - 지역성을 활용하여 캐시에 저장할 데이터를 정하는 방식이다.
 - 지역성의 전제조건은 프로그램은 모든 코드나 데이터를 균등하게 참조하지 않는다는 특성을 기본으로 한다.
 - 즉 프로그램은 어느 한 순간 특정 부분을 집중 참조함을 의미한다.
 - 이러한 경향을 활용하여 캐시 메모리에 저장할 데이터를 정하는 방법으로 캐시 히트를 향상시킬 수 있다.
 - 캐시의 지역성은 대표적으로 시간 지역성과 공간 지역성 그리고 순차적 지역성으로 나뉜다.
<br>

### 시간 지역성

 - 최근 참조된 명령이나 데이터에 다시 접근하려는 특성으로, 동일한 데이터나 명령어에 반복적으로 액세스 되는 경향.

➡️ 비효율적인 예제 
```
// 같은 반복문에서 초기화와 합 계산을 동시에 하여 arr에 대한 접근이 한 번만 일어난다.
let arr = new Array(1000);
let sum = 0;

for (let i = 0; i < 1000; ++i) {
    arr[i] = i;
    sum += arr[i];
}
```
➡️ 효율적인 예제
```
let arr = new Array(1000);
let sum = 0;

for (let i = 0; i < 1000; ++i) {
    array[i] = i;
}

for (let i = 0; i < 1000; ++i) {
    sum += arr[i];
}
```
<br>

### 공간 지역성

- 한 번 접근한 데이터의 주변에 있는 데이터에 접근하려는 특성이다.
- CPU캐시나 디스크 캐시의 경우 한 메모리 주소에 접근할 때 해당 블록 전체를 캐시에 가져온다.

➡️ 비효율적인 예제
```
 // arr의 요소들은 메모리에 연속적으로 저장되는데, 접근은 순차적으로 이뤄지지 않는다.
 for (i = 0; i < columns ; i++){
  for (j = 0; j < rows; j++){
    arr[j][i] = pow(arr[j][i]) 
  }
 }
```
➡️ 효율적인 예제
```
  for (i = 0; i < columns ; i++){
  for (j = 0; j < rows; j++){
    arr[j][i] = pow(arr[i][j])
  }
 }
```
<br>

### 순차적 지역성
- 비순차적 실행이 아닌 이상, 명령어들이 메모리에 저장된 순서대로 실행되는 특성을 고려하여 다음 순서의 데이터가 곧 사용될 가능성이 높은 경향.

- 공간 지역성에 포함하여 설명하기도 한다.

<br>
<br>

# 캐싱 라인
> 캐싱 라인(CachingLine) - 캐시 메모리의 매핑 프로세스


**캐싱 라인의 필요성**
- 캐시 메모리는 메인 메모리에 비해 크기가 매우 작기 때문에, 메인 메모리와 1:1 매칭이 불가능하다.
- 캐시가 아무리 CPU에 가깝게 위치해도 데이터가 캐시 내의 어느 곳에 저장되어 있는지 찾기 어렵다면 비효율적인 시스템이 된다.
- 그래서 캐시에 데이터를 저장할 떄는 캐시 라인이라는 단위의 묶음으로 저장하는데, 빈번하게 사용되는 데이터의 주소들을 일정한 태그들로 묶어 찾기 수월하게 한다.
- 이 때 캐싱 라인은 캐시에 데이터를 저장하는 여러가지 방법을 의미하며, 대표적으로 직접 매핑, 연관 매핑, 직접 연관 매핑으로 나뉜다.
<br>

### 직접 매핑(Direct Mapping)
 - 주기억장치의 블록들이 정해진 캐시 메모리의 위치에 매핑하는 방법.
 - 동일한 캐시 메모리 위치로 매핑된 데이터를 사용할 경우 충돌이 발생하게 된다.
 - 간단하고 구현 비용이 적게 들지만, 적중률이 보장되지 않는다.
<br>

### 연관 매핑(Associative Mapping)
 - 직접 매핑 방식의 단점을 보완한 방식
 - 캐시 메모리의 빈 공간에 임의로 주소를 저장하는 방식으로, 저장은 쉽지만 데이터 탐색이 어렵다.
 - 모든 태그들을 병렬로 검사하기 때문에 적중률은 높으나 복잡하고 비용 또한 높다.
<br>

### 직접 연관 매핑(Set Associative Mapping)
 - 직접 매핑과 연관 매핑의 장점만을 취한 방식으로 주로 사용되는 방식이다.
 - 빈 공간에 임의로 주소를 저장하되, 미리 정해둔 특정 행에만 저장하는 방식.
 - 캐시를 일정한 개수의 집합으로 나눈 뒤, 각 집합마다 캐싱 라인을 할당한다. 그래서 캐시에 데이터를 저장할 떄, 어떤 집합에 저장할지를 먼저 결정한 후에 해당 집한 내에서 빈 캐싱라인을 찾아 데이터를 저장한다.
<br>

# :confetti_ball: 캐시 메모리의 구조

`캐시 라인 !== 캐싱 라인`

![메모리](https://github.com/AucSuSu/CS-study/assets/139415941/2220c70a-4e81-4784-9f93-df267bde5988)
- 캐시 메모리는 S개의 집합으로 이루어져 있고, 각 집합은 E개의 캐시 라인을 지정하고 있다.
- 하나의 캐시 라인은 메인 메모리에서 가져오는 블록 하나(Valid bit)와, 그것이 유효한지 나타내는 유효비트 그리고 동일한 집합에 들어올 수 있는 다른 블록들과 구별하기 위한 Tag를 저장한다.
- 따라서 메인 메모리의 각 블록이 B 바이트라고 할 때, 캐시 메모리의 총 사이즈는 (S * B * E)Byte가 된다.
- 이 때, `세트 당 캐시 라인 수(E) = 1이면, 직접 매핑 캐시, E > 1 이면 직접 연관 매핑 캐시`라고 한다.

# :round_pushpin: References
[https://wookcode.tistory.com/183](https://wookcode.tistory.com/183)

[https://parksb.github.io/article/29.html](https://parksb.github.io/article/29.html)
