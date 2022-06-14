# Day37 2022-06-14

## AI 플랫폼을 활용한 웹서비스 개발

### 웹 - 데이터베이스 연동

#### 1. 파일 업로드할 때 저장 위치 설정

- Util.java에 파일 경로를 입력한다.
  - 기존에는 아래처럼 입력하였으나 경로가 바뀌었을 때 수정에 어려움이 있어 2번째 코드로 변경한다.

```java
String dir = "C:\\spring\\shopadmin\\src\\main\\resources\\static\\img\\";
String dir2 = "C:\\spring\\shop\\src\\main\\resources\\static\\img\\";
```

````java
package com.multi.frame;

import java.io.FileOutputStream;
import org.springframework.web.multipart.MultipartFile;

public class Util {
	public static void saveFile(MultipartFile mf, String admindir, String userdir) {
		byte [] data;
		String imgname = mf.getOriginalFilename();
		try {
			data = mf.getBytes();
			FileOutputStream fo = 
					new FileOutputStream(admindir+imgname);
			fo.write(data);
			fo.close();
			FileOutputStream fo2 = 
					new FileOutputStream(userdir+imgname);
			fo2.write(data);
			fo2.close();
		}catch(Exception e) {
			
		}
		
	}
	
}
````

- application.properties에서 아래의 코드를 추가한다.

```properties
admindir = C:\\spring\\shopadmin\\src\\main\\resources\\static\\img\\
userdir = C:\\spring\\shop\\src\\main\\resources\\static\\img\\
```

> `""`, `;` 은 빼고 주소만 입력한다.

- controller.java에 아래의 코드를 추가한다.

```java
@Value("${admindir}")
String admindir;
	
@Value("${userdir}")
String userdir;
```

- controller에 함수를 정의할 때 Util.saveFile에 (admindir, userdir)을 추가한다.

```java
@RequestMapping("/addimpl")
public String addimpl(Model m, ProductVO p) {
    String imgname = p.getMf().getOriginalFilename();

    try {
        biz.register(p);
        p.setImgname(imgname);
        Util.saveFile(p.getMf(), admindir, userdir);
    } catch (Exception e) {
        e.printStackTrace();
    }

    return "redirect:select";
}
```

#### 2. 로그인

- 로그인 페이지를 만든다.

```html
<script>
$(document).ready(function(){
	$('#login_bt').click(function(){
		
		$('#login_form').attr({
			'method':'post',
			'action':'/loginimpl' 
		});
		$('#login_form').submit();
	});
});
</script>

<div class="container">
  <div class="col-sm-offset-2 col-sm-10">
  	<h2>Horizontal form</h2>
  </div>
  <form class="form-horizontal" id="login_form">
    <div class="form-group">
      <label class="control-label col-sm-2" for="id">ID:</label>
      <div class="col-sm-6">
        <input type="text" class="form-control" id="id" placeholder="Enter id" name="id">
      </div>
    </div>
    <div class="form-group">
      <label class="control-label col-sm-2" for="pwd">Password:</label>
      <div class="col-sm-6">          
        <input type="password" class="form-control" id="pwd" placeholder="Enter password" name="pwd">
      </div>
    </div>
  	<div class="form-group">        
      <div class="col-sm-offset-2 col-sm-10">
        <button id="login_bt" class="btn btn-default">LOGIN</button>
      </div>
  	</div>
  </form>
  
</div>
```

```java
@RequestMapping("/login")
public String login(Model m) {
    m.addAttribute("center", "/login");
    return "/index";
}

@RequestMapping("/loginimpl")
public String loginimpl(Model m, String id, String pwd, HttpSession session) {
    AdminVO admin = null;
    try {
        admin = biz.getAdmin(id);
        if(admin == null) {
            throw new Exception();
        }
        if(admin.getPwd().equals(pwd)) {
            session.setAttribute("loginadmin", admin);
        }else {
            throw new Exception();
        }

    } catch (Exception e) {
        return "redirect:/login";
    }
    return "index";
}
```

> throw new Exception();를 이용해서 코드를 간단하게 작성할 수 있다.

- 로그인 버튼을 눌렀을 때의 경로를 입력한다.

```html
<a th:href="@{/login}">Login</a>
```

#### 3. 로그아웃

```java
@RequestMapping("/logout")
public String logout(Model m, HttpSession session) {
    if(session != null) {
        session.invalidate();
    }
    return "/index";
}
```

- 로그아웃 버튼 눌렀을 때의 경로 입력

```html
<a class="btn btn-primary" th:href="@{/logout}">Logout</a>
```