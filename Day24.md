# Day24 2022-05-16

## AI 플랫폼을 활용한 웹서비스 개발

### JavaScript

자바 언어를 기반으로 하여, wed browser에서 실행하는 스크립트 언어이다.

#### 1. JavaScript 필요성

1. HTML 내부 Content를 자유롭게 변경 추가가 가능하게 한다.
2. CSS를 자유롭게 변경 한다.
3. Event 처리 및 Validation 행위를 제어할 수 있다.
4. Web Page에서 프로그램 부분을 담당한다.

#### 2. JavaScript 위치

1.  JavaScript in `<head>`

   ```html
   <head>
   <script>
   function myFunction() {
    document.getElementById("demo").innerHTML = 
    "Paragraph changed.";
   }
   </script>
   </head>
   ```

2. JavaScript in `<body>`

   ```html
   <body>
   <h1>My Web Page</h1>
   <p id="demo">A Paragraph</p>
   <button type="button" onclick="myFunction()">Try it</button>
   <script>
   function myFunction() {
    document.getElementById("demo").innerHTML = "Paragraph 
   changed.";
   }
   </script>
   </body>
   ```

3. External JavaScript

   ```html
   <!DOCTYPE html>
   <html>
   <body>
   <script src="myScript.js"></script>
   </body>
   </html>
   ```

   ```js
   /* myScript.js */
   function myFunction() {
    document.getElementById("demo").innerHTML = 
    "Paragraph changed.";
   }
   ```

#### 3. JavaScript Identifiers (식별자)

변수 선언, 함수 선언, Object 생성 시 명명 규칙

- 문자, 숫자, _, $ 기호를 포함할 수 있다.
- 스페이스는 포함할 수 없다.
- 문자(대소문자 구분) 또는 _, $로 시작해야 하며 숫자로 시작할 수 없다.
- 예약어는 사용할 수 없다.

#### 4. JavaScript Data Type

JavaScript에서는 모든 변수 선언을 var로 한다.

- undefined: 어떠한 데이터 타입도 선언되지 않은 상태이다.
- number: 실수, 정수 모두 포함한다.
- boolean: true(1), false(0)를 의미 한다.
- string: 문자열, character를 모두 포함한다. “”,’’로 표현한다(주로'' 사용).
- array: 배열, 다양한 타입을 포함 할 수 있다. 데이터 타입은 object 로 표현한다. []로 사용한다.
- object: 객체 type 을 의미 {}로 사용한다.
- function: 함수를 의미, JavaScript에서는 함수를 하나의 타입으로 선언 하여 사용 할 수 있다.

#### 5. JavaScript Number

Number로 변경할 수 있는 함수이다.

1. Global 함수
   - Number(): number 타입이 아닌 변수를 number로 변경한다.
   - parseInt(): 정수 형으로 변경한다.
   - parseFloat(): 실수 형으로 변경한다.
2. Number 함수
   - toString(): number를 string으로 변경한다.
   - toFixed(): 특정 수까지 반올림 하여 문자로 변환한다.
   - toPrecision(): 특정 수에서 반올림 하여 문자로 변환한다.

#### 6. JavaScript Function

1. 함수 선언 방법

   - 함수 명 선언

   ```html
   <script>
    function funcA(){
    alert(‘Function A’);
    };
    funcA(); // 함수 실행
   </script>
   ```

   - 변수 명 선언

   ```html
   <script>
    var funcB = function(){
    alert(‘Function B’);
    };
    funcB(); // 함수 실행
   </script>
   ```

2. Argument와 Return Value의 타입을 명시 하지 않는다.

3. Argument와 Return Value에 함수도 사용 할 수 있다

#### 7. JavaScript 내장 함수

- eval(string): string을 자바스크립트 코드로 실행한다.
- isFinite(number): number가 무한 값인지 확인한다.
- isNaN(number): number가 숫자가 아닌지(not a number) 확인한다.

#### 8. JSON (JavaScript Object Notation)

- 경량 형식으로 데이터들을 시스템 간 전송 하는데 사용된다.
- JavaScript에서 해석과 데이터 추출이 편리하다.

#### 9. 출력

1. alert 창

   ```html
   <script>
   alert('Hello JavaScript 1');
   </script>
   ```

2. window.onload

- <body>와 사</body>가 정상 동작 이후 window.onload 함 수가 동작한다.

```html
<script>
// 화면 출력 후 함수가 동작
window.onload = function(){
alert('Hello JavaScript 1');
};
</script>
```

3. 특정 영역에 스트링 값을 출력

```html
<script>
window.onload = function(){
 document.getElementById("demo1").innerHTML = ‘DEMO1 TEST’;
 document.getElementById("demo2").innerHTML = “DEMO2 TEST”;
};
</script>
```

4. Web Browser 콘솔에 log 출력

```html
<script>
window.onload = function(){
 var a = 100;
 var b = 200.0;
 c = a * b;
 console.log(‘Log Test:’+c);
};
</script>
```

