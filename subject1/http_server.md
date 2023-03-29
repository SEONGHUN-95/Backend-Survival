# HTTP Server

## Java ServerSocket

Java에서 ServerSocket은 java.net 패키지에 속해 있는 클래스로서 서버 역할을 수행한다. ServerSocket은 소켓을 생성하고 클라이언트의 요청을 기다리다가 요청이 들어오면 accept 메서드를 통해 통신용 소켓을 만들어 반환한다. 이후 이 소켓을 통해 요청과 응답이 이루어진다.
```
// 예시코드
try(){
    ServerSocket listener = new ServerSocket(8080);
    while(true){
        Socket socket = listener.accept();
        // 클라이언트와 통신을 위한 소켓 사용
        socket.close();
    }
    catch(IOException e){
        // 예외처리
    }
}
```

## Blocking vs Non-Blocking

소켓 프로그래밍에서 블로킹이란 입출력 함수 등이 호출될 때 해당 함수가 반환될 때까지 다음 코드가 실행되지 않는 상태를 의미한다. 논블로킹 상태는 입출력 함수가 호출되면 해당 함수가 즉시 반환되고 데이터를 읽거나 쓸 수 없을 때에는 에러코드를 반환하는 방식이다. 지금 입출력 함수가 반환되지 않으면 뒤에 있을 코드가 의미 없는 경우이거나 데이터가 즉시 사용 가능할 때에는 블로킹 방식을 사용하는 것이 적합하고,입출력 가능 여부를 확인하고 아니라면 다른 작업을 해야할 때는 논블로킹 방식이 유용하다. 