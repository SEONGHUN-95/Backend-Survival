# Java HTTP Server
## Java HTTP Server
Java에서는 고수준의 HTTP 서버 API를 제공한다. 이는 Java HTTP Server 모듈로 제공되며 크게 네가지 단계로 나누어 클래스를 사용할 수 있다. 우선, HTTPServer의 객체를 생성하고 HTTPServer 컨텍스트 추가, HTTPServer 시작, HTTPServer 종료하는 단계로 데이터를 주고 받을 수 있다. 컨텍스트 추가 단계에서 핸들러를 통해 클라이언트의 변경된 요청을 URL을 통해 확인하고 그에 해당하는 요청 처리 코드를 적용할 수 있다.
## Java NIO
Java Non-Blocking I/O는 작업을 지원하는 새로운 방식을 제공한다. 기존의 Blocking I/O는 대규모 작업시 발생하는 비효율성과 확장성의 제한을 해결하기 위해 나온 새로운 방식이다. 채널과 버퍼를 사용하며 하나 이상의 채널이 구축되어 있어야만 작업이 수행된다.
## Java Lambda expression(람다식)
람다식은 이름이 없는 메서드이다. 매개변수와 구현부로만 이루어져 있는데 다음과 같이 쓸 수 있다.
```
//람다식 예시
(parameter) -> {body}
(int a, int b) -> {return a+b;}

// 매개변수가 하나면 괄호 생략 가능, 매개변수 타입 추론 가능하면 타입도 생략 가능
a -> {return a};

// 구현부에 return 있으면 중괄호 생략 불가하기 때문에 표현식으로 변경 사용 가능
(a,b) -> a > b ? a: b
```
### Java Functional interface(함수형 인터페이스)
함수형 인터페이스란 하나의 추상메서드만을 가지고 있는 인터페이스를 말한다. 또, 추상메서드는 선언부만 있고 구현부는 미구현된 메서드를 의미한다. 람다식을 사용하여 함수형 인터페이스를 구현하면 클래스를 정의하지 않고 간단하게 사용할 수 있다. 이를 통해 코드의 가독성을 향상하고 불필요한 코드의 중복을 줄이며 컴파일러로 하여금 함수형 인터페이스임을 알릴 수 있다. 

```
// Function 함수형 인터페이스
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```

```
// 람다식을 통해 Function 인터페이스를 구현한 코드
Function<String, Integer> getLength = str -> str.length();
int length = getLength.apply("Hello, world!");
System.out.println(length); // 13
```
위의 예시와 같이 Function 인터페이스에 있는 apply()라는 유일한 추상 메서드를 람다식으로 구현하고 사용할 수 있다. Function 인터페이스를 람다식으로 구현하고 이를 getLength 변수에 저장, 인터페이스에 있던 apply 메서드를 호출하여 입력값을 처리한 결과를 얻는 것이다.
