---
title: "Spring 컴포넌트 스캔"

categories: [Framework, Spring]
tags: [Spring, Spring Boot, Spring Component Scan]

date: 2022-06-07T21:11:00+09:00
last_modified_at: 2022-06-08T00:00:00+09:00
---

Spring 컴포넌트 스캔에 대한 정리
{:.notice--primary}

## 컴포넌트란?

Controller, Service, Repository 등과 같이 Spring 애플리케이션을 구성하는 요소를 컴포넌트라고 부르며, 클래스에 `@Component` 어노테이션을 적용하여 만들 수 있다.

## 컴포넌트 스캔 과정

별도로 설정하지 않으면 스프링 애플리케이션이 실행될 때, `@ComponentScan` 어노테이션이 적용된 클래스가 속한 패키지와 하위 패키지를 대상으로 컴포넌트 스캔을 수행한다. 해당 스캔 과정에서 `@Component` 어노테이션이 적용된 클래스가 발견되면, 해당 컴포넌트를 스프링 컨테이너에 스프링 빈(Bean)으로 등록한다. 이렇게 생성된 스프링 빈은 기본적으로 싱글톤으로 등록되어, 하나의 인스턴스를 여러 곳에서 공유하여 사용하게 된다.

참고로 `@Controller`, `@Service`, `@Repository` 어노테이션은 내부적으로 `@Component` 어노테이션을 포함하고 있기 때문에, 해당 어노테이션이 적용된 클래스에 `@Component` 어노테이션을 따로 적용하지 않아도 스프링 빈으로 자동 등록된다.

### @SpringBootApplication

`@SpringBootApplication` 어노테이션은 아래와 같이 `@ComponentScan` 어노테이션을 포함하고 있기 때문에, 스프링 부트를 이용하여 스프링 프로젝트를 시작했다면 `@ComponentScan` 어노테이션을 따로 적용하지 않아도 컴포넌트 스캔을 자동으로 수행한다.

``` java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication { ... }
```

## 참고 자료

- [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)