---
title: "[Computer Science] 객체 지향 프로그래밍 (OOP, Object Oriented Programming)"

categories: Computer-Science

date: 2021-11-06
last_modified_at: 2021-11-06
---

객체 지향 프로그래밍에 대한 정리
{:.notice--primary}

## 객체 지향 프로그래밍이란?

- 데이터 추상화의 산물인 객체를 기반으로 하는 프로그래밍 패러다임
- 프로그래밍에 필요한 데이터를 추상화하여 속성(변수)과 행위(함수)를 갖는 객체 형태로 만들고, 이렇게 만든 객체들 간의 상호작용을 통해 로직을 구성하는 프로그래밍 방법

## 클래스와 객체

### 클래스

- 객체를 만들기 위한 틀의 역할
- 특정 집단의 속성과 행위를 변수와 함수로 정의한 것

### 객체

- 클래스에서 정의한 것을 기반으로 실제 메모리에 할당시킨 것

## 객체 지향 프로그래밍의 특징

### 데이터 추상화

- 객체들의 공통된 속성과 행위를 도출해내는 것
- 객체 지향에서는 클래스를 설계하는 것을 의미

### 캡슐화

- 추상화를 통해 도출된 속성과 행위를 결합시켜 묶는 것
- 객체 지향에서는 캡슐화 단계에서 접근 지정자를 사용하여 정보 은닉을 달성
- 정보 은닉이란 객체가 외부에 노출하지 말아야 할 정보를 숨기는 것을 의미

### 상속

- 부모 클래스의 속성과 행위를 이어받아 자식 클래스를 생성하는 것
- 자식 클래스에서는 상속을 통해 변경이 필요한 일부분만 수정하고 추가하여 사용하는 것이 가능

### 다형성

- 동일한 이름의 속성과 행위가 상황에 따라 다른 동작을 하도록 하는 것
- 오버로딩(Overloading)과 오버라이딩(Overriding)을 통해 달성

## 객체 지향 설계 원칙 (SOLID)

| 문자 | 명칭 | 개념 |
| --- | --- | --- |
| S | 단일 책임 원칙 (SRP, Single Responsibility Principle) | 모든 클래스는 하나의 책임만 가지며, 클래스가 제공하는 모든 서비스는 그 하나의 책임을 수행하는 데 집중되어 있어야 한다는 원칙 |
| O | 개방-폐쇄 원칙 (OCP, Open-Closed Principle) | 소프트웨어 요소는 확장에는 열려있으나, 수정에는 닫혀있어야 한다는 원칙 |
| L | 리스코프 치환 원칙 (LSP, Liskov Substitution Principle) | 하위 클래스의 객체는 언제나 자신의 상위 클래스 객체로 치환 가능해야 한다는 원칙 |
| I | 인터페이스 분리 원칙 (ISP, Interface Segregation Principle) | 클래스는 자신이 사용하지 않는 함수를 구현하면 안된다는 원칙 |
| D | 의존 역전 원칙 (DIP, Dependency Inversion Principle) | 상위 클래스가 하위 클래스에 의존하면 안된다는 원칙 |