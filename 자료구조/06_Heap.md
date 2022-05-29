# 6. Heap

## Heap

- 완전 이진 트리
- 부모 노드가 가진 값은 항상 자식 노드가 가진 요소의 값보다 크거나 작음 (자식 간의 요소 크기 비교는 x)
- **개발자 필기 시험 단골 문제**

### 용어

- `Max Heap`(최대 힙): 부모 노드가 가진 값은 항상 자식 노드가 가진 요소의 값보다 크거나 같음

- `Min Heap`(최소 힙): 부모 노드가 가진 값은 항상 자식 노드가 가진 요소의 값보다 작거나 같음

![img](06_heap.assets/types-of-heap.png)

### 용도

- 데이터에서 최대값과 최소값을 빠르게 찾기 위해 구현
- 우선 순위 큐를 구현할 때 사용

  - `우선 순위 큐`: FIFO 구조가 아닌 들어간 순서에 상관없이 우선순위가 높은 데이터가 먼저 나오는 큐
  - 배열이나 연결리스트로 우선순위 큐를 구현하면 삽입 시 시간복잡도가 `O(n)`

  | **구현 방법**                           | **enqueue()** | **dequeue()** |
  | --------------------------------------- | ------------- | ------------- |
  | 배열 (unsorted array)                   | O(1)          | O(N)          |
  | 연결 리스트 (unsorted linked list)      | O(1)          | O(N)          |
  | 정렬된 배열 (sorted array)              | O(N)          | O(1)          |
  | 정렬된 연결 리스트 (sorted linked list) | O(N)          | O(1)          |
  | 힙 (heap)                               | O(logN)       | O(logN)       |

- CPU 스케쥴링
- Dijstra shortes path algorithm

### 시간 복잡도

- 데이터를 찾을 때 `O(log n)`

### 구현

- 힙을 저장하는 표준적인 자료구조는 **배열**
  - 완전 이진 트리이므로 중간에 비어있는 요소가 없기 때문에 인덱스를 사용하는 선형 구조인 배열이 용이
- 왼쪽 자식의 인덱스 = 부모의 인덱스 \* 2
- 오른쪽 자식의 인덱스 = 부모의 인덱스 \* 2 \+ 1
- 부모의 인덱스 = 자식의 인덱스 / 2

### 이진 탐색 트리와 힙

- 이진 탐색 트리와 달리 힙은 중복을 허용
- 이진 탐색트리는 왼쪽 서브 노드, 부모 노드, 오른쪽 서브 노드 순으로 크기 증가 (좌우 관계)
- 힙은 부모 노드가 가장 크고 자식 노드가 작음(max heap 기준) (상하 관계)

### 삽입

1. 힙에 새로운 요소가 들어오면, 일단 새로운 노드를 힙의 마지막 노드에 이어서 삽입한다.
2. 새로운 노드를 부모 노드와 비교해서 힙의 성질을 만족시키도록 교환한다.
3. 2번 과정을 반복한다.

```java
/* 최대힙 삽입 */
void insert_max_heap(int x){
    maxHeap[++heapSize] = x; // 힙 크기를 하나 증가하고 마지막 노드에 x를 넣는다.

    for (int i=heapSize; i>1; i/=2) {
        // 마지막 노드가 자신의 부모 노드보다 크면 swap
        if (maxHeap[i/2] < maxHeap[i]) {
            swap(i/2, i);
        } else {
            break;
        }
    }
}
```

### 삭제

1. 최대 힙에서 최대값은 루트 노드이므로 루트 노드가 삭제된다.
2. 삭제된 루트 노드에는 힙의 마지막 노드를 가져온다.
3. 힙의 성질을 만족하도록 힙을 재구성한다. \- `heapify`
   - 자식 노드 중 더 큰 값과 교환한다.

```java
/* 최대힙 삭제 */
int delete_max_heap(){
    if (heapSize == 0) // 배열이 빈 경우
        return 0;

    int item = maxHeap[1]; // 루트 노드의 값을 저장한다.
    maxHeap[1] = maxHeap[heapSize]; // 마지막 노드의 값을 루트 노드에 둔다.
    maxHeap[heapSize--] = 0; // 힙 크기를 하나 줄이고 마지막 노드를 0으로 초기화한다.

    for (int i=1; i*2<=heapSize;) {
        // 마지막 노드가 왼쪽 노드와 오른쪽 노드보다 크면 반복문을 나간다.
        if (maxHeap[i] > maxHeap[i*2] && maxHeap[i] > maxHeap[i*2+1]) {
            break;
        }
        // 왼쪽 노드가 더 큰 경우, 왼쪽 노드와 마지막 노드를 swap
        else if (maxHeap[i*2] > maxHeap[i*2+1]) {
            swap(i, i*2);
            i = i*2;
        }
        // 오른쪽 노드가 더 큰 경우, 오른쪽 노드와 마지막 노드를 swap
        else {
            swap(i, i*2+1);
            i = i*2+1;
        }
    }
    return item;
}
```

<br>

> 출처
>
> [[자료구조, Java\] 트리(Tree) 개념 정리 및 구현 - Readerr](https://readerr.tistory.com/35)
>
> https://github.com/junh0328/prepare_frontend_interview/blob/main/data_structure.md#%ED%8A%B8%EB%A6%AC