# Day38 2022-06-15

## AI 플랫폼을 활용한 웹서비스 개발

### 웹 - 데이터베이스 연동

#### 1. 카테고리를 선택하면 해당 제품 출력하기

- 제품을 출력할 페이지에 div영역을 만든다.
- 제목로 th:text="${menu} + ' List'" 작성하여 각 카테고리의 이름이 출력된다.
- `ORDER` 버튼을 클릭하면 modal이 실행된다.

```html
<h1 th:text="${menu} + ' List'"></h1>
<hr>
<div th:each="p:${plist}">
	<div class="row row-no-gutters">
	  <div class="col-sm-3">
	  	<img id="pimg" th:src="@{'/img/'+${p.imgname}}">
	  </div>
	  <div class="col-sm-6">
		<h3 th:text="${p.catename}"></h3>
		<h3 th:text="${p.name}"></h3>
		<h3 th:text="${p.price}"></h3>
	  </div>
	  <div class="col-sm-3">
		 <button type="button" class="btn btn-info btn-lg" data-toggle="modal" 
		 th:data-target="'#'+${p.id}">ORDER</button>
	  </div>
	</div>
</div>
```

- Controller에는 아래의 코드를 추가한다.

```java
@Autowired
ProductBiz pbiz;

@RequestMapping("/getproduct")
public String getproduct(Model m, int id, String name) {
    List<ProductVO> plist = null;
    try {
        plist = pbiz.selectproduct(id);
        m.addAttribute("center", "product");
        m.addAttribute("menu", name);
        m.addAttribute("plist", plist);
    } catch (Exception e) {
        e.printStackTrace();
    }
    return "main";
}
```

#### 2. Modal 이용하기

- modal은 새로운 창의 띄워 그 창에 포커스를 맞춘다.
- modal창에 상품의 상세 정보와 구매 갯수를 select로 넣었다.
- `CART` 버튼을 클릭하면 해당 상품과 갯수가 cart에 담긴다.

```html
<!-- Modal -->
<div th:id="${p.id}" class="modal fade" role="dialog">
    <div class="modal-dialog">

        <!-- Modal content-->
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal">&times;</button>
                <h4 class="modal-title" th:text="${p.name}">Modal Header</h4>
                <img width="300px" th:src="@{'/img/'+${p.imgname}}">
            </div>
            <div class="modal-body">
                <p th:text="${p.catename}"></p>
                <p th:text="${p.price}"></p>
            </div>
            <div th:if="${session.logincust != null}" class="modal-body">
                <form action="cart" method="post">
                    <div class="form-group">
                        <input type="hidden" th:value="${p.id}" id="pid">
                        <input type="hidden" th:value="${session.logincust.id}" id="cid">
                        <label for="cnt">Count:</label>
                        <select class="form-control" id="cnt">
                            <option value="1">1</option>
                            <option value="2">2</option>
                            <option value="3">3</option>
                            <option value="4">4</option>
                        </select>
                    </div>
                    <button id="inputcart" type="button" class="btn btn-default">CART</button>
                </form>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
            </div>
        </div>
    </div>
</div>
<!-- End Modal -->
```

- AJAXController에 아래의 코드를 작성한다.

```java
@Autowired
CartBiz cartbiz;

@RequestMapping("addcart")
public void addcart(String uid, int pid, int cnt) {
    try {
        cartbiz.register(new CartVO(uid, pid, cnt));
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

