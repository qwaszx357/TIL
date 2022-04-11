# Day03_2022-04-11

## AI 플랫폼을 활용한 웹서비스 개발

### 1. Switch문

- switch문 구조

```java
switch (key) {
case value:
			
	break;

default:
	break;
}
```

- key 값에는 실수(double, float)가 들어갈 수 없다. string은 가능하다.

- case에는 범위를 설정할 수 없다.

- default는 if문에서 else와 같은 역할을 한다.

  > switch문은 어떠한 요구사항을 정의할 때 사용하면 합리적이다.
  >
  > ex) 1등은 냉장고, 세탁기, 핸드폰을 받고, 2등은 세탁기, 핸드폰을 받고, 3등은 핸드폰만 받는다.

### 2. For문

- for문 구조

```java
for (int i = 0; i < args.length; i++) {
			
		}
```

- i의 시작과 끝을 설정하고, 반복적인 증가를 입력한다.

### 3. While문

- while문 구조

```java
while (it.hasNext()) {
	type type = (type) it.next();
		
}
```

- while문을 사용하기 전에는 증감을 나타낼 변수(s)를 선언하여 초기화한다.
- if문과 마찬가지로 시작과 끝을 설정하고, 반복적인 증가를 입력한다.

> for문과 while문의 차이점은 for문에서의 i는 그 안에서만 유효하고, while문에서의 s는 변수를 선언하였기 때문에 계속 사용이 가능하다.

### 4. Random

- random 구조

```java
import java.util.Random;

Random r = new Random();
int n = r.nextInt(100);
```

- 랜덤으로 난수를 발생시킬 때 사용한다.
- nextInt(100); 을 적으면 0 ~ 99까지의 범위에서 난수가 발생한다.
- 초기 범위를 바꾸고 싶을 때는 nextInt(100) + 1;로 적으면 1 ~ 100까지의 범위에서 난수가 발생한다.

### 5. 함수에서 제어할 때 사용하는 명령어

- continue 

  -밑에 내용은 스킵하고 반복문은 계속 진행된다.

- break

  -반복문을 종료시킨다.

  