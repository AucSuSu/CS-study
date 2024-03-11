# :bell: 인덱스란?
데이터를 빠르게 찾을 수 있도록 하는 장치<br>
-> DB의 **검색 성능 향상**시킬 수 있는 장치
![image](https://github.com/AucSuSu/CS-study/assets/75782242/95583edf-ed8a-47e9-b854-7b814cd14be6)

- 데이터가 수십만 개가 들어 있을 때, 특정 컬럼 값을 조회하기 위해서, 데이터베이스를 처음부터 끝까지 조회하면 트래픽에 따라 성능 저하가 일어남
- 이를 해결하기 위해서 자주 조회되는 column에 대한 Index Table을 따로 만들어 select 문이 들어왔을 때, **정렬된** Index Table의 값들로 결과 값 조회
- 검색 연산 실행 시, 성능 향상 가능

> 데이터 = 책의 내용<br>인덱스 = 책의 목차<br>물리적 주소 = 책의 페이지 번호




## 1. 인덱스의 장단점
### :grey_exclamation: 장점
1. 데이터가 정렬되어 있기 때문에 테이블에서 검색과 정렬 속도 향상
2. 인덱스를 사용하여 테이블 행의 고유성 강화
3. 시스템의 전반적인 부하 감소

### :grey_exclamation: 단점
1. `insert`, `update`, `delete` 명령어가 입력될 때마다 정렬된 상태를 계속 유지시켜야 함
2. 데이터의 양이 많으면 인덱스 스캔이 성능향상에 도움되지만, 데이터의 양이 적다면 인덱스 스캔보다 풀 스캔이 더 빠름
3. 인덱스를 관리하기 위해서 데이터베이스의 저장공간이 추가 할당되어야 함

### :heavy_check_mark: 인덱스를 사용하면 좋은 경우
- 데이터의 range가 넓고 중복이 적은 컬럼
- 조회가 많거나 정렬된 상태가 유용한 컬럼 == where나 order by, join이 자주 사용되는 컬럼
- insert, update, delete 작업이 자주 발생하지 않는 컬럼

## 2. 인덱스의 자료구조: B+- Tree, Hash Table
### Hash Table
> 컬럼의 값(data)와 물리적 주소를 `(key, value)`의 쌍으로 저장하는 정렬되지 않은 자료구조


그런데 많이 사용하지 않는다!


해시 테이블은 등호(=) 연산에 최적화되어 있는데 데이터베이스에서는 주로 부등로(<,>) 연산이 더 잦기 때문<br>
### B+ Tree
> 대부분의 DBMS와 오라클에서 중점적으로 사용하고 있는 가장 보편적인 인덱스, `Root Node(기준)`/`Branch Node(중간)`/`Leaf Node(말단)`으로 구성되며 계층적 구조, B- Tree의 확장된 버전


![image](https://github.com/AucSuSu/CS-study/assets/75782242/5ee707ef-ddf7-41b1-aab0-087df36716ef)



1. leaf node에만 key와 data(실제 데이터)가 존재한다. **-> 메모리 손실 줄임**<br>
   -> leaf node가 아닌 노드: 인덱스 노드(자식 포인터 저장), leaf node 자료: 데이터 노드
2. 모든 **leaf node가 연결 리스트로** 이어져 있다. <br>
    -> branch node, root node로 이동하지 않고 leaf node끼리 이동 가능 -> 선형 검사 가능 -> **시간 복잡도 감소**

<br>

### B- Tree
> leaf node 이외의 노드들도 key와 data를 가질 수 있다. 노드 내부는 항상 정렬된 상태이며 현재 노드 안에 있는 데이터의 범위를 이용하여 자식 노드 생성, 각 노드는 최대 (n+1)개의 자식 노드를 가질 수 잇다


![image](https://github.com/AucSuSu/CS-study/assets/75782242/5d4c0fad-dda4-4920-89a4-8710a0ce4147)

- 루트 노드 안에 3개의 값 -> 범위: (0-7), (7-15), (15-)
- 최대 3(2+1)의 자식노드 생성 가능
- 한 데이터 검색에는 효율적이지만, 모든 데이터를 한 번 순회하기 위해서는 트리의 모든 노드를 방문해야 함 -> 이런 단점을 개선하여 **B+ Tree 생성**
<br>



#### B- Tree vs B+ Tree
![image](https://github.com/AucSuSu/CS-study/assets/75782242/643541a3-55c7-46c2-a51b-0d91bdae944b)


## :pushpin: 참고자료
https://velog.io/@alicesykim95/DB-%EC%9D%B8%EB%8D%B1%EC%8A%A4Index%EB%9E%80
https://github.com/Seogeurim/CS-study/blob/main/contents/database/materials/%EC%9C%A4%EA%B0%80%EC%98%81_database_&Index.pdf
https://rebro.kr/167
