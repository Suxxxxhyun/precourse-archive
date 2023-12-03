### Enum 캐싱방법(정적 팩토리 메소드 내용 일 부 발췌)
**mathCount**에 따른 RANK 를 얻어오고 싶다면

```
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

- Enum 종류가 `N`개 있다고 했을때, 매번 `O(N)`의 복잡도의 연산을 수행하게 된다.
- 종류가 많다면, 그리고 조회가 빈번하다면 개선할 필요성이 보인다.

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

• 찾으려는 속성에 따른 Enum을 HahMap에 저장해놓고(캐싱), Enum을 찾아 반환하면 `O(N)`이었던 복잡도를 `O(1)`로 줄일 수 있게 된다.
