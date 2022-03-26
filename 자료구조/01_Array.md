# 1. Array

- **동일한 자료형**의 데이터를 연속된 메모리에 저장하기 위한 **선형적 자료 구조**
  - OS가 spatial locality를 고려하여 데이터를 가져올 때 활용될 수 있음

- 논리적 저장 순서와 물리적 저장 순서가 일치하여 index를 이용해 element에 접근 가능
- 다차원 배열도 사용할 수 있지만 현실적으로 이해하기 쉬운 2차원까지만 자주 쓰임
- 장점
  - 정렬, 순회가 쉬움
  - 변수의 선언을 줄여줌
  - 연속적이므로 메모리 관리가 편리
- 단점
  - **길이가 고정되어 있음**, 따라서 메모리 낭비 가능
  - 삽입과 삭제가 어려움
  - 배열의 크기를 수정할 수 없음

<br>

### 표현

- 자료형 타입 + `[]` + 배열 이름
- 자료형 타입 + 배열 이름 + `[]`

```java
int[] odds = {1, 3, 5, 7, 9};

int even [] = {2, 4, 6, 8, 10};
```

<br>

### 선언

- 배열 변수를 먼저 생성한 후 그 값은 나중에 대입하는 방법
- 초기값 없이 배열 변수를 만들 때는 반드시 길이를 명시해야 함
- **배열의 길이와 타입은 변경 x**

```java
String[] weeks = new String[8];

String weeks [] = new String[8];

weeks[0] = "월";
weeks[1] = "화";
weeks[2] = "수";
weeks[3] = "목";
weeks[4] = "금";
weeks[5] = "토";
weeks[6] = "일";
```

<br>

### 순회

- `[배열 이름].length` : 배열의 길이

```java
String[] weeks = {"월", "화", "수", "목", "금", "토", "일"};

for (int i=0; i<weeks.length; i++) {
    System.out.println(weeks[i]);
};

for (string day : weeks) {
    System.out.println(day);
};
```

<br>

**예제**

```java
import java.util.Scanner;
public class ArrayEx04 {
	public static void main(String[] args) {
		int[] num = new int[5]; // 배열 생성
		int max, min;
		Scanner sc = new Scanner(System.in);
		System.out.println("5개의 정수를 입력하시오.");
		for (int i = 0; i < num.length; i++) {
			num[i] = sc.nextInt(); // 데이터 입력 및 배열에 저장
		}
		max = num[0]; // 최대값 초기 설정
		min = num[0]; // 최소값 초기 설정
		for (int i = 0; i < num.length; i++) {
			if (max < num[i]) { // 최대값 비교
				max = num[i]; // 최대값 할당
			}
			if (min > num[i]) { // 최소값 비교
				min = num[i]; // 최대값 할당
			}
		}
		System.out.println("최대값 : " + max);
		System.out.println("최소값 : " + min);
	}
}
```

<br>

### 오류

- `ArrayIndexOutOfBoundsException`: 런타임 발생시, 인덱스 초과 오류

<br>

### 2차원 배열

- 자료형 타입 + `[][]` + 배열 이름
- 길이를 모를 때는 행의 길이, 열의 길이 명시

```java
int[][] num = { {4, 3, 4},
               {3, 7, 6},
               {5, 8, 7},
               {9, 9, 10}};

int[][] num = new int[4][3];
num[0][0]= 4; num[0][1]= 3; num[0][2]= 4;
num[1][0]= 3; num[1][1]= 7; num[1][2]= 6;
num[2][0]= 5; num[2][1]= 8; num[2][2]= 7;
num[3][0]= 9; num[3][1]= 9; num[3][2]= 10;
```

<br>

### 가변 배열

- 행마다 다른 길이의 배열을 요소로 저장 가능
- 열의 길이 명시 x

```java
int[][] arr = new int[3][];

arr[0] = new int[2];
arr[1] = new int[4];
arr[2] = new int[1];
```

<br>

### 복사

- 자바 배열은 길이를 변경할 수 없으므로 더 많은 데이터를 저장하기 위해 더욱 큰 배열을 만들고 기존 배열을 복사해야 함
- 4가지 방법
  - `arraycopy(arr1, n, arr2, m, k)`:  arr1의 n번째 원소부터 k길이만큼 복사해서, arr2의 m번째 원소부터 붙여넣기
  - **`Arrays.copyOf(arr1, n)`**: arr1에서 n길이만큼 복사
  - `arr1.clone()`: arr1을 얕은 복사

```java
import java.util.Arrays;

int[] arr1 = new int[]{1, 2, 3, 4, 5};
int newLen = 10;


// 1. System 클래스의 arraycopy() 메소드
int[] arr2 = new int[newLen];
System.arraycopy(arr1, 0, arr2, 0, arr1.length);

// 2. Arrays 클래스의 copyOf() 메소드
int[] arr3 = Arrays.copyOf(arr1, 10);

// 3. Object 클래스의 clone() 메소드
int[] arr4 = (int[])arr1.clone();

// 4. for 문과 인덱스를 이용한 복사
int[] arr5 = new int[newLen];
for (int i = 0; i < arr1.length; i++) {
    arr5[i] = arr1[i];
}
```

<br>

### 시간 복잡도

- 접근

  - 인덱스로 element에 접근
  - O(1)
- 검색

  - 순차검색

  - O(n)
- 추가, 삭제

  - 인덱스를 알고있다면 O(1), 모른다면 O(n)
- 같은 길이를 가지는 두 Array를 연결
  - O(n)




## 출처

- https://wikidocs.net/206
- https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=heartflow89&logNo=220950491600
- http://www.tcpschool.com/java/java_array_application