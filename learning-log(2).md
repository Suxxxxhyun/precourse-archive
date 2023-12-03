### 결론 : getter를 사용하면 캡슐화가 깨진다. 따라서, getter를 지양해야하며 getter를 지양하기 위해서는 객체에 메시지를 전달하여 원하는 결과를 얻도록 한다. 또, 객체의 값을 외부로 표현해줘야하는 경우에는 getter 사용을 피할 수 없다.

- 캡슐화 : 객체지향 프로그래밍의 핵심원칙으로, 외부에서 객체 내부 상태에 직접 접근해서는 안된다고 명시한다. 이를 위해 접근제어자를 private으로 두고, getter/setter를 사용하라.

### 1. **getter지양 이유**

1) **getter를 사용함으로써 캡슐화가 깨진다고 한다.** 

**캡슐화는 외부에서 객체 내부에 어떤 속성이 있는지 완벽하게 알지 못하게 해야한다**는 것인데, 

**getter를 통해 객체의 특정필드가 있다는 사실을 외부에 노골적으로 공개하게 되기 때문이다.**

**2) getter를 사용함으로써 객체가 객체스럽지 못하게 된다.** 

객체지향 프로그래밍은 객체가 스스로 일을 하도록 하는 프로그래밍이다.

모든 멤버변수에 getter를 생성해 놓고 상태값을 꺼내 그 값으로 객체 외부에서 로직을 수행한다면, 객체가 로직(행동)을 갖고 있는 형태가 아니고 메시지를 주고 받는 형태도 아니게 된다. 또한, 객체 스스로 상태값을 변경하는 것이 아니고, 외부에서 상태값을 변경할 수 있는 위험성도 생길 수 있다.

따라서 이는 객체가 객체스럽지 못한 것이다.

**3) getter를 사용함으로써 디미터 법칙을 위반하게 된다.**

- 디미터 법칙 : 다른 객체가 어떠한 자료를 갖고 있는지 속사정을 몰라야 한다는 것을 의미

### 2. **getter지양 방법 - 객체에 메시지를 보내자**

A. getter를 무분별하게 사용한 코드

```
public class Main {
    public static void main(String[] args) {
        Number number = new Number(5);

        int value = number.getNumber();

        OutputView.printOverFive(value >= 5);
//      "5 이상입니다!"
        OutputView.printParsedNumber(value);
//      "-----" 출력
    }
}

// #OutputView
public static void printOverFive(boolean isOverFive) {
    if (isOverFive) {
        System.out.println("5 이상입니다!");
    }
    if(!isOverFive) {
        System.out.println("5 미만입니다!");
    }
}
```

B. 객체에 메시지를 보내 getter를 지양한 코드

```
public class Main {
    public static void main(String[] args) {
        Number number = new Number(5);

        int value = number.getNumber();

        OutputView.printOverFive(number.isOverFive());
//      "5 이상입니다!"
        OutputView.printParsedNumber(value);
//      "-----" 출력
    }
}
```

```fortran
public class Number {
    private int number;

    public Number(int number) {
        this.number = number;
    }
    public int getNumber() {
        return number;
    }
    public boolean isOverFive() {
        return number >= 5;
    }
}
```

### 3. **getter를 무조건 지양해야해?**

객체의 값을 외부로 표현(Presentation) 해 주어야 하는 경우에는 getter를 사용하는 것을 피할 수 없다. 출력을 위한 값 등 순수 값 프로퍼티를 가져오기 위해서라면 어느정도 getter는 허용된다고 한다.

- 참조블로그
    - (1) https://kong-dev.tistory.com/m/226
    - (2) https://mangkyu.tistory.com/147
    - (3) https://tecoble.techcourse.co.kr/post/2020-04-28-ask-instead-of-getter/
