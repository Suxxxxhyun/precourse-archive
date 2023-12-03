### 결론 : **일급 컬렉션**을 사용하면 **상태과 로직을 따로 관리**할 수 있기 때문에 로직이 사용되는 **클래스의 부담**을 줄일 수 있고, **중복코드**를 줄일 수 있다.

### 불변성을 보장하기 위해 **unmodifiable을 사용한다.**

### 1. 일급컬렉션이란?

**Collection을 Wrapping**하면서, **Wrapping한 Collection 외 다른 멤버 변수가 없는 상태**를 말한다.

### 2. 일급컬렉션 사용이유

```java
// GSConvenienceStore.class
public class GSConvenienceStore {
    // 편의점에는 여러 개의 아이스크림을 팔고 있을 것이다.
    private List<IceCream> iceCreams;

    public GSConvenienceStore(List<IceCream> iceCreams) {
        this.iceCreams = iceCreams;
    }
    ...
}

// IceCream.class
public class IceCream {
    private String name;
    ...
}
```

GS편의점에 아이스크림을 팔고 있고, 아이스크림이 10개밖에 판매하지 않는다고 가정하였을때, 이 같은 상황이 CU에서도 생긴다면? 

→ CUConvenienceStore에서도 생성해야할 것이다.

아이스크림뿐만 아니라 다른 과자, 껌 종류 등의 상품도 판매하며, 이들도 10개밖에 판매하지 않는다면, 검증로직이 방대해질 것이다.

따라서, 아이스크림을 일급컬렉션으로 만든다. 

```java
// IceCream.class
public class IceCreams {
    private List<IceCream> iceCreams;

    public IceCreams(List<IceCream> iceCreams) {
        validateSize(iceCreams)
        this.iceCreams = iceCreams
    }

    private void validateSize(List<IceCream> iceCreams) {
        if (iceCreams.size() >= 10) {
            new throw IllegalArgumentException("아이스크림은 10개 이상의 종류를 팔지않습니다.")
        }
    }

    public IceCream find(String name) {
        return iceCreams.stream()
            .filter(iceCream::isSameName)
            .findFirst()
            .orElseThrow(RuntimeException::new)
    }
    // ...
}
```

위처럼 한다면, GSConvenienceStore클래스가 아래처럼 바뀌게 되어, 편의점 class가 했던 역할을 아이스크림, 과자, 라면 등 각각에게 **위임**하여 **상태와 로직을 관리**할 것이다.

```java
// GSConvenienceStore.class
public class GSConvenienceStore {
    private IceCreams iceCreams;

    public GSConvenienceStore(IceCreams iceCreams) {
        this.iceCreams = iceCreams;
    }

    public IceCream find(String name) {
        return iceCreams.find(name);
    }
    // ...
}

// CUConvenienceStore.class
public class CUConvenienceStore {
    private IceCreams iceCreams;

    public CUConvenienceStore(IceCreams iceCreams) {
        this.iceCreams = iceCreams;
    }

    public IceCream find(String name) {
        return iceCreams.find(name);
    }
    // ...
}
```

### 3. 일급컬렉션 불변성

불변성을 하기 위한 방법으로 **unmodifiableList** 사용한다.

```java
public class Lotto {
    private final List<LottoNumber> lotto;

    public Lotto(List<LottoNumber> lotto) {
        this.lotto = new ArrayList<>(lotto);
    }

    public List<LottoNumber> getLotto() {
        return Collections.unmodifiableList(lotto);
    }
}
```

**unmodifiableList**를 사용하면 lotto는 불변이 되고, getter로 return해서 사용될 때 변경이 불가능하다.
