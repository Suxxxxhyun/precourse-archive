## DTO 왜 사용해? Entity와 DTO를 왜 분리해?

### 요약 : DTO(Data Transfer Object)를 사용하는 이유는 크게 두가지로 요약할 수 있다.

### 1. 필요한 데이터만을 View에 전달하기 위해

### 2. View와 Domain간의 결합도를 낮추기 위해

### Entity와 DTO를 분리하는 이유는 관심사의 분리 및 역할 분리를 위해서이다.

### 1. DTO를 사용하는 이유

- **필요한 데이터만을 View에 전달하기 위해**

```java
public class User {

    public Long id;
    public String name;
    public String email;
    public String password; //외부에 노출되서는 안 될 정보
    public DetailInformation detailInformation; //외부에 노출되서는 안 될 정보

    //비즈니스 로직, getter, setter 등 생략
}
```

```java
@GetMapping
public ResponseEntity<User> showArticle(@PathVariable long id) {
    User user = userService.findById(id);
    return ResponseEntity.ok().body(user);
}
```

- Controller가 클라이언트의 요청에 대한 응답으로 Domain인 User를 넘겨주면 다음과 같은 문제점이 발생한다.
    - Model객체는 UI에서 사용하지 않을 불필요한 데이터(ex: 이메일)를 보유하고 있음.
    - User의 민감한 정보가 외부에 노출되는 보안문제가 발생함.

- **View와 Domain간의 결합도를 낮추기 위해**

클라이언트(프론트영역)은 User의 id와 name정보만 필요했다고 가정해보자. User라는 객체에 id, name필드만 있고, User라는 객체를 그대로 반환했다고 해보자. 클라이언트(프론트영역)는 아래 객체를 그대로 응답받았을 것이다.

```java
{
	id: 1,
	name: "박수현"
}
```

그런데, User객체에 password라는 필드가 추가되었다고 가정해보자.

```java
{
	id: 1,
	name: "박수현",
	password: "1234"
}
```

위 필드가 추가됨으로써, 클라이언트는 예상치 못한 응답값을 응답받았기 때문에, View에서 정보를 가공해야하는 이슈가 발생한다.

### 2. ****'Entity와 DTO를 분리하는 이유는 DB와 View 사이의 역할 분리를 위해서'****

DTO(Data Transfer Object)는 Entity 객체와 달리 각 계층끼리 주고받는 우편물이나 상자의 개념입니다. 순수하게 데이터를 담고 있다는 점에서 Entity 객체와 유사하지만, 목적 자체가 전달이므로 읽고, 쓰는 것이 모두 가능하고, 일회성으로 사용되는 성격이 강합니다.

특히 JPA를 이용하게 되면 Entity 객체는 단순히 데이터를 담는 객체가 아니라 실제 데이터베이스와 관련된 중요한 역할을 하며, 내부적으로 EM(EntityManager)에 의해 관리되는 객체라는 것을 알 수 있습니다.

또한 DTO가 일회성으로 데이터를 주고받는 용도로 사용되는 것과 다르게 Entity의 생명주기(Life Cycle)도 전혀 다르다는 점도 분리해야 하는 이유가 됩니다.

그리고 테이블에 매핑되는 정보와 실제 View에서 요청되는 정보가 다를 경우 테이블에 필요한 정보에 맞게 데이터를 변환하는 로직이 필요할 수 있는데, 해당 로직이 Entity에 들어가게 되는 것은 일반적으로 생각해도 깔끔하지 못합니다.

DB로부터 조회된 Entity를 그대로 View로 넘기게 되었을 때 불필요한 정보 및 노출되면 안 되는 정보까지 노출될 수 있고, 이를 막기 위한 로직을 따로 구현해야 한다는 점도 이유가 됩니다.

- 참조 블로그
    - https://tecoble.techcourse.co.kr/post/2021-04-25-dto-layer-scope/
    - https://blog.naver.com/PostView.naver?blogId=jscode-pro&logNo=222954300331
    - https://wildeveloperetrain.tistory.com/101
