---

title: QueryDSL - Spring Data JPA 와 결합하기
layout: post
description: QueryDSL Spring Data JPA 에서 사용하기
post-image: https://cdn.inflearn.com/public/courses/324476/course_cover/c712dd1a-80e3-413f-93af-ca89bddd6fe9/kyh_DSL2.png

tags:
- QueryDSL
- Spring Data JPA

---

# 현재 내가 Spring Data JPA를 사용하고 있고, QueryDSL을 도입하고싶은데..?

> 현재 사용하고 있는 Spring Data JPA의 레포지토리를 UserRepository 라고 한다면
> 
> QueryDSL을 사용하기 위해선, UserRepositoryCustom 과 같은 커스텀 레포지토리 인터페이스를 만들고
> 
> 이에 대한 구현체인 UserRepositoryImpl 을 만들어 구현체에 QueryDSL 동적쿼리 내용을 담으면 된다.

---

# 왜 이렇게 하는 것인가

> 기존 Spring Data JPA Repository에 QueryDSL을 작성하여 사용해도 된다. 당연히 안될건 아니다.
> 
> 하지만 이렇게 되면 **인터페이스를 쓰는 의미가 없어**진다. 
> 
> **인터페이스는 사용자에게 내부적인 로직을 감춰** 보다 **클린한 코드를 제공**할 수 있을 뿐만 아니라, **구현체의 내용이 바뀌어도 그 인터페이스를 상속받는 다른 클래스는 변화를 주지 않아도 된다.** (다형성)


---

# 코드로 예시를 들어보자

## Spring Data JPA에 QueryDSL을 적용하기 전의 레포지토리

### UserRepository.java
    public interface UserRepository extends JpaRepository<User, Long> {
    
        Optional<User> findByUserId(Long id);
    }

## Spring Data JPA에 QueryDSL을 적용한 후의 레포지토리
    
### UserRepositoryCustom.java

    public interface UserRepositoryCustom {
    
        List<UserRequestDto> search(UserSpec userSpec);
    }

> 1.QueryDSL 전용 인터페이스를 만든 후

<br>

### UserRepositoryCustomImpl.java

    @Repository
    @RequiredArgsConstructor
    public class MemberRepositoryImpl implements MemberRepositoryCustom {
    
        private final JPAQueryFactory queryFactory;

    public List<UserRequestDto> search(UserSpec userSpec) {
        return queryFactory
                .select(user.id)
                .from(user)
                .where(usernameEq(condition.getUsername())
                .fetch();
    }

> 2.QueryDSL 전용 인터페이스의 구현체를 만들고

<br>

### UserRepository.java
    public interface UserRepository extends JpaRepository<User, Long>, UserRepositoryCustom {
    
        Optional<User> findByUserId(Long id);
    }

> 3.기존 Spring Data JPA 레포지토리에 QueryDSL 전용 인터페이스를 상속받아 사용하면 된다.

---

# 결론

> 위와 같은 방법으로 QueryDSL을 적용하면, **사용자는 기존의 코드를 전혀 변경하지 않아도**
>
> **UserRepository에 대한 의존성 주입** 하나로 **QueryDSL의 동적쿼리**와 **Spring Data JPA의 정적쿼리**를 사용할 수 있게 된다.

# [!] 부연설명 - 정적쿼리? 동적쿼리?

> **정적쿼리란** Spring Data JPA 를 한 번씩 써봤다면 쉽게 이해할 수 있다.
> 
> **findByUserId** 와 같은 **네이밍 컨벤션으로 Spirng Data JPA 메서드 등록**을 하면 어떤 결과가 나오게 되는지 예상이 될 것이다.
> 
> 바로 **해당 유저에 대한 테이블의 모든 값을 가져와** List든 Optional 이든 반환을 하게 된다.
> 
> 이처럼 **특정 row 를 전부 가져오는 것이 정적 쿼리**라고 생각하면된다.

<br>

> 그러면 **동적쿼리는**? 만약에 유저 테이블에 id, username, pw, email 이 있다고 치자.
> 
> 나는 여기서 **id 가 1인 유저의 username만 가져오고 싶으면** 기존에 Spring Data JPA 에서는 어떻게 했는가?
> 
> 나같은 경우는 **@Query 어노테이션을 붙이고, native 쿼리를 직접 작성해왔었다.**
> 
> SELECT username FROM user WHERE id = 1;
> 
> 위는 해당 상황에 대한 쿼리 예시이다. 
> 
> 이처럼 **특정 row의 특정 column만 가져오는 것을 동적 쿼리**라고한다.
> 
> **Select 뿐만 아니라 Update나 Delete 도 마찬가지다.**

<br>

## QueryDSL이 동적쿼리에서 뛰어난 이유?

> 근데 **일일히 이렇게 Native쿼리를 짜는것은 코드도 길어질 뿐만 아니라** **무엇보다 생산성이 너무 떨어진다.**
> 
> 이 말이 **와닿지 않는다면, Native 쿼리로 10개 가까이 되는 테이블에 대한 동적쿼리를 작성**해본 후, **자신의 Repository 코드를 훑어보면 바로 느낄 것**이다.
> 
> 내가 짠 내용인데도 주석없이는 어떤 메서드인지 구별도 힘들다.
> 
> **QueryDSL은 이러한 상황에서, 더 좋은 방법을 제시해 주는 도구이다.**
> 
> **Java 문법으로 native Query를 작성할 수 있게 해주는 것으로**, Java 개발자인 우리에겐 아주 좋은 도구라고 본다.