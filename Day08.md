# Day08 2022-04-19

## AI 플랫폼을 활용한 웹서비스 개발

### 1. 상속

- 상속의 효과
  1. 자식 클래스를 빠른 시간에 개발할 수 있다.
  2. 유지, 보수가 편리하다.
  3. 다형성이 있다.
- 상속 대상 제한
  - 상위 클래스에서 함수를 private으로 하면 상속할 수 없고, 재정의도 할 수 없다.
  - public으로 하는 것이 일반적이다.
- 상속의 특징
  - 1개의 클래스만 상속 받을 수 있다. 2개의 클래스에서 상속 받을 수 없다.
  - 만들어질 때 기본적인 속성을 가지고 태어난다. (extands Object)

### 2. 매소드 재정의

- 부모 클래스의 상속 메소드를 수정해 자식 클래스에서 재정의 할 수 있다.
- public을 default나 private으로 수정 불가능하다. public으로 해야한다.

### 3. Super

- 상위 메소드를 사용할 때 작성한다.

```java
super.부모 메소드();
```

### 4. Final 키워드의 용도

- final 필드 - 수정 불가 필드이다.
- final 클래스 - 부모로 사용 불가한 클래스이다. 상속할 수 없다.
- final 메소드 - 자식이 재정의 할 수 없는 매소드이다.

```java
public final class 클래스{
    
}
```

### 5. 접근 제한자 (Protected)

1. piblic - 모든 접근이 가능하다.
2. protected - 상속 관계에서 접근이 가능하다.
3. default - 같은 패키지에서 접근이 가능하다. 아무것도 쓰지 않는 것이다.
4. private - 해당 클래스에서만 접근이 가능하다.

### 6. Polymorphism (다형성)

- 의미는 같지만 클래스의 객체마다 다르게 표현된다.
- 조건
  1. 상속 관계이다.
  2. 함수의 재정의가 일어난다.

### 7. 자동 타입 변환 (Promotion)

- 프로그램 실행 도중 자동 타입 변환이 일어나는 것이다.

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220419124800137.png" alt="image-20220419124800137" style="zoom:33%;" />

```java
// 가능
A a = new A();
A a = new B();
A a = new C();
A a = new D();
A a = new E();

B b = new C();
D d = new E();

// 불가능
B b = new D();
C c = new B();
```

- public void method (A a){   } 일 경우 method에는 a의 객체가 들어가지만 b, c, d, e도 들어갈 수 있다.

### 8. for each

```java
for (Shape shape : s) {

}
```

- Shape라는 배열을 돌아가며 shape라는 객체를 하나씩 끄집어 낼 때 사용한다.
- for use index on array는 i값을 활용할 때 주로 사용한다.