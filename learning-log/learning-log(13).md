## 싱글톤 패턴이 객체 지향에 위반이 된다는데? 왜?

싱글톤 패턴은 **객체의 인스턴스를 한개만 생성되게 하는 패턴으로, 메모리,데이터 공유 측면에서 이점이 있다. 하지만 그렇다고 해서 무조건 좋은 것이 아니다. 여러 클래스의 인스턴스에서 싱글톤 인스턴스의 데이터에 동시에 접근하게 되면 동시성 문제가 발생하기 때문 !** 

### 결론 : 싱글톤 패턴은 안티패턴으로 불릴 만큼 단독으로 사용한다면 객체 지향에 위반되는 사례가 많다. 스프링 컨테이너 같은 프레임워크의 도움을 받으면 싱글톤 패턴의 문제점들을 보완하면서 장점의 혜택을 누릴 수 있다. 실제로 스프링 빈은 컨테이너의 도움을 받아 싱글톤 스콥으로 관리되고 있다. 프레임워크 도움없이 싱글톤 패턴을 적용하고 싶다면, 위에서 살펴본 장단점의 trade-off를 잘 고려하여 사용하는 것이 좋다고 한다.

### 기초 구현 방식

```java
public class Settings{

  private static Settings instance;

  public static Settings getInstance(){
    if(instance == null){
      instance == new Settings();
    }

    return instance;
  }
}
```

멀티스레드 환경에서 동시성 문제가 발생하기 때문에 Thread-Safe환경을 보장하지 못함.

 

동시성 문제를 해결하기 위한 방법

### static초기화

아래는 자판기, 지하철 노선도 경로조회때 사용한 방식으로, 런타임 시가 아닌 최초 JVM이 Class Loader를 이용해서 class path 내에 있는 모든 클래스들을 로드할 때 미리 인스턴스를 생성해주는 방법이다.

```java
public class Settings{
  // 클래스 생성시 instance를 초기화해줌
  private static final Settings INSTANCE = new Settings();

  // if문 불필요
  public static Settings getInstance(){
    return instance;
  }
}
```

*App.java(main 메소드)*

```java
public class App{
  public static void main(Stirng[] args){
    Settings settings = Settings.getInstance();
  }
}
```

**위 방식의 문제점**

해당 클래스를 약 300초 뒤에 사용한다고 했을 때 이 static 초기화 방식을 사용하면 300초 뒤에 사용하나 3초 뒤에 사용하나 무조건 최초 클래스가 로딩될 때 만들어지기 때문에 아직 필요하지 않은 시점에도 불필요하게 메모리 자원을 선점하고 있게 된다.

심지어 사용하지 않게 되는 경우가 있을 수도 있는데 그런 경우 불필요한 자원 낭비가 될 수 있다.

### LazyHolder방식

```java
class Singleton {
	private Singleton() {}

	public static Singleton getInstance() {
	    return LazyHolder.INSTANCE;
	}
	
	private static class LazyHolder {
	private static final Singleton INSTANCE = new Singleton();
	}
}
```

위와 같이 `LazyHolder` 라는 Inner Class를 선언해서 사용함으로써,

`Singleton` 클래스가 최초 클래스 로딩 단계에서 로드가 되더라도 `LazyHolder` 클래스에 대한 변수를 가지고 있지 않아 함께 초기화되지 않는 점을 이용한 방법이다.

그렇기 때문에 `getInstance()`가 호출될 때 `LazyHolder` 클래스가 로딩되며 인스턴스를 생성하게 되는 것이다.

클래스를 로드하고 초기화하는 단계에서는 thread safety가 보장되기 때문에 별도의 `synchronized`, `volatile` 키워드 없이도 동시성 문제를 해결할 수 있는 아주 훌륭한 방법이라고 한다.

### 총평

싱글톤 패턴은 매번 new 연산을 통해 객체를 생성하지 않아 메모리를 효율적으로 사용할 수 있다. 또한 인스턴스가 static으로 선언되어 있기 때문에 클래스 간에 데이터 공유가 쉽다.

하지만 싱글톤 패턴을 구현하기 위해서 부가적인 코드를 작성해야 하고 멀티쓰레드 환경에서 오류가 발생하지 않게 synchronized를 사용하는 등의 방법으로 개발해야한다.

사실 싱글톤 패턴은 객체지향의 원칙을 위반할 가능성이 매우 높은 디자인 패턴이다. 싱글톤 패턴을 사용할 때는 trade-off를 고려하여 적절하게 사용해야한다.

- 참조 블로그
    - https://seunghyunson.tistory.com/28
    - https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/
    - [https://velog.io/@xeonu/싱글톤-패턴-이야기](https://velog.io/@xeonu/%EC%8B%B1%EA%B8%80%ED%86%A4-%ED%8C%A8%ED%84%B4-%EC%9D%B4%EC%95%BC%EA%B8%B0)
    - [https://velog.io/@seongwon97/싱글톤Singleton-패턴이란](https://velog.io/@seongwon97/%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80)
