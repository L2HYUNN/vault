### 개요

스프링으로 백엔드를 포팅하는 과정에서, DDD에서 주로 쓰이는 VO와 기존에 쓰던 DTO의 차이, 그리고 DAO와 Repository같이 상당히 유사한 기능을 함에도 이름이 다르고 용처가 다른 것들에 대해서 그 이유가 무엇인지 궁금해졌고, 이에 대해 알아보려고 한다.

### DTO?

DTO(Data Transfer Object)란, 프로세스 간에 데이터를 전달하는 객체다.

현재 웹 서비스에서는 주로 계층들, 특히 컨트롤러 - 서비스(표현 - 도메인) 단계에서 서로 주고 받는 데이터 양식이다. 주클라이언트에서 서버 쪽으로 전송하는 요청 데이터, 서버에서 클라이언트 쪽으로 전송하는 응답 데이터 형식으로 데이터가 전송된다. (Request && Response)

마틴 파울러의 정의에 따르면, 서비스 계층이란 어플리케이션의 비즈니스 로직 즉, 도메인을 보호하는 계층이다. 즉, 이 정의를 명확히 지키기 위해서는 응용 계층(컨트롤러)에 도메인을 노출해서는 안된다. 따라서 도메인은 서비스 계층에서 DTO로 변환되어 컨트롤러로 전달되어야 한다.

DTO는 전송해야 하는 데이터를 담는 컨테이너 역할을 하며, 한 계층에서 다른 계층으로 데이터를 매핑하는 역할을 담당한다. 이를 통해 계층 간의 결합을 줄이고 애플리케이션의 성능과 유지보수성을 개선할 수 있다. DTO는 어떠한 비즈니스 로직을 가져서는 안 되며, 저장, 검색, 직렬화, 역직렬화 로직만을 가져야 한다.

여러 호출에서 전송할 수 있는 데이터를 집계하지만 한 번의 호출로만 제공되는 객체(DTO)를 사용하면 비용을 줄일 수 있다. 또한, 필요한 정보들로만 래핑하기 때문에, 불필요한 정보들을 제공하지 않게끔 할 수 있다 - 데이터를 캡슐화 할 수 있다.

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F982e0fe4-a919-4325-bccf-a2d9265502af%2FUntitled.png&blockId=1a8cd1b8-25aa-4539-a451-5b8abc5519d4)

왼쪽의 HTML, Json ![](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==) UserDTO에서 데이터의 변환과정이 직렬화(DTO → HTML, JSON)와 역직렬화(HTML, JSON → DTO)다. 위 사진에서 중요한 것은 Mapper다. DTO Mapper는 데이터 전송 객체(DTO)와 도메인 객체(Domain) 간의 매핑을 담당하는 일종의 변환기다.

위 사진을 아래와 같은 예시로 생각해보자.

public class User { private Long id; private Roles role; private String email; // 생성자, getter && setter } public class UserDTO { private Long id; private Roles role; // 생성자, getter && setter }

Java

복사

위 방식으로 DTO를 설정한다면, 우리는 ‘email’이라는 데이터를 캡슐화 한 것이다. DTO는 getter와 setter를 가지므로, Mapper를 다음과 같이 작성해볼 수 있다.

public class UserMapper { public static UserDTO toUserDTO(User user) { UserDTO userDTO = new UserDTO(); userDTO.setId(user.getId()); userDTO.setName(user.getName()); return userDTO; } public static User toUser(UserDTO userDTO) { User user = new User(); user.setId(userDTO.getId()); user.setName(userDTO.getName()); return user; } }

Java

복사

•

Mapper에게 DTO 클래스 → 엔티티(Entity) 클래스로 변환하는 작업을 위임함으로써 Controller는 더이상 두 클래스의 변환 작업을 신경쓰지 않아도 된다. 역할 분리로 인해 코드 자체가 깔끔해진다.

•

Mapper가 엔티티 클래스를 DTO 클래스로 변환해주기때문에 서비스 계층에 있는 엔티티 클래스를 API 계층에서 직접적으로 사용하는 문제가 해결된다.

•

[MapStruct를 통한 DTO Mapper 고려해보기](https://medium.com/naver-cloud-platform/%EA%B8%B0%EC%88%A0-%EC%BB%A8%ED%85%90%EC%B8%A0-%EB%AC%B8%EC%9E%90-%EC%95%8C%EB%A6%BC-%EB%B0%9C%EC%86%A1-%EC%84%9C%EB%B9%84%EC%8A%A4-sens%EC%9D%98-mapstruct-%EC%A0%81%EC%9A%A9%EA%B8%B0-8fd2bc2bc33b)

### DAO?

DAO(Data Access Object)란, 영속성(Persistence) 계층에 인터페이스를 제공하는 패턴이다. DAO는 애플리케이션 호출을 영속성 계층에 매핑함으로써 DB 세부 정보를 노출하지 않고 데이터에 접근하게 해준다. 이러한 분리방식은 단일 책임 원칙(SRP)을 지킬 수 있게 해준다.

DAO 사용의 가장 큰 장점은 서로에 대해 알 필요가 없는 두 계층을 엄격하게 분리할 수 있다는 점이다. 비즈니스 로직은 지속적으로 DAO 인터페이스에 의존할 수 있으며, 이에 따라 영속성 로직 변경은 DAO를 사용하는 계층에 영향을 미치지 않는다.

DAO 사용의 단점으로는 코드 중복, 추상화 역전(Abstractin Inversion, 클래스 사용자가 클래스 내부에 구현되어 있지만 인터페이스에 노출되지 않은 함수를 필요로 할 때 발생하는 안티 패턴) 등이 있다. 특히, DAO를 일반 객체로 추상화하면 각 데이터 접근별 비용에 대한 인지가 모호해질 수 있다. 또한, 한 번의 작업으로 반환될 수 있는 정보(DTO)를 검색하기 위해 실수로 여러 데이터베이스 쿼리를 수행할 수 있다. 더해서, 여러 개의 DAO가 필요한 경우 각 DAO에 대해 동일한 생성, 읽기, 업데이트, 삭제 코드를 작성해야 할 수 있다.

DTO와 DAO의 차이점은 다음과 같다.

•

이름: 데이터 전송(Data Transfer)과 데이터 접근(Data Access)이라는 부분에서 차이점이 있다.

•

구성: DTO는 데이터와 게터/세터 메서드로 이루어져 있는데, DAO는 데이터CRUD(Create, Read, Update, Delete) 메서드로 이루어져 있다. DTO는 자체 데이터의 저장, 검색, 직렬화 및 역직렬화를 제외하고는 어떠한 동작도 수행하지 않는다. DTO는 비즈니스 로직을 포함해서는 안 되는 단순한 객체이지만 데이터를 전송하기 위한 직렬화 및 역직렬화 메커니즘을 포함할 수 있다.

•

적용 시점: DTO는 주로 서비스 계층(Service Layer)에서 비즈니스 로직과 함께 사용되며, DAO는 주로 데이터 접근 계층(Data Access Layer)에서 사용된다.

### VO?

VO(Value Object, 값 객체)란, 도메인에서 특정 값이나 개념을 나타내는 객체다. 일반적으로 원시 타입들로 구성된다. VO는 특정 값을 나타내기 위해 여러 속성들을 묶은 [도메인 객체](https://hudi.blog/domain-domain-model-domain-object/)다.

VO의 주요 특징 중 하나는 불변성(Immutable)이다. 일단 생성되면 내부 값을 변경할 수 없게끔 설정한다. 이는 VO의 세터를 만들지 않음으로써 가능하다.

VO의 또 다른 중요한 특징은 값 동일성(Value Equality)으로, 두 VO가 모든 속성에 대해 동일한 값을 가지면 동일하다는 것을 의미한다.VO가 같은지 아닌지는 프로퍼티들에 의해 정해진다는 것이다. 이것은 별개의 식별자를 가지는 Entity와는 구분된다. 고유한 ID를 갖는 엔티티 객체와는 다르게 값 객체는 ID와 상관 없이 그 속성으로만 구분하는 것이다.

클라이언트에서 값 객체를 유효하지 않은 상태로 만들거나 버그가 발생하는 동작을 유발할 수 없는 것이 장점이다.

값 객체를 적극 활용하면, 비즈니스적 의미를 갖는 객체들을 명시할 수 있고, 또 그 비즈니스 로직들을 포함시켜 서비스와 같은 다른 계층들의 무게를 덜어내줄 수 있다.

### DTO vs VO

DTO는 애플리케이션 자체의 계층 간 데이터를 주고 받기 위해서, 필요한 데이터들만 구성하기 위해서라면, VO는 현재 애플리케이션의 비즈니스에 사용되는 의미가 담긴 값을 의미하며, 비즈니스 로직을 가질 수 있다는 점에서 DTO와 다르다. 즉, 둘 다 필요한 데이터들을 포장한 것은 맞지만, 그 의도와 쓰임새에서 차이가 있는 것이다.

### Repository?

리포지토리 패턴(Repository Pattern)은 데이터 계층을 애플리케이션의 나머지 부분과 분리하는 디자인 패턴이다. 이 패턴은 데이터 계층과 비즈니스 계층을 매개하고, 나머지 앱에 대한 데이터 액세스를 위한 API를 제공한다. 리포지토리 패턴은 데이터를 검색하여 [엔티티 모델](https://happy-coding-day.tistory.com/entry/%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0%EB%A5%BC-%EA%B0%96%EB%8A%94-%EA%B0%9D%EC%B2%B4-%EC%97%94%ED%8B%B0%ED%8B%B0Entity%EB%9E%80)에 매핑하고 비즈니스 로직을 기본 데이터 소스 또는 웹 서비스와의 상호 작용에서 분리한다. 리포지토리 패턴을 사용하면, 엔티티와 관련된 모든 기본 데이터 작업을 하나의 기본 클래스에 통합할 수 있으므로 코드를 보다 효과적으로 유지 관리하기가 용이하다.

비즈니스 로직은 데이터 계층을 구성하는 데이터 유형에 영향을 받지 않아야 한다. 리포지토리 패턴을 사용하면 새 리포지토리만 작성하여 모델의 영속성(예: 데이터베이스)을 변경할 수 있으므로 앱의 다른 코드를 건드릴 필요가 없다.

### DAO vs Repository

DAO와 Repository의 주요 차이점은 DAO는 데이터 영속성을 추상화한 반면, 리포지토리는 도메인 개체 모음을 추상화했다는 점이다. DAO는 데이터베이스에 더 가깝고 테이블 중심적인 반면, 리포지토리는 도메인 계층에 더 가깝다. 리포지토리는 일반적으로 더 좁은 인터페이스로서 Get(id), Find(Specification), Add(Entity)와 같은 메서드를 사용한다. 반면에 DAO는 데이터 매핑/액세스 계층으로, 보기에, 그리고 사용하기에 까다로운 쿼리들을 가지고 있으며, 더 저수준의 개념으로 간주될 수 있다.

DAO는 좀 더 유연하고 일반적인 반면, 리포지토리는 좀 더 구체적이고 특정 유형으로 제한될 수있다. 리포지토리라고 불리는 구현이 실제로는 DAO에 더 가깝기 때문에 둘의 차이에 대해 약간의 혼란이 있을 수도 있다.

요약하자면, DAO는 DB에 더 가까운 저수준 개념으로 간주되는 반면, 리포지토리는 도메인 개체에 더 가까운 상위 수준 개념이다. DAO는 데이터 매핑/액세스 계층인 반면, 리포지토리는 도메인과 데이터 액세스 계층 사이에 있다.

#### 참고자료

[

DAO vs Repository Patterns | Baeldung

Understand the difference between the DAO and Repository patterns with a Java example.

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fwww.baeldung.com%2Fwp-content%2Fthemes%2Fbaeldung%2Ffavicon%2Fandroid-chrome-192x192.png&blockId=d79ea311-870b-4373-8aab-e8ee66c02bd5&width=32)

https://www.baeldung.com/java-dao-vs-repository

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fwww.baeldung.com%2Fwp-content%2Fuploads%2F2016%2F10%2Fsocial-Persistence-On-Baeldung-1.jpg&blockId=d79ea311-870b-4373-8aab-e8ee66c02bd5&width=512)





](https://www.baeldung.com/java-dao-vs-repository)

[

A Better Way to Project Domain Entities into DTOs

Imagine you have a nicely designed Domain Layer that uses Repositories to handle getting Domain Entities from your database with an ORM, e.g. Entity Framework, into an MVC view or a Web API controller. Problem is, the Presentation Layer needs objects of a different shape than your Domain Layer Aggregates. By different shape, I mean that this layer might need data combined from multiple Aggregates, or only portions of an Aggregate.

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fbuildplease.com%2Ffavicon.ico&blockId=78226a11-681a-408b-80f7-3afa0ea18770&width=32)

https://buildplease.com/pages/repositories-dto/

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fbuildplease.com%2Fimg%2FRetroTV.png&blockId=78226a11-681a-408b-80f7-3afa0ea18770&width=512)





](https://buildplease.com/pages/repositories-dto/)

[

DTO의 개념과 사용범위

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fhudi.blog%2Ficons%2Ficon-512x512.png%3Fv%3D783d202426de32b9eb1449c3fca1b518&blockId=bbd74ecc-8327-466d-b0ea-a48cbb224c61&width=32)

https://hudi.blog/data-transfer-object/

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fhudi.blog%2Fog-image.png&blockId=bbd74ecc-8327-466d-b0ea-a48cbb224c61&width=512)





](https://hudi.blog/data-transfer-object/)

[

Data access object

In software, a data access object (DAO) is a pattern that provides an abstract interface to some type of database or other persistence mechanism. By mapping application calls to the persistence layer, the DAO provides data operations without exposing database details. This isolation supports the single responsibility principle. It separates the data access the application needs, in terms of domain-specific objects and data types (the DAO's public interface), from how these needs can be satisfied with a specific DBMS (the implementation of the DAO).

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fen.wikipedia.org%2Fstatic%2Fapple-touch%2Fwikipedia.png&blockId=90e01319-8169-4152-932a-fc5737f1e437&width=32)

https://en.wikipedia.org/wiki/Data_access_object







](https://en.wikipedia.org/wiki/Data_access_object)

[

Data transfer object

In the field of programming a data transfer object (DTO&#91;1&#93;&#91;2&#93;) is an object that carries data between processes. The motivation for its use is that communication between processes is usually done resorting to remote interfaces (e.g., web services), where each call is an expensive operation.&#91;2&#93; Because the majority of the cost of each call is related to the round-trip time between the client and the server, one way of reducing the number of calls is to use an object (the DTO) that aggregates the data that would have been transferred by the several calls, but that is served by one call only.&#91;2&#93;

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fen.wikipedia.org%2Fstatic%2Fapple-touch%2Fwikipedia.png&blockId=255093b6-b6ee-4061-993f-746aff19e626&width=32)

https://en.wikipedia.org/wiki/Data_transfer_object







](https://en.wikipedia.org/wiki/Data_transfer_object)

[

Value object

In computer science, a value object is a small object that represents a simple entity whose equality is not based on identity: i.e. two value objects are equal when they have the same value, not necessarily being the same object.&#91;1&#93;&#91;2&#93;

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fen.wikipedia.org%2Fstatic%2Fapple-touch%2Fwikipedia.png&blockId=22c0e622-95cf-4007-9c1e-267a1e913811&width=32)

https://en.wikipedia.org/wiki/Value_object







](https://en.wikipedia.org/wiki/Value_object)

[

혼란스러운 도메인, 도메인 모델, 도메인 객체 용어 정리 😵‍💫

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fhudi.blog%2Ficons%2Ficon-512x512.png%3Fv%3D783d202426de32b9eb1449c3fca1b518&blockId=0ab71fb1-67ea-41b6-bfc2-126a727d5cf5&width=32)

https://hudi.blog/domain-domain-model-domain-object/

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fhudi.blog%2Fog-image.png&blockId=0ab71fb1-67ea-41b6-bfc2-126a727d5cf5&width=512)


](https://hudi.blog/domain-domain-model-domain-object/)


출처 https://80000coding.oopy.io/dcd0f224-053f-456c-af22-d7e0946fa868

https://devlopsquare.tistory.com/106