# Day36 2022-06-13

## AI 플랫폼을 활용한 웹서비스 개발

### 웹 - 데이터베이스 연동

#### 1. 차트 데이터 연동하기

- https://www.highcharts.com 원하는 차트를 선택한다.

- 메인 페이지에 아래의 스크립트를 추가한다.

```html
<script src="https://code.highcharts.com/highcharts.js"></script>
<script src="https://code.highcharts.com/modules/exporting.js"></script>
<script src="https://code.highcharts.com/modules/export-data.js"></script>
<script src="https://code.highcharts.com/modules/accessibility.js"></script>
```

- 차트가 그려질 페이지에 영역을 추가한다.

```html
<div id="container"></div>
```

- style에서 차트의 크기를 설정한다.

```html
<style>
	#container{
		width: 500px;
		height: 300px;
	}
</style>
```

- script에서 ajax와 연동한다.

```html
<script>
function display(data){
	Highcharts.chart('container', {
	    chart: {
	        plotBackgroundColor: null,
	        plotBorderWidth: null,
	        plotShadow: false,
	        type: 'pie'
	    },
	    title: {
	        text: 'Browser market shares in January, 2018'
	    },
	    tooltip: {
	        pointFormat: '{series.name}: <b>{point.percentage:.1f}%</b>'
	    },
	    accessibility: {
	        point: {
	            valueSuffix: '%'
	        }
	    },
	    plotOptions: {
	        pie: {
	            allowPointSelect: true,
	            cursor: 'pointer',
	            dataLabels: {
	                enabled: true,
	                format: '<b>{point.name}</b>: {point.percentage:.1f} %'
	            }
	        }
	    },
	    series: [{
	        name: 'Brands',
	        colorByPoint: true,
	        data: data
	    }]
	});
};
function getdata(){
	$.ajax({
		url:'/chart1',
		success:function(data){
			// alert(data);
			display(data);
		}
	});
	display();
};
$(document).ready(function(){
	getdata();
});
</script>
```

> 차트를 display에 작성하고, getdata에서 ajax로 데이터를 불러온다.

- AJAXController에서 chart1함수를 작성한다.

```java
@Autowired
ProductBiz pbiz;

@RequestMapping("/chart1")
public Object chart1() {
    //[{},{}]
    JSONArray ja = new JSONArray();
    List<ProductAVGVO> list = null;
    try {
        list = pbiz.get3();
        for (ProductAVGVO p : list) {
            JSONObject jo = new JSONObject();
            jo.put("name", p.getCatename());
            jo.put("y", p.getAvg());
            ja.add(jo);
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
    return ja;
}
```

#### 2. 검색 기능 추가하기

- MainBiz.java에 아래의 코드를 추가한다.

```java
public List<ProductVO> searchproduct(String txt) throws Exception{
    return dao.searchproduct(txt);
}
```

- MainMapper.xml에 아래의 코드를 추가한다.

```xml
<select id="searchproduct" parameterType="String" resultType="productVO">
    SELECT p.id, p.name, p.regdate, p.imgname,
    p.cid, p.price, c.name as catename
    FROM product p
    INNER JOIN cate c ON p.cid = c.id
    WHERE p.name LIKE CONCAT('%',#{txt},'%')
</select>
```

- search.html 파일을 생성한다.

```html
<style>
	#main{
		width:200px;
	}
</style>

    <!-- Page Heading -->
    <div class="d-sm-flex align-items-center justify-content-between mb-4">
        <h1 class="h3 mb-0 text-gray-800">Search Page</h1>
    </div>

    <!-- Content Row -->
    <div class="row" th:each="p:${sproduct}">

        <!-- Earnings (Monthly) Card Example -->
        <div class="col-xl-12 col-md-12 mb-12">
            <div class="card border-left-primary shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col-xl-4">
                            <a th:href="@{/product/detail(id=#{p.id})}">
                            <img id="main" src="@{'/img/+${p.imgname}}">
                            </a>
                        </div>
                        <div class="col-xl-8">
	                        <div class="h5 font-weight-bold text-primary mb-0"
	                        th:text="${p.catename}">
                                Category</div>  
                            <div class="h5 font-weight-bold text-primary mb-0"
                            th:text="${p.name}">
                                Customer Count</div>
                            <div class="h6 mb-0 font-weight-bold text-gray-800"
                            th:text="${p.price}">
                            	30000
                            </div>
                            <div class="h6 mb-0 font-weight-bold text-gray-800"
                            th:text="${p.regdate}">
                            	2020-06-10
                            </div>
                        </div>
                        
                    </div>
                </div>
            </div>
        </div>
     
    </div>
```

- 검색을 할 페이지에 아래의 코드를 작성한다.

```html
<!-- Topbar Search -->
    <form id="searchform"
          class="d-none d-sm-inline-block form-inline mr-auto ml-md-3 my-2 my-md-0 mw-100 navbar-search">
        <div class="input-group">
            <input type="text" name="txt" class="form-control bg-light border-0 small" placeholder="Search for..."
                   aria-label="Search" aria-describedby="basic-addon2">
            <div class="input-group-append">
                <button id="searchbtn" class="btn btn-primary" type="button">
                    <i class="fas fa-search fa-sm"></i>
                </button>
            </div>
        </div>
    </form>
```

- MainController에 search 함수를 추가한다.

```java
@RequestMapping("search")
public String search(Model m, String txt) {
    List<ProductVO> list  = null;
    try {
        list = biz.searchproduct(txt);
        m.addAttribute("sproduct", list);
    } catch (Exception e) {
        e.printStackTrace();
    }
    m.addAttribute("center", "search");
    return "/index";
}
```

#### 3. NEW 테이블 만들고 자바와 연동하기

- MySQL에서 admin 테이블을 생성한다.

```sql
CREATE TABLE admin(
	ID VARCHAR(10) PRIMARY KEY,
    PWD VARCHAR(20)
);
```

- vo 폴더에 AdminVO.java 파일을 생성한다.

```java
package com.multi.vo;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
@AllArgsConstructor
@NoArgsConstructor
public class AdminVO {
	private String id;
	private String pwd;
}
```

- mainmapper.xml 파일에서 아래의 코드를 작성한다.

```xml
<select id="getadmin" parameterType="String" resultType="adminVO">
    SELECT * FROM ADMIN WHERE ID=#{id}
</select>
```

- MainMapper.java 파일에 아래의 코드를 추가한다.

```java
public AdminVO getadmin(String id) throws Exception;
```

- MainBiz.java 파일에 아래의 코드를 추가한다.

```java
public AdminVO getAdmin(String id) throws Exception{
    return dao.getadmin(id);
}
```

- Test를 실행해본다.

```java
package com.multi.main;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import com.multi.biz.MainBiz;
import com.multi.vo.AdminVO;

@SpringBootTest
class GetAdminTests {
	
	@Autowired
	MainBiz biz;
	
	@Test
	void contextLoads() {
		AdminVO admin = null;
		try {
			admin = biz.getAdmin("admin");
			System.out.println(admin);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

}
```

