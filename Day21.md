# Day21 2022-05-11

## AI 플랫폼을 활용한 웹서비스 개발

### 1. 기본 태그

1. 글자 태그

   - 제목 글자 

     | 태그 |             설명              |
     | :--: | :---------------------------: |
     |  h1  |  첫 번째로 큰 제목 글자 생성  |
     |  h2  |  두 번째로 큰 제목 글자 생성  |
     |  h3  |  세 번째로 큰 제목 글자 생성  |
     |  h4  |  네 번째로 큰 제목 글자 생성  |
     |  h5  | 다섯 번째로 큰 제목 글자 생성 |
     |  h6  | 여섯 번째로 큰 제목 글자 생성 |

   - 본문 글자

     | 태그 |      설명      |
     | :--: | :------------: |
     |  p   | 본문 문단 생성 |
     |  br  |    줄 바꿈     |
     |  hr  |  수평 줄 삽입  |

   - 앵커 태그

     | 태그 |      설명       |
     | :--: | :-------------: |
     |  a   | 하이퍼링크 생성 |

     - 빈 링크: 웹 표준을 지키면서 이동하지 않는 a 태그를 만들 때는 href 속성에 '#'을 입력한다.

     ```html
     <a href="#">언론사 전체보기</a>
     ```

2. 목록 태그

   | 태그 |         설명          |
   | :--: | :-------------------: |
   |  ul  | 순서가 없는 목록 생성 |
   |  ol  | 순서가 있는 목록 생성 |
   |  li  |    목록 요소 생성     |
   
   - 순서가 없는 기본 목록 만들기
   
   ```html
   <ul>
       <li>사과</li>
       <li>바나나</li>
       <li>오렌지</li>
   </ul>
   ```
   
   - 순서가 있는 목록 만들기
   
   ```html
   <ol>
       <li>First</li>
       <li>Second</li>
       <li>Thrid</li>
   </ol>
   ```
   
   - 중첩 목록 만들기
   
   ```html
   <ul>
       <li>
           <b>과일</b>
           <ol>
               <li>사과</li>
               <li>바나나</li>
               <li>오렌지</li>
           </ol>
       </li>
       <li>
           <b>채소</b>
           <ol>
               <li>상추</li>
               <li>치커리</li>
               <li>양배추</li>
           </ol>
       </li>
   </ul>
   ```
   
3. 테이블 태그

   | 태그  |       설명        |
   | :---: | :---------------: |
   | table |      표 삽입      |
   |  tr   |   표에 행 삽입    |
   |  th   | 표에 제목 셀 생성 |
   |  td   | 표의 일반 셀 생성 |

   - 테이블의 제목은 thead 태그 안에 입력하고, 테이블의 내용은 tbody 태그 안에 입력한다.

4. 미디어 태그

   | 내용물을 가질 수 있는 태그 | 내용물을 가질 수 없는 태그 |
   | :------------------------: | :------------------------: |
   |        audio, video        |            img             |

   - img 태그

   ```html
   <img src="penguins.jpg" alt="펭귄" width="300">
   ```

   - video 태그

   ```html
   <video controls="controls">
   	<source src="Wildlife.mp4" type="video/mp4">
       <source src="Wildlife.webm" type="video/webm">
   </video>
   ```

### 2. 입력 양식 태그

1. 입력 양식 개요
   - form 태그는 method 속성의 방식으로 action 속성 장소에 데이터를 전달한다.
   - GET 방식은 주소에 데이터를 입력해서 전달한다.
   - POST 방식은 비밀스럽게 별도로 데이터를 전달한다.

```html
<form>
    <input type="text" name="search">
    <input type="submit">
</form>

<!-- GET 전송 방식 -->
<form method="get">
    <input type="text" name="search">
    <input type="submit">
</form>

<!-- POST 전송 방식-->
<form method="post">
    <input type="text" name="search">
    <input type="submit">
</form>
```

2. 입력 양식 종류

   - form 태그

   |    속성     |            설명            |
   | :---------: | :------------------------: |
   | 보이지 않음 | 입력 양식의 시작과 끝 표시 |

   - input 태그

   2. - 

   |   속성   |          설명           |
   | :------: | :---------------------: |
   |   text   |   글자 입력 양식 생성   |
   |  button  |        버튼 생성        |
   | checkbox |     체크 박스 생성      |
   |   file   |   파일 입력 양식 생성   |
   |  hidden  |  해당 내용 표시 안 함   |
   |  image   |    이미지 형태 생성     |
   | password | 비밀번호 입력 양식 생성 |
   |  radio   |    라디오 버튼 생성     |
   |  reset   |    초기화 버튼 생성     |
   |  submit  |     제출 버튼 생성      |

   - textarea 태그

   |    속성     |                             설명                             |
   | :---------: | :----------------------------------------------------------: |
   | cols / rows | 여러 행의 글자 입력 양식 생성, cols는 너비, rows는 높이를 지정 |

   - select / optgroup / option
     - 선택 양식 생성 / 옵션 그룹화 / 옵션 생성
   - fieldset / legend
     - 입력 양식의 그룹 지정 / 입력 양식 그룹의 이름 지정
   - label 태그
     - input 태그를 설명할 때 사용한다.
     - label 태그릐 for 속성에 input 태그의 id 속성을 입력해서 label 태그가 어떤 input 태그를 나태내는지 알려준다.

