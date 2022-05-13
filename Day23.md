# Day23 2022-05-13

## AI 플랫폼을 활용한 웹서비스 개발

### 1. HTML

1. jsp

   - src > main > webapp > WEB-INF > views 파일에 각 페이지의 jsp를 만든다.
   - main.jsp

   ```html
   <%@ page language="java" contentType="text/html; charset=EUC-KR"
       pageEncoding="EUC-KR"%>
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   
   <!DOCTYPE html>
   <html>
   <head>
   <meta charset="EUC-KR">
   <title>Insert title here</title>
   <link rel="stylesheet" href="css/main.css">
   <style>
   
   </style>
   </head>
   <body>
   <header>
   	<h1><a href="/">HTML5 & CSS3.0</a></h1>
   </header>
   <nav>
   	<ul>
   		<li><a href="html5">HTML5</a></li>
   		<li><a href="css3">CSS3</a></li>
   		<li><a href="js">JavaScript</a></li>
   		<li><a href="media">Media</a></li>
   	</ul>
   </nav>
   <section>
   	<jsp:include page="${center}.jsp"></jsp:include>
   </section>
   <footer></footer>
   </body>
   </html>
   ```

   - center.jsp

   ```html
   <%@ page language="java" contentType="text/html; charset=EUC-KR"
       pageEncoding="EUC-KR"%>
   <div class="center">
   	<h1>Main Center</h1>
   </div>
   ```

   - MainController.java
     - ModelAndView로 html5를 요청하면 센터에  html5를 불러오고, main 페이지를 보여준다.

   ```java
   package com.shop.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.servlet.ModelAndView;
   
   @Controller
   public class MainController {
   	
   	@RequestMapping("/")
   	public ModelAndView main(ModelAndView mv) {
   		mv.addObject("center", "center");
   		mv.setViewName("main");
   		return mv;
   	}
   	
   	@RequestMapping("/media")
   	public ModelAndView media(ModelAndView mv) {
   		mv.setViewName("media");
   		return mv;
   	}
   	
   	@RequestMapping("/html5")
   	public ModelAndView html5(ModelAndView mv) {
   		mv.addObject("center", "html5");
   		mv.setViewName("main");
   		return mv;
   	}
   	
   	@RequestMapping("/js")
   	public ModelAndView js(ModelAndView mv) {
   		mv.addObject("center", "js");
   		mv.setViewName("main");
   		return mv;
   	}
   
   	@RequestMapping("/css3")
   	public ModelAndView css3(ModelAndView mv) {
   		mv.addObject("center", "css3");
   		mv.setViewName("main");
   		return mv;
   	}
   }
   ```

   - main.css
     - style영역을 따로 빼서 작성한다.

   ```css
   @charset "EUC-KR";
   /* Global CSS start ------------------- */
   *{
   	margin: 0;
   	padding: 0;
   }
   a{
   	text-decoration: none;
   	color: black;
   }
   ul,ol{
   	list-style: none;
   }
   /* Global CSS end ------------------- */
   
   /* Header CSS start ------------------- */
   header{
   	width: 600px;
   	height: 100px;
   	background-color: red;
   	margin: 0 auto;
   }
   header > h1{
   	text-align: center;
   	line-height: 100px;
   }
   /* Header CSS end ------------------- */
   
   /* Nav CSS start ------------------- */
   nav{
   	width: 600px;
   	height: 30px;
   	background-color: pink;
   	margin: 0 auto;
   }
   nav > ul{
   	width: 500px;
   	margin: 0 auto;
   }
   nav > ul > li{
   	float: left;
   	width: 125px;
   }
   nav > ul > li > a{
   	display: block;
   	text-align: center;
   	line-height: 30px;
   }
   /* Nav CSS end ------------------- */
   
   /* Section CSS start ------------------- */
   section{
   	width: 600px;
   	height: 500px;
   	background-color: gray;
   	margin: 0 auto;
   }
   section > .center{
   	width: 550px;
   	height: 450px;
   	background-color: white;
   	display: inline-block;
   	margin: 25px;	
   }
   /* Section CSS end ------------------- */
   
   /* Footer CSS start ------------------- */
   footer{
   	width: 600px;
   	height: 30px;
   	background-color: black;
   	margin: 0 auto;
   }
   /* Footer CSS end ------------------- */
   ```

2. thymeleaf

   - src > main > resource > templates에 각 페이지의 html을 만든다.
   - main.html에 <html xmlns:th="http://www.thymeleaf.org">를 입력한다.
   - main.html

   ```html
   <!DOCTYPE html>
   <html xmlns:th="http://www.thymeleaf.org">
   <head>
   <meta charset="UTF-8">
   <title>Insert title here</title>
   <link rel="stylesheet" href="css/main.css">
   </head>
   <body>
   	<header>
   		<h1><a href="/">HTML5 & CSS3.0</a></h1>
   	</header>
   	<nav>
   		<ul>
   			<li><a href="html5">HTML5</a></li>
   			<li><a href="css3">CSS3</a></li>
   			<li><a href="js">JavaScript</a></li>
   			<li><a href="media">Media</a></li>
   		</ul>
   	</nav>
   	<section th:include="${center}"></section>
   	<footer></footer>
   </body>
   </html>
   ```

   - html5.html
     - class 명을 지정한다.

   ```html
   <div class="center">
   	<h1>HTML5 Center</h1>
   </div>
   ```

   - MainController
     - String type으로 입력되며, Model m 객체를 만들어 html5를 요청했을 때 main 페이지가 나타나고 center에는 html5가 보여진다.

   ```java
   package com.shop.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.servlet.ModelAndView;
   
   @Controller
   public class MainController {
   	
   	@RequestMapping("/")
   	public String main(Model m) {
   		m.addAttribute("center", "center");
   		return "main";
   	}
   	
   	@RequestMapping("/media")
   	public ModelAndView media(ModelAndView mv) {
   		mv.setViewName("media");
   		return mv;
   	}
   	
   	@RequestMapping("/html5")
   	public String html5(Model m) {
   		m.addAttribute("center", "html5");
   		return "main";
   	}
   	
   	@RequestMapping("/js")
   	public String js(Model m) {
   		m.addAttribute("center", "js");
   		return "main";
   	}
   
   	@RequestMapping("/css3")
   	public String css3(Model m) {
   		m.addAttribute("center", "css3");
   		return "main";
   	}
   }
   ```

>`jsp`와 `thymeleaf`의 두 방법이 조금 차이가 있으니 각 차이점 알아두기!
>
>현업에서는 어떤 방법을 활용할지 모르니 두 방법 모두 숙지하기!

### 2. Bootdstrap theme

- 부트스트랩 자료가 나와있는 사이트

[**https://startbootstrap.com/**](https://startbootstrap.com/)

[**https://templated.co/**](https://templated.co/)

[**https://bootstrapmade.com**](https://bootstrapmade.com/)

[**https://uicookies.com/free-bootstrap-portfolio-templates/**](https://uicookies.com/free-bootstrap-portfolio-templates/)

[**https://themewagon.com/41-free-high-quality-responsive-personal-portfolio-website-html5-bootstrap-templates-2018/**](https://themewagon.com/41-free-high-quality-responsive-personal-portfolio-website-html5-bootstrap-templates-2018/)

### 3. Color Names

- 다양한 색상을 적용할 때 유용할 것 같다.

https://htmlcolorcodes.com/color-names/