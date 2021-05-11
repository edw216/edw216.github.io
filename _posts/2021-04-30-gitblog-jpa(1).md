---
title:  "[JPA]JPA소개"
excerpt: 자바 ORM 표준 JPA(JAVA PERSISTENCE API) 프로그래밍 - 김영한 

categories:
  - JPA
tags:
  - [JPA, ORM]

toc: true
toc_sticky: true
 
date: 2021-04-30
last_modified_at: 2021-04-30
---

  > JPA 공부를 어떻게 하지? 하다가 김영한님의 JPA책을 추천받았다. 책이 두껍긴 하다.. 이것을 다 읽는다면 분명 얻는게 엄청 많을 거 같다. 책 내용을 기반으로 정리 하려고 한다. 

# 기존 JDBC API  
기존 자바 애플리케이션은 JDBC API를 사용하여 SQL을 데이터베이스에 전달한다.
(그림1)<br>

![JDBC API와 SQL](https://github.com/edw216/edw216.github.io/blob/master/assets/images/jpa/jpa_img1.png?raw=true)
<br>(그림 1)

기존 애플리케이션의 문제는 비즈니스 로직보다 SQL과 JDBC API를 작성하는데 더 오래 걸린다.<br>
그러다 SQL Mapper를 사용하면서 JDBC API 사용코드를 많이 줄이게 된다.<br>
그럼에도 여전히 CRUD(생성,조회,수정,삭제)용 SQL은 필드가 늘어나면 거기에 대한 SQL문을 반복해서 작성해야 했고 너무 비생산적인 문제를 느꼈다.<br>
그래서 객체와 관계형 데이터베이스 간의 차이를 중간에서 해결해 줄 수 있는 자바 진영의 ORM(Object-Relational Mapper) 표준기술인 JPA를 도입하게 되었다.<br>

먼저 JPA를 소개하기 전에 어떤 문제들이 있었는지 알아보자.<br><br>

# JPA 사용 이유 (문제점)

## 1. SQL을 직접 다룰 때 발생하는 문제점

### 1.1 CRUD의 무한 반복

다음과 같이 기존의 회원 객체와 객체와 데이터베이스의 데이터 접근 객체(DAO)가 있다.
MemberDAO의 find()메소드는 SQL문을 통해서 회원의 정보를 조회한다.

```java
public class Member{
    private String memberId;
    private String name;
    ...
}
```

```java
public class MemberDAO{
    public Member find(String memberId){...}
}
```

```sql
SELECT MEMBER_ID, NAME FROM MEMBER M WHERE MEMBER_ID = ?
```

회원의 정보를 저장하는 기능을 추가 한다고 하자. 그럼 다음과 같이 savd()메소드가 추가가 되고 메소드에 맞는 SQL문을 작성해야한다.

```java
public class MemberDAO{
    public Member find(String memberId){...}
    public void save(Member member){...} //추가

}
```

```sql
SELECT MEMBER_ID, NAME FROM MEMBER M WHERE MEMBER_ID = ?
INSERT INTO MEMBER(MEMBER_ID,NAME) VALUES (?,?)
```

이처럼 개발자는 애플리케이션과 데이터베이스 중간에서 변환 작업을 직접 해줘야 한다.
몇번의 작성으로 끝나는게 아니라 너무나도 많은 SQL과 JDBC API 작업을 반복해야하는 문제이다.<br><br><br>

### 1.2 SQL에 의존적인 개발

위 같이 회원 객체와 관리하는 MemberDAO를 완성했다. 그런데 여기서 회원에 대한 전화번호와 팀소속을 나타내는 필드가 회원 객체에 필요하다는 요청이 들어와서 다음과 같이 추가를 했다.

```java
public class Member{
    private String memberId;
    private String name;
    private String tel; //추가
    private Team team;  //추가
    ...
}
//추가된 팀
class Team{
    ...
    private String teamName;
    ...
}
```

```java
public class MemberDAO{
    public Member find(String memberId){...}
    public Member findWithTeam(String memberId){...} //추가
}
```

이때 팀과 메소드를 추가함으로써 작성했던 모든 SQL과 코드들을 변경을 해야하고 문제가 발생하면 DAO를 열어서 직접 SQL을 확인해야 원인을 알수가 있고 해결을 할 수가 있다.<br>
이처럼 문제가 발생하면 직접 SQL 문을 확인해야 하는 번거로움이 존재하고 SQL에 의존해야하는 문제들이 발생한다.<br><br>

SQL을 직접 다룰 때 발생하는 문제점 요약

- 진정한 의미의 계층 분할이 어렵다.
- 엔티티(모델링한 객체)를 신뢰할 수 없다.
- SQL에 의존적인 개발을 피하기 어렵다.<br><br>


### 1.3 문제해결

JPA를 사용하면 객체를 데이터베이스에 저장하고 관리 할 떄, 개발자가 직접 SQL을 작성하는 것이 아니라 JPA가 제공하는 API를 사용하면 된다.<br> 그러면 JPA가 SQL을 생성하여 데이터베이스에 전달한다.

조회 기능을 통해서 비교를 해보자.<br><br><br>

**조회 기능**

<u>직접 구현한 MemberDAO의 find() 조회 기능</u>

1. 회원 조회용 SQL작성 <br>

`SELECT MEMBER_ID,NAME FROM MEMBER M WHERE MEMBER_ID = 'hello';`

2. JDBC API를 사용해서 SQL을 실행 <br>

`ResultSet rs = stmt.executeQuery(sql);`

3. 조회 결과를 Member 객체로 매핑 <br>
   
`String memberId = rs.getStirng("MEMBER_ID");` <br>
`String name = rs.getString("NAME");` <br>
`Member member = new Member()` <br>
`member.setMemberId(memberId);` <br>
`member.setName(name);` <br>
<br>
다음 JPA의 CRUD API를 보자.<br><br>
<u>JPA를 사용한 조회 기능</u>

`String memberId = "hello";`<br>
`Member member = jpa.find(Member.class, memberId);`


jpa에서 제공하는 find()메소드를 통해서 적절한 SELECT SQL을 생성하여 데이터베이스에 전달하고 결과를 반환받는다.<br>
이처럼 SQL문을 개발자 대신 작성해서 실행해주는 것 이상의 기능들을 제공한다.


**저장 기능**

`jpa.persist(member);`

**수정 기능**

`Member member = jpa.find(Member.class, memeberId)`<br>
`member.setName("이름변경")`


**연관된 객체 조회**

`Member member = jpa.find(Member.class, memberId)`<br>
`Team team = member.getTeam()`


## 2. 패러다임의 불일치


객체와 관게형 데이터베이스는 지향하는 목적이 서로 다르므로 둘의 기능과 표현 벙법도 다르다. 이것을 객체와 관계형 데이터베이스의 **패러다임 불일치 문제**라 한다.

이러한 패러다임 불일치 문제는 개발자가 중간에서 많은 시간과 코드를 통해서 해결해야 한다.

### 2.1 상속

![객체 모델과 테이블 모델](https://github.com/edw216/edw216.github.io/blob/master/assets/images/jpa/jpa_img2.png?raw=true)
<br>(그림 2)

위 그림의 왼쪽은 객체, 오른쪽은 데이터베이스 테이블이다. <br>
객체는 상속이라는 기능을 가지고 있지만 데이터베이스는 상속과는 약간 다른 슈퍼타입과 서브타입의 관계를 사용한다.

JDBC API를 사용하여 코드를 완성하려면 부모 객체에서 부모 테이터만 꺼내서 ITEM용 INSERT SQL을 작성하고 자식 객체에서 자식 데이터만 ALBUM용 INSERT SQL을 작성해야한다. SQL을 작성하는 부분은 코드량이 만만치 않고 자식의 타입에 따라서 DTYPE도 저장을 해야한다.

조회하는 부분도 ITEM과 ALBUM테이블을 조인해서 그 결과를 ALBUM 객체를 생성해야 한다.<br>
이러한 과정은 모두 패러다임 불일치를 해결하려고 소모하는 비용이 된다.<br>


**JAP와 상속**

JPA는 상속과 관련된 패러다임 불일치 문제를 개발자를 대신해서 해결해준다.<br>
개발자는 자바 컬렉션에 데이터를 저장하듯이 JPA를 통해 객체를 저장만 하면된다.<br>

`jpa.persist(album)`<br>

jpa는 위와 같이 persist()메소드를 통해서 album객체를 저장하면 jpa는 ITEM과 ALBUM 테이블에 대한 INSERT SQL을 생성하여 실행한다.

조회 또한 jpa는 `Album album = jpa.find(Album.class, "id100");`<br>
find()메소드를 통해서 `SELECT I.*, A.* FROM ITEM I JOIN ALBUM A ON I.ITEM_ID = A.ITEM_ID` join 쿼리를 생성하여 데이터를 조회하고 객체를 반환받는다.<br>


상속 이외에도 객체를 테이블에 맞춰 모델링, 객체 그래프 탐색, 동일성 비교 등의 문제점들을 jpa는 해결을 해준다.



# JPA 소개
JPA(Java Persistence Api)는 자바 진영의 ORM(Object-Relational Mapping)기술 표준이다.<br>

ORM은 이름 그대로 객체와 관계형 데이터베이스를 매핑한다는 뜻이다.
ORM 프레임워크는 객체와 테이블을 매핑해서 패러다임 불일치 문제를 대신 해결해준다.

**JPA 동작**<br>

![](https://github.com/edw216/edw216.github.io/blob/master/assets/images/jpa/jpa_img3.png?raw=true)


JPA는 위 그림과 같이 애플리케이션과 JDBC 사이에서 동작한다.


![](https://github.com/edw216/edw216.github.io/blob/master/assets/images/jpa/jpa_img4.png?raw=true)

JPA 저장 


![](https://github.com/edw216/edw216.github.io/blob/master/assets/images/jpa/jpa_img5.png?raw=true)

JPA 조회 



