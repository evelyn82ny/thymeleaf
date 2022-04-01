# Thymeleaf

## 텍스트 출력

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

escape 기능을 사용하지 않으려면 ```th:utext``` 또는 ```[(...)]``` 을 사용하면 된다. ```Hello <b>Spring!</b>``` 데이터를 넘겨주어도 원하는대로 결과가 출력된다.

```html
<li>th:utext = <span th:utext="${data}"></span></li>
<li><span th:inline="none">[(...)] = </span>[(${data})]</li>
```

- 웹 브라우저에 출력된 결과: th:utext = Hello **Spring!**
- page source: <li>th:utext = <span>Hello <b>Spring!</b></span></li>
