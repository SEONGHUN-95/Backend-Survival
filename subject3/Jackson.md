# Jackson ObjectMapper
Java 객체와 JSON 데이터 간의 변환을 처리해주는 Java 라이브러리이다. 빠른 성능을 가지고 있으며 다양한 JSON 데이터 형식을 지원한다. Maven, Gradle 등으로 Jackson 라이브러리를 프로젝트에 추가하여 사용할 수 있으며 아래와 같은 기능을 수행할 수 있다.

- Java 객체를 JSON 형식으로 변환
- JSON 데이터를 Java 객체로 변환
- JSON 데이터를 트리 형식으로 파싱
- JSON 데이터를 스트리밍 형식으로 파싱

```
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;

public class JacksonExample {

    public static void main(String[] args) throws JsonProcessingException {
        // create an object mapper instance
        ObjectMapper objectMapper = new ObjectMapper();

        // 직렬화
        Person person = new Person("John", "Doe");
        String jsonString = objectMapper.writeValueAsString(person);
        System.out.println(jsonString);

        // 역직렬화
        String json = "{\"firstName\":\"Jane\",\"lastName\":\"Doe\"}";
        Person person2 = objectMapper.readValue(json, Person.class);
        System.out.println(person2.getFirstName() + " " + person2.getLastName());

        // 트리 형식으로 파싱
        String json2 = "{\"name\":\"John\",\"age\":30,\"city\":\"New York\"}";
        JsonNode node = objectMapper.readTree(json2);
        String name = node.get("name").asText();
        int age = node.get("age").asInt();
        String city = node.get("city").asText();
        System.out.println(name + ", " + age + ", " + city);
    }

    static class Person {
        private String firstName;
        private String lastName;

        public Person(String firstName, String lastName) {
            this.firstName = firstName;
            this.lastName = lastName;
        }

        public String getFirstName() {
            return firstName;
        }

        public String getLastName() {
            return lastName;
        }
    }
}

```
`ObjectMapper.writeValueAsString()`를 통해 Java 객체를 JSON 형식으로 변환할 수 있다.
`ObjectMapper.readValue()`를 통해 JSON 문자열을 Java 객체로 변환할 수 있다.
`objectMapper.readTree()`를 통해 트리 형식으로 변환할 수 있다.

## `@JsonProperty`
Jackson Json 라이브러리에서 사용되는 어노테이션으로 Java 클래스의 필드를 JSON 속성으로 매핑하는 데 사용된다.
JSON으로 직렬화할 때 해당 필드 또는 메소드의 이름을 지정할 수 있다.
```
public class Person {
    // JSON 객체에서 name 필드와 매핑
    @JsonProperty("name")
    private String personName;

    // JSON 객체에서 age 필드와 매핑
    @JsonProperty("age")
    private int personAge;
    
    public String getPersonName() {
        return personName;
    }
    
    public void setPersonName(String personName) {
        this.personName = personName;
    }
    
    public int getPersonAge() {
        return personAge;
    }
    
    public void setPersonAge(int personAge) {
        this.personAge = personAge;
    }
}

```