## 트리의 개념

- 비선형 구조 (1: N 또는 N : N의 관계) 
- 원소들 간에 1 : N 관계를 가지는 자료구조
- 원소들 간에 계층관계를 가지는 계층형 자료구조
- 상위 원소에서 하위 원소로 내려가면서 확장되는 나무(트리) 모양의 구조

<br/>

##  트리 용어정리

![](https://media.geeksforgeeks.org/wp-content/uploads/20221124153129/Treedatastructure.png)

#### **한 개 이상의 노드로 이루어진 유한 집합이며 다음 조건을 만족한다.**

- 노드 중 최상위 노드를 루트(root)라 한다.
- 나머지 노드들은 n개의 분리 집합(subtree)으로 분리 될 수 있다. 

> #### 노드(node) - 트리의 원소

- 트리의 노드 : A ~ P

> #### 간선 - 노드를 연결하는 선. 부모 노드와 자식 노드를 연결

> #### 루트 노드(root node) - 트리의 시작 노드

- 트리의 루트 노드 : A

> #### 형제 노드(sibling node) - 같은 부모 노드의 자식 노드들

- B,C 는 형제 노드

> #### 조상 노드 - 간선을 따라 루트 노드까지 이르는 경로에 있는 모든 노드들

- K의 조상 노드 : H, D, B, A

> #### 서브 트리(subtree) - 부모 노드와 연결된 간선을 끊었을 때 생성 되는 트리

> #### 자손 노드 - 서브 트리에 있는 하위 레벨의 노드들

- D의 자손 노드 : H, K, L

> #### 차수(degree)

- 노드의 차수 : 노드에 연결된 자식 노드의 수.
  - B의 차수 = 2, D의 차수 = 1

- 트리의 차수 : 트리에 있는 노드의 차수 중에서 가장 큰 값
- 단말 노드(리프 노드) : 차수가 0인 노드. 자식 노드가 없는 노드.

> #### 높이

- 노드의 높이 : 루트에서 노드에 이르는 간선의 수. 노드의 레벨
  - B의 높이 = 1, F의 높이 = 2
- 트리의 높이 : 트리에 있는 노드의 높이 중에서 가장 큰 값. 최대 레벨
  - 트리의 높이 = 4

<br/>

## 이진 트리

> #### 이진 트리

**모든 노드들이 둘 이하(0,1,2 개)의 자식을 가진 트리**이다.

![](https://velog.velcdn.com/images/dlgosla/post/0faeb483-bc6f-4a83-93fc-dad54ffeed7a/image.png)

> #### 이진 탐색 트리(Binary Search Tree, BST)

**왼쪽 자식은 부모보다 작고 오른쪽 자식은 부모보다 큰 이진 트리**이다.

![](https://velog.velcdn.com/images/dlgosla/post/55da2b0a-a375-4898-9120-1ec935e45f6b/image.png)

BST는 삽입, 삭제, 탐색과정에서 모두 트리의 높이만큼 탐색하기 때문에 O(logN)의 시간 복잡도를 가진다.

문제는 트리가 편향트리가 되어버렸을 때 결국 배열과 다름 없어지고 시간 복잡도는 O(N)이 된다.

중위순회(inorder traversal)를 하면, 오름차순으로 정렬된 순서로 Key값을 얻을 수 있다.

참고로 순회 종류는 아래와 같다.

 **전위 순회(preorder traverse)** : 뿌리(root)를 먼저 방문

- 뿌리 -> 왼쪽 자식 -> 오른쪽 자식
  ( 8 -> 1 -> 3 -> 6 -> 4 -> 7 ....)

**중위 순회(inorder traverse)** : 왼쪽 하위 트리를 방문 후 뿌리(root)를 방문

- 왼쪽자식 -> 뿌리 -> 오른쪽 자식
  ( 1 -> 3 -> 4 -> 6 -> 7 -> 8 -> ...)

**후위 순회(postorder traverse)** : 하위 트리 모두 방문 후 뿌리(root)를 방문

- 왼쪽자식-> 오른쪽 자식 -> 뿌리
  (1 -> 4 -> 7 -> 6 -> 3 -> 13 -> ..)


> #### 완전 이진 트리**
**모든 레벨의 노드가 완전히 채워져 있고, 모든 리프 노드가 같은 레벨에 있는 이진트리**

![](https://velog.velcdn.com/images/dlgosla/post/a1cf3edc-11ba-4753-8915-92ed281d217f/image.png)

> #### 편향 이진 트리(skewed binary tree)

같은 높이의 이진 트리 중에서 최소 개수의 노드 개수를 가지면서

왼쪽 혹은 오른쪽 서브트리 만을 가지는 이진트리 이다.

즉 **모든 노드가 왼쪽에 있거나 반대로 오른쪽에 있는 트리**이다.

![](https://velog.velcdn.com/images/dlgosla/post/25081d19-cb52-474b-938e-3b68ccec1ecd/image.png)

> #### 균형 이진 트리(balanced binary tree)

높이 균형이 맞춰진 이진트리이다.

**왼쪽과 오른쪽 트리의 높이 차이가 모두 1만큼 나는 트리**이다.

![](https://velog.velcdn.com/images/dlgosla/post/3c46257d-5882-45f8-abdc-d560d9c7b0d2/image.png)

> #### 높이 균형 이진 탐색 트리 (Adelson-Velsky and Landis, AVL 트리)

![](https://velog.velcdn.com/images/dlgosla/post/781dac3d-c6de-4a28-85c8-c8968cf90933/image.png)

균형을 잡을 때 핵심되는 것은 바로 Balance Factor라는 것인데

Balance Factor는 왼쪽과 오른쪽의 자식의 높이 차이를 뜻한다.

이 때 균형이 잡혔다는 것은 **BF가 최대 1까지 차이나는 것**, 즉 -1부터 1까지일 때를 의미한다.



이진탐색 트리는 삽입, 삭제, 탐색 모두 평균은 O(logN)이지만 최악은 **O(N)**인데,

AVL트리는 삽입, 삭제, 탐색 모두 평균이든 최악의 경우든 **O(logN)**이다.



AVL 트리에서 삽입, 삭제 연산을 수행할 때 트리의 균형을 유지하기 위해

LL-회전, RR-회전, LR-회전, RL-회전연산이 사용된다.

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F40606ff5-6e87-4349-a569-92bcaf582c93%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.13.23.png&blockId=155c65bd-4341-4303-9cab-179395e4c5e8)

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F0a075c48-12ec-4079-a3d5-4b35c67dffef%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.13.41.png&blockId=e8147d78-19f6-435a-98e0-3478ffa787c9)

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F403af98d-adcc-402e-b91a-3288c87dbc76%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.13.50.png&blockId=38cbcda6-0fd3-4fe1-bed1-a65904cf8723)
![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fce32da22-e0d3-42e4-8656-acf3926022fe%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.14.01.png&blockId=fd63df1b-0f2e-46b9-9159-1ea5b4d6de1f)

> #### Red-Black 트리

![](https://velog.velcdn.com/images/dlgosla/post/45995d9f-715c-4943-a4a5-7532e7541e0c/image.png)

1. 모든 노드는 빨간색 혹은 검은색이다.
2. 루트 노드는 검은색이다.
3. 모든 리프 노드(NIL)들은 검은색이다. (NIL : null leaf, 자료를 갖지 않고 트리의 끝을 나타내는 노드)
4. 빨간색 노드의 자식은 검은색이다.  

   No Double Red(빨간색 노드가 연속으로 나올 수 없다)

5. 모든 리프 노드에서 Black Depth는 같다.

   리프노드에서 루트 노드까지 가는 경로에서 만나는 검은색 노드의 개수가 같다.
