# Day26 2022-05-18

## AI 플랫폼을 활용한 웹서비스 개발

### jQuery

자바스크립트 라이브러리 중 하나로, 프로그래밍을 아주 쉽고 빠르게 작업할 수 있다.

### 1. Function (함수)

#### 1. HTML Element를 보여주거나 사라지게 하는 함수

|  함수명  |                         설명                         |
| :------: | :--------------------------------------------------: |
|  hide()  | 해당 Element를 없어 지게 한다. 해당 영역도 사라진다. |
|  show()  |          해당 Element를 다시 나타나게 한다.          |
| toggle() | 한번 클릭 시 없어 지고 두 번 클릭 시 나타나게 한다.  |

|    함수명    |                            설명                             |
| :----------: | :---------------------------------------------------------: |
|   fadeIn()   | 해당 Element를 서서히 없어 지게 한다. 해당 영역도 사라진다. |
|  fadeOut()   |          해당 Element를 서서히 다시 나타나게 한다.          |
| fadeToggle() |   한번 클릭 시 없어지고 두번 클릭 시 다시 나타나게 한다.    |

| 함수명        | 설명                                              |
| ------------- | ------------------------------------------------- |
| slideDown()   | 해당 Element를 서서히 하단으로 펼쳐지게 한다.     |
| slideUp()     | 해당 Element를 서서히 위로 닫아준다.              |
| slideToggle() | 한번 클릭 시 내려가고 두번 클릭 시 위로 올라간다. |

#### 2. HTML Element의 Content와 Attributes를 가지고 오거나 변경하는 함수

| 함수명 | 설명                                                |
| ------ | --------------------------------------------------- |
| text() | 해당 Element의 content를 스트링으로 받아 온다.      |
| html() | 해당 Element의 content를 HTML 스트링으로 받아 온다. |
| val()  | Form에서 입력 된 값을 받아 온다.                    |
| attr() | 해당 Element의 속성 값을 가지고 온다.               |

#### 3. HTML Element를 추가 하거나 삭제 하는 함수

| 함수명    | 설명                                                     |
| --------- | -------------------------------------------------------- |
| append()  | Element 영역의 가장 뒷 부분에 HTML을 추가 할 수 있다.    |
| prepend() | Element 영역의 가장 앞 부분에 HTML을 추가 할 수 있다.    |
| after()   | 특정 Element의 앞에 HTML를 추가 한다.                    |
| before()  | 특정 Element의 뒤에 HTML를 추가 한다.                    |
| remove()  | 선택한 Element 영역을 삭제 한다. 영역까지 사라진다.      |
| empty()   | 선택한 Element의 내부 정보만 삭제 한다. 영역은 유지한다. |

#### 4. HTML Element의 CSS를 변경 하는 함수

| 함수명        | 설명                                                    |
| ------------- | ------------------------------------------------------- |
| addClass()    | 특정 Element에 선언 된 class를 삽입할 수 있다.          |
| removeClass() | 특정 Element에 선언 된 class를 제거할 수 있다.          |
| toggleClass() | 한번 클릭 시 class 적용, 두번 클릭 시 class를 제거한다. |

#### 5. 동시에 여러 개의 Element를 선택 하였을 때 특정 위치의 속성을 지정 하는 함수

| 함수명                     | 설명                                                    |
| -------------------------- | ------------------------------------------------------- |
| $(‘td’).first()            | td들 중에 첫 번째 id를 선택한다.                        |
| $(‘td’).last()             | td들 중에 마지막 id를 선택한다.                         |
| $(‘td’).eq(0)              | td들 중에 특정 위치에 있는 td를 선택한다(0부터 시작).   |
| $(‘td’). filter(".intro")  | td들 중에 특정 속성의 td만을 선택한다.                  |
| $(‘td’).not($(‘td’).eq(2)) | td들 중에 선택 한 td를 제외하고 나머지 모두를 선택한다. |

### 2. jQuery 반복문

- jQuery에는 반복문이 없기 때문에 each를 이용해 반복문을 만든다.

```html
var txt = '';
$(data).each(function(index,item){
txt += '<tr>';
    txt += '<td>' + item.id + '</hd>';
txt += '<td>' + item.name + '</hd>';
txt += '<td>' + item.age + '</hd>';
txt += '</tr>';
});
```

### 3. 실습

1. setInterval을 이용해 1초에 한번씩 이미지가 바뀌게 한다.

   ```html
   <meta charset="UTF-8">
   <script>
   $(document).ready(function(){
   	var imgs= ['img/pocky1.jpg','img/pocky2.jpg','img/pocky3.jpg'];
   	var cnt = 0;
   	setInterval(function(){
   		$('#image').attr('src',imgs[cnt % imgs.length]);
   		$('h1').text(cnt);
   		cnt++;
   	}, 1000);
   	
   });
   </script>
   <h1>JQ12 Main</h1>
   <img id="image">
   ```

   > 이미지를 imgs 배열에 넣은 이유는 여러 개의 이미지를 관리하기 위해서이다.
   >
   > `imgs[cnt % imgs.length]`를 통해 무한 루프를 돌 수 있게 만든다.

2. 스크롤이 끝까지 내려오면 데이터를 더 불러온다.

   ```html
   <script>
   $(document).ready(function(){
   	var getdata = function(){
   		for(var i = 0; i < 11; i++){
   			$('#data').append('<img id="simg"src="img/pocky2.jpg">');
   		};
   	};
   	getdata();
   	$(window).scroll(function(){
   		var doch = $(document).height();
   		var winh = $(window).scrollTop() + $(window).height()
   		if (doch <= winh + 50){
   			getdata();
   		};
   	});
   });
   </script>
   ```

   > 스크롤은 window에서 제공하는 기능이기 때문에 $(window)로 불러온다.
   >
   > `if (doch <= winh + 50){}`에서 + 50을 하는 이유는 스크롤이 끝까지 안내려가도 데이터를 불러올 수 있도록 오차범위를 지정한 것이다.

3. keyup, keydown을 이용해 입력 글자수를 제한한다.

   ```html
   <script>
   $(document).ready(function(){
   	$('#n1').keyup(function(){	// 키업과 다운 똑같이 7에서 넘어감
   		if ($(this).val().length > 6){
   			$('#n2').focus();
   		};
   	});
   });
   </script>
   ```

   > keyup과 keydown 모두 7글자까지만 입력되고 다음 입력으로 넘어간다. 
   >
   > 차이점은 keyup은 키보드에서 손을 떼기 전까지는 7글자수 이상 입력되고, 
   >
   > keydown은 키보드에서 손을 떼지 않아도 7글자만 입력된다.

4. 화면이 실행되고 0.5초 후에 박스의 길이가 늘어난다.

   ```html
   <script>
   $(document).ready(function(){
   	$('#box').css({
   		'width':'100px',
   		'height':'100px',
   		'background':'red'
   	}).animate({
   		'width':'500px',
   		'opacity':'0.6'
   	}, 500);
   });
   </script>
   ```

   