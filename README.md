# Thymeleaf

## 텍스트 출력

> commit: https://github.com/evelyn82ny/thymeleaf/commit/1807f6a35e593dec0b9afc218487d1bc5804b110

- ```th:text``` : HTML tag 속성에 기능을 정의해 HTML contents 에 데이터를 출력할 때 사용
- ```[[...]]``` : HTML contents 영역안에 직접 데이터 출력하고 싶을 때 사용

```html
<li>데이터 출력 <span th:text="${data}"></span></li>
<li>데이터 출력 = [[${data}]]</li>
```

### escape

HTML 문서는 ```<```, ```>``` 같은 특수문자를 기반으로 정의된다. 만약 Hello **Spring!** 문자에서 ```Spring!``` 을 강조하고 싶어 ```Hello <b>Spring!</b>``` 데이터를 작성하면 제대로 강조될까?

```html
<li>th:text = <span th:text="${data}"></span></li>
<li><span th:inline="none">[[...]] = </span>[[${data}]]</li>
```

- 웹 브라우저에 출력된 결과: th:text = Hello <b>Spring!</b>
- page source: <li>th:text = <span>Hello &lt;b&gt;Spring!&lt;/b&gt;</span></li>

```Spring!``` 문구가 강조되지 않았고 강조하기 위해 사용했던 ```<b>, </b>``` 태그가 화면에 그대로 출력되었다. 태그가 적용되지 않고 웹 브라우저에 문자로 출력된 이유는 thymeleaf 가 제공하는 **escape** 때문이다.<br>

웹 브라우저는 ```<``` 특수 문자를 HTML 태그의 시작으로 인식한다. 그렇기 때문에 ```<``` 같은 특수 문자가 포함된 데이터가 전달되면 ```<``` 를 테그의 시작이 아닌 **문자로 표현**하는데 이 방법을 **HTML 엔티티**라고 한다. 그리고 HTML 에서 사용하는 특수 문자를 HTML 엔티티로 변경하는 것을 escape 라고 한다.<br>

즉, thymeleaf 가 제공하는 Escape 로 인해 ```<``` 가 ```&lt;``` 로, ```>``` 가 ```&gt;``` 로 변경되었다. 이로 인해 ```<b>, </b>``` 태그를 추가해도 원하는 의도로 출력할 수 없지만 생각했던 것과 반대로 escape 는 중요한 기능이다.<br>

사용자들이 HTML 에서 사용하는 특수 문자를 그냥 작성한다면 HTML 이 정상 렌더링 되지 않아 화면이 깨지는 등 수 많은 문제가 발생하기 떄문에 **escape 가 기본으로 설정**되어있다.

### unescape 

> commit: https://github.com/evelyn82ny/thymeleaf/commit/bdb9ee471e7da148fbdad6d9e3dd9c36cba6447e

escape 기능을 사용하지 않으려면 ```th:utext``` 또는 ```[(...)]``` 을 사용하면 된다. ```Hello <b>Spring!</b>``` 데이터를 넘겨주어도 원하는대로 결과가 출력된다.

```html
<li>th:utext = <span th:utext="${data}"></span></li>
<li><span th:inline="none">[(...)] = </span>[(${data})]</li>
```

- 웹 브라우저에 출력된 결과: th:utext = Hello **Spring!**
- page source: <li>th:utext = <span>Hello <b>Spring!</b></span></li>

<br>

## SpringEL

thymeleaf 에서 변수를 사용할 때 ```${...}``` 변수 표현식을 사용한다.

```${...}``` 변수 표현식에 Spring 이 제공하는 SpringEL 을 사용할 수 있다.

> commit: https://github.com/evelyn82ny/thymeleaf/commit/95a04641f2124ff6d533893429a33ea212a9df0b

```java
@Data
static class User {
    private String name;
    private int age;
}

User userA = new User();
List<User> list = new ArrayList<>();
Map<String, User> map = new HashMap<>();

model.addAttribute("user", userA);
model.addAttribute("users", list);
model.addAttribute("userMap", map);
```
User 객체와 User 객체를 담고 있는 list, map 을 model 에 담아 넘겨준다.

```html
<li><span th:text="${user.username}"></span></li>
<li><span th:text="${user['username']}"></span></li>
<li><span th:text="${user.getUsername()}"></span></li>
```

- ```user.username``` : user 의 username property 에 접근 -> ```user.getUsername()```
- ```user['username']``` : 위와 방식과 동일
- ```user.getUsername()``` : ```getUsername()``` 직접 호출

```html
<li><span th:text="${users[0].username}"></span></li>
<li><span th:text="${userMap['userA'].username}"></span></li>
```
List, Map 도 같은 방식이다.

### 지역 변수 선언

> commit: https://github.com/evelyn82ny/thymeleaf/commit/5c2be6bc8cb305bc2c3dc9e0851f01a06c87d228

```th:with``` 로 지역 변수를 선언할 수 있다.

```html
<h1>지역 변수 - th:with</h1>
<div th:with="user=${users[0]}">
    <p>첫번째 사용자 이름은 <span th:text="${user.username}"></span></p>
</div>>
```