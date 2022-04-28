# Day07_2022-04-18

## AI 플랫폼을 활용한 웹서비스 개발

### 객체 지향 프로그램

#### 1. 상속

-  Employee라는 직원 관리 프로그램과 Manager라는 매니저 관리 프로그램이 있을 때 매니저는 직원이기도 하다. 이를 Manager가  Employee를 상속한다고 한다.
- Manager is a Employee. (매니저는 직원이다.)
- 작성 방법

```java
public class Manager extends Employee{
    
}
```

- Manager는 Employee를 상속하기 때문에 Employee를 상위 클래스, Manager를 하위클래스 라고 하고 하위클래스는 따로 작성하지 않아도 상위클래스가 실행된다.

#### 2. Overloading, Overriding

- Overloading - 하나의 클래스 안에 동일한 cunstructor(생성자), 함수가 존재할 수 있다. 단, argument가 달라야한다.

- Overriding - 상속이라는 관계에 있어서 상위의 함수를 하위에서 다시 정의할 수 있다.

  - ex) 직원에게는 보너스가 없고, 매니저에게만 보너스가 있는 경우, 연봉을 계산하는 방법이 달라진다.
  - 실행 방법

  ```java
  @Override
  public double annsalary() {
      double sum = 0.0;
  //	sum += getSalary() * 12 + this.bonus;   // private 일 때는 이렇게 해야 접근 가능
  //    sum += salary * 12 + this.bonus;   // protected 라고 하면 접근 가능함
      sum = super.annsalary() + this.bonus;   // 상속
      return sum;
  ```

#### 상속 관계에서 비교할 때 (instanceof)

- 실행 방법

```java
for (int i = 0; i < e.length; i++) { 
			if (e[i] instanceof Manager) {
				Manager m = (Manager)e[i];
				System.out.println(m.getBonusTex());
			}
```

- 평소 사용하는 비교연산자 if (e[i] == Manager)로는 비교할 수 없어 instanceof를 사용한다.