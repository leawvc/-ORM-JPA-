# JPA의 소개(section 1-2)

 [JPA의 소개(section 1)](https://www.notion.so/JPA-section-1-2ea1a07ee9374652826ce3d03b6de57e) 

<aside>
📖 **JPA란 JAVA ORM의 표준**이다. 그 말이란 즉 위의 포스팅에서 말했던 객체적으로 만든 데이터를 DB에 밀어넣을때 ORM 프레임워크인 JPA로 맵핑을 해주겠단 소리이다. 그렇기에 JPA에 대해서 알아보자

</aside>

### JPA의 동작 원리

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/86f2fd7a-b02a-4fd9-a24d-0b3b43ea6d70/Untitled.png)

예를 들어 JPA에게 MemberDAO에 값을 넣고자 member객체를 넘기게 된다면 JPA가 자동적으로 insert쿼리를 생성하고 DB에 보내고 값을 받게 된다. **JPA란 즉 원하는 동작에 맞는 쿼리를 자동적으로 생성해주는 것이다.**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7de596fe-4abf-45db-a18d-2408bd5406c7/Untitled.png)

---

### 그렇다면 JPA를 사용해야 하는 이유는 뭘까?

- 생산성 : 이미 코드가 다 만들어져있기 때문에 아래처럼 불러쓰면 된다
    1.  특히 수정 부분에서 set만을 사용하는 것만으로 수정이 된다. 컬렉션에서 사용한다고 생각하면 이해하기 편한데 **JAVA 컬렉션에서 값을 찾은 후 수정시에 다시 값을 넣지 않기 때문에** 이런 부분에서 편하다고 할 수 있다.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8309eb92-6a47-4fe5-bbe5-8f8844b6581c/Untitled.png)
    
- 유지 보수 : 쿼리 단을 생각할 필요가 없다!
    
    기본 적으로 연락처를 추가 시에 아래의 쿼리문을 수정해야 했다면 JPA를 사용하게 되면 JPA에서 자체적으로 쿼리문을 만들어 수행하기 때문에 SQL의 쿼리문을 수정 할 필요가 없게 된다.!
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/17557984-eb8a-4478-a7ac-c3e9afff0a64/Untitled.png)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/65501846-7469-48e9-94c7-f63c38ba6525/Untitled.png)
    
- 패러다임의 불일치 해결!
    
    기본 적으로 아래의 데이터 관계를 구상하고 직접 쿼리문을 짜야 했다면 이제는 JPA를 사용하게 편리하게 된다. 
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b54474d8-d456-4865-880e-cb55b19c1159/Untitled.png)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6f1e8844-4515-4f1d-b83f-34c0d8283855/Untitled.png)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e007b3e2-7d9e-4187-80d5-3098360ed082/Untitled.png)
    
    JPA를 이용해서 아래의 member 객체를 들고 온다면 JPA의 지연 로딩(구문이 실행되는 순간 작동하는 구문 예를 들어 member.getTeam이 시작되는 시점에 member객체에 쿼리문을 사용하여 데이터를 채워준다.)을 이용하여 아래의 구문을 신뢰 할 수 있다.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ea80cd41-55d9-4681-b819-ebbd5d0c1c94/Untitled.png)
    
- 성능 최적화
