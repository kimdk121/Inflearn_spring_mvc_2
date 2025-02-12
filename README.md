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
- 조건부평가
  - `<span th:text="'미성년자'" th:if="${user.age lt 20}"></span>`
  - `<span th:text="'미성년자'" th:unless="${user.age ge 20}"></span>`
  - `<td th:switch="${user.age}">`
    - `<span th:case="10">10살</span>`
    - `<span th:case="20">20살</span>`
    - `<span th:case="*">기타</span>`
- 주석
  - `<!--/* [[${data}]] */-->`
- 블록
  - `<th:block th:each="user : ${users}">`
     - `<div>`
     -   사용자 이름1 `<span th:text="${user.username}"/>`
     -   사용자 나이1 `<span th:text="${user.age}"/>`
     - `</div>`
     - `<div>`
     -   요약 `<span th:text="${user.username} + ' / ' + ${user.age}"/>`
     - `</div>`
     - `</th:block>`
- 자바스크립트 인라인
  - `<script th:inline="javascript">`
  - 자바스크립트에서 타임리프 문법 편하게 도와줌
- 템플릿 조각
  - `<div th:insert="~{template/fragment/footer :: copy}"/>`
  - `<div th:replace="~{template/fragment/footer :: copyParam ('데이터1', '데이터2')}"/>`

## 타임리프 - 스프링 기능
- 입력 폼 처리
  - `<form action="item.html" th:action th:object="${item}" method="post">`
        `<div>`
            `<label for="id">상품 ID</label>`
            `<input type="text" id="id" class="form-control" th:field="*{id}" readonly>`
        `</div>`
- 체크박스
  - `<input type="checkbox" id="open" name="open" th:field="*{open}" class="form-check-input">`
  - `<!--<input type="hidden" name="_open" value="on">--> <!-- th:field가 자동으로 만들어줌 -->`
  - checked="checked" 도 해줌
- 멀티 체크박스
  - `<div th:each="region : ${regions}" class="form-check form-check-inline">`
  -     `<input type="checkbox" th:field="*{regions}" th:value="${region.key}" class="form-check-input">`
  -     `<label th:for="${#ids.prev('regions')}" th:text="${region.value}" class="form-check-label">서울</label>`
  - `</div>`











     
