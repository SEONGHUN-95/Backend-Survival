# DTO(Data Transfer Object)

DTO는 프로세스 간 혹은 계층(F/E - B/E)간 데이터를 전달할 때 사용되는 객체이다. DTO는 일반적으로 POJO 구조이고, 데이터를 담기 위한 field, getter/setter 등으로 구성되어 있으며 비즈니스 로직을 가질 수는 없다.

DTO를 사용할 때의 장점으로는, 데이터를 불러올 때 발생할 수 있는 여러 원격 호출들을 단일 호출로 일괄 처리가 가능하게 한다. 그리고 도메인 대신 DTO를 사용함으로써 UI가 도메인을 직접 접근할 수 있는 가능성을 차단하여 보안적 측면에서 효율적이다. 또한 DTO 계층에 data validation 기능을 배분하여 Entity에서는 비즈니스 로직 구현에 집중할 수 있는 기능 분리가 가능하다.
 
* Remote Facade는 서비스 제공자와 클라이언트 간 데이터 전송을 지원하는 원격 인터페이스이다. 클라이언트가 서버의 세부 구현 상태를 알 필요가 없고 간단하고 일관성 있는 인터페이스를 통해 서비스를 호출 할 수 있어 유지 보수성과 확장성을 높일 수 있다.

# 프로세스 간 통신(IPC, Inter-Process Communication)

서로 다른 프로세스 간에 데이터를 주고 받는 통신 기술이다. IPC를 위한 기술에는 File, Socket 등이 있으며 Java에서는 RPC와 RMI가 있다.

## RPC(Remote Procedure Call)

RPC는 분산 시스템에서 원격으로 서버의 프로시저를 호출하여 결과값을 반환받는 프로시저 호출 프로토콜이다. 즉, 네트워크상에서 다른 컴퓨터의 프로그램이나 함수를 호출할 수 있게 해주는 프로그래밍 기술이다. 파일 공유, 프린터 공유 등과 같은 분산 애플리케이션에서 사용할 수 있다.

## RMI(Remote Method Invocation)

RMI는 Java가 제공하는 분산 객체 기술이다. 자바 객체를 직접 전송하고 반환 받을 수 있어서 객체간 데이터 전달 및 처리가 더 용이하다.
예를 들어, 서버에서 서버-클라이언트 통신을 위한 메서드를 가진 인터페이스를 정의하고, 이 인터페이스를 구현한 서버 클래스를 작성한 후 RMI 레지스트리에 등록하면 클라이언트에서 서버 클래스의 메서드를 호출할 수 있게 된다.

```
// 서버 클래스 정의 및 구현

import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;

public class Client {
    private Client() {}

    public static void main(String[] args) {
        try {
            Registry registry = LocateRegistry.getRegistry(null);
            Server stub = (Server) registry.lookup("Server");
            String response = stub.sayHello();
            System.out.println("response: " + response);
        } catch (Exception e) {
            System.err.println("Client exception: " + e.toString());
            e.printStackTrace();
        }
    }
}

import java.rmi.RemoteException;
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;
import java.rmi.server.UnicastRemoteObject;

public class ServerImpl implements Server {
    public ServerImpl() {}

    public String sayHello() {
        return "Hello, world!";
    }

    public static void main(String args[]) {
        try {
            ServerImpl obj = new ServerImpl();
            Server stub = (Server) UnicastRemoteObject.exportObject(obj, 0);

            Registry registry = LocateRegistry.createRegistry(1099);
            registry.bind("Server", stub);

            System.err.println("Server ready");
        } catch (Exception e) {
            System.err.println("Server exception: " + e.toString());
            e.printStackTrace();
        }
    }
}
```

```
// 클라이언트 코드
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;

public class Client {
    private Client() {}

    public static void main(String[] args) {
        try {
            Registry registry = LocateRegistry.getRegistry(null);
            Server stub = (Server) registry.lookup("Server");
            String response = stub.sayHello();
            System.out.println("response: " + response);
        } catch (Exception e) {
            System.err.println("Client exception: " + e.toString());
            e.printStackTrace();
        }
    }
}

```
## Anemic Domain Model(무기력한 도메인 모델)

Anemic Domain Model은 도메인 모델이 비즈니스 로직은 포함하지 않고 데이터 저장, 조회 기능만 제공하는 구조를 가진 도메인 모델이다.
이 모델이 안티패턴으로 여겨지는 이유는 객체지향의 핵심 원리인 캡슐화와 추상화를 달성하기 힘들기 때문이다. 비즈니스 로직이 분리되어 있어 로직 변경 시 메서드 변경이 필요하고 중복 코드 발생 가능성이 높아져 유연성과 확장성이 떨어지게 된다.

Anemic Domain Model과 이와 상반되는 모델인 Rich Domain model의 예시는 다음과 같다.

```
// Anemic Domain model
public class Order {
    private Long id;
    private Date date;
    private Customer customer;
    private List<OrderLine> orderLines;

    // getters and setters

    public BigDecimal calculateTotal() {
        BigDecimal total = BigDecimal.ZERO;
        for (OrderLine line : orderLines) {
            total = total.add(line.getProduct().getPrice().multiply(BigDecimal.valueOf(line.getQuantity())));
        }
        return total;
    }
}
```

```
// Rich Domain Model
public class Order {
    private Long id;
    private Date date;
    private Customer customer;
    private List<OrderLine> orderLines;

    // getters and setters

    public void addOrderLine(Product product, int quantity) {
        orderLines.add(new OrderLine(product, quantity));
    }

    public void removeOrderLine(OrderLine orderLine) {
        orderLines.remove(orderLine);
    }

    public BigDecimal calculateTotal() {
        BigDecimal total = BigDecimal.ZERO;
        for (OrderLine line : orderLines) {
            total = total.add(line.calculateLineTotal());
        }
        return total;
    }
}

public class OrderLine {
    private Product product;
    private int quantity;

    // getters and setters

    public BigDecimal calculateLineTotal() {
        return product.getPrice().multiply(BigDecimal.valueOf(quantity));
    }
}

```

# 자바빈즈(JavaBeans)

자바빈즈는 재사용이 가능한 소프트웨어 컴포넌트를 개발하기 위한 규약이다. 클래스의 속성을 private으로 선언하고, getter와 setter 메서드로 속성에 접근하는 것이 중점이다.
또한, Serialize 인터페이스를 구현하여 객체를 직렬화할 수 있고 MVC 패턴에서 Model 역할을 수행할 수 있으며, PorpertyChangeSuppoert 클래스를 제공한다. DTO를 자바빈즈 규약으로 작성하곤 한다.

# EJB(Enterprise JavaBeans)

EJB는 자바 기반 엔터프라이즈 애플리케이션 개발을 위한 서버 측 컴포넌트 모델이다. 분산 환경에서 실행되며 분산 트랜잭션 처리를 지원한다. 컨테이너에 의해 관리되며 트랜잭션 처리, 보안, 네트워크 통신 등의 기술을 사용할 수 있다.
EJB를 사용해서 JPA를 구현하여 데이터베이스와의 상호작용에 사용하곤 한다.

* 컴포넌트 모델 : 컴포넌트를 개발하고 구성하는데 필요한 인터페이스와 규약을 제공하는 소프트웨어 아키텍처이다. 표준화된 구성, 배포, 실행 상호작용 방식을 통해 개발자가 쉽게 컴포넌트를 재사용할 수 있다. 

# Java의 record

Java14부터 도입된 record는 class와 비슷한 구조를 가진 클래스이다. Immutable하고 간단한 데이터 표현용으로 사용된다. 생성자, getter, toString() equals() hashCode()함수들을 기본으로 제공해준다.
내부의 데이터를 변경할 수 없어 멀티스레드 환경에서 안전하고 예측 가능한 동작을 보장한다. 그로 인해 record는 DTO 및 Value Object를 생성하는데 적합하다.

```
// record 사용 예시
public record UserDTO(String name, int age) {

}
```

# DAO(Data Access Object)

DAO는 추상화된 API를 통해 응용 / 비즈니스 계층과 DB 계층을 분리하여 사용할 수 있게 하는 디자인 패턴이다. 계층간의 분리를 통해 각 계층이 각자 진화할 수 있는 조건을 형성한다. Entity에 원하는 로직대로 데이터를 뽑아 쓸 수 있고, SQL query를 직접 사용하여 Database를 조작할 수도 있다.
Repository와 기능이 유사하지만 DAO는 매핑이 SQL 차원인 점, Repository는 객체 차원인 점에서 차이가 있다.

# ORM(Object-Relational Mapping)

ORM은 객체 지향 프로그래밍 언어와 관계형 데이터베이스의 데이터 변환을 자동화하는 기술이다. 개발자는 ORM Tool을 통해 SQL문을 사용하지 않고 객체와 메서드를 호출하여 데이터에 접근, 조작할 수 있다. Java의 JPA, Hibernate Flask의 SQLAlchemy Django의 자체 ORM 등이 있다.

![ORM](/Users/kooseonghun/Downloads/tcpip-7.jpg)

## JPA

JPA는 Java ORM 기술에 대한 API 표준 명세를 의미한다. ORM 사용을 위한 인터페이스를 모아둔 것이므로 구현부는 없으며, 이를 구현한 Hibernate 등의 ORM 프레임워크를 통해 JPA를 활용할 수 있다. 나중에 자세히 알아보자..