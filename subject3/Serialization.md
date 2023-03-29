# 직렬화(Serialization)

직렬화는 저장소, 다른 시스템으로 전송할 수 있도록 객체의 상태를 변환하는 과정을 의미한다. 문자열 형태(CSV, JSON)나 이진 직렬화 형식으로 이 때 객체의 필드나 메서드 등의 정보가 바이트 형태로 변환되고, 이를 파일로 저장하거나 다른 시스템으로 전송할 수 있다. 

직렬화를 하는 이유는 다음과 같다.
Primitive한 데이터를 전송할 때는 데이터 그 자체를 보내도 데이터를 가지고 다른 프로세스, 드라이브 등으로 이동할 수 있다. 하지만 각 프로세스별 메모리의 주소값을 가지고 있는 Object형의 데이터들은 그저 메모리 값만을 가지고 있기 때문에 결국 무의미한 데이터가 이동하게 되는 것이다.
이를 해결하기 위해 객체를 '직렬화'해서 객체가 객체가 내포하고 있는 데이터를 전송할 수 있게 되는 것이다.

- 객체는 메모리에 저장되고 추상적인 반면, 직렬화된 데이터 즉, String 혹은 bytes들은 드라이브에 저장되거나 통신선을 따라 데이터가 이동할 수 있다.
- 직렬화를 하게 되면 객체의 메모리 구조를 그대로 저장하기 때문에 역직렬화시 객체를 새로 생성할 소요가 줄어 효율적이다.
- 직렬화를 하지 않았을 경우 텍스트 형태 등으로 저장하게 되는데 이는 데이터를 불러오거나 저장할 때 텍스트를 객체로 변환해야하기 때문에 오버헤드가 크다. 


Java에서 직렬화가 되기 위해서는 java.io.Serializable 인터페이스를 상속받아야 한다.

```
// 직렬화할 클래스
import java.io.Serializable;

public class MyClass implements Serializable {
    private int id;
    private String name;

    public MyClass(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}

```

Java.io.ObjectOutputStream 객체를 활용하여 직렬화한다.
```
// 직렬화
import java.io.*;

public class SerializationExample {

    public static void main(String[] args) {

        MyClass obj = new MyClass(1, "Hello");

        try {
            FileOutputStream fileOut = new FileOutputStream("myclass.ser");
            ObjectOutputStream out = new ObjectOutputStream(fileOut);
            out.writeObject(obj);
            out.close();
            fileOut.close();
            System.out.println("Serialized data is saved in myclass.ser");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

역직렬화할 때는 ObjectInputStream 클래스를 활용한다.

```

// 역직렬화
import java.io.*;

public class DeserializationExample {

    public static void main(String[] args) {

        MyClass obj = null;

        try {
            FileInputStream fileIn = new FileInputStream("myclass.ser");
            ObjectInputStream in = new ObjectInputStream(fileIn);
            obj = (MyClass) in.readObject();
            in.close();
            fileIn.close();
        } catch (IOException e) {
            e.printStackTrace();
            return;
        } catch (ClassNotFoundException c) {
            System.out.println("MyClass class not found");
            c.printStackTrace();
            return;
        }

        System.out.println("Deserialized data:");
        System.out.println("Id: " + obj.getId());
        System.out.println("Name: " + obj.getName());
    }
}

```
# 마샬링

직렬화하는 과정에서 네트워크 프로토콜에 맞게 변환하는 과정을 포함하는 것이 마샬링이다. 

# JSON(Javascript Object Notation)

JSON이란 사람이 읽을 수 있는 텍스트를 사용하여, key - value 형태로 구성된 데이터 교환 형식이다. Javascript에서 파생되었으나 언어와 무관하게 사용이 가능하다. JSON은 상태 비저장 방식의 실시간 서버 - 브라우저 통신 프로토콜을 위해서 개발되었다. 최근 XML을 몰아내고 간단하고 가벼운 특징으로 데이터 형식 세계를 독점하고 있다.

- JSON은 숫자, 문자열, 배열, 불리언, 객체들을 포함할 수 있고 MIME 공식 유형은 "application/json"이다.
- JSON은 데이터 포맷으로, 프로퍼티만 포함할 수 있고 메서드 사용은 불가하다.
- JSON의 문자열과 프로퍼티 이름 작성 시 큰 따옴표만 사용 가능하다.
- 배열로도 사용할 수 있다.

```
// JSON 객체 생성
import org.json.*;

JSONObject jsonObject = new JSONObject();
jsonObject.put("name", "John");
jsonObject.put("age", 30);
String jsonString = jsonObject.toString();
//{ "name" : "John", "age" : 30 }
```
