# Day33 2022-05-31

## AI 플랫폼을 활용한 웹서비스 개발

### Spring Boot

 #### 1. 프로젝트 생성하기

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220602204654892.png" alt="image-20220602204654892" style="zoom:50%;" />

- Spring Starter Project  클릭 => next

  <img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220602204940837.png" alt="image-20220602204940837" style="zoom:50%;" />

- name 입력하기
- Group 이름과 Package 이름 동일하게 설정하기
- Type: Maven / Packageing: War / Java Version: 8 클릭 => next

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220602205153270.png" alt="image-20220602205153270" style="zoom:50%;" />

- Spring Boot DevTools, Lombok, MyBatis Framework, MySQL Driver, Thmeleaf, Spring Wed 선택하기 => Finish

#### 2. 환경 구축하기

- pom.xml 열고 `dependencies` 안에 아래의 코드 추가하기

```xml
<!-- @Inject -->
<dependency>
    <groupId>javax.inject</groupId>
    <artifactId>javax.inject</artifactId>
    <version>1</version>
</dependency>
<!-- Servlet -->

<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
    <scope>provided</scope>
</dependency>


<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.0.1</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>

<!-- json request -->   

<dependency>
    <groupId>com.googlecode.json-simple</groupId>
    <artifactId>json-simple</artifactId>
    <version>1.1</version>
</dependency>
```

- 탐색기에서 **C:\spring\day041\src\main\resources**로 이동한 후 application 파일을 메모장으로 열고 아래의 내용 추가하기

```
server.port=80

spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/shopdb?serverTimezone=Asia/Seoul

spring.datasource.username=admin1
spring.datasource.password=111111

mybatis.type-aliases-package=com.multi.vo
mybatis.mapper-locations=com/multi/mybatis/*.xml
```

- Lombok 설치하기
  -  [lombok.jar](..\Downloads\lombok.jar) 

#### 3. Package, Class 생성하기

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220602210259015.png" alt="image-20220602210259015" style="zoom:50%;" />

- 개발에 필요한 package와 class 등을 생성한다.

> 기존의 Spring과 다른 점은 Spring Boot에는 `src/test/java` 안에 test 파일이 존재한다. 
>
> 더 간편하게 테스트를 해볼 수 있다.

- com.multi.frame > Biz.java (Interface)

```java
package com.multi.frame;

import java.util.List;

public interface Biz<K, V> {
	public void register(V v) throws Exception;
	public void modify(V v) throws Exception;
	public void remove(K k) throws Exception;
	public V get(K k) throws Exception;
	public List<V> get() throws Exception;
}
```

- com.multi.vo > CustVO.java

```java
package com.multi.vo;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@ToString
public class CustVO {
	private String id;
	private String pwd;
	private String name;
}
```

> 기존에는 Getter, Setter, Constructor, ToString을 생성해야 했지만 Boot에서는 import를 이용해 쉽게 사용할 수 있다.

- com.multi.biz > CustBiz.java

```java
package com.multi.biz;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.multi.frame.Biz;
import com.multi.mapper.CustMapper;
import com.multi.vo.CustVO;

@Service("custbiz")
public class CustBiz implements Biz<String, CustVO> {
	
	@Autowired
	CustMapper dao;
	
	@Override
	public void register(CustVO v) throws Exception {
		dao.insert(v);
	}

	@Override
	public void modify(CustVO v) throws Exception {
		dao.update(v);
	}

	@Override
	public void remove(String k) throws Exception {
		dao.delete(k);
	}

	@Override
	public CustVO get(String k) throws Exception {
		return dao.select(k);
	}

	@Override
	public List<CustVO> get() throws Exception {
		return dao.selectall();
	}
	
}
```

- com.multi.mapper > CustMapper.java (interface)

```java
package com.multi.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

import com.multi.vo.CustVO;

@Repository
@Mapper
public interface CustMapper {
	public void insert(CustVO cust) throws Exception;
	public void delete(String id) throws Exception;
	public void update(CustVO cust) throws Exception;
	public CustVO select(String id) throws Exception;
	public List<CustVO> selectall() throws Exception;
}
```

- src/test/java > com.multi.cust > CustInsertTests.java

```java
package com.multi.cust;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import com.multi.biz.CustBiz;
import com.multi.vo.CustVO;

@SpringBootTest
class CustInsertTests {
	
	@Autowired
	CustBiz biz;

	@Test
	void contextLoads() {
		CustVO c = new CustVO("id22", "pwd22", "kee");
		try {
			biz.register(c);
			System.out.println("Registered OK");
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

}
```

- 위의 insert와 동일한 방법으로 CRUD 프로그램 작성하기

#### 4. Web과 연동하기

- Main page를 만든다.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<h1>Main Page</h1>
	<h3><a href="custadd">Cust ADD</a></h3>
	<h3><a href="custselect">Cust SELECT</a></h3>
	<h3><a href="productadd">Product ADD</a></h3>
	<h3><a href="productselect">Product SELECT</a></h3>
</body>
</html>
```

- MainController에 웹과 연동하는 코드를 작성한다.

```java
package com.multi.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import com.multi.biz.CustBiz;
import com.multi.vo.CustVO;

@Controller
public class MainController {
	
	@Autowired
	CustBiz biz;
	
	@RequestMapping("/")
	public String main() {
		return "main";
	}
    
	@RequestMapping("/custadd")
	public String custadd() {
		return "custadd";
	}
	
    @RequestMapping("/custselect")
	public ModelAndView custselect(ModelAndView mv) {
		List<CustVO> list = null;
		try {
			list = biz.get();
			mv.addObject("allcust", list);
		} catch (Exception e) {
			
		}
		mv.setViewName("custselect");
		return mv;
	}
}
```

> ModelAndView를 이용해 쉽게 접근하고 결과를 리턴 받아 올 수 있다.

- each로 리스트 안에 내용을 테이블에 출력한다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>Cust Select Page</h1>
	<table>
		<thead>
			<tr><th>ID</th><th>NAME</th></tr>
		</thead>
		<tbody>
			<tr th:each=c:${allcust}>
				<td><a href="custdetail" th:href="@{/custdetail(id=${c.id})}" th:text=${c.id}>ID</a></td>
				<td th:text="${c.name}">NAME</td>
			</tr>
		</tbody>
	</table>
</body>
</html>
```

> ID에 a 태그를 걸어 상세정보로 이동할 수 있도록 연결한다.

- a 태그에 delete와 update를 연결한다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>Cust Detail Page</h1>
	<h3 th:text="${dcust.id}"></h3>
	<h3 th:text="${dcust.pwd}"></h3>
	<h3 th:text="${dcust.name}"></h3>
	<h3>님의 회원정보 입니다.</h3>
	<a href="" th:href="@{/custdelete(id=${dcust.id})}">DELETE</a>
	<a href="" th:href="@{/custupdate(id=${dcust.id})}">UPDATE</a>
</body>
</html>
```

> `DELETE`를 클릭하면 해당 정보가 삭제되고, `UPDATE`를 클릭하면 수정 페이지로 이동한다.

