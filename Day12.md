# Day12 2022-04-25

## AI 플랫폼을 활용한 웹서비스 개발

### 1. Multi thread

- 멀티 프로세스 (Multi Process)

  - 독립적으로 프로그램을 실행하고 여러 가지 작업을 처리하는 것이다.
  - ex) 여러 개의 브라우저 창의 실행하는 것

- 멀티 스레드 (Multi Thread)

  - 한 개의 프로그램을 실행하고 내부적으로 여러 작업을 처리하는 것이다.
  - ex) 한 개의  브라우저 창에서 여러 개의 탭을 실행하는 것

- 스레드 제어문 (sleep)

  - 처리과정을 확인하고 싶을 때 속도에 제한을 두어 제어할 수 있다.
  - 예외가 발생할 수 있으니 try - catch와 함께 사용한다.
  - ()안에 숫자는 밀리세컨즈 단위라서 1000이 1초를 의미한다.
  - 실행 방법

  ```java
  while (i <= 300) {
      System.out.println(i);
      i++;
      try {
          Thread.sleep(1000);
      } catch (InterruptedException e) {
          e.printStackTrace();
      }
  }
  ```

- Thread 만드는 방법

  1. 클래스를 상속한다.
     - 클래스를 만들어 Thread를 상속한 뒤 Override한다.
     - 메인클래스에는 아래와 같이 실행한다.

  ```java
  public class MyThread1 extends Thread{
  
      @Override
      public void run() {
          int i = 0;
          while (i <= 100) {
              i++;
              System.out.println("MyThread1: " + i);
              try {
                  Thread.sleep(1000);
              } catch (InterruptedException e) {
                  e.printStackTrace();
              }
  
          }
      }
  }
  ```

  ```java
  MyThread1 t1 = new MyThread1();
  t1.start();
  ```

  2. 인터페이스를 상속한다.
     - 클래스를 만들어 인터페이스를 상속한 뒤 Override한다.
     - 메인클래스에는 아래와 같이 실행한다.

  ```java
  public class MyThread2 implements Runnable{
  
      @Override
  	public void run() {
  		int i = 0;
  		while (i <= 100) {
  			i++;
  			System.out.println("MyThread2: " + i);
  			try {
  				Thread.sleep(1500);
  			} catch (InterruptedException e) {
  				e.printStackTrace();
  			}
  			
  		}
  	}
  
  }
  ```

  ```java
  MyThread2 t2 = new MyThread2();
  Thread tt2 = new Thread(t2);
  tt2.start();
  ```

### 2. MySQL

- Java Application과 데이터 베이스를 연동한다.

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220425173930513.png" alt="image-20220425173930513" style="zoom:50%;" />

- CRUD 프로그램 만들기
  1. MySQL Workbench

```MySql
CREATE TABLE CUST(
	id VARCHAR(20) PRIMARY KEY,
    pwd VARCHAR(20),
    name VARCHAR(30)
);

CREATE TABLE ITEM(
	id int PRIMARY KEY,
    name VARCHAR(30),
    price FLOAT
);
-- CRUD
-- Create
INSERT INTO CUST VALUES('id01', 'pwd01', '이말숙');
INSERT INTO CUST VALUES('id02', 'pwd02', '이말숙');
-- Update
UPDATE CUST SET pwd = '1111', name = '홍말숙' WHERE id = 'id01';
-- Delete
DELETE FROM CUST WHERE id = 'id02';
-- Read
SELECT * FROM CUST;
```

​		2. Eclipse - insert 실행하기

```java
// JDBC(Java Database Connectivity Program)

// 변수 선언
Connection con = null;
PreparedStatement ps = null;
String sql = "INSERT INTO CUST VALUES (?,?,?)";

// MySQL JDBC Driver Loading
try {
    Class.forName("com.mysql.cj.jdbc.Driver");
    System.out.println("Driver Loading...");
} catch (ClassNotFoundException e) {
    e.printStackTrace();
}

// MySQL Connect
String url = "jdbc:mysql://192.168.35.191:3306/shopdb?serverTimezone=Asia/Seoul";	// 127.0.0.1 ->
String mid = "admin1";
String mpwd = "111111";
try {
    con = DriverManager.getConnection(url, mid, mpwd);
    System.out.println("MySQL Server Connected...");
} catch (SQLException e) {
    e.printStackTrace();
}

// SQL을 이용한 요청
try {
    ps = con.prepareStatement(sql);
    ps.setString(1, "id11");
    ps.setString(2, "pwd11");
    ps.setString(3, "이말숙");
    // 요청 결과를 확인
    int result = ps.executeUpdate();
    System.out.println(result);
} catch (SQLException e) {
    e.printStackTrace();
} finally {
    // MySQL Close
    if (ps != null) {
        try {
            ps.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    if (con != null) {
        try {
            con.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

- 주의할 점
  - 대소문자를 구분한다.
  - 오타 없는지 다시 확인한다.