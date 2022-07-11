---
title: "제어의 역전(Inversion of Control)"

categories: [Computer-Science, Software-Engineering]
tags: [Inversion of Control, IoC, Framework, Library]

date: 2022-07-11T20:32:00+09:00
last_modified_at: 2022-07-11T20:32:00+09:00
---

제어의 역전(Inversion of Control)에 대한 정리
{:.notice--primary}

## 제어의 역전이란?

프로그램의 제어 흐름을 직접 컨트롤하는 것이 아니라, 외부에서 관리하도록 하는 형태를 의미한다.

기존의 프로그래밍 방식에서는 프로그래머가 작성한 코드에서 필요한 기능이 호출되지만, 제어의 역전에서는 프로그래머가 작성한 코드가 외부에서 호출되는 형태로 구성된다.

## 프레임워크(Framework)

제어의 역전이 이루어져 프로그래머가 작성한 코드를 제어하고, 대신 실행하는 소프트웨어를 프레임워크라고 한다.

## 라이브러리(Library)

프로그래머가 작성한 코드에서 직접 제어의 흐름을 관리하여, 필요에 따라 호출되는 소프트웨어를 라이브러리라고 한다.

## 참고 자료

- [스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)