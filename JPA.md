복합키 일 때 JPA 사용법
===================

@EmbeddedId

<pre>
<code>
@Entity
@Data
public class Parent {

  @EmbeddedId
  private ParentId id;

  private String name;

}

@Data
@Embeddable
public class ParentId implements Serializable {

  @Column(name = "PARENT_ID1")
  private String id1;

  @Column(name = "PARENT_ID2")
  private String id2;
}

public static void save(EntityManager entityManager) {
  Parent parent = new Parent();
  ParentId parentId = new ParentId();
  parentId.setId1("id1");
  parentId.setId2("id2");
  parent.setId(parentId);
  parent.setName("wonwoo");
  entityManager.persist(parent);
}

private static void find(EntityManager entityManager) {
  ParentId parentId = new ParentId();
  parentId.setId1("id1");
  parentId.setId2("id2");
  Parent parent = entityManager.find(Parent.class, parentId);
  System.out.println(parent);
}
</pre>
</code>

### @IdClass   
<pre>
<cdoe>
@Entity
@Data
@IdClass(ParentId.class)
public class Parent {

  @Id
  @Column(name = "PARENT_ID1")
  private String id1;

  @Id
  @Column(name = "PARENT_ID2")
  private String id2;

  private String name;
}

@Data
public class ParentId implements Serializable{

  private String id1;

  private String id2;
}

public static void save(EntityManager entityManager){
  Parent parent = new Parent();
  parent.setId1("id1");
  parent.setId2("id2");
  parent.setName("name");
  entityManager.persist(parent);
}


private static void find(EntityManager entityManager) {
  ParentId parentId = new ParentId();
  parentId.setId1("id1");
  parentId.setId2("id2");
  Parent parent = entityManager.find(Parent.class, parentId);
  System.out.println(parent);
}

@Entity
@Data
public class Child {

  @Id
  private String id;

  @ManyToOne
  @JoinColumns({
    @JoinColumn(name = "PARENT_ID1",referencedColumnName = "PARENT_ID1"),
    @JoinColumn(name = "PARENT_ID2",referencedColumnName = "PARENT_ID2")
  })
  private Parent parent;
}

</pre>
</code>

@IdClass를 사용할 때 식별자 클래스는 다음 조건을 만족해야 한다.
1. 식별자 클래스의 속성명과 엔티티에서 사용하는 식별자의 속성명이 같아야 한다. Parent.id1과 ParentId.id1, 그리고 Parent.id2과 ParentId.id2 가 같다.
2. Serializable를 구현해야 한다.
3. equals, hashCode를 구현해야 한다.
4. 기본 생성자가 있어야 한다.
5. 식별자 클래스는 public 이어야 한다.



### @IdClass vs @EmbeddedId
@IdClass 와 @EmbeddedId는 각각 장담점이 있으므로 본인의 취향에 맞는 것을 일관성 있게 사용하면 된다.   
@EmbeddedId가 @IdClass와 비교해서 더 객체지향적이고 중복도 없어서 좋아보이긴 하지만 특정 상황에 JPQL이 조금 더 길어 질 수 있다.    
복합 키에는 @GenerateValue를 사용 할 수 없다.


Specification
=============

<pre>
<code>
@Entity
public class Post {
    @Id @GeneratedValue
    private Long id;
    private String title;
    private String tag;
    private int likes;
...
}
public interface PostRepository extends JpaRepository<Post, Long> {
    List<Post> findAllByTitle(String title);
    List<Post> findAllByTag(String tag);
    List<Post> findAllByLikesGreaterThan(int likes);
}
@RestController
public class PostController {

    private final PostRepository postRepository;

    public PostController(PostRepository postRepository) {
        this.postRepository = postRepository;
    }

    @GetMapping("/post/list")
    public List<Post> getPostList(@RequestParam(required = false) String title,
                                  @RequestParam(required = false) String tag,
                                  @RequestParam(required = false) Integer likes) {
        if (title != null) {
            return postRepository.findByTitle(title);
        } else if (tag != null) {
            return postRepository.findByTag(tag);
        } else if (likes != null) {
            return postRepository.findByLikesGreaterThan(likes);
        } else {
            return postRepository.findAll();
        }
    }
}
</pre>
</code>
