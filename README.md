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
  - ${errors?.containsKey('quantity')} = errors가 null 이면 NullPointerException 대신 errors에 null을 반환
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

## 타임리프 - 입력폼 기능
- 입력 폼 처리
  - `<form action="item.html" th:action th:object="${item}" method="post">`
    - `<div>`
      - `<label for="id">상품 ID</label>`
      - `<input type="text" id="id" class="form-control" th:field="*{id}" readonly>`
    - `</div>`
- 체크박스
  - `<input type="checkbox" id="open" name="open" th:field="*{open}" class="form-check-input">`
  - `<!--<input type="hidden" name="_open" value="on">--> <!-- th:field가 자동으로 만들어줌 -->`
  - checked="checked" 도 해줌
- 멀티 체크박스
  - `<div th:each="region : ${regions}" class="form-check form-check-inline">`
    - `<input type="checkbox" th:field="*{regions}" th:value="${region.key}" class="form-check-input">`
    - `<label th:for="${#ids.prev('regions')}" th:text="${region.value}" class="form-check-label">서울</label>`
  - `</div>`

## 메시지 국제화
- 스프링
  - spring.messsages.basename=messages
  - resources -> messages.properties, messages_en.properties 로 만들면 기본적으로 실행됨
  - @Autowired
  - MessageSource messageSource;
  - String result = ms.getMessage("hello", null, null);
- 타임리프
  - `<th:text="#{label.item.itemName}">`

## BindingResult
- 입력 폼 유효성 검사에서 사용
  - public String addItemV2(@ModelAttribute Item item, BindingResult bindingResult, RedirectAttributes redirectAttributes, Model model) {
  - bindingResult.addError(new ObjectError("item", null, null, "가격 * 수량의 합은 10,000원 이상이어야 합니다. 현재 값 = " + resultPrice));
  - `<div th:if="${#fields.hasGlobalErrors()}">` `<p class="field-error" th:each="err : ${#fields.globalErrors()}" th:text="${err}">글로벌 오류 메세지</p>` `</div>`
  - bindingResult.addError(new FieldError("item","itemName", item.getItemName(), false, null, null, "상품 이름은 필수입니다."));
  - `<div class="field-error" th:errors="*{itemName}">`

## MessageCodesResolver
- bindingResult.addError(new FieldError("item","itemName", item.getItemName(), false, new String[] {"required.item.itemName"}, null, ""));
  - 위 코드에서 메세지는 new String[] {"required.item.itemName"} 이 부분인데
  - MessageCodesResolver 가 자동으로 에러코드, 객체명, 필드명 으로 메세지를 찾아서 넣어준다
  - 그러면 위의 코드에서 new String[] {"required.item.itemName"} 는 넣을 필요 없이
  - bindingResult.rejectValue("itemName", "required"); 이렇게 사용가능
  - 객체의 타입에러는 typeMismatch.java.lang.Integer 이런식으로 메세지를 찾는다

## Validator
- public class ItemValidator implements Validator {
  - public boolean supports(Class<?> aClass) { return Item.class.isAssignableFrom(aClass); }
  - public void validate(Object o, Errors errors) {
    - Item item = (Item) o;
    - // 검증 로직 if (!StringUtils.hasText(item.getItemName())) { errors.rejectValue("itemName", "required"); }

## Bean Validation
- Bean Validation을 구현한 기술중에 일반적으로 사용하는 구현체는 하이버네이트 Validator

## Login Session
- // 세션이 있으면 있는 세션 반환, 없으면 신규 세션을 생성
- HttpSession session = request.getSession();
- // 알아서 세션 UUID를 클라이언트에 쿠키로 저장하고
- // 세션에 로그인 정보 저장
- session.setAttribute(SessionConst.LOGIN_MEMBER, loginMember);
- // 다음번 접속 때 세션에 저장되어 있는 로그인 값을 조회해서 로그인 되어 있는지 확인
- Member loginMember = (Member)session.getAttribute(SessionConst.LOGIN_MEMBER);

## Filter, Interceptor
- 서블릿 필터와 스프링 인터셉터는 웹과 관련된 공통 관심사를 해결하기 위한 기술
- HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 인터셉터 -> 컨트롤러
- Filter = doFilter 로 핸들링 request, response는 Servlet을 쓰지만 HttpServletRequest로 다운 캐스팅해도 됨
- HandlerInterceptor
  - preHandle - 컨트롤러 호출 전에 호출
  - postHandle - 컨트롤러 호출 후에 호출
  - afterCompletion - 뷰가 렌더링 된 이후 호출
- 특별히 필터를 사용해야 하는 상황이 아니면 인터셉터 사용하는 것이 더 편리

## Exception
- 흐름
  - 컨트롤러(예외 발생) -> 인터셉터 -> 서블릿 -> 필터 -> WAS (에러찾음)
  - WAS '/error-page/500' 다시 요청 -> 필터 -> 서블릿 -> 인터셉터 -> 컨트롤러 -> view
- 오류화면
  - public class WebServerCustomizer implements WebServerFactoryCustomizer<ConfigurableWebServerFactory> {
    - @Override
    - public void customize(ConfigurableWebServerFactory factory) {
      - ErrorPage errorPage404 = new ErrorPage(HttpStatus.NOT_FOUND, "/error-page/404");
      - factory.addErrorPages(errorPage404, errorPage500, errorPageEx);
      - }
    - }
  - }
- 그러나 위 흐름대로라면 필터를 두번 부르게 되는데 그건 비효율적, 그래서 DispatcherType이 제공됨
- 필터 Bean 등록시에 타입을 넣어야 활성화됨 저걸 넣지 않으면 기본은 REQUEST만 되어있음
- filterRegistrationBean.setDispatcherTypes(DispatcherType.REQUEST,DispatcherType.ERROR);
- 인터셉터는 /error-page/** 경로 설정으로 제외할 수 있음
- excludePathPatterns("/css/**","*.ico","/error", "/error-page/**");
- 스프링부트의 경우 내장되어 있는 BasicErrorController 덕분에 resources/templates/error/ 경로에 html파일만 만들어 두면 됨
- 500, 404, 4xx 같은 식으로










