# Day10 2022-04-21

## AI 플랫폼을 활용한 웹서비스 개발

### 1. TableView

- 테이블 뷰를 만들면 테이블 창이 새로 만들어진다.
- 실행 방법

```java
Frame f;

public void setView() {
    f.add(b, "North");
    f.setSize(300, 200);
    f.setVisible(true);
    // 프레임에 이벤트를 붙이겠다는 선언. 프레임에 이벤트가 발생하면 이 클래스를 실행한다.
    f.addWindowListener(new WindowAdapter() {  // 클래스를 괄호 안에 선언하는 방법 
        @Override
        public void windowClosing(WindowEvent e) {  // x 누르면 종료.
            System.exit(0);
        }  

    });  

}
```

- 버튼 실행 방법

``` java
Button b;

public MyFrame() {
    f = new Frame("My Frame");
    b = new Button("Click");
    // 버튼에 이벤트 붙이기. 버튼 클릭하면 아래 실행
    b.addActionListener(new ActionListener() {
        @Override
        public void actionPerformed(ActionEvent e) {
            b.setLabel("Clicked");
        }
    });
}
```

### 2. 예외 처리

- Error (에러) - 컴퓨터 하드웨어의 오동작 또는 고장으로 인해 응용프로그램 실행 오류가 발생하는 것이다.
- Exception (예외) - 사용자의 잘못된 조작 또는 개발자의 잘못된 코딩으로 인해 발생하는 프로그램 오류이다.
  - 사용자의 잘못된 조작 예 - 숫자 입력란에 문자를 입력
- Exception은 최상위라서 다른 오류를 포함하기 때문에 자세한 오류 이름 생략하고 Exception 만 적어도 된다.

### 3. Try Catch

- try - 실행하고자 하는 코드를 작성한다.
- catch - try 실행 중 예외가 발생하면 catch를 실행한다.
- finally - 예외가 발생할 경우 try의 나머지를 실행하지 않기 때문에 예외 상황에서도 실행해야 할 코드를 작성한다.

```java
try {
    n = Integer.parseInt(num);
    result = 100 / n;
    System.out.println(result);
    // sc.close();		// 문제 발생하면 프로그램은 끝나지만 close는 안됨.
} catch (ArithmeticException e) {
    System.out.println("분모가 0입니다.");
} catch (NumberFormatException e) {
    System.out.println("숫자가 아닙니다.");
} finally {
    sc.close();
    System.out.println("End");
}
```

