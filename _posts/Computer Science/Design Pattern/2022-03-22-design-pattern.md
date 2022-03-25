---
title: "디자인 패턴"

categories: [Computer-Science, Design-Pattern]
tags: [Design Pattern, Gang of Four, GoF]

date: 2022-03-22T20:10:00+09:00
last_modified_at: 2022-03-25T18:42:00+09:00
---

디자인 패턴에 대한 정리
{:notice--primary}

## 디자인 패턴이란?

- 소프트웨어 설계 과정에서 자주 발생하는 문제들에 대한 해법을 정형화하여 기록해놓은 것이다.
- 디자인 패턴을 활용하면 설계자로 하여금 재사용이 가능한 설계는 선택하고, 그렇지 못한 설계는 배제할 수 있도록 도움을 준다.

## 목적에 따른 분류

| 생성 패턴 | 구조 패턴 | 행동 패턴 |
| --- | --- | --- |
| 추상 팩토리(Abstract Factory) | 어댑터(Adapter) | 책임 연쇄(Chain of Responsibility) |
| 빌더(Builder) | 브리지(Bridge) | 커맨드(Command) |
| 팩토리 메서드(Factory Method) | 컴포지트(Composite) | 인터프리터(Interpreter) |
| 프로토타입(Prototype) | 데코레이터(Decorator) | 반복자(Iterator) |
| 싱글톤(Singleton) | 퍼사드(Façade) | 중재자(Mediator) |
| | 플라이웨이트(Flyweight) | 메멘토(Memento) |
| | 프록시(Proxy) | 옵저버(Observer) |
| | | 상태(State) |
| | | 전략(Strategy) |
| | | 템플릿 메서드(Template Method) |
| | | 방문자(Visitor) |

### 생성 패턴(Creational Pattern)

- 인스턴스를 만드는 절차를 추상화하는 패턴으로 객체를 생성·합성하는 방법과 시스템을 분리해준다.
- 시스템이 어떤 구체 클래스를 사용하는지에 대한 정보를 캡슐화함으로써, 인스턴스들이 생성되는 과정과 합성되는 과정을 완전히 가려준다.

### 구조 패턴(Structural Pattern)

- 더 큰 구조를 형성하기 위해 클래스와 객체를 합성하는 방법과 관련된 패턴이다.
- 복잡한 구조를 갖는 시스템을 효율적으로 개발하기 위한 방법을 제공해준다.

### 행동 패턴(Behavioral Pattern)

- 클래스나 객체들이 상호작용하는 방법과 책임을 분산하는 방법을 정의하는 패턴이다.
- 런타임 단계에서 다른 객체로 행동이 옮겨가거나 알고리즘이 대체되는 경우에 대한 처리 방법을 제공해준다.

## 참고 자료

- Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides, 「Design Patterns: Elements of Reusable Object-Oriented Software」, Addison-Wesley Professional, 1994