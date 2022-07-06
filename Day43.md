# Day43 2022-07-05

## AI 플랫폼을 이용한 웹서비스 개발

### NCP

#### 1. AI Services

1. CLOVA OCR

   OCR(Optical Character Recognition)은 광학문자인식 기술

- 인쇄물 상의 글자와 이미지를 디지털 데이터로 변환해 주는 자동 인식 솔루션에 활용
- 활용 사례 - 문서 파일/인쇄물 판독, 문자 자동 번역, 차량번호 자동 인식
- OCR Process 
  - 도메인 생성 => 템플릿 빌더 실행 => 템플릿 생성 => 샘플 설정 => 이미지 판독 => 결과확인

2. CLOVA Speech

   CLOVA Speech는 길이가 긴 오디오/비디오 파일을 업로드 하여 해당 오디오/비디오 파일의 음성 인식 결과 확인

- 적용 제안 - 네이버 뉴스 자막, 고객센터, 자동 자막 생성

3. CLOVA Chatbot

#### 2. AI NAVER API

1. CLOVA Speech Recognition(CSR)

   사람의 목소리를 인식하여 작동하는 비서 애플리케이션, 챗봇, 음성 메모 등의 서비스를 만들 때 활용할 수 있는 음성 인식 API 서비스

   - 활용 사례 - 가전 제품 및 공동 주택의 홈 네트워크 제어, 배달 주문, 금융 서비스에 적용

2. CLOVA Face Recognition(CFR)

   입력된 비전 데이터를 통해 얼굴을 인식하거나 얼굴 감지를 이용한 애플리케이션을 만들 때 유용한 API 서비스

   - 활용 사례 - 얼굴 사진으로 나이 추측, 닮은 꼴 유명인 혹은 인종 찾는 서비스

---

### Kakao API 

#### 1. 환경 구축

1. Kakao Developers 접속 - https://developers.kakao.com

2. 로그인하기
3. 내 애플리케이션 > 애플리케이션 클릭
4. 앱 키 확인하기
   - 지도 API 이용했을 때는 JavaScript 키를 이용
   - 서버와 연동해서 이용할 때는 REST API 키를 이용
5. 플랫폼 > 웹 > 도메인 추가하기

#### 2. API - 키워드로 장소 검색하기

https://developers.kakao.com/docs/latest/ko/local/dev-guide#search-by-keyword

#### 3. API 적용하기

1. 메인 페이지에 `kakaotest`를 클릭하면 kakao 함수를 실행한다.

```html
<body>
	<h1>Main page</h1>
	<h2><a href="/kakao">KAKAOTEST</a></h2>
</body>
```

2. MainController에 kakao 함수를 입력한다.

```java
@Controller
public class MainController {
    
	@RequestMapping("/kakao")
	public String kakao() {
		return "kakao";
	}
}
```

3. kakao.html 파일을 생성한다.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
$(document).ready(function(){
	$('#bt').click(function(){
		var param = $('#param').val();
		$.ajax({
			url:'/kakaolocal',
			data:{'keyword':param},
			success:function(data){
				alert(data);
			}
		});
	});
});
</script>
</head>
<body>
	<h1>Kakao page</h1>
	<input type="text" id="param">
	<button id="bt">Click</button>
</body>
</html>
```

> 데이터를 ajax로 전송하기 때문에 AJAXController 생성하기

4. AJAXController에 함수를 입력한다.

```java
@RestController
public class AJAXController {

	@Autowired
	KakaoAPI kakaoapi;
	
	@RequestMapping("kakaolocal")
	public Object Kakaolocal(String keyword) throws Exception {
		System.out.println(keyword);
		String result = kakaoapi.kakaolocalapi(keyword);
		return result;
	}
}
```

5. KakaoAPI.java 파일을 생성한다.
   - src/main/java > com.ncp.restapi package 생성 > KakaoAPI.java 생성

```java
@Component
public class KakaoAPI {

	public String kakaolocalapi(String keyword) throws Exception {
		String address = "https://dapi.kakao.com/v2/local/search/keyword.JSON";
		
        String param = "query=" + keyword
                //+ "&category_group_code=" + "FD6"
                + "&x=" + "37.5606326"	// 현재 위치
                + "&y=" + "126.9433486"
                + "&radius=" + "1000";	// 반경 1K 이내
                
        String apiKey = "${REST_API_KEY}";	//발급받은 restapi key
		
		
		
		URL url = new URL(address);  			//접속할 url 설정
		HttpURLConnection conn;					//httpURLConnection 객체
		conn = (HttpURLConnection) url.openConnection();	//접속할 url과 네트워크 커넥션을 연다.
		conn.setRequestMethod("POST");             
		conn.setDoOutput(true);
        conn.setUseCaches(false);
		conn.setRequestProperty("Authorization", "KakaoAK " + apiKey);	//Property 설정

		OutputStreamWriter ds = new OutputStreamWriter(conn.getOutputStream());
		ds.write(param);
		ds.flush();
		ds.close();
		
		
		int responseCode = conn.getResponseCode();		//responseCode를 받아옴.
	
		InputStream inputStream = conn.getInputStream();	//데이터를 받아오기 위한 inputStream
		BufferedReader br;		//inputStream으로 들어오는 데이터를 읽기 위한 reader
		String json = null;
		Charset charset = Charset.forName("UTF-8");
		if(responseCode == 200) {
			br = new BufferedReader(new InputStreamReader(inputStream,charset));
			json = br.readLine();
			br.close();
		}
		else {
			System.out.println(" ERROR !!! ");
		}
	
		inputStream.close();
		conn.disconnect();
		
		return json;
	}
}
```

> `post` 방식으로 전송해야 한글이 깨지지 않는다.

6. TestApp을 통해 테스트한다.

```java
@SpringBootTest
class KaKaoTests {
	
	@Autowired
	KakaoAPI kakaoapi;
	
	@Test
	void contextLoads() throws Exception {
		String result = "";
		result = kakaoapi.kakaolocalapi("fitness");
		System.out.println(result);
	}
}
```

