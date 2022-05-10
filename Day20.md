# Day20 2022-05-10

## Al 플랫폼을 활용한 웹서비스 개발

### 1. HTML

- `인터넷`은 전 세계를 연결하는 국제 정보 통신망으로, 컴퓨터나 스마트폰 같은 디지털 기기로 연결되어 사람들이 정보를 공유할 수 있는 공간이다.
- `W3C`는 HTML 표준을 비롯한 웹 표준안을 제작하거나 제안하는 일을 하는 국제적인 웹 표준화 단체이다.
- `web`은 요청과 응답이라는 간단한 형태로 동작한다.
  - 요청하는 쪽은 클라이언트(사용자)라고 하며, 응답하는 쪽을 서버(제공자)라고 한다.
- `URL`은 웹에서 어떤 대상을 구분하는 주소이다.
-  `HTML5`는 큰 의미로 웹 표준 기술을 총칭한다(CSS3와 자바스크립트를 모두 포함). 작은 의미로는 웹 페이지를 구성하는 HTML 마크업 언어 그 자체이다.
- `CSS`는 HTML 페이지에 스타일을 지정하는 스타일시트를 작성할 때 사용하는 언어이다.
- `자바스크립트`는 HTML 페이지에서 사용자 반응 등을 처리하는 스크립트를 작성하는 언어이다.

### 2. STS 환경 설정

1. eclipse 실행

2. Help > Eclipse Marketplace 선택

3. STS 검색

4. Spring Tools 3와 Spring Tools 3 Add-On 설치

5. eclipse 재실행

6. Window > Preference > Java > Installed JRE

   Edit -> C:\Program Files\Java\jdk1.8.0_321

### 3. SpringBoot Web Application 환경 셋팅

1. project 생성

2. Spring Boot > Spring Starter Project 선택

3. 프로젝트명 입력

4. Group과 Package 명 선택, 반드시 두개 이상의 package 명으로 입력

5. Dependencies 선택
   - Spring Boot DevTools
   - Spring Web

6. pom.xml 추가

```HTML5
        <!-- @Inject -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>
		<!-- Servlet -->

		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
			<scope>provided</scope>
		</dependency>


		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.0.1</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>
		
		<!-- json request -->   

		<dependency>
			<groupId>com.googlecode.json-simple</groupId>
			<artifactId>json-simple</artifactId>
			<version>1.1</version>
  		</dependency>
```

- <dependencies> </dependencies> 사이에 추가한다.

7. Directory 생성
   - src > main > webapp > WEB-INF > views

8. Proejct Properties > Java Build Path > Source에서 Add Folder
   - WEB-INF > views 선택 추가

9. src/main/resources 에 applications.properties 파일 수정 (반드시 메모장으로 열것)

```
server.port=80
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```

10. 80 port 문제 시 
    - 제어판 > 시스템 및 보안 > 관리도구 > 서비스 > World Wide Web Publishibg  서비스 중지 및 수동으로 전환

### 4. HTML 기본 구조

```Html
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title her</title>
</head>
<body>

</body>
</html>
```

- `<!DOCTYPE html>`은 HTML 문서라는 것을 알려준다.
- `<html>`은 HTML 페이지의 기본 요소로, 모든 태그는 이 태그 안에 작성한다.
- `<head>`는 body 태그에 필요한 스타일시트와 자바스크립트를 제공한다.
- `<title>`은 웹 브라우저에 표시하는 제목을 지정한다.
- `<body>`는 사용자에게 실제 보이는 부분을 작성하는 곳이다.

### 5. CSS 외부파일 연동

1. src/main/resources > static 에 css 폴더를 만든다.
2. New > CSS File 클릭 후 이름은 html이름.css로 한다.
3. 디자인 영역을 작성한다.

```html
    h1{
        color:hotpink;
    }
    h2{
        color:green;
    }
    button{
        color:yellow;
        background:black;
    }
```

4. html에서 <html> 태그 안에 연동한다.

```html
<link rel = "stylesheet" href = "css/index.css">
```

### 6. Script 외부파일 연동

1. src/main/resources > static 에 js 폴더를 만든다.
2. New > JavaScript File 클릭 후 이름은 html이름.js로 한다.
3. 함수를 정의한다.

```html
	function go(){
		alert('Clicked');
	};
```

4. html에서 <html> 태그 안에 연동한다.

```html
<script src = "js./index.js"></script>
```

### 7. 이미지 삽입

1. src/main/resources > static 에 img 폴더를 만든다.
2. 폴더 안에 이미지를 넣는다.
3. html에서 <body> 태그 안에 입력한다.

```
<img src = "img/Lotto UML.png">
```

