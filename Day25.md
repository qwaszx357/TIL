# Day25 2022-05-17

## AI 플랫폼을 활용한 웹서비스 개발

### JavScript

자바 언어를 기반으로 하여, wed browser에서 실행하는 스크립트 언어이다.

#### 1. DOM (Document Object Model)

-  DOM은 문서를 액세스하기 위한 표준을 정의한다.
- W3C DOM 표준은 3개의 부분으로 구분된다.
  - 햄심 DOM, XML의 DOM, HTML DOM

#### 2. BOM

- BOM은 JavaScript를 통해 Brower에서 제공하는 기능을 제어하는 방법을 제공한다.
- 다음과 같은 정보를 사용할 수 있다.
  - window, screen, location, navigator, popup, timing

### jQuery

자바스크립트 라이브러리 중 하나로, 프로그래밍을 아주 쉽고 빠르게 작업할 수 있다.

> jQuery는 사용 전에 선언하고 사용되며, 자바스크립트와 함께 사용된다.

#### 1. jQuery 특징

- 이벤트 처리가 쉽다.
- HTML DOM, CSS, AJAX 관련 처리가 쉽다.
- 모든 브라우저에서 동일하게 동작한다.
- 다양한 플러그인을 제공한다.

#### 2. jQuery 설치

1. jQuery.org 사이트에서 라이브러리를 다운받는다.

2. CDN (Content Delivery Network) 방식을 이용한다.

   ```html
   <head>
   <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js">
   </script>
   </head>
   ```

#### 3. jQuery 구조

1. jQuery Structure (기본 구조)

   ```html
   <script>
   $(document).ready(function(){
   $('h1').css('color','red');
   });
   </script>
   ```

2.  jQuery syntax(구문)

   - $(selector).action()
   - 위의 JavaScript에서의 window.onload를 jQuery에서는 아래와 같이 작성한다.

   ```html
   // JavaScript
   <script>
   window.onload = function(){
    
   };
   </script>
   ```

   ```html
   // jQuery
   <script>
   $(document).ready(function(){
   });
   </script>
   ```

#### 4. function

| 함수명             | 설명                                                      |
| ------------------ | --------------------------------------------------------- |
| click()            | 클릭 이벤트 처리                                          |
| dblclick()         | 더블 클릭 이벤트 처리                                     |
| mouseenter()       | 마우스가 해당 element 안으로 이동했을 때 이벤트 처리      |
| mouseleave()       | 마우스가 해당 element 밖으로 이동했을 때 이벤트 처리      |
| mousedown()        | 마우스가 해당 element에서 클릭했을 때 이벤트 처리         |
| mouseup()          | 마우스가 해당 element에서 클릭을 해제했을 때 이벤트 처리  |
| hover()            | 마우스가 해당 element에 들어가고 나갈 때 각각 이벤트 처리 |
| focus() and blur() | form tag에서 포커스가 들어가고 나갈 때 각각 이벤트 처리   |