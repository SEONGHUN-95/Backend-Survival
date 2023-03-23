# CORS(Cross-Origin Resource Sharing)
서버가 브라우저의 자체 출처 이외에의 출처에서 리소스 로딩을 허용하는 HTTP 헤더 기반의 메커니즘이다.

브라우저는 Same Origin Policy라는 보안 메커니즘을 적용하는데, 이는 동일한 출처에서 가져온 리소스만 서로 상호작용할 수 있도록 하는 정책이다. 여기서 출처란 프로토콜과 호스트명 그리고 포트 번호를 의미한다. 이 세가지가 동일해야 동일 출처라고 할 수 있다. 동일 출처 정책은 다른 출처에서 가져온 리소스에서 직접 접근하여 데이터를 가져오는 것은 보안상 위험하기 때문에 마련되었다. 

예컨대, A 도메인의 웹 페이지에서 B 도메인의 이미지를 가져온다고 할 때 B 도메인의 이미지에 악성 스크립트가 속해 있을 때에도 무심코 가져와 브라우저에 있는 데이터를 탈취당하는 불상사가 발생할 수도 있다. 
그로 인해 리소스를 제공할 도메인에 대해서만 백엔드 서버에 CORS를 설정하고 이외의 도메인에 대해서는 신중한 설정이 필요하다.

`Access-Control-Allow-Origin: *` 
이와 같은 코드로 다른 출처의 리소스를 조절하여 접근할 수 있다.

기본 동작원리는 다음과 같다. 브라우저는 본인의 Origin 헤더에 자신의 Origin을 설정한 후 요청을 보내고, 서버에서 오는 응답헤더를 확인하여 `Access-Control-ALlow-Origin`에 본인 출처가 있으면 서버의 응답을 받아들이고 없다면 받지 않는다. 

- `<img>`, `<video>`, `<script>`, `<link>` 태그들을 사용한 요청은 기본적으로 CORS가 적용된다.
- XMLHttpRequest, Fetch API 스크립트는 Same-Origin 정책을 따른다.

# JSONP(JSON with Padding)
다른 도메인에서 CORS를 사용하지 않고 자바스크립트 코드 형식으로 데이터를 가져오는 방식 중 하나이다.
JSONP는 XMLHttpRequest 등의 메서드 대신, `<script>` 태그 안에 요청할 도메인과 콜백함수를 담아 데이터를 요청한다. 그럼 서버에서는 JSON 데이터를 포함한 콜백함수를 실행시켜 받아온 데이터를 작동시킨다. 
SOP를 우회할 수 있으나 보안상 CORS 사용이 추천된다고 한다.

## `@CrossOrigin`
@CrossOrigin 애너테이션은 특정 URL에서 어플리케이션에 대한 HTTP 요청을 수락할 수 있게 해주는 애너테이션이다.
클래스와 메서드 모두에 붙일 수 있으며 매개변수를 통해 조작이 가능하다.

`@CrossOrigin(origins = "http://localhost:8080",allowedHeaders = {"Authorization", "Content-Type"}, allowCredentials = "true")`
origins의 출처에서 전달된 요청 허용, 허용된 헤더, 자격 증명 포함 요청 허가 여부 설정 등이 가능하다.