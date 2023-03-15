## URI & URL & URN
### URI(Uniform Resource Identifier)
URI는 각 리소스 식별을 위해 사용되는 리소스 식별자를 의미한다.
URI에는 URL과 URN이 있으며, URL은 리소스가 저장된 주소를 포함하고 URN은 리소스의 이름을 포함하고 있다. WEB에서 대부분 URL을 사용한다.
### URL(Uniform Reource Locator)
URL은 다음과 같은 형태의 구문을 갖는다.
** 프로토콜://도메인:포트번호//경로?쿼리#앵커 **
프로토콜은 브라우저가 사용하는 프로토콜, 도메인은 IP주소와 대응되는 주소, 포트번호는 IP 내 애플리케이션을 구분하기 위한 번호, 경로는 리소스의 상세 경로를 표시하며 쿼리는 웹서버에 전달해야 할 추가 매개변수를 담고 있고 앵커는 리소스 내의 특정 지점을 담고 있다.

## MIME(Multipurpose Internet Mail Extensions)
MIME는 클라이언트로 전송되는 문서타입 정보를 표현하는 방식이다. HTTP 헤더에서 Content-Type 속성으로 전달되며 IANA에 등록된 타입을 사용하거나 등록할 수도 있다.
같은 리소스에서 다른 MIME 타입으로 전달된다면 클라이언트는 MIME를 확인하고 각자 다른 다음 동작을 결정한다. 그러므로 MIME 타입 지정이 중요하다.

### 자주 쓰이는 MIME 형식
- text/plain
- text/html
- text/css
- text/javascript
- application/xml 
- application/json
