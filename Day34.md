# Day34 2022-06-09

## AI 플랫폼을 활용한 웹서비스 개발

### 웹 - 데이터베이스 연동

MySQL의 shoppingdb와 웹 페이지를 연동한다.

페이지 테마는 부트스트랩을 이용했다.

![image-20220609225202847](C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220609225202847.png)

#### Cust

- `Add` 페이지에서는 cust 정보를 등록할 수 있다.
- ajax를 이용해 사용 가능한 아이디인지 바로 확인해 정보를 제공한다.
- input된 값을 전송해 데이터베이스에 보낸다.

```html
<meta charset="UTF-8">

<script>
function sendId(id){
	$.ajax({
		url:'/checkid',
		data:{'id':id},
		success:function(data){
			if(data == '1'){
				$('#ispan').text('사용 불가능한 ID');
			}else{
				$('#ispan').text('사용 가능한 ID');
			}
		}
	});
};

$(document).ready(function(){
	$('input[name="id"]').keyup(function(){
		var id = $(this).val(); 
		sendId(id);
	});
	
	$('#registerbtn').click(function(){
		$('.user').attr({
			'method':'post',
			'action':'addimpl' 
		});
		$('.user').submit();
	});
});
</script>
  <div class="col-lg-6">
      <div class="p-5">
          <div class="text-center">
              <h1 class="h4 text-gray-900 mb-4">Customer Register</h1>
          </div>
          <form class="user">
              <div class="form-group">
                  ID: <span id="ispan"></span><input type="text" class="form-control form-control-item" name="id">
              </div>
              <div class="form-group">
                 PWD: <input type="text" class="form-control form-control-item" name="pwd">
              </div>
              <div class="form-group">
                  NAME: <input type="text" class="form-control form-control-item" name="name">
              </div>
              <div class="form-group">
                 ADDR: <input type="text" class="form-control form-control-item" name="addr">
              </div>
              
              <a id="registerbtn" href="#" class="btn btn-primary btn-user btn-block">
                  REGISTER
              </a>
             
          </form>
         
      </div>
  </div>
```

- `Select` 페이지에서는 등록된 cust정보를 확인할 수 있다.
- cust id에 a태그를 걸어 cust id를 클릭하면 해당 아이디의 상제정보(detail) 페이지로 이동한다.

```html
<!-- Page Heading -->
<h1 class="h3 mb-2 text-gray-800"> Cust Tables</h1>

<!-- DataTales Example -->
<div class="card shadow mb-4">

    <div class="card-body">
        <div class="table-responsive">
            <table class="table table-bordered" id="dataTable" width="100%" cellspacing="0">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>PWD</th>
                        <th>NAME</th>
                        <th>ADDRESS</th>
                        <th>REGDATE</th>
                    </tr>
                </thead>
                <tfoot>
                    <tr>
                        <th>ID</th>
                        <th>PWD</th>
                        <th>NAME</th>
                        <th>ADDRESS</th>
                        <th>REGDATE</th>
                    </tr>
                </tfoot>
                <tbody>
                    <tr th:each="c : ${clist}">
                    	<!-- /cust/detail?id=id01 -->
                        <td><a href="" th:text="${c.id}" th:href="@{detail(id=${c.id})}">id01</a></td>
                        <td th:text="${c.pwd}">pwd01</td>
                        <td th:text="${c.name}">Edinburgh</td>
                        <td th:text="${c.addr}">Seoul</td>
                        <td th:text="${#dates.format(c.regdate,'yyyy/MM/dd')}">2011/04/25</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</div>
```

- `Detail` 페이지에서는 각 아이디별 상세 정보를 확인할 수 있다.
- update 버튼을 클릭하면 수정이 가능하다.

```html
<script>
$(document).ready(function(){
	$('#updatebtn').click(function(){
		$('.user').attr({
			'method':'post',
			'action':'update' 
		});
		$('.user').submit();
	});
});

</script>


  <div class="col-lg-6">
      <div class="p-5">
          <div class="text-center">
              <h1 class="h4 text-gray-900 mb-4">Customer Information</h1>
          </div>
          <form class="user">
              <div class="form-group">
                  <input type="text" class="form-control form-control-item"
                      name="id" value="id01" th:value="${c.id}" readonly="readonly">
              </div>
              <div class="form-group">
                  <input type="text" class="form-control form-control-item"
                      name="pwd" value="pwd01" th:value="${c.pwd}">
              </div>
              <div class="form-group">
                  <input type="text" class="form-control form-control-item"
                      name="name" value="james" th:value="${c.name}">
              </div>
              <div class="form-group">
                  <input type="text" class="form-control form-control-item"
                      name="addr" value="seoul" th:value="${c.addr}">
              </div>
              <div class="form-group">
                  <input type="text" class="form-control form-control-item"
                      name="regdate"  value="22/06/09" th:value="${#dates.format(c.regdate,'yyyy/MM/dd')}">
              </div>
              <a id="updatebtn" href="#" class="btn btn-primary btn-user btn-block">
                  UPDATE
              </a>
             
          </form>
         
      </div>
  </div>

```

- `CustController` 에서 함수를 정의한다.

```java
package com.multi.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import com.multi.biz.CustBiz;
import com.multi.vo.CustVO;

@Controller
@RequestMapping("/cust")
public class CustController {

	@Autowired
	CustBiz biz;
	
	@RequestMapping("add")
	public String add(Model m) {
		m.addAttribute("center", "cust/add");
		return "/index";
	}
	
	@RequestMapping("addimpl")
	public String addimpl(Model m, CustVO obj) {
		
		try {
			biz.register(obj);
		} catch (Exception e) {
			e.printStackTrace();
		}
		
		return "redirect:detail?id="+obj.getId();
	}
	
	@RequestMapping("/select")
	public String select(Model m) {
		List<CustVO> list = null;
		try {
			list = biz.get();
			m.addAttribute("clist", list);
		} catch (Exception e) {
			e.printStackTrace();
		}
		m.addAttribute("center", "cust/select");
		return "/index";
	}
	
	@RequestMapping("/detail")
	public String detail(Model m, String id) {
		CustVO obj = null;
		try {
			obj = biz.get(id);
			m.addAttribute("c", obj);
		} catch (Exception e) {
			e.printStackTrace();
		}
		m.addAttribute("center", "cust/detail");
		return "/index";
	}
	
	@RequestMapping("/update")
	public String update(Model m, CustVO obj) {
		try {
			biz.modify(obj);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return "redirect:detail?id="+obj.getId();
	}	
}
```

- `AJAXController`에서는 ajax관련 함수를 정의한다.

```java
package com.multi.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.multi.biz.CustBiz;
import com.multi.vo.CustVO;

@RestController
public class AJAXController {

	@Autowired
	CustBiz biz;

    @RequestMapping("checkid")
	public String checkid(String id) {
		String result = "";
		CustVO c = null;
		
		try {
			c = biz.get(id);
			if(c == null) {
				result = "0";
			}else {
				result = "1";
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
		
		return result;
	}

}
```

> Controller를 따로 관리하는 이유는 AJAX는 `@RestController`을 이용하기 때문이다.