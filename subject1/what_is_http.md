# What is HTTP?

## HTTP(Hypertext Transfer Protocol)

HTTP란 인터넷 상에서 HTML를 포함한 문서들을 주고 받을 때 사용하는 통신규약을 의미한다.

## HTTP와 HTTPS의 차이(TLS)

HTTPS는 기존 HTTP에서 데이터 암호화 과정(TLS)을 추가하여 더 안전한 통신을 가능케 하는 프로토콜이다. 클라이언트와 서버가 통신할 때 최초 세션키를 암호화하여 공유한 후, 이후 데이터를 주고 받을 때마다 매번 데이터를 암호화해서 통신한다.

[HTTPS](https://www.encryptionconsulting.com/education-center/what-is-https/#:~:text=How%20HTTPS%20works%3F,keys%20to%20secure%20the%20communication.)
[TLS](https://www.cloudflare.com/ko-kr/learning/ssl/transport-layer-security-tls/)

## 클라이언트-서버 모델

클라이언트가 서버에 리소스를 요청하면, 서버가 응답하면서 클라이언트에 요청에 대한 리소스를 보내주는 구조를 의미한다. 

## stateless와 stateful

HTTP는 무상태(stateless) 프로토콜인데, 이는 매번 통신이 이루어질 때마다 독립적이고 서버가 클라이언트의 정보를 저장하지 않기 때문에 요청마다 클라이언트가 누군지 서버에 밝혀야 한다는 의미이다. http가 stateless한 이유는 서버 확장을 위해서라고 할 수 있다. 만약, 수 많은 서버가 존재하는데 stateful하다면 서버끼리 해당 요청을 한 클라이언트에 대한 정보를 공유하여야 하기에 비효율적이다.  

## HTTP Cookie와 HTTP Session

쿠키와 세션은, stateless한 HTTP 프로토콜에서 클라이언트의 정보를 유지하기 위해 사용하는 데이터를 의미한다.
쿠키는 서버에서 클라이언트로 보내는 데이터 조각으로서 웹 브라우저가 이를 저장한 후 추후 request 때 제시함으로써 클라이언트로 하여금 현재 본인이 과거의 쿠키를 받았던 사람임을 입증할 수 있게 하는 도구이다.
쿠키에는 쿠키 이름, 유효기간, 유효범위 등의 값을 가지고 있으며 HTTP 헤더에 저장되어 클라이언트로 전달된다.
세션(ID)은 서버가 클라이언트의 ID를 부여하여 요청이 올 때마다 서버에서 가지고 있는 ID값과 대조하여 기존 사용자인지를 확인한다. 클라이언트는 쿠키에 세션 값을 포함하여 전달하고, 쿠키보다 보안성은 높지만 사용자가 많아질수록 서버에 저장해야하는 세션 ID 값이 많아지기 때문에 성능 저하를 초래한다. 

## HTTP 메시지 구조

HTTP 메시지의 구조는 Start-Line, Header, Blank-Line, Body로 이루어져 있다. 

### HTTP 요청(Reuqest)와 응답(Response)

#### HTTP 요청

HTTP 요청의 시작줄에는 HTTP 메서드, URL, HTTP 버전이 들어간다. 즉, 어느 버전의 프로토콜로 어느 위치에 어떠한 행동을 요청하는 것이다. Header에는 데이터의 상세 정보가 포함된다(내용 길이, 내용 형식, 구체화된 요청사항 등). Body에는 메시지의 본문이 포함된다.(GET, HEAD 등의 값을 가져오는 요청에는 본문 없음.)

#### HTTP 응답

HTTP 응답의 시작줄에는 *** 상태코드 ***, 프로토콜 버전, 상태 텍스트 등이 포함된다. Header에는 메타데이터가 포함되어 있고, Body에는 단일, 다중 리소스 본문이 포함되어 전송된다.

### multipart/form-data

multipart/form-data는 HTML Form이 가지고 있는 내용을 전송할 때 쓰는 Content-Type을 의미한다. 여러 종류의 정보를 한 번에 전송할 수 있고 '--'를 기준으로 각각의 정보가 나뉘며 헤더도 각자 가지고 있다.
HTML에서는 아래처럼 사용할 수 있다.
```
<form th:action="@{/menu/upload}" method="post" enctype="multipart/form-data">
  <h4>사진 업로드</h4>
  <input type="file" name="file">
  <h4>여러 사진 한 번에 업로드</h4>
  <input type="file" multiple="multiple" name="files">
  <input type="submit">
</form>
```

### HTTP 요청 메서드(HTTP request methods)

요청 메서드에는 GET, HEAD, POST, PUT, PATCH, DELETE, OPTIONS 등이 있다.
GET과 POST를 주로 사용한다.GET은 서버에게 값을 받아오는 메서드이며 Body에 본문이 없다. 금지되어 있지는 않으나 특정 구현체에게 요청을 거부받을 수 있으니 GET을 사용할 때는 본문이 없는 것이 바람직하다. POST는 서버로 데이터를 전송하여 변경사항을 만드는 메서드이다.

#### 멱등성

멱등성이란 동일한 메서드의 요청을 여러 번 보내는 것이 한 번 보낸 것과 동일한 결과를 초래하면 멱등성이 성립된다고 한다. 예컨대, 서버에 있는 사진을 GET 요청을 통해 불러오면 같은 값을 불러와 멱등성이 성립하지만, POST 요청을 해서 서버로 사진을 보내면 계속 추가되는 결과가 나와 멱등성이 성립되지 않는다. 

### HTTP 응답 상태 코드(HTTP response status code)

HTTP 응답 상태 코드는 HTTP 요청에 대한 결과가 어떠한 결과인지를 코드 값으로 알려주는 것을 의미한다. 
100번대 -> 정보 전달
200번대 -> 성공(200 OK, 201 Created, 204 No Content) 등 요청이 정상적으로 처리됨을 의미한다.
300번대 -> 리다이렉션
400번대 -> 클라이언트 오류(404 Not Found(요청한 페이지 없음))
500번대 -> 서버 오류(서버 개발자의 잘못으로 나오는 코드..)

#### 리다이렉션

HTTP에서 리다이렉션이란 클라이언트의 요청에 대해 서버가 다른 URL로 안내하는 것을 의미한다. 
과정은 클라이언트 요청 -> 서버 리다이렉트 응답 -> 클라이언트 재요청 -> 서버 응답의 과정을 따른다. 
영속적인 리다이렉션(301, 308), 일시적인 리다이렉션(302, 303, 307), 특수 리다이렉션(304, 300) 등이 있으며 304 코드는 캐시 값으로 리다이렉트됨을 의미한다.