# Day28 2022-05-20

## AI 플랫폼을 활용한 웹서비스 개발

### AJAX (Asynchronous JavaScript and XML)

비동기 통신을 지원 하는 새로운 방식으로, 새로운 언어가 아닌 JavaScript에 추가된 통신 방식이다.

#### 1. AJAX 실습

1. AJAX를 이용해 3초마다 데이터를 입력받아 출력한다.

```html
<script>
function display(data){
	var str = '';
	$(data).each(function(index,item){
		str += '<h3>';
		str += item.id + ' ' + item.name + ' ' + item.age;
		str += '</h3>';
	});
	$('#result').html(str);
};
function getdata(){
	$.ajax({
		url: 'getdata',
		success: function(data){
			// [{},{},{}...] -> JSON
			display(data);
		}
	});
};

$(document).ready(function(){
	getdata();
	setInterval(() => {
		// alert('get data...');
		getdata();
	}, 3000);

});
</script>
```

```java
@RequestMapping("/getdata")
public Object getdata() {
    JSONArray ja = new JSONArray();
    for (int i = 0; i < 6; i++) {
        JSONObject jo = new JSONObject();
        jo.put("id", "id0" + i);
        jo.put("name", "james" + i);
        Random r = new Random();
        int a = r.nextInt(50) + 1;
        jo.put("age", a);
        ja.add(jo);
    }
    return ja;
}
```

> controller에서 JSONArray 배열을 선언하고, JSONObject에 데이터를 입력받아 JSONArray에 넣고 리턴한다.
>
> Random을 이용해 age의 값을 변경할 수 있다.

#### 2. Chart 만들기

- 아래의 링크에 들어가면 다양한 차트 소스를 받아올 수 있다.
  - https://www.highcharts.com/demo 
- 소스를 복사해서 script영역에 붙여넣기한다.

```html
<script>
function display(){
	const chart = Highcharts.chart('container', {
	    title: {
	        text: 'Chart.update'
	    },
	    subtitle: {
	        text: 'Plain'
	    },
	    xAxis: {
	        categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
	    },
	    series: [{
	        type: 'column',
	        colorByPoint: true,
	        data: [29.9, 71.5, 106.4, 129.2, 144.0, 176.0, 135.6, 148.5, 216.4, 194.1, 95.6, 54.4],
	        showInLegend: false
	    }]
	});
	
	document.getElementById('plain').addEventListener('click', () => {
	    chart.update({
	        chart: {
	            inverted: false,
	            polar: false
	        },
	        subtitle: {
	            text: 'Plain'
	        }
	    });
	});

	document.getElementById('inverted').addEventListener('click', () => {
	    chart.update({
	        chart: {
	            inverted: true,
	            polar: false
	        },
	        subtitle: {
	            text: 'Inverted'
	        }
	    });
	});

	document.getElementById('polar').addEventListener('click', () => {
	    chart.update({
	        chart: {
	            inverted: false,
	            polar: true
	        },
	        subtitle: {
	            text: 'Polar'
	        }
	    });
	});
};

$(document).ready(function(){
	display();
});
</script>
```

```java
@RequestMapping("/getchart")
public Object getchart() {
    JSONArray ja = new JSONArray();
    // [10,20,30,...]
    for (int i = 0; i < 15; i++) {
        Random r = new Random();
        int data = r.nextInt(50) + 1;
        ja.add(data);
    }
    return ja;
}
```

> 위의 소스코드로 고정 값이 나타나는 차트를 만들 수 있고, 
>
> controller에 아래의 소스코드를 입력해 변경 값이 나타나는 차트를 만들 수도 있다(data에 변수 값을 입력한다).

#### 3. Workshop

부트스트랩 템플릿을 활용해서 개인 홈페이지를 만든다.

1. 주제 선정하기
2. 화면 설계하기
   - https://ovenapp.io
3. 테마 서치하기
4. 개발 환경 구축하기
5. 시스템 개발하기

---

1. 주제 선정하기

   - 대면 수업으로 바뀌면서 매일 점심 메뉴를 고민하던 중이라, 점심 메뉴를 정하는데 도움을 줄 수 있는 홈페이지를 만들기로 정하였다.

2. 화면 설계하기

   - 아래 사진과 같이 왼쪽에 버튼을 만들어 버튼을 클릭하면 해당 페이지가 실행되게 할 예정이다.

   <img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220520215514997.png" alt="image-20220520215514997" style="zoom:50%;" />

3. 테마 서치하기

   - https://startbootstrap.com 링크에 들어가서 원하는 테마를 선정하였다.

4. 개발 환경 구축

   - day06 프로젝트를 생성한 뒤 기본 세팅을 완료했다.
   - 테마를 적용시킨 뒤, 필요한 html파일과 controller를 생성하였다.

5. 시스템 개발

   - 음식 사진을 0.5초마다 바뀌게 하여 랜덤 메뉴를 정하는 random.html을 완성했다.
   - 아직 메뉴 목록과 추천메뉴는 완성하지 못하였으며, 테마를 적용시켜서인지 배치에 어려움이 있다.