# Composite 패턴
> 클라이언트가 개별 객체와 복합 객체를 **동일**하게 다룰 수 있도록 함.
> 객체들을 트리 구조로 구성, 개별 객체와 복합 객체를 일관된 방식으로 처리 가능하게 함.

## Composite 패턴의 구성 요소
**1. Component:**
- 개별 객체와 복합 객체에 대한 공통 인터페이스 정의
- 클라이언트는 Component 인터페이스를 통해 모든 객체를 동일하게 처리 가능
**2. Leaf:**
- Component 인터페이스를 구현하는 개별 객체를 나타냄
- Leaf는 더 이상의 자식을 가지지 않음
**3. Composite:**
- 여러 개의 Component를 갖는 복합 객체를 나타냄
- 자식 Component를 관리할 수 있는 메소드를 제공

![image](https://github.com/AucSuSu/CS-study/assets/64372881/864e1c59-de3f-4a32-9296-c8121fd6b960)

## Composite 패턴의 목적 및 사용
- 컴포지트 패턴은 클라이언트가 복잡한 트리 구조를 가진 객체를 단일 객체처럼 쉽게 다룰 수 있도록 함
- 객체의 추가, 제거 작업을 투명하게 처리 가능
- 파일 시스템에서 폴더와 파일을 동일한 방식으로 취급 가능.<br>(폴더는 Composite, 파일은 Leaf)

## 예시
- **Composite**: FileSystemElement
- **Leaf**: File
- **Composite**: Directory

``` java
interface FileSystemElement {
    void print();
}

class File implements FileSystemElement {
    private String name;

    public File(String name) {
        this.name = name;
    }

    @Override
    public void print() {
        System.out.println("File: " + name);
    }
}

class Directory implements FileSystemElement {
    private List<FileSystemElement> elements = new ArrayList<>();
    private String name;

    public Directory(String name) {
        this.name = name;
    }

    public void add(FileSystemElement element) {
        elements.add(element);
    }

    public void remove(FileSystemElement element) {
        elements.remove(element);
    }

    @Override
    public void print() {
        System.out.println("Directory: " + name);
        for (FileSystemElement element : elements) {
            element.print();
        }
    }
}
```

## 장점
- **투명성**: 클라이언트는 개별 객체와 복합 객체를 구분하지 않고 동일한 인터페이스를 통해 다룸
- **단순성**: 트리 구조를 이용하여 복잡한 파트와 전체를 동일하게 관리

## 단점
- **디자인 복잡성**: 특정 구성요소에 특화된 기능을 구현할 때 제한적일 수 있음. <br>일부 작업은 Leaf 객체에만 의미가 있거나, 반대로 Composite 객체에만 필요할 수 있다.

## 사용
> 개발자는 복잡한 객체 계층을 보다 쉽게 관리하고, 확장할 수 있다.
- GUI 구성요소
- 그래픽 편집기
- 파일 시스템
