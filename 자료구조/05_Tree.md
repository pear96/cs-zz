# 5. Tree

## Tree

![img](https://blog.kakaocdn.net/dn/bxJkfF/btqHO0KMt0f/2kTepCu6RN8reUsE7vG9MK/img.png)

![트리 구조](https://camo.githubusercontent.com/56499d1e8aa6a244a4e5a10c761b613d5828131235197e2157829bc606b656c9/68747470733a2f2f7777772e66756e2d636f64696e672e6f72672f30305f496d616765732f747265652e706e67)

- 비선형 자료구조
- 계층 구조
- `node`와 `branch`를 사용

### 용어

`root`: 트리 구조 중 최상위에 존재하는 노드

`node`: 트리에서 각각의 구성 요소

`level`: 트리에서 각각의 층을 나타내는 단어

`sibling`: 형제 노드, 같은 레벨의 노드

`edge`: 간선, 노드와 노드를 연결하는 선

`parent node`: 부모노드, 한 노드를 기준으로 바로 상위에 존재하는 노드

`child node`: 자식 노드, 한 노드를 기준으로 바로 하위에 존재하는 노드

`leaf node`: 자식 노드가 하나도 없는 노드

### 용도

- 이진 트리의 구조로 탐색 알고리즘 구현을 위해 사용
- 배열의 길이만큼 전부 순회하는 것보다 기준을 가지고 탐색하므로 더욱 빠름
  - 어떤 값을 가진 데이터가 존재하는지 유무를 파악하기 용이

### 구현

1. 데이터와 연결 상태를 저장할 클래스 공간(노드) 생성
2. 각각의 노드들에 값 저장
3. 노드간 연결 상태 정의

> Node를 구성하는 변수와 생성자, 필요한 메서드로 구성된 java 파일

```java
package tree;

import java.util.ArrayList;
import java.util.List;

public class Node {

    private Node parent;
    private ArrayList<Node> childList;
    private int depth;
    private boolean isIfRoot;
    private boolean isIfleaf;
    private Object data;

    public Node(Object data,int depth, boolean isIfRoot, boolean isIfleaf){
        parent=null;
        childList=new ArrayList<Node>();
        this.depth=depth;
        this.data=data;
        this.isIfRoot=isIfRoot; // root 노드 여부
        this.isIfleaf=isIfleaf; // leaf 노드 여부
    }

    // 부모 노드와 연결해주는 메서드
    public void makeParentTree(Node parent){
        if(this.parent!=null){
            this.parent=null;
        }
        this.parent=parent;
    }

    // 자식 노드와 연결해주는 메서드
    public void makeChildTree(Node child){
        this.childList.add(child);
    }

    // 해당 노드의 값을 return
    public Object getData(){ return this.data; }

    // 부모 노드의 값을 return
    public Node getParentTree(){ return this.parent; }

    // 자식 노드의 값을 return
    public List<Node> getChildTree() { return this.childList; }

}
```

> Node를 실제로 생성하고 각 노드 사이의 관계를 연결하는 Main 메서드를 포함한 java 파일

```jaa
package tree;

public class Test {
    public static void main(String[] args) {
        Node node1=new Node("자재",1,true,false);
        Node node2=new Node("공사물품",2,false,false);
        Node node3=new Node("락볼트",3,false,false);
        Node node4=new Node("락볼트22",4,false,true);
        Node node5=new Node("락볼트33",4,false,true);
        Node node6=new Node("숏크리트",3,false,false);
        Node node7=new Node("숏크리트22",4,false,true);

        node2.makeParentTree(node1);
        node3.makeParentTree(node2);
        node4.makeParentTree(node3);
        node5.makeParentTree(node3);
        node6.makeParentTree(node2);
        node7.makeParentTree(node6);

        node1.makeChildTree(node2);
        node2.makeChildTree(node3);
        node2.makeChildTree(node6);
        node3.makeChildTree(node4);
        node3.makeChildTree(node5);
        node6.makeChildTree(node7);

        System.out.println("노드 2의 부모노드 : "+node2.getParentTree().getData());
        System.out.println("노드 3의 부모노드 : "+node3.getParentTree().getData());
        System.out.println("노드 4의 부모노드 : "+node4.getParentTree().getData());
        System.out.println("노드 5의 부모노드 : "+node5.getParentTree().getData());
        System.out.println("노드 6의 부모노드 : "+node6.getParentTree().getData());
        System.out.println("노드 7의 부모노드 : "+node7.getParentTree().getData());
        System.out.println("=====================================================");
        System.out.println("노드 1의 자식노드 : "+node1.getChildTree().get(0).getData());
        System.out.println("노드 2의 자식노드 : "+node2.getChildTree().get(0).getData());
        System.out.println("노드 3의 자식노드 : "+node3.getChildTree().get(0).getData());
        System.out.println("노드 6의 자식노드 : "+node6.getChildTree().get(0).getData());
    }
```

### 순회

- 전위 순회 pre-order: 루트 노드를 먼저 순회한 후, 왼쪽 하위 → 오른쪽 하위 순으로 순회하는 방법
  - `1 - 2 - 4 - 5 - 3 - 6 - 7`
- 중위 순회 in-order: 왼쪽 가장 하위 노드를 먼저 순회한 이후, 상위 노드 → 오른쪽 하위 순으로 순회하는 방법
  - `4 - 2 - 5 - 1 - 6 - 3 - 7`
- 후위 순회 post-order: 왼쪽 가장 하위 노드를 순회한 이후, 오른쪽 하위 노드 → 상위 노드 순으로 순회하는 방법
  - `4 - 5 - 2 - 6 - 7 - 3 - 1`

<br>

## Binary Tree

- 노드의 최대 브랜치가 2개인 트리
- Binary Tree와 Binary Search Tree는 같지 않음

### 용어

`complete binary tree`: 완전 이진 트리, 마지막 레벨 외에는 모든 레벨이 채워져 있는 이진 트리. 마지막 레벨의 경우 왼쪽부터 차있는 트리.

`full binary tree`: 포화 이진 트리. 마지막 레벨의 노드도 모두 차있는 이진 트리.

### 구현

- Array로 구현할 시
  - 구현이 쉬움
  - 부모, 자식 노드의 위치를 바로 계산할 수 있어 찾기 쉬움
  - 높이가 `H`일 때, Array의 크기는 `2^H - 1`
  - 어떤 노드의 인덱스가 `i`라면 `left child`의 인덱스는 `2i + 1`
  - 어떤 노드의 인덱스가 `i`라면 부모 노드의 인덱스는 `floor((i-1)/2)`
  - 완전 이진 트리가 아닐 경우 빈 값을 가진 요소들이 생겨 공간 낭비가 생길 수 있음

- Linked List로 구현할 시
  - 저장 공간에 낭비가 없음
  - 삽입, 삭제 구현이 쉬움
  - 임의의 노드를 찾기 위해 `root`부터 탐색해야함

### 검색 시간

노드 수가 `n`인 이진 트리의 검색 시간 복잡도는 `O(n`). 원하는 노드를 찾기 위해 트리를 전부 탐색

<br>

## Binary Search Tree

- Binary Tree에서 추가 조건이 있음
  - 왼쪽 노드는 해당 노드보다 작은 값을 가짐
  - 오른쪽 노드는 해당 노드보다 큰 값을 가짐

### 용도

- 데이터 검색

### 장점

- 이진 검색 알고리즘\- 탐색 속도 개선

![이진 탐색 트리](https://camo.githubusercontent.com/6c3fdc744c1a25d023f22050796c00134c7aacebd6e6ebff86b98effb19264b0/68747470733a2f2f7777772e6d61746877617265686f7573652e636f6d2f70726f6772616d6d696e672f696d616765732f62696e6172792d7365617263682d747265652f62696e6172792d7365617263682d747265652d736f727465642d61727261792d616e696d6174696f6e2e676966)

### 검색 시간

노드 수가 `n`인 이진 트리의 검색 시간 복잡도는 `O(log n). 정렬되어 있어 이진 검색 알고리즘을 사용.

<br>

> 출처
>
> [[자료구조, Java\] 트리(Tree) 개념 정리 및 구현 - Readerr](https://readerr.tistory.com/35)
>
> https://github.com/junh0328/prepare_frontend_interview/blob/main/data_structure.md#%ED%8A%B8%EB%A6%AC