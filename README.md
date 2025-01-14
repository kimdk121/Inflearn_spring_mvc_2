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
