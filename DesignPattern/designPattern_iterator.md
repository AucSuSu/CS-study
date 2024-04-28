# Iterator Pattern (이터레이터 / 반복자)

## 0. 개념

- 행위 패턴
- 항목들에 대해서 반복작업을 수행
- <u>**컬렉션 구현 방법을 노출시키지 않으면서도**</u> 그 집합체안에 들어있는 모든 항목에 접근할 수 있게 해 주는 방법을 제공해 주는 패턴

#### 컬렉션이 뭐지 :question:

- 자바에서 제공하는 자료구조들의 인터페이스
- 컬렉션 인터페이스를 상속받는 클래스들에 대해 Iterator 인터페이스 사용이 가능
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDhAEy%2FbtradRzasBQ%2FCjKa3OnW5k8tGYrkrqjVJ1%2Fimg.png)

### 기존의 반복문은 어떻게 쓰는가

```java
for(int i=0;i<arr.length;i++){

System.out.println(arr[i]);

}
```

- _만약 arr이 배열이 아니라, 다른 자료구조라면? => **반복문은 수정 되어야 함**_

- LinkedList

```java
LinkedList<Integer> arr = new LinkedList<>();
// 또는 다른 자료형을 사용할 수 있습니다.

for (int i = 0; i < arr.size(); i++) {
    System.out.println(arr.get(i));
}
```

- HashSet

```java
HashSet<Integer> set = new HashSet<>();

Object[] elements = set.toArray();

for (int i = 0; i < elements.length; i++) {
    System.out.println(elements[i]);
}
```

### :star: _통일해서 사용할 수는 없을까? 라는 아이디어에서 시작 => Iterator (반복자)_

## 1. 요소

![](https://velog.velcdn.com/images%2Fcham%2Fpost%2F58959069-44eb-40c7-aacb-368f168477c5%2Fimage.png)

### 집합체 (Aggregate)

```java
// Aggregate interface
public interface ItemCollection {
    Iterator<String> createIterator();
}

```

### 집합체 구현체 (Concrete Aggregate)

```java
// Concrete Aggregate
public class ShoppingList implements ItemCollection {
    private List<String> items;

    public ShoppingList(List<String> items) {
        this.items = items;
    }

    @Override
    public Iterator<String> createIterator() {
        return new ItemIterator(items);
    }
}

```

### 반복자 (Iterator)

```java
// Iterator interface
public interface Iterator<T> {
    boolean hasNext();
    T next();
}

```

### 반복자 구현체 (Concrete Iterator)

```java
// Concrete Iterator
public class ItemIterator implements Iterator<String> {
    private List<String> items;
    private int position = 0;

    public ItemIterator(List<String> items) {
        this.items = items;
    }

    @Override
    public boolean hasNext() {
        return position < items.size();
    }

    @Override
    public String next() {
        if (hasNext()) {
            return items.get(position++);
        }
        return null;
    }
}

```

### Client에서 사용하기

```java

public class Main {
    public static void main(String[] args) {

        // List 
        List<String> shoppingItemsList = Arrays.asList("Apple", "Banana", "Milk", "Eggs", "Bread");
        ItemCollection shoppingList = new ShoppingList(shoppingItemsList);

        Iterator<String> iterator = shoppingList.createIterator(); // iterator
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }

        // HashSet
        Set<String> shoppingItemsSet = new HashSet<>(Arrays.asList("Apple", "Banana", "Milk", "Eggs", "Bread"));
        shoppingList = new ShoppingList(new ArrayList<>(shoppingItemsSet));

        iterator = shoppingList.createIterator(); // 동일 iterator, 동일 방식 
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }

        // LinkedList
        LinkedList<String> shoppingItemsLinkedList = new LinkedList<>(Arrays.asList("Apple", "Banana", "Milk", "Eggs", "Bread"));
        shoppingList = new ShoppingList(shoppingItemsLinkedList);

        iterator = shoppingList.createIterator(); // 동일 iterator, 동일 방식 
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
}


```

### 특징

- 클라이언트는 Concrete Aggregate 인터페이스를 통해 컬렉션에 접근 => 즉, 실제 세부 구현사항(구현체)은 숨길 수 있음
- 배열, 리스트, 맵 어떤 구조에서도 사용가능한거임 !

## 2. 장단점

### 장점

- 구현과 사용을 분리시켜줌 : 내부 구현을 모르고도 사용가능하므로
- 유연성 & 확장성을 가짐 : 새로운 컬렉션 타입을 추가하거나 기존의 컬렉션을 변경해도 클라이언트 코드를 수정할 필요가 없음 => 같은 Iterator를 쓰면 되니까

> 집합체를 다룰 때는 개별적인 클래스에 대해 데이터를 읽는 방법을 알아야 하기 때문에 각 컬렉션에 접근이 힘듦 =>  Iterator를 쓰게 되면 어떤 컬렉션이라도 동일한 방식으로 접근이 가능하기에 그 안에 있는 항목들에 접근할 수 있는 방법을 보다 쉽게 제공
### 단점

- 오버헤드 발생 : 단순 순회만 구현하는 경우 클래스가 복잡해짐 (객체생성, 메서드 호출, hasNext(인덱스 검사)...)

## 3. For-each (강화된 for 반복문)

- 근데 사실.. 자료구조가 다 다르더라도, for-each를 사용하면 되는거 아닌가?
  - _엥? for-each는 그러면 뭐지?_

```java
for (Integer element : set) {
    System.out.println(element);
}
```

### ✔️ For-each 반복문은 이터레이터의 단점을 극복하기 위해 나온 강화된 반복문이다.

- for-each도 내부적으로는 iterator의 구현체임

#### Iterator의 단점

- Iterator타입의 변수를 생성하고 초기화해야함
- Iterator를 직접 다뤄야함

#### For-each

- 간결한 문법 사용 : 반복자를 직접 다루지 않고도 간결하게 요소에 접근할 수 있음
- 안전성 : 읽기 전용이기 때문에, 실수로 접근해서 수정, 삭제할 일이 없음
  - 수정, 삭제 시도시 ConcurrentModificationException 발생
- 빈 컬렉션에 대해서도 추가적인 null 체크가 필요하지 않음. => hasNext() 필요 없음

#### 언제 쓰지?

- 간편하게 조회만 할 용도라면 읽기 전용인 For-each를 써서 가독성도 높이고, 간편하게 구현하기!
- 수정, 삭제가 필요하다면 Iterator사용하기
