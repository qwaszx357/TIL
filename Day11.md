# Day11 2022-04-22

## AI 플랫폼을 활용한 웹서비스 개발

### 1. API 클래스

- 자바 API - 자바에서 기본적으로 제공하는 라이브러리이다.  ex) Random, Scanner 등.
- API 도큐먼트 - 쉽게 API를 찾아 이용할 수 있도록 문서화한 것이다.
- java.lang 패키지 - 자바 프로그램의 기본적인 클래스를 담은 패키지이다.
  - 주요 클래스 - Object, System, Class, String, Wrapper 등.
  - Object 클래스 - 모든 클래스가 상속하는 최상위 클래스이다.
  - String 클래스 - String 메소드는 문자열의 추출, 비교, 찾기, 분리 등 다양한 메소드를 가진다.
- Random
  - 설정한 범위 내의 난수를 발생시킨다.

```java
// 0.0 <= x < 1.0 사이의 난수 발생
double d = Math.random();  
System.out.println(d);

// 1 ~ 6 사이의 난수 발생
int i1 = (int)(Math.random() * 6) + 1;
System.out.println(i1);

// 1 ~ 45 사이의 난수 발생. ex)로또 번호 생성
int i2 = (int)(Math.random() * 45) + 1; 
System.out.println(i2);

// 1 ~ 45 i2와 같음
Random random = new Random();
int i3 = random.nextInt(45) + 1;
System.out.println(i3);

// 1부터 ~
double dd = (random.nextDouble() * 10000000000.0) + 1;	
System.out.println(dd);
System.out.printf("%.2f", dd);  // 소수점 2째자리까지 출력
```

- Wrapper
  - 기본 타입의 데이터를 갖는 객체를 만들 때 사용한다.
  - 문자열을 기본 타입으로 변환할 때 사용한다.

```java
int a = 10;
Integer i = 10;

// 서로 타입은 다르지만 연산 가능
int result = a + i;	
System.out.println(result);

if (a == i) {
    System.out.println("ok");
}
```

- Date / Calendar
  - 날짜 프로그램에서 사용되는 데이터이다.
  - Date는 날짜를 표현하는 클래스이고, Calender는 달력을 표현한 클래스로 Calender가 더 많은 기능을 가지고 있다.

```java
// Date안에 toString이 있어서 가능. 컴퓨터 시간이 출력
Date date = new Date();	
System.out.println(date);

// 예쁘게 출력하고 싶으면
// () 안에 출력하고 싶은대로 만들기
SimpleDateFormat sdf = 
    new SimpleDateFormat("YYYY:MM:dd-hh:mm:ss");	// DD로 하면 1월1일부터의 날짜
String str = sdf.format(date);
System.out.println(str);

// Los_Angeles의 현재시간 출력
TimeZone tz = TimeZone.getTimeZone("America/Los_Angeles");

SimpleDateFormat sdf2 = 
    new SimpleDateFormat("YYYY:MM:dd-hh:mm:ss");
sdf2.setTimeZone(tz);
System.out.println(sdf2.format(new Date()));


Calendar cal = Calendar.getInstance(tz);
int yy = cal.get(Calendar.YEAR);
int mm = cal.get(Calendar.MONTH) + 1;
int dd = cal.get(Calendar.DAY_OF_MONTH);
int hh = cal.get(Calendar.HOUR);
int mi = cal.get(Calendar.MINUTE);
int ss = cal.get(Calendar.SECOND);
System.out.printf("%d %d %d %d %d %d", yy, mm, dd, hh, mi, ss);
```

- Format
  - 다양항 형식 클래스를 이용해 원하는 형식으로 출력할 수 있다.

```java
double num = 123456.789;

// 숫자 형식 클래스
// 세 자리마다 쉼표가 있고, 소수점 한자리까지 출력
DecimalFormat df = new DecimalFormat("#,###.0");

String str = df.format(num);
System.out.println(str);

// 날짜 형식 클래스
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd, hh:mm:ss, E a");
String str2 = sdf.format(new Date());
System.out.println(str2);
```

- StringBuffer
  - 문자열을 저장하는 String은 저장된 문자열을 수정할 수 없다.
  - StringBuffer는 내부 버퍼에 문자열을 저장해 두고, 그 안에서 추가, 수정, 삭제 작업을 할 수 있다.

```java
StringBuffer sb = new StringBuffer("abcdef");
sb.append("ghi");
System.out.println(sb);
sb.reverse();
System.out.println(sb);
// 새로운 문자열 만들어지지 않음
```

