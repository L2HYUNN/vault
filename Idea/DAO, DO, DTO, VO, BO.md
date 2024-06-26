## DAO(Data Access Object)
데이터 사용기능 담당 클래스. DataBase 접근을 하기 위한 로직과 비지니스 로직을 분리하기 위해 사용. 때문에 DB Connection 로직까지 설정되어있는 경우가 많음. DB를 사용해 데이터를 CRUD(조회, 조작)하는 기능을 전담.

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TestDao {

    public void add(TestDto dto) throws ClassNotFoundException, SQLException {
        Class.forName("com.mysql.jdbc.Driver");
        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost/test", "root", "root");

        PreparedStatement preparedStatement = connection.prepareStatement("insert into users(id,name,password) value(?,?,?)");


        preparedStatement.setString(1, dto.getName());
        preparedStatement.setInt(2, dto.getValue());
        preparedStatement.setString(3, dto.getData());
        preparedStatement.executeUpdate();
        preparedStatement.close();
        
        connection.close();

    }
}
```

한마디로 Database의 data에 access하는 트랜잭션 객체입니다. 일종의 객체라는 것을 잊으시면 안됩니다. 사용적인 측면에서 DAO는 저수준의 Logic 과 그급 비지니스 Logic을 분리하고, Domain logic으로 부터 Persistence mechanism을 숨기기 위해 사용합니다.( persistence 계층 : Database(영구저장소)에 data를 CRUD하는 계층 )

기능과 구조적인 측면에서 DAO는 데이터베이스 연결/입력/수정/삭제/검색/출력 등 모든 데이터베이스와의 처리를 담당하는 클래스 구조입니다.

## DTO(Data Transfer Object)
데이터 저장 담당 클래스. Controller, Service, View 등 계층간 데이터 교환을 위해 사용되는 객체이다. 로직을 갖지 않는 순수한 데이터 객체이며 getter, setter 메소드만을 포함한다. 가변의 성격을 가진다.

```java
public class PersonDTO {

    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

## VO(Value Object)
데이터 저장 담당 클래스. DTO와 혼용해서 쓰이지만, VO는 값(Value)을 위해 쓰이는 객체로 불변(read only)의 속성을 가진다. 보통 getter의 기능만을 포함한다.

## BO(Business Object)
비즈니스 로직을 포함하는 오브젝트. VO인데 비즈니스관련내용을 담은 VO라고 보면될듯.  
