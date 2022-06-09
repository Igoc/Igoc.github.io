---
title: "Spring 자바 기반 설정"

categories: [Framework, Spring]
tags: [Spring, Spring Boot, Spring Configuration, Spring Java Based Configuration]

date: 2022-06-09T21:22:00+09:00
last_modified_at: 2022-06-09T21:22:00+09:00
---

Spring 자바 기반 설정에 대한 정리
{:.notice--primary}

## @Configuration

`@Configuration` 어노테이션이 적용된 클래스 안에 `@Bean` 어노테이션이 적용된 메서드를 만듦으로써, 스프링 컨테이너에 직접 스프링 빈을 등록할 수 있다. 해당 방식은 정형화되지 않은 클래스를 스프링 빈으로 등록해야 하거나, 상황에 따라 구체 클래스를 변경해야 하는 경우에 주로 사용된다.

``` java
@Configuration
public class SpringConfig {

    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
}
```

### 의존 관계 설정

`@Bean` 어노테이션이 적용된 다른 메서드를 직접 호출함으로써 의존 관계를 설정할 수 있다.

``` java
@Configuration
public class SpringConfig {

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
}
```

## 참고 자료

- [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)