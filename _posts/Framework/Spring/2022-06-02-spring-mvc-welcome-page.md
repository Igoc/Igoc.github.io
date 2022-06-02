---
title: "Spring MVC Welcome Page"

categories: [Framework, Spring]
tags: [Spring, Spring Boot, Spring MVC, Spring Web MVC]

date: 2022-06-02T21:47:00+09:00
last_modified_at: 2022-06-02T21:47:00+09:00
---

Spring MVC Welcome Page에 대한 정리
{:.notice--primary}

## Welcome Page란?

웹사이트의 시작점 역할을 하는 페이지로, Spring Boot에서는 정적 Welcome Page와 템플릿 Welcome Page를 모두 지원한다.

## 탐색 순서

<a href="https://user-images.githubusercontent.com/22683489/171633093-854053b9-35b4-410e-9bc3-9cfd91ef075c.png">
    ![탐색 순서](https://user-images.githubusercontent.com/22683489/171633093-854053b9-35b4-410e-9bc3-9cfd91ef075c.png){:.align-center}
</a>

Welcome Page에 대한 요청이 들어오면 가장 먼저 정적 컨텐츠 위치에서 index.html 파일이 있는지 탐색한다. 만약 파일이 있다면 해당 파일을 Welcome Page로 사용하고, 없다면 index 템플릿을 찾아 Welcome Page로 사용한다.

## 참고 자료

- [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)