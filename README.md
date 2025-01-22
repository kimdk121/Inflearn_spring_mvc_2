# Inflearn_spring_mvc_2
스프링 MVC 2편 - 백엔드 웹 개발 활용 기술

## 타임리프 - 기본 기능
- HTML 엔티티
  - th:text, [[...]] 기본적으로 escape를 제공함
  - th:utext, [(...)] 를 사용하면 unescape 가능
- SpringEL 표현식
  - Object = user.username
  - List = users[0].username
  - Map = userMap['userA'].username
- 지역변수 선언
  - th:with = `<div th:with="first=${users[0]}">`
  - `<p>처음 사람의 이름은 <span th:text="${first.username}"></span></p>`
- 객체 사용
  - `<span th:text="${param.paramData}"></span>`
  - `<span th:text="${session.sessionData}"></span>`
  - `<span th:text="${@helloBean.hello('Spring!')}"></span>`
- URL 링크
  - `@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}` -> `/hello/data1/data2`
  - `@{/hello/{param1}(param1=${param1}, param2=${param2})}` -> `/hello/data1?param2=data2`
- Literal
  - `<span th:text="|hello ${data}|"></span>`
- 연산
  - Elvis 연산자 -> `<li>${data}?: '데이터가 없습니다.' = <span th:text="${data}?: '데이터가 없습니다.'"></span></li>`
  - No Operation -> `<li>${nullData}?: _ = <span th:text="${nullData}?: _">데이터가 없습니다.</span></li>`
- 속성
  - `<input type="text" name="mock" th:name="userA"/>`
  - `<input type="text" class="text" th:classappend="large"/>`
  - `checked x <input type="checkbox" name="active" th:checked="false"/>`
- 반복
  - `<tr th:each="user, userStat : ${users}">`
  - `<td th:text="${userStat.count}">count</td>`
  - `<td th:text="${user.username}">username</td>`
