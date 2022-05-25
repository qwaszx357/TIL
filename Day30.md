# Day30 2022-05-25

## AI 플랫폼을 활용한 웹서비스 개발

### 지도 출력하기2

#### 1. 마커에 클릭 이벤트, 마우스 이벤트 등록하기

- https://apis.map.kakao.com
- 위의 링크에서 `마커에 클릭 이벤트 등록하기`, `마커에 마우스 이벤트 등록하기`를 적용해 보았다.
- 지난번과 동일하게 환경을 세팅한다.
- script영역에 아래의 코드를 추가한다.

```html
<script>
    // 마커에 커서가 오버됐을 때 마커 위에 표시할 인포윈도우를 생성합니다
	var iwContent = '<div style="padding:5px;">Hello World!</div><h3><a href="">GO</a>		</h3><img width="50px" src="img/pocky1.jpg">'; // 인포윈도우에 표출될 내용으로 HTML 문자열이나 document element가 가능합니다

	// 인포윈도우를 생성합니다
	var infowindow = new kakao.maps.InfoWindow({
	    content : iwContent
	});
    
	// 마커에 클릭 이벤트를 등록합니다
	kakao.maps.event.addListener(marker, 'click', function() {
	    // 마커에 마우스아웃 이벤트가 발생하면 인포윈도우를 제거합니다
	    location.href="aj06";
	});
        
    // 마커에 마우스오버 이벤트를 등록합니다
	kakao.maps.event.addListener(marker, 'mouseover', function() {
	  // 마커에 마우스오버 이벤트가 발생하면 인포윈도우를 마커위에 표시합니다
	    infowindow.open(map, marker);
	});

	// 마커에 마우스아웃 이벤트를 등록합니다
	kakao.maps.event.addListener(marker, 'mouseout', function() {
	    // 마커에 마우스아웃 이벤트가 발생하면 인포윈도우를 제거합니다
	    infowindow.close();
	});
</script>
```

> 마커를 클릭하면 인포윈도우에 div 영역이 출력되도록 했다.
>
> div영역에는 a 태그와 이미지를 넣었다.

#### 2. 여러개 마커에 표시하기

- 동일하게 지도를 생성한다.
- 객체 배열을 만들어 title과 위치, 이동할 곳을 입력한다.

```html
<script>
function getmarkers(loc){
	var pos = null;
	var pos = [
	    {
	        content: '<div>카카오1</div>', 
	        lat: 37.55041692365908,
	    	lng: 126.91037178013711,
	    	target: 'js01'
	    },
	    {
	    	content: '<div>카카오2</div>', 
	        lat: 37.55041692365908,
	    	lng: 126.92037178013711,
	    	target: 'js02'
	    },
	    {
	    	content: '<div>카카오3</div>', 
	        lat: 37.55041692365908,
	    	lng: 126.93037178013711,
	    	target: 'js03'
	    },
	    {
	    	content: '<div>카카오4</div>', 
	        lat: 37.55041692365908,
	    	lng: 126.94037178013711,
	    	target: 'js04'
	    }
	];
	displaymarker(pos);
};
</script>
```

> ```html
> latlng: new kakao.maps.LatLng(33.450705, 126.570677)
> ```
>
> 위의 코드처럼 한 줄로 위치를 입력하는 것도 가능하다.
>
> `target`을 이용하면 클릭했을 마커를 클릭했을 때 원하는 곳으로 이동이 가능하다.

#### 3. 버튼을 클릭하면 원하는 위치로 이동하기

- 위치를 이동시킬 버튼을 생성한다.

```html
<button id="s">Seoul</button>
<button id="b">Busan</button>
<button id="k">Kwangju</button>
```

- 문서가 준비되고 버튼을 클릭했을 때, 버튼 아이디에 따라 이동할 장소를 입력한다.

```html
<script>
$(document).ready(function(){
	displaymap();
	
	$('#s').click(function(){
		gomap('s');
	});
	$('#b').click(function(){
		gomap('b');
	});
	$('#k').click(function(){
		gomap('k');
	});

});
    
function gomap(loc){
	var latlng = null;
	
	if (loc == 's') {
		latlng = new kakao.maps.LatLng(37.55041692365908, 126.91037178013711)
		getmarkers('s');
	} else if (loc == 'b') {
		latlng = new kakao.maps.LatLng(35.17642453774257, 129.16669784099807)
		getmarkers('b');
	} else if (loc == 'k') {
		latlng = new kakao.maps.LatLng(35.16173425533525, 126.88758871719189)
		getmarkers('k');
	}
	map.panTo(latlng);
	
};
</script>
```

> 위치를 이동하는 함수로 `panTo`를 이용했는데 `setCenter`도 가능하다.

#### 4. AJAX를 이용해 버튼을 클릭하면 마커 추가하기

- script 영역에 getmarkers 함수를 추가한다.

```html
<script>
function getmarkers(loc){
	$.ajax({
		url: 'getmarkers',
		data: {'loc': loc},
		success: function(data){
			// alert(data);
			displaymarker(data);
		}
	});
};
</script>
```

- AJAXController에 아래의 코드를 추가한다.

```java
@RestController
public class AJAXController {
	@RequestMapping("/getmarkers")
    public Object getmarkers(String loc) {
        JSONArray ja = new JSONArray();
        if(loc.equals("s")) {
            JSONObject jo1 = new JSONObject();
            jo1.put("content", "<div>카카오1</div>");
            jo1.put("lat", 37.55041692365908);
            jo1.put("lng", 126.91037178013711);
            jo1.put("target", "js01");
            ja.add(jo1);
        } else if(loc.equals("b")) {
            JSONObject jo1 = new JSONObject();
            jo2.put("content", "<div>카카오2</div>");
            jo2.put("lat", 35.17642453774257);
            jo2.put("lng", 129.16669784099807);
            jo2.put("target", "js02");
            ja.add(jo2);
        } else if(loc.equals("k")) {
            JSONObject jo3 = new JSONObject();
            jo3.put("content", "<div>카카오3</div>");
            jo3.put("lat", 35.16173425533525);
            jo3.put("lng", 126.88758871719189);
            jo3.put("target", "js03");
            ja.add(jo3);
        }
        return ja;
    }   
}
```

> 버튼을 클릭하면 해당 위치에 마커가 추가되고 target위치로 이동하게 된다.