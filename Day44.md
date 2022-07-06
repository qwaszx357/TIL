# Day44 2022-07-06

## AI 플랫폼을 활용한 웹서비스 개발

### AI NAVER API 

#### 1. Application 등록하기

1. Services > AI·NAVER API 클릭

2. `Application 등록` 클릭

3. Application 이름 설정 후 원하는 Service 선택

![image-20220706093105072](C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220706093105072.png)

4. 서비스 환경 등록
   - Web 서비스 URL 등록 - `http://127.0.0.1`
5. 인증 정보 확인
   - Application에서 인증 정보를 확인한다.
   - Client id, Client Secret 기억해놓기

#### 2. API 적용하기 - Papago Text Translation

1. 메인 페이지에서 `papago test`를 클릭하면 papago함수를 실행한다.

```html
<body>
	<h1>Main page</h1>
	<h2><a href="/kakao">KAKAO TEST</a></h2>
    <h2><a href="/papago">PAPAGO TEST</a></h2>
</body>
```

2. MainController에 papago함수를 입력한다.

```java
@Controller
public class MainController {
    
		@RequestMapping("/papago")
	public String papago() {
		return "papago";
	}
}
```

3. papago.html 파일을 생성한다.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
function display(data){
	var trdata = data.message.result.translatedText;
	// alert(trdata);
	$('#result').text(trdata);
}

$(document).ready(function(){
	$('#bt').click(function(){
		var param = $('#param').val();
		$.ajax({
			url:'/papagotr',
			data:{'txt':param},
			success:function(data){
				display(data);
			}
		});
	});
});
</script>
</head>
<body>
	<h1>Papago page</h1>
	<input type="text" id="param">
	<button id="bt">Click</button>
	<h2 id="result"></h2>
</body>
</html>
```

> 데이터를 ajax로 전송하기 때문에 AJAXController 생성하기

4. AJAXController에 함수를 입력한다.

```java
@RestController
public class AJAXController {

	@Autowired
	NaverAPI naverapi;
	
	@RequestMapping("papagotr")
	public Object papagotr(String txt) throws Exception {
        Object obj = naverapi.papago(txt);
		return obj;
	}
}
```

5. PapagoAPI.java 파일을 생성한다.
   - src/main/java > com.ncp.restapi package 생성 > PapagoAPI.java 생성

```java
@Component
public class NaverAPI {

	String clientId = "${client Id}";// 애플리케이션 클라이언트 아이디값";
	String clientSecret = "${client Secret}";// 애플리케이션 클라이언트 시크릿값";

	public Object papago(String txt) {
		String result = "";
	      try {
	            String text = URLEncoder.encode(txt, "UTF-8");
	            String apiURL = "https://naveropenapi.apigw.ntruss.com/nmt/v1/translation";
	            URL url = new URL(apiURL);
	            HttpURLConnection con = (HttpURLConnection)url.openConnection();
	            con.setRequestMethod("POST");
	            con.setRequestProperty("X-NCP-APIGW-API-KEY-ID", clientId);
	            con.setRequestProperty("X-NCP-APIGW-API-KEY", clientSecret);
	            // post request
	            String postParams = "source=ko&target=en&text=" + text;
	            con.setDoOutput(true);
	            DataOutputStream wr = new DataOutputStream(con.getOutputStream());
	            wr.writeBytes(postParams);
	            wr.flush();
	            wr.close();
	            int responseCode = con.getResponseCode();
	            BufferedReader br;
	            if(responseCode==200) { // 정상 호출
	                br = new BufferedReader(new InputStreamReader(con.getInputStream()));
	            } else {  // 오류 발생
	                br = new BufferedReader(new InputStreamReader(con.getErrorStream()));
	            }
	            String inputLine;
	            StringBuffer response = new StringBuffer();
	            while ((inputLine = br.readLine()) != null) {
	                response.append(inputLine);
	            }
	            br.close();
	            result = response.toString();
	            System.out.println(response.toString());
	        } catch (Exception e) {
	            System.out.println(e);
	        }
	      
	      JSONParser parser = new JSONParser();
	      Object obj = null;
	      try {
			obj = parser.parse(result);
		} catch (ParseException e) {
			e.printStackTrace();
		}
	    return obj;
	}
}
```

> `post` 방식으로 전송해야 한글이 깨지지 않는다.

6. TestApp을 통해 테스트한다.

```java
@SpringBootTest
class PAPAGOTest {

	@Autowired
	NaverAPI naverapi;
	
	@Test
	void contextLoads() {
		String txt = "밥 먹었어?";
		naverapi.papago(txt);
	}	
}
```

