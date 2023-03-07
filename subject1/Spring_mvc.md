# Spring
## Spring
스프링은 모듈화된 구조로 필요한 모듈만을 선택하여 Java 기반의 애플리케이션을 쉽게 개발하는 것에 도움을 주는 프레임워크이다. 스프링은 코드를 변경하지 않고 확장할 수 있으며, 프레임워크지만 다양한 관점을 수용하고 신중한 진화를 통해 강력한 하위 호환성을 유지하여 유지보수가 용이하게 한다.
또한 원한다면 높음 품질의 코드를 유지할 수 있도록 깔끔한 코드 구조를 가지고 있다.

## Spring Boot
스프링부트는 스프링 프레임워크를 사용할 때 해야 하는 환경설정을 자동화하고 단순화함으로써 애플리케이션 개발과 운영을 더 쉽게 만들어주는 도구이다.
## Spring initializer
Spring Initializer은 스프링 프레임워크를 원하는 조건으로 빠르게 환경설정할 수 있게 하는 웹 기반 도구이다.
## Web Server와 Web Application Server(WAS)
WS와 WAS 모두 웹 애플리케이션을 실행시키기 위한 소프트웨어이다. WS는 클라이언트의 요청에 따른 HTML 등 정적인 데이터를 제공하고 
### Tomcat
## Model-View-Controller(MVC) 아키텍처 패턴
## 관심사의 분리(Seperation of Concern)
## Spring MVC
## Java Annotation
## Spring Annotation
스프링 어노테이션은 클래스, 변수 위에 붙여 사용함으로써 스프링에서 제공하는 기능을 적용하는 표기법을 의미한다. 
### @RestController
해당 클래스가 컨트롤러 역할을 하면서 반환 내용을 그대로 HTTP 응답 본문에 반환하여 클라이언트로 보내게 하는 어노테이션이다. 
### @Controller
Spring MVC에서 해당 자바 파일이 컨트롤러 역할을 할 수 있게 하는 어노테이션이다.
### @ResponseBody
메서드가 반환하는 데이터를 HTTP Response Body에 직접 사용하기 위해 붙이는 어노테이션이다.
### @GetMapping()
컨트롤러에서 메서드 위에 붙여 사용하며, '()'안의 경로에 대한 GET 요청이 들어왔을 때 어떤 메서드가 실행되게 지정하는 어노테이션이다.
### @RequestMapping
요청 URI를 메서드 또는 클래스와 매핑시키는 역할을 하는 어노테이션이다. 클래스 위에 붙일 경우 클래스에 속하는 모든 메서드에 대한 기본 URI를 지정할 수 있다.
