# Day29 2022-05-24

## AI 플랫폼을 활용한 웹서비스 개발

### 지도 출력하기

- https://apis.map.kakao.com
- 위의 링크로 접속 후 로그인하기
- 카카오 개발자 사이트 접속
- 개발자 등록 및 앱 생성하기
- 웹 플랫폼 추가하기
- 사이트 도메인 등록하기
- 페이지 우측 상단의 App 키 발급하기
- javascript 키 복사하기
- 원하는 지도 스타일 선택 후 가이드 페이지 따라 실행하기

> 가이드 페이지 : https://apis.map.kakao.com/web/guide

```html
<script>
$(document).ready(function(){
	
	var infowindow = new kakao.maps.InfoWindow({zIndex:1});	//지도를 담을 영역의 DOM 레퍼런스

	var mapContainer = document.getElementById('map');	//지도를 생성할 때 필요한 기본 옵션
	var mapOption = { 
	    center: new kakao.maps.LatLng(37.504856, 127.049078),	//지도의 중심좌표.
	    level: 3 	 //지도의 레벨(확대, 축소 정도)
	};
	//지도 생성 및 객체 리턴
	var map = new kakao.maps.Map(mapContainer, mapOption);
	
	// 일반 지도와 스카이뷰로 지도 타입을 전환할 수 있는 지도타입 컨트롤을 생성합니다
	var mapTypeControl = new kakao.maps.MapTypeControl();

	// 지도에 컨트롤을 추가해야 지도위에 표시됩니다
	// kakao.maps.ControlPosition은 컨트롤이 표시될 위치를 정의하는데 TOPRIGHT는 오른쪽 위를 의미합니다
	map.addControl(mapTypeControl, kakao.maps.ControlPosition.TOPRIGHT);

	// 지도 확대 축소를 제어할 수 있는  줌 컨트롤을 생성합니다
	var zoomControl = new kakao.maps.ZoomControl();
	map.addControl(zoomControl, kakao.maps.ControlPosition.RIGHT);
	
	
	// 장소 검색 객체를 생성합니다
	var ps = new kakao.maps.services.Places(); 

	// 키워드로 장소를 검색합니다
	ps.keywordSearch('선릉역 맛집', placesSearchCB); 

	// 키워드 검색 완료 시 호출되는 콜백함수 입니다
	function placesSearchCB (data, status, pagination) {
	    if (status === kakao.maps.services.Status.OK) {

	        // 검색된 장소 위치를 기준으로 지도 범위를 재설정하기위해
	        // LatLngBounds 객체에 좌표를 추가합니다
	        var bounds = new kakao.maps.LatLngBounds();

	        for (var i=0; i<data.length; i++) {
	            displayMarker(data[i]);    
	            bounds.extend(new kakao.maps.LatLng(data[i].y, data[i].x));
	        }       

	        // 검색된 장소 위치를 기준으로 지도 범위를 재설정합니다
	        map.setBounds(bounds);
	    } 
	}

	// 지도에 마커를 표시하는 함수입니다
	function displayMarker(place) {
	    
	    // 마커를 생성하고 지도에 표시합니다
	    var marker = new kakao.maps.Marker({
	        map: map,
	        position: new kakao.maps.LatLng(37.503504, 127.0503552)
	    });
	    
	    // 마커에 클릭이벤트를 등록합니다
	    kakao.maps.event.addListener(marker, 'click', function() {
	        // 마커를 클릭하면 장소명이 인포윈도우에 표출됩니다
	        infowindow.setContent('<div style="padding:5px;font-size:12px;">' + place.place_name + '</div>');
	        infowindow.open(map, marker);
	    });
	}
});

</script>
```

![image-20220524200938566](C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220524200938566.png)

>  `지도 이동시키기`, `지도에 컨드롤 올리기`, `키워드로 장소 검색하기`를 적용시켜 선릉역 맛집 지도를 만들었다. 