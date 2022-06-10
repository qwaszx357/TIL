# Day35 2022-06-10

### 웹 - 데이터베이스 연동

#### 1. 테이블 안에서 if문 사용

- 상위 카테고리 (pid == 0)일 경우 "TOP"가 출력되고, 하위 카테고리는 pid를 출력한다.

``` html
<table class="table table-bordered" id="dataTable" width="100%" cellspacing="0">
    <thead>
        <tr>
            <th>ID</th>
            <th>NAME</th>
            <th>PID</th>
        </tr>
    </thead>
    <tbody>
        <tr th:each="c : ${clist}">
            <td><a href="" th:text="${c.id}" th:href="@{detail(id=${c.id})}">id</a></td>
            <td th:text="${c.name}">name</td>
            <td th:if="${c.pid == 0}" th:text="TOP"></td>
            <td th:unless="${c.pid == 0}" th:text="${c.pid}"></td>
        </tr>
    </tbody>
</table>
```

> 값이 true, false일 때 모두 조건을 같게 쓰고, if와 unless로 작성한다.

#### 2. Delete 확인 창 띄우기

- sctipt영역에 아래와 같이 코드를 작성한다.
- location.href를 이용한다.

```html
<script>
$(document).ready(function(){
	$('#deletebtn').click(function(){
		var id = $('input[name="id"]').val();
		var c = confirm('삭제 하시겠습니까?');
		if (c == true){
			location.href='delete?id='+id;
		}
	});
}
</script>
```

#### 3. Select로 존재하는 값만 보여주기

- Category를 add할 때 존재하는 상위카테고리만 참조하기 위해 select를 이용한다.
- div 영역에 select를 입력한다.
- 존재하는 값을 slist에 넣어 each로 반복한다. 

```html
<div class="form-group">
    PID: 
    <select name="pid" class="form-control">
        <option value="0">TOP</option>
        <option th:each="c:${slist}" 
                th:value="${c.id}" 
                th:text="${c.name}"></option>
    </select>
</div>
```

- Controller에서는 list의 내용을 slist에 담는다.

```java
@RequestMapping("/add")
public String add(Model m) {
    List<CateVO> list = null;
    try {
        list = biz.getmain();
        m.addAttribute("slist", list);
    } catch (Exception e) {
        e.printStackTrace();
    }
    m.addAttribute("center", "cate/add");
    return "/index";
}
```

<img src="C:\Users\hasun\Desktop\image-20220610112734044.png" style="zoom:80%;" />

> 위의 each를 활용할 때, th:text를 이용해 상위카테고리의 이름으로 보여지게 설정할 수 있다.

#### 4. image 업로드하기

- image를 입력할 곳의 input type는 **file** 로 설정한다.

```html
<form>
    <div class="form-group">
        IMG: <input type="file" class="form-control form-control-item" name="mf">
    </div>
</form>
```

- script 영역에는 아래와 같은 함수를 정의한다.
- img 파일을 전송하기 위해 `'enctype':'multipart/form-data'`를 추가한다.

```html
<script>
$(document).ready(function(){
	$('#registerbtn').click(function(){
		$('.product').attr({
			'enctype':'multipart/form-data',
			'method':'post',
			'action':'addimpl' 
		});
		$('.product').submit();
	});
});
</script>
```

> 파일을 서버로 전송하기 위해서는 method를 반드시 post로 설정한다.

- ProductVO파일에서는 아래의 코드를 추가한다.

```java
import org.springframework.web.multipart.MultipartFile;

public class ProductVO {
	private MultipartFile mf;
}
```

- Controller에 아래 함수 추가한다.

```java
@RequestMapping("/addimpl")
public String addimpl(Model m, ProductVO p) {
    String imgname = p.getMf().getOriginalFilename();
    p.setImgname(imgname);

    try {
        biz.register(p);
        Util.saveFile(p.getMf());
    } catch (Exception e) {
        e.printStackTrace();
    }

    return "redirect:select";
}
```

> mf에서 imgname을 끄집어낸다.

- com.multi.frame 폴더 안에 Util.java 파일을 생성한다.
- 아래의 코드를 추가한다.

```java
package com.multi.frame;

import java.io.FileOutputStream;
import org.springframework.web.multipart.MultipartFile;

public class Util {
	public static void saveFile(MultipartFile mf) {
		String dir = "C:\\spring\\shopadmin\\src\\main\\resources\\static\\img\\";
		byte [] data;
		String imgname = mf.getOriginalFilename();
		try {
			data = mf.getBytes();
			FileOutputStream fo = 
					new FileOutputStream(dir+imgname);
			fo.write(data);
			fo.close();
		}catch(Exception e) {
			
		}
    }
}
```

> dir에는 입력받은 img 파일을 추가할 경로를 입력한다.

- window > Preferences > Genaral > Workspace 로 이동한다.
- `Refresh using native hooks or polling`을 선택 후 Apply한다.

<img src="C:\Users\hasun\Desktop\image-20220610142417526.png" style="zoom:80%;" />

#### 5. Image 출력하기

- 입력받은 img를 다시 화면에 출력하기 위해 아래의 코드를 작성한다.

```html
<img th:src="@{'/img/'+${p.imgname}}">
```

