# Day27 2022-05-19

## AI 플랫폼을 활용한 웹서비스 개발

### AJAX (Asynchronous JavaScript and XML)

비동기 통신을 지원 하는 새로운 방식으로, 새로운 언어가 아닌 JavaScript에 추가된 통신 방식이다.

#### 1. AJAX 특징

- 화면 전체를 재로드 하지 않고도 서버에서 특정 데이터를 송수신 할 수 있다(비동기적 업데이트 가능).
- 빠르고 동적인 웹 페이지를 만들기 위한 기술이다.
- ex) 주가, 실시간 검색어

#### 2. jQuery를 이용한 AJAX

- jQuery에서는 기본적으로 $.ajax()를 제공한다.
- 작성 예시

```javascript
$("button").click(function(){
    $.ajax({
        url: “serverApp.jsp",
        success: function(result){
        $("#div1").html(result);
        }
    });
});
```

#### 3. 실습

1. ajax를 이용해 1초마다 서버 시간을 받아서 출력한다.

   ```html
   <meta charset="UTF-8">
   <script>
   function display(data){
   	$('h3').text(data);
   };
   
   function getdata(){
   	$.ajax({
   		url: 'gettime',
   		success: function(data){
   			display(data);
   		},
   		error: function(e){
   			alert('Error' + e.responseText);
   		}
   	});
   };
   
   $(document).ready(function(){
   	setInterval(function(){
   		getdata();
   	}, 1000);
   });
   </script>
   <h1>AJ01 Main</h1>
   
   <h3></h3>
   ```

   ```java
   @RestController
   public class AJAXController {
   	
   	@RequestMapping("/gettime")
   	public Object gettime() {
   		Date d = new Date();
   		return "서버 시간: " + d.toString();
   	}
   }
   ```

   > AJAX를 이용할 때는 따로 Controller를 만들어서 작업하는 것이 편리하다.

2. 입력 값에 따른 관심사를 출력한다.

   ```html
   <meta charset="UTF-8">
   <style>
   	#result{
   		width: 500px;
   		border: 2px solid red;
   	}
   </style>
   <script>
   function display(data){
   	var txt = '<h2>' + data + '</h2>';
   	$('#result').html(txt);
   };
   
   function getdata(txt){
   	$.ajax({
   		url: 'search',
   		data: {'s': txt},
   		success: function(data){
   			display(data);
   		}
   	});
   };
   
   $(document).ready(function(){
   	$('button').click(function(){
   		var txt = $('#txt').val();
   		getdata(txt);
   	});
   });
   </script>
   <h1>AJ02 Main</h1>
   <input type="text" id="txt">
   <button>GETDATA</button>
   <div id="result"></div>
   ```

   ```java
   @RequestMapping("/search")
   public Object search(String s) {
       String data = "";
       if (s.equals("a")) {
           data = "주식에 관심";
       } else if (s.equals("b")) {
           data = "햄버거에 관심";
       } else {
           data = "밥에 관심";
       }
       return data;
   }
   ```

   > 데이터베이스와 연동하면 유용한 프로그램이 될 것 같다.

3. 회원가입 화면에서 아이디 입력시 사용 가능한 아이디인지 확인한다.

   ```html
   <meta charset="UTF-8">
   <style>
   	#result{
   		width: 500px;
   		border: 2px solid red;
   	}
   </style>
   <script>
   function checkid(id){
   	$.ajax({
   		url: 'checkid',
   		data: {'id':id},
   		success: function(data){
   			if (data =='1'){
   				$('#iid').text("사용 가능 합니다.");
   			} else {
   				$('#iid').text("사용 불가능 합니다.");
   			}
   		}
   	});
   };
   
   $(document).ready(function(){
   	
   	$('button').attr('disabled','disabled');
   	
   	$('button').click(function(){
   		$('#register_form').attr({
   			'action':'register_formimpl',
   			'method':'post'
   		});
   		$('#register_form').submit();
   	});
   	
   	$('input[name="pwd2"]').keyup(function(){
   		var pwd = $('input[name="pwd"]').val();
   		var pwd2 = $('input[name="pwd2"]').val();
   		if (pwd == pwd2){
   			$('#ipwd').text("Password가 일치 합니다.");
   			$('button').removeAttr('disabled');
   		}
   	});
   	
   	$('input[name="id"]').keyup(function(){
   		var id = $(this).val();
   		// 길이가 3자리 미만이면 span에 "3자리 이상이어야 합니다." 출력하기
   		if (id.length < 3){
   			$('#iid').text("3자리 이상이어야 합니다.");
   		} else {
   			$('#iid').text('');
   			checkid(id);
   		};
   	});
   });
   </script>
   <h1>AJ03 Main</h1>
   <form id="register_form">
   ID<input type="text" name="id"><span id="iid"></span><br>
   PWD<input type="password" name="pwd"><span id="ipwd"></span><br>
   PWD2<input type="password" name="pwd2"><br>
   <button>REGISTER</button>
   </form>
   ```

   ```java
   @RequestMapping("/checkid")
   public Object checkid(String id) {
       String result = "";
       if (id.equals("aaaa") || id.equals("bbbb") || id.equals("cccc")) {
           result = "0";
       } else {
           result = "1";
       }
       return result;
   }
   ```

   ```java
   @RequestMapping("/register_formimpl")
   public String register_formimpl(Model m, String id, String pwd) {
       System.out.println(id + " " + pwd);
       m.addAttribute("center", "ajax/registerok");
       m.addAttribute("left", "ajax/left");
       m.addAttribute("rid", id);
       return "main";
   }
   ```

   > 사용 가능한 아이디인지는 AJAX를 이용해 확인하고, jQuery를 이용해 아이디에 대한 제한을 두고 패스워드가 동일한지 확인할 수 있다.

