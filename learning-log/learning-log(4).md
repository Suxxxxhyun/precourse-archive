## 전역에러를 처리하는 ControllerAdvice, RestControllerAdvice
- Spring은 전역적으로 ExceptionHandler를 적용할 수 있는 `@ControllerAdvice`와 `@RestControllerAdvice` 어노테이션을 제공하고 있다.
- `@ControllerAdivce`는 여러 컨트롤러에 대해 전역적으로 ExceptionHandler를 적용해준다. 다음과 같이 에러를 핸들링하는 클래스를 만들어 어노테이션을 붙여주면 에러 처리를 위임할 수 있다.

**@RestControllAdvice와 @ControllAdvice의 차이는?**

차이는 크게 없다. @Controller와 @RestController 와 동일하게 @ResponseBody의 유무 차이만 있을 뿐이다. 그러므로 결과 타입을 Json으로 내려줄지 아니면 View 페이지로 전달할지에 따라서 사용하면 되겠다.

위를 적용하기 위해 자동차 경주(2차)때 아래처럼 `@ControllerAdvice` 를 대신할 GlobalException을 만들어 사용해보았다. 하지만, 5시간내에 구현해야하는 타임어택이라는 점을 감안하여 그냥 `DuplicateException`, `ConsoleException` 등 `IllegalArgumentException` 을 상속받도록 수정하였다. 

```jsx
public class GlobalException extends RuntimeException{
    public GlobalException(final String message) {
        super(message);
    }
}
```

```jsx
public class DuplicateException extends GlobalException{
    public DuplicateException(final String message) {
        super(message);
    }
}
```

```jsx
public class ConsoleException extends GlobalException{
    public ConsoleException(final String message) {
        super(message);
    }
}
```

- 참조 블로그
    - https://incheol-jung.gitbook.io/docs/q-and-a/spring/controlleradvice-exceptionhandler
    - [https://velog.io/@u-nij/Spring-전역-예외-처리-RestControllerAdivce-적용](https://velog.io/@u-nij/Spring-%EC%A0%84%EC%97%AD-%EC%98%88%EC%99%B8-%EC%B2%98%EB%A6%AC-RestControllerAdivce-%EC%A0%81%EC%9A%A9)
