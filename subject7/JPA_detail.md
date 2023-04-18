# JPA 활용 전략

JPA가 어떻게 등장하게 되었는지, 어떤 점을 주의해서 사용해야 하는지, Best Practice는 어떤건지 알아보자.

# 기존 문제점 

Java는 객체지향 언어이다. 그래서 코드가 다 클래스와 메서드로 이루어져 있다. 
이 객체지향 언어로 영구 저장소의 데이터를 불러오고 저장하고 하는 과정은 다음과 같다. 

1. SQL문 작성

```

String query = "SELECT NAME, HEIGHT FROM MEMBER WHERE NAME = ?";

```

2. JDBC로 실행

```

ResultSet rs = stmt.executeQuery(query);

```

3. 객체 - DB 연관관계 매핑

```

String memberName = rs.getString("NAME");
String memberHeight = rs.getString("HEIGHT");

Member member = new Member();
member.setName(memberName);
member.setHeight(memberHeight);

```

데이터베이스에서 데이터를 CRUD하는 코드 하나 하나 다 위와 같은 과정을 거쳐야 한다.
이렇게 되면 코드가 DB에 강하게 의존하게 된다. 

또, 요구사항 변화로 데이터베이스에 테이블이 바뀐다고 치자. 그럼 함께 바꾸어야 하는건 뭘까?
위 과정을 한 번 더 실행해주어야 한다. 클래스의 필드 추가는 물론이고 JDBC 코드를 바꾸며, DAO를 수정해주어야 한다.
최초 코드 작성 후 이러한 변동 사항이 있을 때마다 모든 수정 사항에 대해 다 매핑해주어야 한다. 

# 하지만 JPA라면 어떨까?

JPA는 객체와 DB를 매핑해주는 인터페이스라고 하였다.
객체에 수정 사항을 적용하고 싶다면 객체를 수정하고, 객체를 다루는 메서드만 변경해주면 개발자가 영속성 게층에 직접 쿼리를 날릴 필요 없이 수정이 가능하다.

```

class Member{
    String id;
    String name;
    int height

    // 수정 필드
    int salary;
}

// 새로 추가된 필드 조건을 적용한 메서드를 사용하여 데이터 불러오기

jpa.findBySalarayGraterThanEqual(salary);

```

이외에도 객체지향의 상속 개념, 연관관계 등을 쉽게 설정하고 Java의 컬렉션에서 데이터를 다루듯 사용할 수 있어 편리하다.

# 주의점

# Best Pratice의 흐름