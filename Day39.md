# Day39 2022-06-16

## AI 플랫폼을 활용한 웹서비스 개발

### 웹 - 데이터베이스 연동

#### 1. Thymeleaf 기본 문법

1. 태그에 값 셋팅하기

   ```html
   <div th:text="${sports}"></div>
   <div th:text="${sports.id}"></div>
   <div th:text="${session.logincust.id}"></div>
   ```

2. 태그에 값 셋팅하기 - 연산

   ```html
   <span th:text="${sports}-1"></span><span th:text="${sports-1}"></span>
   ```

3. html 태그가 들어있는 텍스트를 태그로 삽입하고 싶을 때

   ```html
   <p th:utext="${sports}"></p>
   sports = "<span>TEST</span>";
   ```

4. 문자열 합치기

   ```html
   <div th:text="'Hello, ' + ${name} + '!'"></div><div th:text="|Hello, ${name}!|"></div>
   ```

5. Value 값 셋팅하기

   ```html
   <input type="hidden" th:value="${sports}"/>
   ```

6. 반복문

   ```html
   <ul th:each="sports : ${SPORTS}">
      <li th:text="${sports.name}"></li>
      <li th:text="${sports.event}"></li>
   </ul>
   ```

7. if 문

   - 한 가지 조건

   ```html
   <span th:if="${sports.name != null}">
   ```

   - and 또는 or 조건 추가

   ```html
   <span th:if="${sports.name != null and sports.name != ''  }">
   ```

8. if - else 문

   ```html
   <span th:if="${sports['count'] != '1'}">
   <span th:unless="${sports['count'] != '1'}">
   ```

   > if문과 unless문 모두 같은 조건을 써야한다.

9. switch case 문

   ```html
   <td th:switch="${join.gender}">
   <span th:case="'M'" th:text="Male"/>
   <span th:case="'F'" th:text="Female"/></td>
   ```

   > 서로 다른 케이스별로 한 번씩만 적용됨

10. 이미지 태그에 src 속성

    ```html
    <img th:src=" '/img/'+ ${imgname}" alt=""/>
    ```

11. 링크 url

    - 기본적인 링크 삽입

    ```html
    <a th:href="@{/index}"/>
    ```

    - 고정된 url과 변수를 함께 사용

    ```html
    <a th:href="${'http://www.naver.com' + sports}" target="_blank">링크</a>
    ```

    - 링크에 파라미터를 보낼 때

    ```html
    <a th:href="@{/index(id=${id})}"/>
    ```

12. onclick 함수에 값 전달하기

    ```html
    <button th:onclick="addcart([[${p.id}]])" />
    ```

    