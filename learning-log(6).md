## 정적 팩토리 메소드 권장 이유
- 정적 팩토리 메소드 : of, from 등 메소드 이름을 지정하고, 생성자 호출 방식이 아닌, 메서드 호출 방식으로 객체를 생성하는 것

### 정적 팩토리 메소드 장점 (5가지)

1. 이름을 가질 수 있다.
2. 호출 될 때마다 인스턴스를 새로 생성하지 않아도 된다.
3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.

### 1. 이름을 가질 수 있다.

기름이 없는 자동차, 기름을 가지고 자동차를 만드는 방식 → 명확히 할 수 있음.

**A. new를 이용한 예**

```java
public class Car {

    private final String name;
    private final int oil;

    public Car(String name, int oil) {
        this.name = name;
        this.oil = oil;
    }

    public Car(String name){
        this.name = name;
        this.oil = 0;
    }
}
```

```java
public static void main(String[] args) {
    Car fullOilCar = new Car("fullOilCar", 10);
    Car noOilCar = new Car("noOilCar");
}
```

**B. 정적 팩토리 메소드를 사용한 예**

```java
public class Car {

    private final String name;
    private final int oil;

    public static Car createCar(String name, int oil) {
        return new Car(name, oil);
    }

    public static Car createNoOilCar(String name) {
        return new Car(name, 0);
    }

    private Car(String name, int oil) {
        this.name = name;
        this.oil = oil;
    }
}
```

```java
public static void main(String[] args) {
	Car fullOilCar = createCar("car1", 10);
	Car noOilCar = createNoOilCar("car2");
}
```

### 2. 호출될때마다 객체를 새롭게 생성하지 않아도 된다.

**mathCount**에 따른 RANK 를 얻어오고 싶다면, `getRankByMatchCount` 함수처럼 진행해야될 것이고 이에 따른 시간복잡도가 O(N)이 된다. 종류가 많고, 조회가 빈번하다면, 시간이 그만큼 느려질 것이다. 

```jsx
public enum Rank {
    RANK1(5),
    RANK2(4),
    RANK3(3),
    RANK4(2),
    RANK5(1),
    FAIL(0);

    private final int matchCount;

    Rank(final int matchCount) {
        this.matchCount = matchCount;
    }

		public static Rank getRankByMatchCount(final int matchCount) {
        return Arrays.stream(values())
                .filter(rank -> rank.matchCount == matchCount)
                .findFirst()
                .orElse(FAIL);
    }
}
```

```
import java.util.HashMap;
import java.util.Map;

public enum Rank {
    RANK1(5),
    RANK2(4),
    RANK3(3),
    RANK4(2),
    RANK5(1),
    FAIL(0);

    private static final Map<Integer, Rank> RANK_MAP = new HashMap<>();

    static {
        for (Rank rank : values()) {
            RANK_MAP.put(rank.matchCount, rank);
        }
    }

    private final int matchCount;

    Rank(final int matchCount) {
        this.matchCount = matchCount;
    }

    public static Rank getRankByMatchCount(final int matchCount) {
        if(!RANK_MAP.containsKey(matchCount)){
            return Rank.FAIL;
        }
        return RANK_MAP.get(matchCount);
    }
}
```

HashMap을 이용해 미리 저장을 해둔다면(캐싱), 찾으려는 속성에 따른 Enum을 O(N) → O(1)로 단축할 수 있다.

### 3. 하위 자료형 객체를 반환할 수 있다.

```java
interface SmarPhone {}

class Galaxy implements SmarPhone {}
class IPhone implements SmarPhone {}
class Huawei implements SmarPhone {}

class SmartPhones {
    public static SmarPhone getSamsungPhone() {
        return new Galaxy();
    }

    public static SmarPhone getApplePhone() {
        return new IPhone();
    }

    public static SmarPhone getChinesePhone() {
        return new Huawei();
    }
}
```

### 4. 인자에 따라 다른 객체를 반환하도록 분기할 수 있다

```java
interface SmarPhone {
	public static SmarPhone getPhone(int price) {
		if(price > 100000) {
			return new IPhone();

		if(price > 50000) {
        return new Galaxy();
    }

		return new Huawei();
}
```

```java
interface Grade {
String toText();
}

class A implements Grade {
@Override
public String toText() {return "A";}
}

class B implements Grade {
@Override
public String toText() {return "B";}
}

class C implements Grade {
@Override
public String toText() {return "C";}
}

class D implements Grade {
@Override
public String toText() {return "D";}
}

class F implements Grade {
@Override
public String toText() {return "F";}
}
```

```java
class GradeCalculator {
	// 정적 팩토리 메서드
	public static Grade of(int score) {
		if (score >= 90) {
			return new A();
		} else if (score >= 80) {
			return new B();
		} else if (score >= 70) {
			return new C();
		} else if (score >= 60) {
			return new D();
		} else {
			return new F();
		}
	}
}
```

- 참조 블로그
    - [https://velog.io/@cjh8746/정적-팩토리-메서드Static-Factory-Method](https://velog.io/@cjh8746/%EC%A0%95%EC%A0%81-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9CStatic-Factory-Method)
    - https://unluckyjung.github.io/java/2021/09/26/Enum-Caching/
    - [https://inpa.tistory.com/entry/GOF-💠-정적-팩토리-메서드-생성자-대신-사용하자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%A0%95%EC%A0%81-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9C-%EC%83%9D%EC%84%B1%EC%9E%90-%EB%8C%80%EC%8B%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EC%9E%90)
