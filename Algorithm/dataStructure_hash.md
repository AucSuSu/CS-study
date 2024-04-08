# 해시(Hash)
: 입력 데이터를 고정된 길이의 데이터로 변환된 값
![image](https://github.com/AucSuSu/CS-study/assets/75782242/bcb9a989-9731-4500-abe1-78c39c0a5c70)

해시 값: 데이터의 key 값이 해시 함수를 통해서 변환된 간단한 정수


정수로 변환된 해시 값은 배열의 인덱스, 위치, 데이터 값을 저장하거나 검색할 때 활용



## 자료구조의 특징
- Key에 데이터(Value)를 매핑할 수 있는 데이터 구조
- 해시 함수를 통해 키를 데이터를 배열에 저장할 수 있는 주소(인덱스 번호)를 계산
- 조회, 저장 속도가 빠름


![image](https://github.com/AucSuSu/CS-study/assets/75782242/41247631-58eb-49e0-be74-278c51c6d49a)
```
해시 함수: 임의의 데이터를 고정된 길이의 값으로 리턴해주는 함수
해시 테이블: Key와 Value를 함께 저장해 둔 데이터 구조
해싱: 해시 함수에서 해시를 출력하고, 해시 테이블에 저장하는 과정까지의 행위
해시 값: 해시 테이블의 index에 해당
```


## 해시 자료구조의 장단점, 용도
### 장점
- 데이터 저장, 읽기 속도가 빠름 -> 시간 복잡도: O(1)
- 키에 대한 데이터가 있는지 확인이 쉬움

### 단점
- 저장공간이 많이 필요
- 여러 키에 해당하는 주소(인덱스)가 동일한 경우 충돌을 해결하기 위한 별도 자료구조가 필요
  - **Chaining 기법**:저장소(bucket)에서 충돌이 나면 새로운 값과 기존 값을 연결 리스트로 연결
  <img width="664" alt="image" src="https://github.com/AucSuSu/CS-study/assets/75782242/2d9079de-f61e-4aeb-a76c-a5eceb8a71d9">
  - **개방주소기법**: 비어있는 해시 값을 찾아 저장하는 기법
  <img width="391" alt="image" src="https://github.com/AucSuSu/CS-study/assets/75782242/915a565c-56bc-4793-902a-c1a0f7a11caa">


### 용도
- 검색이 많이 필요한 경우
- 저장, 삭제, 읽기가 빈번한 경우
- 캐시 구현