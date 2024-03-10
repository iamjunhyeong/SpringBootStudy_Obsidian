
#### 댓글 Entity
![[Pasted image 20240309205150.png]]

#### JPA Repository

CRUD 기능 뿐만 아니라 데이터 정렬 & 조회도 가
![[Pasted image 20240309205246.png]]

## 🟥 Comment

```
@Entity
@Getter
@AllArgsConstructor
@NoArgsConstructor
@ToString
public class Comment {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne // 해당 댓글  엔티티 여러개가, 하나의 Article에 연관된다!
    @JoinColumn(name = "article_id")    // FK 의 이름 "article_id"로 설정
    private Article article;

    @Column
    private String nickname;

    @Column
    private String body;
}
```

### @ManyToOne
다대일 관계 설정 어노테이션 

### @JoinColumn(name = "article_id")
FK 컬럼 이름을 다음과 같이 설정.

### @Param
parameter를 인식하지못할 때 사용


## 🟥 CommentRepository

### JpaRepository
Paging & Sorting methods
+ CrudRepository 기능보다 더 많은 기능지원

```
import com.example.springboot_hongpark.entity.Comment;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import java.util.List;
import org.springframework.data.repository.query.Param;

public interface CommentRepository extends JpaRepository<Comment, Long> {
    
    // 특정 게시글의 모든 댓글 조회
    @Query(value =
            "SELECT * " +
                    "FROM comment " +
                    "WHERE article_id = :articleId",
            nativeQuery = true)
    List<Comment> findByArticleId(@Param("articleId") Long articleId);
    
    // 특정 닉네임의 모든 댓글 조회
    List<Comment> findByNickname(@Param("nickname") String nickname);
}
```

## 🟥 Test
### @DisplayName
테스트 결과에 보여줄 이름 설정

### 특정 게시글의 모든 댓글 조회
```
   @Test
    @DisplayName("특정 게시글의 모든 댓글 조회")
    void findByArticleId() {
        /* Case 1 : 4번 게시글의 모든 댓글 조회 */
        {
            // 입력 데이터 준비
            Long articleId = 4L;

            // 실제 수행
            List<Comment> comments = commentRepository.findByArticleId(articleId);

            // 예상하기
            Article article = new Article(4L, "인생영화?", "댓글");
            Comment a = new Comment(1L, article, "Park", "대부");
            Comment b = new Comment(2L, article, "go", "대부");
            List<Comment> expected = Arrays.asList(a, b);

            // 검증
            assertEquals(expected.toString(), comments.toString(), "4번 글의 모든 댓글을 출력!");
        }

        /* Case 2 : 1번 게시글의 모든 댓글 조회 */
        {
            // 입력 데이터 준비
            Long articleId = 1L;

            // 실제 수행
            List<Comment> comments = commentRepository.findByArticleId(articleId);

            // 예상하기
            Article article = new Article(1L, "가", "1111");
            List<Comment> expected = Arrays.asList();

            // 검증
            assertEquals(expected.toString(), comments.toString(), "1번 글은 댓글이 없음.");
        }

        /* Case 3 : 9번 게시글의 모든 댓글 조회 */
        {
            // 입력 데이터 준비
            Long articleId = 1L;

            // 실제 수행
            List<Comment> comments = commentRepository.findByArticleId(articleId);

            // 예상하기
            Article article = new Article(1L, "가", "1111");
            List<Comment> expected = Arrays.asList();

            // 검증
            assertEquals(expected.toString(), comments.toString(), "9번 게시글이 없음.");
        }

        /* Case 4 : 9999번 게시글의 모든 댓글 조회 */
        {
            // 입력 데이터 준비
            Long articleId = 9999L;

            // 실제 수행
            List<Comment> comments = commentRepository.findByArticleId(articleId);

            // 예상하기
            Article article = new Article(1L, "가", "1111");
            List<Comment> expected = Arrays.asList();

            // 검증
            assertEquals(expected.toString(), comments.toString(), "9999번 게시글이 없음.");
        }

        /* Case 5 : -1번 게시글의 모든 댓글 조회 */
        {
            // 입력 데이터 준비
            Long articleId = -1L;

            // 실제 수행
            List<Comment> comments = commentRepository.findByArticleId(articleId);

            // 예상하기
            Article article = new Article(1L, "가", "1111");
            List<Comment> expected = Arrays.asList();

            // 검증
            assertEquals(expected.toString(), comments.toString(), "-1번 게시글이 없음.");
        }
}
```

### 특정 닉네임의 모든 댓글 조회
```
    @Test
    @DisplayName("특정 닉네임의 모든 댓글 조회")
    void findByNickname() {
        /* Case 1: Park의 모든 댓글 조회 */
        {
            // 입력 데이터 준비
            String nickname = "Park";

            // 실제 수행
            List<Comment> comments = commentRepository.findByNickname(nickname);

            // 예상하기
            Comment a = new Comment(1L, new Article(4L, "인생영화?", "댓글"), nickname, "대부");
            List<Comment> expected = Arrays.asList(a);

            // 검증
            assertEquals(expected.toString(), comments.toString(), "Park의 모든 댓글을 출력!");
        }
        /* Case 2: Kim의 모든 댓글 조회 */
        {
            // 입력 데이터 준비
            String nickname = "kim";

            // 실제 수행
            List<Comment> comments = commentRepository.findByNickname(nickname);

            // 예상하기
            Comment a = new Comment(3L, new Article(5L, "소울푸드?", "댓글 ㄱ"), nickname, "치킨");
            List<Comment> expected = Arrays.asList(a);

            // 검증
            assertEquals(expected.toString(), comments.toString(), "kim 모든 댓글을 출력!");
        }
        /* Case 3: null의 모든 댓글 조회 */
        {
            // 입력 데이터 준비
            String nickname = null;

            // 실제 수행
            List<Comment> comments = commentRepository.findByNickname(nickname);

            // 예상하기
            List<Comment> expected = Arrays.asList();

            // 검증
            assertEquals(expected.toString(), comments.toString(), "null 모든 댓글을 출력!");
        }
        /* Case 4: ""의 모든 댓글 조회 */
        {
            // 입력 데이터 준비
            String nickname = "";

            // 실제 수행
            List<Comment> comments = commentRepository.findByNickname(nickname);

            // 예상하기
            List<Comment> expected = Arrays.asList();

            // 검증
            assertEquals(expected.toString(), comments.toString(), " \" \" 모든 댓글을 출력!");
        }

        /* Case 5: i의 닉네임의 모든 댓글 조회 */
        {
            // 입력 데이터 준비
            String nickname = "i";

            // 실제 수행
            List<Comment> comments = commentRepository.findByNickname(nickname);

            // 예상하기
            List<Comment> expected = Arrays.asList();

            // 검증
            assertEquals(expected.toString(), comments.toString(), "i의 모든 댓글을 출력!");
        }
    }
```


