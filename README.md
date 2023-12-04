## 2023.10.19 ~ 2023.12.
| project | repository |
| --- | --- |
| 숫자야구 | [java-base-ball-6](https://github.com/woowacourse-precourse/java-baseball-6/pull/286) |
| 자동차 경주 | [java-racingcar-6](https://github.com/woowacourse-precourse/java-racingcar-6/pull/63) (1차)<br>[java-racingcar-6](https://github.com/woowacourse-precourse/java-racingcar-6/pull/2390) (2차)  |
| 로또 | [java-lotto-6](https://github.com/woowacourse-precourse/java-lotto-6/pull/206) (1차)<br>[java-lotto-6](https://github.com/woowacourse-precourse/java-lotto-6/pull/2105) (2차) |
| 크리스마스 프로모션 | [java-christmas-6](https://github.com/Suxxxxhyun/java-christmas-6-Suxxxxhyun/pull/1) |
| 자판기 | [java-vendingmachine-precourse](https://github.com/woowacourse/java-vendingmachine-precourse/pull/188) |
| 지하철 노선도 경로 조회 | [java-subway-path-precourse](https://github.com/woowacourse/java-subway-path-precourse/pull/117) |
| 다리 건너기 |  |

## 📄 공통 피드백 모음

[1주차 공통 피드백](https://github.com/Suxxxxhyun/precourse-archive/blob/main/common-feedback/common-feedback-week-1.md)

[2주차 공통 피드백](https://github.com/Suxxxxhyun/precourse-archive/blob/main/common-feedback/common-feedback-week-2.md)
  - 예외사항도 Read.me에 적기
  - 함수의 길이를 15라인이 넘지 않도록 하기

[3주차 공통 피드백](https://github.com/Suxxxxhyun/precourse-archive/blob/main/common-feedback/common-feedback-week-3.md)
  - 연관성이 있는 상수는 static final 대신 Enum을 활용하기
  - getter지양 [Getter지양 방법](https://tecoble.techcourse.co.kr/post/2020-04-28-ask-instead-of-getter/)
  - 객체의 필드의 수를 줄이기 위해 노력하기
  - 테스트코드도 리팩토링을 하기, 파라미터의 값만 바뀌는 경우라면 **@ValueSource**이용하기
  - priavte함수를 테스트하고 싶다면 클래스 분리하기
  - 단위테스트하기 어려운 코드를 단위테스트하기
## 📎 꼭 짚고 넘어가야할 학습자료 

- [JUnit과 AssertJ 사용법](https://techcourse-storage.s3.ap-northeast-2.amazonaws.com/9b82d8a360c548fcadd14c551dbcbe06)
- [Getter지양 방법](https://tecoble.techcourse.co.kr/post/2020-04-28-ask-instead-of-getter/)
- [메서드 시그니처를 수정하여 테스트하기 좋은 메서드로 만들기](https://tecoble.techcourse.co.kr/post/2020-05-07-appropriate_method_for_test_by_parameter/)


## 📒 학습 로그

- [입력값 검증을 InputView에서 할까? VS Domain에서 할까?](https://github.com/Suxxxxhyun/precourse-archive/blob/main/learning-log/learning-log(1).md)
- [getter를 지양해야하는 이유 및 지양 방법, 또 무조건 지양해야해?](https://github.com/Suxxxxhyun/precourse-archive/blob/main/learning-log/learning-log(2).md)
  - 캡슐화 : **외부에서 객체 내부에 어떤 속성이 있는지 완벽하게 알지 못하게 해야한다.**
  - 디미터 법칙 : **다른 객체가 어떠한 자료를 갖고 있는지 속사정을 몰라야 한다는 것을 의미**
- [HashSet 중복여부 판별 재정의](https://github.com/Suxxxxhyun/precourse-archive/blob/main/learning-log/learning-log(3).md)
- [전역에러를 처리하는 ControllerAdvice, RestControllerAdvice](https://github.com/Suxxxxhyun/precourse-archive/blob/main/learning-log/learning-log(4).md)
- [일급컬렉션 사용 이유와 불변성 보장 방법](https://github.com/Suxxxxhyun/precourse-archive/blob/main/learning-log/learning-log(5).md)
  - 일급컬레션 : **Collection을 Wrapping하면서, Wrapping한 Collection 외 다른 멤버 변수가 없는 상태**
- [정적 팩토리 메소드 권장 이유](https://github.com/Suxxxxhyun/precourse-archive/blob/main/learning-log/learning-log(6).md)
  - 정적 팩토리 메소드 : **of, from 등 메소드 이름을 지정하고, 생성자 호출 방식이 아닌, 메서드 호출 방식으로 객체를 생성하는 것**
- [상속을 자제하고 합성을 이용하자](https://github.com/Suxxxxhyun/precourse-archive/blob/main/learning-log/learning-log(7).md)
  - IS-A 관계 : **일반적인 개념과 구체적인 개념의 관계**
    
    > 예시
    > 
    > 
    > 사람은 동물이다.
    > 
    > 소는 동물이다
    > 
    > 새는 동물이다.
    > 
    
    즉, 일반 클래스를 구체화 하는 상황에서 상속을 사용한다.
    
- HAS-A 관계 : **일반적인 포함 개념의 관계**
    - 과목 클래스를 포함하는 학생 클래스의 경우 과목 클래스의 코드를 재사용 하기 위해 상속을 사용하지는 않는다.
- [Enum 캐싱(정적 팩토리 메소드 내용 일부 발췌)](https://github.com/Suxxxxhyun/precourse-archive/blob/main/learning-log/learning-log(8).md)
- [템플릿 콜백 패턴](https://github.com/Suxxxxhyun/precourse-archive/blob/main/learning-log/learning-log(9).md)
- [EnumMap이 HashMap보다 성능이 좋은 이유](https://github.com/Suxxxxhyun/precourse-archive/blob/main/learning-log/learning-log(10).md)
  - EnumMap : **Map의 구현체로, key값으로 Enum이 들어가야하는 구조**
- [DTO 왜 사용해? Entity와 DTO를 왜 분리해?](https://github.com/Suxxxxhyun/precourse-archive/blob/main/learning-log/learning-log(11).md)
- [DTO변환 위치 : Controller일까? Service일까?](https://github.com/Suxxxxhyun/precourse-archive/blob/main/learning-log/learning-log(12).md)
- [싱글톤 패턴이 객체 지향에 위반이 된다는데? 왜?](https://github.com/Suxxxxhyun/precourse-archive/blob/main/learning-log/learning-log(13).md)
  - 싱글톤 패턴 : **객체의 인스턴스를 한개만 생성되게 하는 패턴**
