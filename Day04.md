# Day04_2022-04-12

## AI 플랫폼을 활용한 웹서비스 개발

### 1. 이중 반복문

- 이중 for문의 구조

```java
for (int i = 2; i < 10; i++) {
    System.out.println(i + " 단 시작 -----");
    for (int j = 1; j < 10; j++) {
        System.out.printf("%d x %d = %d \t", i, j, i * j);
    }
    System.out.println("~~~~~~~~~~~");
}
```

- 이중 while문의 구조

```java
int i = 2;
while (i < 10) {
    System.out.println(i + " 단 시작 -----");
    int j = 1;
    while (i < 10) {
        System.out.printf("%d x %d = %d \t", i, j, i * j);
    }
    System.out.println("~~~~~~~~~~~");
}
```

> 이중 반복문은 알고리즘 문제를 풀 때 많이 사용된다.

### 2. 배열(Array)

1. 배열을 선언하는 방법

- 4개의 값을 저장할 수 있는 ar 배열 선언한 뒤 0,1,2,3의 값을 넣는다.

```java
int ar[] = new int[4];
ar[0] = 0;
ar[1] = 1;
ar[2] = 2;
ar[3] = 3;
```

- 0,1,2,3이 들어있는 배열 ar을 만든다.

```java
int ar[] = {0,1,2,3};
```

2. 배열의 특징
   - 배열은 객체형(Reference) 이다.
   - 배열의 초기값은 null이지만 그러면 값을 넣을 수 없다.

3. 배열의 값을 출력하는 방법

- 단순히 배열의 값을 확인하고 싶을 때

```java
System.out.println(Arrays.toString(ar));
```

-  배열의 값을 이용하고 싶을 때 (for문)

```java
for (int i = 0; i < ar.length; i++) {
    System.out.println(ar[i]);
}
```

### 3. 메모리 사용 영역

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220412155629325.png" alt="image-20220412155629325" style="zoom: 50%;" />

1. 세부 영역

- Method 영역
  - java로 작성한 코드를 실행했을 때 코드가 들어가는 영역이다.
- Stack 영역
  - 메소드를 호출할 때마다 frame을 추가(push)하고 메소드가 종료되면 frame을 제거(pop)한다.
- Heap 영역
  - 객체와 배열이 생성되는 영역이다.
  - 참조하는 변수나 필드가 없다면 자동으로 제거한다.

2. Primitive 자료를 넣었을 때 메모리 구조

```java
int a = 10;
int b = 10;
```

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220412160416582.png" alt="image-20220412160416582" style="zoom:50%;" />

3. Reference 자료를 넣었을 때 메모리 구조

```java
String s1 = "abc";
String s2 = "abc";
String s3 = new String("abc");
String s4 = new String("abc");
```

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220412161021381.png" alt="image-20220412161021381" style="zoom:50%;" />

4. 배열에 Primitive 자료를 넣었을 때 메모리 구조

```java
int ar [][] = new int[3][];  // 2차원 배열  ar[행][열]
ar[0] = new int[3];
ar[1] = new int[3];
ar[2] = new int[3];
```

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220412182756638.png" alt="image-20220412182756638" style="zoom:50%;" />

- 실행 순서
  1. null으로 초기값이 들어간다.
  2. ar의 배열이 만들어진다.
  3. 0행의 열이 만들어진다.
  4. 1행의 열이 만들어진다.
  5. 2행의 열이 만들어진다.

5. 배열에 Reference 자료를 넣었을 때 메모리 구조

```java
String sr [] = new String[3];
sr[0] = "ab";
sr[1] = "cd";
sr[2] = "ef";
```

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220412162810605.png" alt="image-20220412162810605" style="zoom:50%;" />

- 실행 순서
  1. null으로 초기값이 들어간다.
  2. sr의 배열이 만들어진다.
  3. 객체형이기 때문에 다시 참조한다.

### 4. 함수를 원하는 곳에서 빠져나오기

- 사용 방법

```java
outter:
for (int i = 2; i < 10; i++) {
    if (i % 2 == 1) {
        continue;
    }
    System.out.println(i + " 단 시작 -----");
    for (int j = 1; j < 10; j++) {
        if ((i * j) == 28) {
            break outter; // 4단만 break에 걸리고 6,8단은 그대로 실행된다.
        }
        System.out.printf("%d x %d = %d \t", i, j, i * j);
    }
    System.out.println("~~~~~~~~~~~");
}
```

- 위의 코드에서 outter를 하지 않으면 4단만 break에 걸려 종료되고 6, 8단은 그대로 실행된다.

### 5. 문자열에서 공백 제거하기(trim)

- 사용 방법

```jav
String ss = "  abc  ";
System.out.println(ss.trim());
```