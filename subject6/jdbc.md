# JDBC(Java Database Conectivity)

JDBC는 Java에서 각 데이터베이스로 연결할 수 있게 하는 API이다. JDBC를 통해 데이터베이스의 연결, 트랜잭션 관리, CRUD, 예외처리 등을 가능케한다. 이를 위해 JDBC는 여러 클래스와 인터페이스를 제공하는데, 해당 DBMS에 맞는 JDBC 드라이버 설치가 필요하다.(MySQL, PostgreSQL, Oracle 용 등의 드라이버가 각각 필요함)

```

// jdbc 연결 예시
import java.sql.*;

public class JdbcExample {
    public static void main(String[] args) {
        // JDBC 드라이버 로드
        try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            System.err.println("JDBC 드라이버 로드 실패: " + e.getMessage());
            return;
        }

        // 데이터베이스 연결 정보
        String url = "jdbc:mysql://localhost/mydb"; // MySQL 데이터베이스 URL
        String user = "username"; // MySQL 데이터베이스 사용자명
        String password = "password"; // MySQL 데이터베이스 비밀번호

        // 데이터베이스 연결
        try (Connection conn = DriverManager.getConnection(url, user, password)) {
            System.out.println("데이터베이스 연결 성공");

            // 쿼리 실행
            String query = "SELECT * FROM users";
            try (Statement stmt = conn.createStatement();
                 ResultSet rs = stmt.executeQuery(query)) {
                // 결과 처리
                while (rs.next()) {
                    int id = rs.getInt("id");
                    String name = rs.getString("name");
                    int age = rs.getInt("age");
                    System.out.println("ID: " + id + ", 이름: " + name + ", 나이: " + age);
                }
            }
        } catch (SQLException e) {
            System.err.println("데이터베이스 연결 실패: " + e.getMessage());
        }
    }
}

```

# JDBCTemplate

JdbcTemplate은 JDBC 인터페이스를 구현한 스프링 프레임워크의 클래스로서 Jdbc를 더 간편하게 사용할 수 있게 도와주는 메서드들을 제공한다. 템플릿을 통해 개발자가 직접 driver를 로딩해서 연결을 직접 해야하는 부분을 설정을 통해 자동화할 수 있다. 또한, 메서드들을 통해 DB 연결, SQL 실행을 비교적 간단하게 실행할 수 있고 트랜잭션 관리 등이 가능하다.

- 트랜잭션(Transaction)
트랜잭션은 데이터베이스에서 실행 단위를 의미하고 한 트랜잭션 안에서 실행되는 모든 작업이 완료될 때에만 전체 작업을 완료시킨다. 
