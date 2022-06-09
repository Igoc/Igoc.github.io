---
title: "Spring 의존성 주입(Dependency Injection)"

categories: [Framework, Spring]
tags: [Spring, Spring Boot, Spring Dependency Injection, Spring DI]

date: 2022-06-08T16:40:00+09:00
last_modified_at: 2022-06-09T10:31:00+09:00
---

Spring 의존성 주입(Dependency Injection)에 대한 정리
{:.notice--primary}

## 의존성 주입이란?

어떤 객체가 의존하고 있는 다른 객체를 외부로부터 주입해주는 기법이다.

## @Autowired

`@Autowired` 어노테이션을 적용하면 스프링이 연관된 스프링 빈을 찾아 자동으로 주입해준다.

### 생성자 주입(Constructor Injection)

다음과 같이 생성자에 `@Autowired` 어노테이션을 적용해서 의존성을 주입받을 수 있으며, 스프링에서 가장 권장하고 있는 방식이다. 만약 클래스가 단일 생성자만 가지고 있다면 `@Autowired` 어노테이션을 생략할 수 있다.

``` java
@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}
```

### 필드 주입(Field Injection)

다음과 같이 필드에 `@Autowired` 어노테이션을 적용해서 의존성을 주입받을 수 있다.

``` java
@Controller
public class MemberController {

    @Autowired
    private MemberService memberService;
}
```

### 수정자 주입(Setter Injection)

다음과 같이 수정자에 `@Autowired` 어노테이션을 적용해서 의존성을 주입받을 수 있다.

``` java
@Controller
public class MemberController {

    private MemberService memberService;

    @Autowired
    public void setMemberService(MemberService memberService) {
        this.memberService = memberService;
    }
}
```

## 참고 자료

- [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)