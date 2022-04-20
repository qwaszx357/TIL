# Day09 2022-04-20

## AI 플랫폼을 활용한 웹서비스 개발

### 1. 추상 클래스

- 실체 클래스들의공통되는 필드와 메소드를 정의한 클래스이다.
- 추상 클래스의 용도
  1. 실체 클래스의 공통된 필드와 메소드의 이름을 통일할 목적이다.
  2. 실체 클래스를 작성할 때 시간을 절약한다.
  3. 실체 클래스 설계 규격을 만들 수 있다.
- 추상 클래스 안에는 추상함수 뿐만 아니라 일반 함수도 정의할 수 있다.

### 2. Team Workshop

- car 패키지를 만들어 자동차를 분류하고 각 자동차에 따른 기능을 추가한다.
- UML 설계하기
  - name - 자동차의 이름
  - no - 자동차 차번호
  - fe - 연비
  - color - 자동차 색깔
  - fsize - 잔여 기름양
  - cfsize - 기름통 크기
  - bsize - 배터리 크기
  - cbsize - 잔여 배터리 양
  - dmod - 드라이브 모드
  - fmod - true(가솔린), false(배터리)
  - ddistance - 주행가능 거리
  - ff - 기름 넣기
  - go - 출발
  - stop - 멈춤

<img src="https://cafeptthumb-phinf.pstatic.net/MjAyMjA0MTlfMjc3/MDAxNjUwMzU2NzkyOTc4.J-PF-fwqcEU0GxJFCEOC_YzWULpm20x-9GVIB3TrBdog.ZP1ViRdgeqyplcZ5CsENZJAW6oxbvery6Y7bF7BbxOUg.PNG/image.png?type=w1600" alt="img" style="zoom: 67%;" />

- 코드

  - class Car

  ```java
  package car;
  
  public abstract class Car {
  	protected String name;
  	protected String no;
  	protected double fe;
  	protected String color;
  	protected double fsize;
  	protected double cfsize;
      
  	public Car() {
  	}
  	public Car(String name, String no, double fe, String color, double fsize, double cfsize) {
  		this.name = name;
  		this.no = no;
  		this.fe = fe;
  		this.color = color;
  		this.fsize = fsize;
  		this.cfsize = cfsize;
  	}
  	public Car(String name, String no, double fe, String color, double fsize) {
  		this.name = name;
  		this.no = no;
  		this.fe = fe;
  		this.color = color;
  		this.fsize = fsize;
  	}
  	
  	public String getName() {
  		return name;
  	}
  	public void setName(String name) {
  		this.name = name;
  	}
  	public String getNo() {
  		return no;
  	}
  	public void setNo(String no) {
  		this.no = no;
  	}
  	public double getFe() {
  		return fe;
  	}
  	public void setFe(double fe) {
  		this.fe = fe;
  	}
  	public String getColor() {
  		return color;
  	}
  	public void setColor(String color) {
  		this.color = color;
  	}
  	public double getFsize() {
  		return fsize;
  	}
  	public void setFsize(double fsize) {
  		this.fsize = fsize;
  	}
  	public double getCfsize() {
  		return cfsize;
  	}
  	public void setCfsize(double cfsize) {
  		this.cfsize = cfsize;
  	}
  	
  	@Override
  	public String toString() {
  		return "Car [name=" + name + ", no=" + no + ", fe=" + fe + ", color=" + color + ", fsize=" + fsize + ", cfsize="
  				+ cfsize + "]";
  	}
  	public abstract void go(double km);
  	
  	public abstract void ff(double amount);
  	public abstract void ff(double amount, boolean fmod);
  	public abstract double ddistance(); 
  	
  	public abstract void changedMod();
  }
  ```

  - class Ice

  ```java
  package car;
  
  public class Ice extends Car {
  
  	public Ice() {
  		
  	}
  	public Ice(String name, String no, double fe, String color, double fsize, double cfsize) {
  		super(name, no, fe, color, fsize, cfsize);
  		
  	}
  	public Ice(String name, String no, double fe, String color, double fsize) {
  		super(name, no, fe, color, fsize);
  		
  	}
  	
  	@Override
  	public void go(double km) {
  		if (km > ddistance()) {  
  			System.out.println("연료가 부족합니다.");
  		}
  		else {
  			cfsize -= (km / fe);
  		}
  	}
  
  	@Override
  	public void ff(double amount) {
  		if (fsize - cfsize < amount) {
  			System.out.println("연료를 넣을 수 없습니다");
  		}
  		else {
  			cfsize += (amount);
  		}
  	}
  
  	@Override
  	public double ddistance() {
  		// TODO Auto-generated method stub
  		return cfsize*fe;
  	}
  
  	@Override
  	public void ff(double amount, boolean fmod) {
  		// TODO Auto-generated method stub
  		
  	}
  
  	@Override
  	public String toString() {
  		return "Ice [name=" + name + ", no=" + no + ", fe=" + fe + ", color=" + color + ", fsize=" + fsize + ", cfsize="
  				+ cfsize + "]";
  	}
  
  	@Override
  	public void changedMod() {
  		// TODO Auto-generated method stub
  		
  	}
  
  }
  ```

  - class Phev

  ```java
  package car;
  
  public class Phev extends Car {
  	
  	private double capacity;
  	private double ccapacity; //현재 잔여
  	private boolean mod;
  	
  	public Phev(String name, String no, double fe, String color, double fsize, double cfsize, double capacity,
  			double ccapacity) {
  		super(name, no, fe, color, fsize, cfsize);
  		this.capacity = capacity;
  		this.ccapacity = ccapacity;
  	}
  	public Phev(String name, String no, double fe, String color, double fsize, double cfsize, double capacity,
  			double ccapacity, boolean mod) {
  		super(name, no, fe, color, fsize, cfsize);
  		this.capacity = capacity;
  		this.ccapacity = ccapacity;
  		this.mod = mod;
  	}
  	public Phev() {
  		// TODO Auto-generated constructor stub
  	}
  
  	public Phev(String name, String no, double fe, String color, double fsize, double cfsize) {
  		super(name, no, fe, color, fsize, cfsize);
  		// TODO Auto-generated constructor stub
  	}
  
  	public Phev(String name, String no, double fe, String color, double fsize) {
  		super(name, no, fe, color, fsize);
  		// TODO Auto-generated constructor stub
  	}
  
  	public double getCapacity() {
  		return capacity;
  	}
  	public void setCapacity(double capacity) {
  		this.capacity = capacity;
  	}
  	public double getCcapacity() {
  		return ccapacity;
  	}
  	public void setCcapacity(double ccapacity) {
  		this.ccapacity = ccapacity;
  	}
  	public boolean isMod() {
  		return mod;
  	}
  	public void setMod(boolean mod) {
  		this.mod = mod;
  	}
  
  	@Override
  	public void go(double km) {
  		if(mod == true) {
  			if (km > ddistance()) {
  				System.out.println("연료가 부족합니다.");
  			}
  			else {
  				cfsize -= (km / fe);
  			}
  		}
  		else if(mod == false) {
  			if (km > ddistance()) {
  				System.out.println("베터리 잔량이 부족합니다.");
  			}
  			else {
  				ccapacity -= (km / fe);
  			}
  		}
  
  	}
  
  	@Override
  	public void ff(double amount) {
  		// TODO Auto-generated method stub
  
  	}
  
  	@Override
  	public void ff(double amount, boolean fmod) {
  		if(mod == true) {
  			if (fsize - cfsize < amount) {
  				System.out.println("연료를 넣을 수 없습니다");
  			}
  			else {
  				cfsize += (amount);
  			}
  		}
  		else if(mod == false) {
  			if (capacity - ccapacity < amount) {
  				System.out.println("과충전입니다.");
  			}
  			else {
  				ccapacity += (amount);
  			}
  		}
  
  	}
  
  	@Override
  	public double ddistance() {
  		double dd = 0.0;
  		if(mod == true) {
  			dd = cfsize*fe;
  		}
  		else if(mod == false) {
  			dd = ccapacity*fe;
  		}
  		return dd;
  		
  	}
  
  
  	@Override
  	public String toString() {
  		return "Phev [capacity=" + capacity + ", ccapacity=" + ccapacity + ", mod=" + mod + ", name=" + name + ", no="
  				+ no + ", fe=" + fe + ", color=" + color + ", fsize=" + fsize + ", cfsize=" + cfsize + "]";
  	}
  
  
  	@Override
  	public void changedMod() {
  		if (this.mod == true) {  // mod가 가솔린이면 배터리모드로 바꾸기
  			mod = false;
  		}else if (this.mod == false) {
  			mod = true;
  		}
  		
  	}
  
  	
  }
  ```

  - class TestApp

```java
package car;

public class TestApp {

	public static void main(String[] args) {	
		Car car1 = new Ice("k5","1234",10.0,"red",100,100);
		System.out.println(car1);
		
		car1.go(900);
		System.out.println(car1.getCfsize());
		
		car1.ff(70);
		System.out.println(car1.getCfsize());
		
		Car car2 = new Phev("Tu", "1123", 20, "blue", 50, 50, 20,20);
		System.out.println(car2);
		
//		Phev에만 있는 함수라서 이렇게 작성
		if (car2 instanceof Phev) {
			Phev p = (Phev)car2;
			p.setMod(false);
		}
//		car2.setMod(true);   car class에서 함수 추가하면 이렇게 적기
		System.out.println(car2);
		
		car2.go(100);
		
		if (car2 instanceof Phev) {
			Phev p = (Phev)car2;
			System.out.println(p.isMod());
		}
		
		car2.changedMod();  // mod 바꾸기
		
		if (car2 instanceof Phev) {
			Phev p = (Phev)car2;
			System.out.println(p.isMod());
		}
	}
}
```

### 3. 인터페이스 

1. 인터페이스의 특징

   - 인터페이스 안에는 함수와 기능 정의만 되어있다.
   - 인터페이스 안에 함수를 선언하면 자동으로 추상함수가 된다.
   - 상속은 1개의 클래스에만 가능하지만 인터페이스는 여러개 가능하다.

2. 인터페이스의 역할

   - 개발 코드와 객체가 서로 연동하는 접점이다.
   - 개발 코드는 인터페이스의 메소드만 알고 있으면 된다.
   - 인터페이스를 두는 이유는 개발 코드를 변경하지 않고, 객체를 변경할 수 있도록 하기 위해서이다.

3. 인터페이스의 구성멤버

   - 상수

   - 추상 메소드

     ```java
     public abstract void 메소드이름 ();
     ```

   - 디폴트 메소드

     ```java
     public default void 메소드 이름 (){
         
     }
     ```

   - 정적 메소드

   > 주로 추상 메소드와 디폴트 메소드를 사용한다.

### 4. ArrayList

- 여러개의 객체들을 하나의 리스트에 담을 때 사용한다.
- 배열과의 다른 점은 배열은 초기 선언할 때 배열의 크기도 함께 선언해야하고 배열에 값만 들어가지만 ArrayList는 객체를 추가할 수 있고 <key, value>값이 같이 들어간다.
- 실행 방법

```java
// 객체를 추가한다.
map.put(key,value);

// 객체의 값을 불러온다.
map.get(key);

// 객체를 지운다.
map.remove();
```

