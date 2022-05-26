# Day31 2022-05-26

## AI 플랫폼을 활용한 웹서비스 개발

### Spring

#### 1. 프레임워크 (Framework)

- 사전적 의미: 어떤 것을 구성하는 구조 또는 뼈대이다.
- 소프트웨어적 의미: 기능을 미리 클래스나 인터페이스 등으로 만들어 제공하는 반제품이다.
- 프레임워크를 사용하는 이유: 일정한 기준에 따라 개발이 이루어지므로 **개발 생선성과 품질이 보장된 애플리케이션을 개발**할 수 있다.

#### 2. DI (Dependency Injection)

- 의존성 주입

- 객체를 직접 생성하는 것이 아니라 외부에서 생성한 후 주입 시켜주는 방식

- DI를 사용하면 코드에서 직접적인 연관관계가 발생하지 않으므로 각 클래스들의 변경이 자유로워진다. 

  > 약한 결합 (loosely coupled)이 이루어진다.

#### 3. IoC (Inversion Of Control)

- 제어 역행
- 메소드나 호출작업을 개발자가 결정하는 것이 아니라, 외부에서 결정되는 것을 의미한다.
- 객체 간의 결합도를 줄이고 유연한 코드를 작성할 수 있게되어, 가독성 및 코드 중복 / 유지 보수가 편해진다.

#### 4. 코드 비교

- 기존의 코드
  - 순방향으로 제어하는  tightly coupled 관계이다.

```java
public static void main(String[] args) {
    System.out.println("Spring Start ..");

    ApplicationContext factory =
        new ClassPathXmlApplicationContext("spring.xml");

    Service service = new UserService(); 

    UserVO user = new UserVO("id01", "pwd01", "lee");
    service.register(user);
}
```

- spring을 사용한 코드
  - IoC를 이용해 제어역행을 해서 loosely coupled 관계이다.

```java
public static void main(String[] args) {
    System.out.println("Spring Start ..");

    ApplicationContext factory =
        new ClassPathXmlApplicationContext("spring.xml");

    Service service = (Service) factory.getBean("uservice");

    UserVO user = new UserVO("id01", "pwd01", "lee");
    service.register(user);
}
```

> new Uservice()로 불러오지 않고 factory.getBean을 이용해 불러올 수 있다. 타입캐스팅은 해야한다. 