# Thymeleaf

## 텍스트 출력

- ```th:text``` : HTML tag 속성에 기능을 정의해 HTML contents 에 데이터를 출력할 때 사용
- ```[[...]]``` : HTML contents 영역안에 직접 데이터 출력하고 싶을 때 사용

```html
<li>데이터 출력 <span th:text="${data}"></span></li>
<li>데이터 출력 = [[${data}]]</li>
```

