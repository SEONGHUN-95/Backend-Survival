# Collection Pattern
컬렉션 패턴이란 RESTful API에서 주로 사용되는 URI를 통해 컬렉션 리소스를 표현하는 방법이다.
REST 제약사항과 함께 컬렉션 패턴을 적용한 URI 디자인 구성 특징은 아래와 같다.
1. 가능한 리소스 URI는 "명사" 기반으로 설계. 만약 그룹명이라면 명사를 복수로 사용.
2. 컬렉션은 URI 경로로 표현하고 계층구조로 설계하여 컬렉션 내 개별 리소스를 직관적으로 알아볼 수 있다.
    ex) /customer/{id}
3. HTTP 메서드를 사용하여 컬렉션의 CRUD를 수행할 수 있다. 같은 리소스에 대해서도 다른 메서드를 사용해서 다른 작업을 수행할 수도 있다.
4. 페이징 및 필터링이 가능하다. URI 내 쿼리에 매개변수를 활용하여 페이징과 필터링을 사용할 수 있다.
```
/users?page=2&per_page=10
/users?name=john
```
5. 리소스 URI 설계시 컬렉션/항목/컬렉션 이상으로 복잡하게 설계하지 말라. 만약 계속해서 컬렉션과 항목이 추가하여 요청할 데이터라면 컬렉션을 기준으로 중간에 나누어서 추가 참조를 하면 되겠다.
```
# 1번 사용자의 99번째 주문 내역의 구매물건들
/customers/1/orders/99/products -> /customers/1/orders(주문에 대한 리소스 참조 지정), /orders/99/products
```

# CQS(Command Query Separation)
CQS는 명령과 조회를 분리하여 서로에게 영향을 미치지 않는 소프트웨어 디자인 패턴의 일종이다. Command(명령)은 작업을 수행하되 결과를 반환하지 않아야 하고, Query(조회)는 데이터만 반환하고 어떠한 작업을 수행하지는 않는다. 이를 통해 코드를 분리하고 간결하게 하여 코드의 유지보수성과 확장성을 향상시킨다.