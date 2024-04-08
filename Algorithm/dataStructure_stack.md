## 스택
### 스택이란?
- 후입선출(LIFO: Last-In-First-Out)
- e.g. 뒤로가기, 되돌리기 기능
![image](https://github.com/AucSuSu/CS-study/assets/64372881/01fa144c-f124-4fb1-a5e8-9a5e7a69b08e)

### 스택 구현
- push: 객체를 스택의 최상위에 삽입
- pop: 스택 최상위 객체를 스택에서 제거
- top: 스택 최상위 객체의 레퍼런스를 반환(제거는 X)
- size: 스택 내 객체의 개수 반환
- isEmpty: 스택이 비어있는지 확인

### 스택 과정
**[push]**
- 스택이 가득 차있는지 확인
  - full이라면 오류 발생 후 종료
- 가득 차있지 않다면 top을 1 증가
- top에 객체를 삽입

**[pop]**
- 스택이 비어있는지 확인
  - 비어있으면 오류 발생 후 종료
- 비어있지 않다면 top이 가리키는 요소를 삭제
- top 1 감소
- 삭제된 요소 반환

### [배열 기반]
![image](https://github.com/AucSuSu/CS-study/assets/64372881/6f33095a-e88a-46a9-a425-f9cc194f98cb)

### [링크드 리스트 기반]
- 추가
![image](https://github.com/AucSuSu/CS-study/assets/64372881/09671578-729e-47b6-aa7a-29748bd51e59)
- 삭제
![image](https://github.com/AucSuSu/CS-study/assets/64372881/8c47da89-ff1a-4791-829c-1e60a9e87b6d)
