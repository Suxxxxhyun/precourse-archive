## DTO변환 위치 : Controller일까? Service일까? 

### 결론 : 정답이 없다.

PostRequest는 toEntity()를 통해 PostRequest(DTO) → Entity로 변환이 가능하다.

```java
@Getter
@Builder
@AllArgsConstructor(access = AccessLevel.PROTECTED)
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class PostRequest {

    private String title;
    private String content;
    private String writer;

    public Post toEntity() {
        return Post.builder()
                .title(title)
                .content(content)
                .writer(writer)
                .build();
    }
}
```

PostReponse에서는 Builder를 통해 Entity → DTO로 변환이 가능하다.

```java
@Getter
@AllArgsConstructor(access = AccessLevel.PROTECTED)
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class PostResponse {

    private Long id;
    private String title;
    private String content;
    private String writer;

    @Builder
    public PostResponse(Post post) {
        this.id = post.getId();
        this.title = post.getTitle();
        this.content = post.getContent();
        this.writer = post.getWriter();
    }
}
```

### 위 Entity ↔ DTO간의 변환작업을 Controller에서 해야할까? Service에서 해야할까?

### A. Entity ↔ DTO간의 변환작업을 ArticleController에서 해준 경우

- ArticleController

```java
@PostMapping
public ResponseEntity<ArticleResponseDto> createArticle(@RequestBody ArticleRequestDto articleRequestDto) {
    //로직 생략
    Article article = articleRequestDto.toEntity();
    Article savedArticle = articleService.createArticle(article);
    ArticleResponseDto articleResponseDto = ArticleResponseDto.from(savedArticle);
    return ResponseEntity.ok().body(articleResponseDto);
}
```

- ArticleService

```java
public Article createArticle(Article article) {
    //로직 생략
    return articleRepository.save(article);
}
```

### B. Entity ↔ DTO간의 변환작업을 ArticleService에서 해준 경우

- ArticleService

```java
public ArticleDto createArticle(ArticleDto articleRequestDto) {
    Article article = articleRequestDto.toEntity();
    //로직 생략
    return ArticleDto.from(articleRepository.save(article));
}
```

A Case 문제점 

- **비즈니스 로직과 혼재**
    - `Controller`가 여러 `Domain`객체들의 정보를 조합해서 `DTO`를 생성해야 하는 경우, 결국 `Service`(응용 계층) 로직이 `Controller`에 포함되게 됩니다.
    - 여러 `Domain` 객체들을 조회해야 하기 때문에 하나의 `Controller`가 의존하는 `Service`의 개수가 비대해집니다.

B Case문제점

- **Service의 복잡도 증가**: `Service`에 변환 작업까지 포함되면 역할이 복잡해질 수 있다.
- **코드 재사용성 하락**: `Service`에서 특정 `DTO`에 의존하게 되면 여러 종류의 `Controller`에서 해당 `Service`를 이용할 수 없어 코드 재사용성이 떨어지게 된다.

- 참조 블로그
    - [https://velog.io/@smc2315/DTO-Entity-변환-메서드-구현-위치는-어디가-좋을까](https://velog.io/@smc2315/DTO-Entity-%EB%B3%80%ED%99%98-%EB%A9%94%EC%84%9C%EB%93%9C-%EA%B5%AC%ED%98%84-%EC%9C%84%EC%B9%98%EB%8A%94-%EC%96%B4%EB%94%94%EA%B0%80-%EC%A2%8B%EC%9D%84%EA%B9%8C)
    - https://blog.naver.com/PostView.naver?blogId=jscode-pro&logNo=222954300331
    - https://tecoble.techcourse.co.kr/post/2021-04-25-dto-layer-scope/
