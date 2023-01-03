# JPA의 소개(section 1)

[JPA의 소개(section 1)](https://www.notion.so/JPA-section-1-2ea1a07ee9374652826ce3d03b6de57e) 

<aside>
📖 JPA를 사용하기 전에 어떻게 발전해왔고 왜 사용하는지를 알고 써야 하기에 정리해봅니다.

</aside>

지금은 객체를 관계형 DB에 넣어서 관리를 한다.

하지만 실상은 테이블 마다 SQL문법을 적어야 했기 때문에 맵핑을 하는 것에 상당량 시간을 허비하게 되었다.

```java
public class Memeber {
	privaet String memberId;
	privaet String name;
}

			INSERT INTO MEMBER(MEMBER_ID, NAME) VALUES
			SELECT MEMBER_ID, NAME FROM MEMBER M
			UPDATE MEMBER SET ... 

위와 같은 쿼리 식을 수행해야 하게 된다. 하지만 기획자가 연락처를 추가하게 
부탁을 하게 되는 경우 모든 쿼리를 다 수정하고 객체를 수정해야 하는 
비 효율적인 부분이 생기게 된다. 쿼리를 수정하는 과정에서 한 부분을
수정하지 않게 되면 코드에서 에러가 날 수 밖에 없게 된다.
```

<aside>
📖 관계형DB vs 객체

</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/892ef3e7-d36a-44fe-b7f2-09e7e7476507/Untitled.png)

- 관계형 DB
    
    상속이 없다.
    
    PK, FK로 필요한 데이터를 찾을 수 있다.
    
- 객체
    
    상속이 있다.
    
    서로 참조를 할 수 있다.
    

<aside>
📖 관계형DB와 객체 조회의 차이점

</aside>

🧐객체의 경우 INSERT시 ALBUM객체를 INSERT후 ITEM에도 INSERT해야 하게 된다. ALBUM을 조회 할 시에는 ITEM과 ALBUM을 JOIN한 후 조회 하는 번거로운 일을 하게 된다. 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e7457bd2-60c3-4dbb-baa0-436abaad3411/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d8c340a7-9b1c-474e-aaec-27f601bc71af/Untitled.png)

---

### 위와 같은 번거로운 일을 하지 않고 컬렉션의 관점에서 본다면? 아래의 코드처럼 아주 간단한 방식으로 조회를 할 수 있는 걸 볼 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ea1a6ea5-f710-40d7-94f5-0da4f7cc9598/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8e3acae4-128f-41ee-9e95-d38d3f6976e5/Untitled.png)

---

<aside>
📖 관계형DB와 객체의 연관관계

</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/54f211e0-0793-45a0-ae1b-fcc3649aa824/Untitled.png)

- [ ]  객체의 경우 참조로 이용해 찾아 갈 수 있지만 테이블의 경우 pk, fk의 값으로 join을 하여 참조 할 수 있다.
- [ ]  객체의 경우 Team에서 Member의 값을 참조하는 부분이 없기에 Team.getMemeber()같이 참조를 할 수 없다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c3471413-75d8-4e76-bf0d-47c4e32f80b4/Untitled.png)

- [ ]  테이블의 경우 TEAM에서 TEAM_ID가 있기에 MEMBER로 갈 수 있으며 MEMBER의TEAM_ID(FK)를 역 참조를 할 수 있다.

### 그래서 우리가 할 일은 아래처럼 테이블형 연관관계의 데이터를 객체dml 연관관계로 하는 것이다. 하지만 이렇게 객체 지향적으로 설계를 할 경우 조회시에 또 문제가 생긴다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a81e56e7-38f5-4d0e-bae9-19e3f699cba7/Untitled.png)

😶‍🌫️조회시 생성되는 문제 마지막 member.setTeam(team)으로 개발자가 직접 연관관계를 셋팅하게 된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e31bcae8-03e9-4e5f-9b5e-3581b7c91786/Untitled.png)

<aside>
📖 객체 지향적으로 컬렉션에 값을 저장한다고 생각하게 한다면 쉽게 변한다. 아래처럼 객체만을 볼 때에는 밑처럼 쉽게 되지만 DB에 넣게 되는 순간 위의 복잡한 쿼리를 직접 수행해야 하는 것이다.

</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/07022371-f6df-46ce-b022-f540035bca91/Untitled.png)

<aside>
📖 객체간의 원하는 데이터를 찾을때도 문제가 생긴다 아래와 같은 그림에서 본다면 Member에서 Delivery객체를 참조 할 수 있어야 하지만 실상은 다르다.

</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b410d8e0-4d67-4cbc-bad2-c1db488d3aa1/Untitled.png)

처음 시작하는 SQL에서 이미 범위가 결정되기 때문에 처음에 MEMBER와 TEAM을 합한 후 데이터를 넣고 그 다음 getOrder을 했을 시에 값이 모두 채워져있다면 값이 가져와지지만 그렇지 않다면 null값을 가져오게 된다. 이럴 경우 **엔티티의 신뢰 문제**가 생긴다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e4a6eb7d-9d4e-4c10-b65f-3be99b2e577b/Untitled.png)

memberDAO의 값이 어떻게 짜여졌는지 확인을 실제로 하기 전에는 사용할 수 없다. 왜냐하면 레이어드 아키텍쳐 구간에서 신뢰의 문제가 발생하기 때문이다.(그 다음 계층에서 신뢰를 할 수 있어야  사용 가능하기 때문에)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a3b0ab1d-9acc-4733-b8e7-d4ba66f3d773/Untitled.png)

아래와 같이 SQL적 문법에서 본다면 member1, member2는 각각 new Member를 생성하기 때문에 결과적으로 값이 달라진다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ac75d7a8-7ab3-440b-936f-45d1c605cf43/Untitled.png)

그렇지만 컬렉션의 관점에서 본다면 아래의 값은 같게 된다. 그렇기 때문에 객체를 컬렉션에 저장하듯이 DB에 저장하는 법을 고민하게 된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5beb36a1-ced7-4f92-8300-99ca9cb0b919/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f45e37ed-1366-44ea-8569-9022d2e121d3/Untitled.png)

<aside>
📖 **그렇기 때문에 나온 것이 바로 신이 내린 선물 JPA이다.**

</aside>
