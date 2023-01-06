# Transactional(section 2)

<aside>
📖 Transactional의 이해

</aside>

```java
package hellojpa; 
import javax.persistence.Entity; 
import javax.persistence.Id; 
@Entity 
@Getter
@Setter
public class Member { 
	@Id 
	private Long id; 
	private String name;  
}
```

<aside>
📖 기본 적인 JPA의 실행 동작원리는 아래와 같은 그림으로 작동된다. Persistence라는 클래스에서 시작을 한 후 설정 정보를 조회 한 후 EntityManagerFactory를 생성한후 동작이 있을때마다 EntityManager를 생성한다. 먼저 JPA를 이용하여 Entity를 가져오게 되면 Transactional을 커밋하는 시점에 체크 한 후 변경점이 있다면 uptade커밋을 만들어 날리고 Transactional을 커밋한다.

</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/593c3afa-470d-4a1a-91e9-fdebc4cd777b/Untitled.png)

# JPA 작동 방식

- [ ]  엔티티 매니저 팩토리를 생성시에 DB연결이 된다.
- [ ]  엔티티 매니저 팩토리는 어플리케이션 로딩 시점에 하나만 생성해야 한다.
- [ ]  엔티티 매니저는 하나의 Transactional(db connection이나 db의 변경점이 생길때마다)을 진행시에 하나씩 만들어야 한다.
- [ ]  그리고 Transactional을 실행 후 값을 진행한다.
- [ ]  제일 중요한 것은 엔티티 매니저는 자바 컬렉션으로 생각하면 된다. 내 자바 컬렉션을 대신 저장해주는 역활을 해준다.

```java
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

//        DB의 커넥션을 하나 얻은 것이다.
        EntityManager em = emf.createEntityManager();
//        Transaction 시작
        EntityTransaction tx = em.getTransaction();
        tx.begin();

        Member member = new Member();
        member.setId(1L);
        member.setName("helloA");
//      JPA저장
        em.persist(member);
//      commit
        tx.commit();

        em.close();
        emf.close();
    }
}
```

<aside>
💡 출력값은 아래와 같으며 DB를 조회 하게 되면 아래와 같이 쿼리문을 작성하지 않았음에도 실제로 데이터들이 DB에 저장되게 된다.

</aside>

```python
<property name="hibernate.show_sql" value="true"/> : 쿼리의 값이 실제로 출력이 되게 하는 구문
<property name="hibernate.format_sql" value="true"/> : 쿼리를 보기 쉽게 포멧팅한 구문
<property name="hibernate.use_sql_comments" value="true"/> : 쿼리가 나온 이유를 알려주는 구문 주석 처리 된 부분
Hibernate: 
    /* insert hellojpa.Member
        */ insert 
        into
            Member
            (name, id) 
        values
            (?, ?)
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a0542658-07af-44cb-a45b-67ffd1b3fed8/Untitled.png)

---

### 하지만 이럴 경우 예외 상황에 대처 할 수 없기에 아래와 같은 코드로 구현하는 것이 정석이다.

```java
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

public class JpaMain {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

//        DB의 커넥션을 하나 얻은 것이다.
        EntityManager em = emf.createEntityManager();
//        Transaction 시작
        EntityTransaction tx = em.getTransaction();
        tx.begin();
        try {
            Member member = new Member();
            member.setId(2L);
            member.setName("helloB");
//      JPA저장
            em.persist(member);
//      commit
            tx.commit();
        } catch (Exception e){
            tx.rollback();
        } finally {
//            EntityManager가 DB커넥션을 물고 동작하기에 사용을 하고 닫아줘야 한다.
            em.close();
        }
        emf.close();
    }
}
```

- 회원 수정
    
    ```java
    package hellojpa;
    
    import javax.persistence.EntityManager;
    import javax.persistence.EntityManagerFactory;
    import javax.persistence.EntityTransaction;
    import javax.persistence.Persistence;
    
    public class JpaMain {
        public static void main(String[] args) {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
    
    //        DB의 커넥션을 하나 얻은 것이다.
            EntityManager em = emf.createEntityManager();
    //        Transaction 시작
            EntityTransaction tx = em.getTransaction();
            tx.begin();
            try {
                Member findMember = em.find(Member.class, 1L);
                System.out.println(findMember.getId());
                System.out.println(findMember.getName());
    //      commit
                tx.commit();
            } catch (Exception e){
                tx.rollback();
            } finally {
    //            EntityManager가 DB커넥션을 물고 동작하기에 사용을 하고 닫아줘야 한다.
                em.close();
            }
            emf.close();
        }
    }
    ```
    
    <aside>
    💡 출력값 : 조회시에 select쿼리가 자동적으로 날아가며 값을 들고 오는 것을 확인 할 수 있다.
    
    </aside>
    
    ```java
    Hibernate: 
        select
            member0_.id as id1_0_0_,
            member0_.name as name2_0_0_ 
        from
            Member member0_ 
        where
            member0_.id=?
    1
    helloA
    ```
    
- 회원 삭제
    
    ```java
    package hellojpa;
    
    import javax.persistence.EntityManager;
    import javax.persistence.EntityManagerFactory;
    import javax.persistence.EntityTransaction;
    import javax.persistence.Persistence;
    
    public class JpaMain {
        public static void main(String[] args) {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
    
    //        DB의 커넥션을 하나 얻은 것이다.
            EntityManager em = emf.createEntityManager();
    //        Transaction 시작
            EntityTransaction tx = em.getTransaction();
            tx.begin();
            try {
                Member findMember = em.find(Member.class, 1L);
                
    						em.remove(findMember);
    //      commit
                tx.commit();
            } catch (Exception e){
                tx.rollback();
            } finally {
    //            EntityManager가 DB커넥션을 물고 동작하기에 사용을 하고 닫아줘야 한다.
                em.close();
            }
            emf.close();
        }
    }
    ```
    
- 회원 수정
    
    ```java
    package hellojpa;
    
    import javax.persistence.EntityManager;
    import javax.persistence.EntityManagerFactory;
    import javax.persistence.EntityTransaction;
    import javax.persistence.Persistence;
    
    public class JpaMain {
        public static void main(String[] args) {
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
    
    //        DB의 커넥션을 하나 얻은 것이다.
            EntityManager em = emf.createEntityManager();
    //        Transaction 시작
            EntityTransaction tx = em.getTransaction();
            tx.begin();
            try {
                Member findMember = em.find(Member.class, 1L);
                findMember.setName("helloJPA");
                
                //      commit
                tx.commit();
            } catch (Exception e){
                tx.rollback();
            } finally {
    //            EntityManager가 DB커넥션을 물고 동작하기에 사용을 하고 닫아줘야 한다.
                em.close();
            }
            emf.close();
        }
    }
    ```
    
    <aside>
    💡 출력 값은 아래와 같이 나오며 컬렉션처럼 활용을 하기에 저장하는 메서드는 필요가 없게 된다. JPA를 통해 Entity를 들고 오게 되면 JPA가 Transactional을 커밋하는 시점에 변경 사항이 있는지 체크하게 된다. 변경점이 있을 시에 자동적으로 커밋하기 직전에 UPDATE쿼리를 만들어서 사용한다.
    
    </aside>
    
    ```java
    Hibernate: 
        select
            member0_.id as id1_0_0_,
            member0_.name as name2_0_0_ 
        from
            Member member0_ 
        where
            member0_.id=?
    Hibernate: 
        /* update
            hellojpa.Member */ update
                Member 
            set
                name=? 
            where
                id=?
    ```
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e82ad421-2e40-48dd-9b95-fa726090fb2d/Untitled.png)
    

## 주의 사항

1. 웹 서버가 올라오는 시점 DB당 *`EntityManagerFactory`*  는 하나만 생성이 된다.
2. *`EntityManager`* 는 고객의 요청이 올 때마다 생성 되었다가 삭제되는 방식을 사용한다.
3. *`EntityManager`* 는 쓰레드마다 공유 할 수 없고 사용하고 버려야 한다.
4. JPA의 모든 데이터의 변경은 *`Transaction`* 안에서만 실행된다.

<aside>
💡 JPQL : 많은 Table중에서 내가 원하는 데이터들을 어떻게 조회 할 것인지를 해결하게 해주는 방식

</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dc4bfbb4-44f0-4c42-bfa6-18da0e24655e/Untitled.png)

1. 검색 쿼리의 경우 데이터를 검색을 해서 들고 와야 한다. 하지만 테이블을 검색시에 문법이 어긋나므로 엔티티 객체를 대상으로 검색을 해야 한다.
2. 검색 조건이 포함된 SQL이 필요한데 실제 물리적인 테이블인 RDB에 쿼리를 날린다면 해당 디비에 종속적인 설계가 되버린다. 그렇기에 엔티티 객체를 대상으로 사용할 수 있는 쿼리가 제공된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/66932ce9-a52f-4abf-901c-31c5ed844229/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c83f0cc7-c06b-4abe-9448-d43ab49e515c/Untitled.png)

<aside>
💡 실행시에 현재 데이터에 맞는 적절한 쿼리문이 생성된다.

</aside>

## 🗒️핵심 내용

1. 모든 변경은 *`Transaction`* 안에서 실행되며 조회 같은 데이터의 값을 변경하지 않는 메서드는 *`Transaction`* 을 선언하지 않아도 잘 작동한다.
2. *`Transaction`* commit을 하지 않는다면 반영이 되지 않는다.
3. *`EntityManager`* 는 하나의 동작이 끝날시 닫고 다른 동작이 발생시 자동적으로 생성된다.
4. WAS가 내려갈 경우 *`ManagerFactory`* 를 닫아줘야만 커넥션풀링 같은 부분의 리소스가 해체되게 된다.
