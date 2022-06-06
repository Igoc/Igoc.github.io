---
title: "Spring 웹 애플리케이션 아키텍처"

categories: [Framework, Spring]
tags: [Spring, Spring Boot, Spring Web Application Architecture, Spring Web Application Layer]

date: 2022-06-06T20:18:00+09:00
last_modified_at: 2022-06-06T20:18:00+09:00
---

Spring 웹 애플리케이션 아키텍처에 대한 정리
{:.notice--primary}

## 웹 애플리케이션의 관심사

일반적인 웹 애플리케이션이 가지고 있는 관심사는 다음과 같다.

- 사용자의 입력을 처리하고, 적절한 응답을 사용자에게 돌려줄 수 있어야 한다.
- 사용자에게 합리적인 에러 메시지를 제공해 줄 수 있는 예외 처리 메커니즘이 있어야 한다.
- 트랜잭션 관리 전략이 있어야 한다.
- 사용자 인증과 권한 부여를 처리할 수 있어야 한다.
- 애플리케이션의 비즈니스 로직을 구현할 수 있어야 한다.
- 데이터 저장소 및 외부 리소스와 통신할 수 있어야 한다.

## Spring 웹 애플리케이션 계층

<a href="https://user-images.githubusercontent.com/22683489/172152721-798663c9-7ad2-46d0-989d-1e086f133eea.png">
    ![Spring 웹 애플리케이션 계층](https://user-images.githubusercontent.com/22683489/172152721-798663c9-7ad2-46d0-989d-1e086f133eea.png){:.align-center}
</a>

Spring에서는 일반적으로 웹 애플리케이션의 관심사를 3개의 계층으로 분리해서 처리한다.

### Web Layer

- 웹 애플리케이션의 최상위 계층으로, 사용자의 입력을 처리하고 적절한 응답을 사용자에게 돌려주는 역할을 수행한다.
- 다른 계층에서 발생한 예외를 처리하는 작업을 담당한다.
- 애플리케이션의 진입점이기 때문에, 사용자 인증 및 권한이 없는 사용자에 대한 일차적인 처리를 수행한다.

### Service Layer

- Web Layer 바로 아래에 있는 계층으로, 애플리케이션 서비스와 인프라 서비스를 제공하는 역할을 수행한다.
- 애플리케이션 서비스에서는 비즈니스 로직과 트랜잭션 처리 등을 다루며, 내부 시스템과 통신할 수 있는 방법을 제공해준다.
- 인프라 서비스에서는 파일 시스템, 데이터베이스, 이메일 서버와 같은 외부 리소스와 통신할 수 있는 방법을 제공해준다.

### Repository Layer

- 웹 애플리케이션의 최하위 계층으로, 데이터 저장소와 통신하는 역할을 수행한다.

## Data Transfer Object (DTO)

단순히 데이터를 보관하기 위한 용도의 객체로, 서로 다른 프로세스나 애플리케이션 계층 간에 데이터를 교환할 때 매개체로 사용된다.

## Domain Model

특정 도메인을 개념적으로 표현한 것으로, 다음과 같이 크게 3가지 요소로 구성된다.

### Domain Service

도메인과 관련된 오퍼레이션을 제공하는 역할을 수행한다.

### Entity

도메인과 관련된 정보를 저장하기 위한 객체로, 각각을 구분 지을 수 있는 고유한 식별자를 갖는다.

### Value Object

어떠한 값 자체를 표현하기 위한 객체로, Entity와 달리 각각을 구분 지을 수 있는 식별자를 갖지 않는다.

## 참고 자료

- [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)
- [Understanding Spring Web Application Architecture: The Classic Way - Petri Kainulainen](https://www.petrikainulainen.net/software-development/design/understanding-spring-web-application-architecture-the-classic-way/)