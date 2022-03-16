---
title: "객체 지향 프로그래밍(OOP)"

categories: [Computer-Science, Programming-Paradigm]
tags: [Programming Paradigm, Object Oriented Programming, OOP]

date: 2022-02-11T14:20:00+09:00
last_modified_at: 2022-03-16T17:25:00+09:00
---

객체 지향 프로그래밍(OOP)에 대한 정리
{:.notice--primary}

## 객체 지향 프로그래밍이란?

데이터를 추상화하여 상태와 행위를 갖는 객체로 만들고, 이들 간의 상호작용을 통해 애플리케이션을 구성하는 프로그래밍 방식을 의미한다. 간단히 말하자면 데이터와 데이터를 사용하는 프로세스가 동일한 모듈 내부에 위치하도록 프로그래밍하는 것이다.

## 역할, 책임, 협력

인터페이스와 클래스에 대해 이야기하기 전에 우선적으로 객체 지향 프로그래밍의 핵심 관점인 역할(Role), 책임(Responsibility), 협력(Collaboration)에 대해 알아보고자 한다.

### 협력

객체 지향 프로그래밍은 앞서 말했듯이 객체들 간의 상호작용을 통해 이루어진다. 이처럼 객체들이 애플리케이션의 기능을 구현하기 위해 수행하는 상호작용을 협력이라고 한다. 조금 더 간단히 표현하자면 협력이란 어떤 객체가 다른 객체에게 무엇인가를 요청하는 것을 의미한다.

객체의 상태는 행위를 하기 위해 필요한 정보에 의해 결정되고, 행위는 협력 내에서 객체가 처리할 메시지로 결정된다. 결과적으로 객체가 참여하는 협력이 객체를 구성하는 행위와 상태 모두를 결정한다.

### 책임

객체를 설계하기 위한 협력을 갖춘 이후엔 협력에 필요한 행동을 수행할 수 있는 적절한 객체를 찾아야 한다. 이때 협력에 참여하기 위해 객체가 수행하는 행동을 책임이라고 부른다. 이러한 객체의 책임은 크게 행위에 해당하는 '하는 것(Doing)'과 상태에 해당하는 '아는 것(Knowing)'으로 나눌 수 있다.

### 역할

객체의 목적은 협력 내에서 객체가 맡게 되는 책임의 집합 형태로 표시된다. 이처럼 객체가 어떤 협력 내에서 수행하는 책임의 집합을 역할이라고 부른다. 조금 다른 관점에서 보면 역할이란 동일한 책임을 수행하는 객체를 교대로 바꿔 끼울 수 있는 일종의 슬롯이라고 생각할 수 있다.

## 인터페이스와 클래스

객체를 역할과 구현으로 잘 분리하여 설계하면 객체 간의 결합도를 낮추고 변경하기 쉬운 코드를 작성할 수 있게 된다. 이렇듯 객체 지향 프로그래밍의 강력한 이점을 활용하기 위해선 역할과 구현을 분리해야 하는데, 이를 달성하기 위해 제공되는 것이 바로 인터페이스(Interface)와 클래스(Class)이다.

### 인터페이스

- 동일한 책임을 수행하는 객체들의 역할을 나타낸다.
- 객체가 이해할 수 있는 메시지의 목록을 정의한 것이다.

### 클래스

- 역할을 실제로 수행할 수 있도록 상태와 행위를 구현한 것에 해당한다.
- 클래스에서 구현한 내용을 통해 애플리케이션 상에서 동작하는 인스턴스를 생성하게 된다.

## 객체 지향 프로그래밍의 특징

객체 지향 프로그래밍은 여러 특징을 가지고 있지만, 그 중 핵심이라고 생각되는 캡슐화(Encapsulation), 상속(Inheritance), 다형성(Polymorphism)에 대해서만 다뤄보려 한다.

### 캡슐화

- 추상화를 통해 도출된 상태와 행위를 묶고, 객체 내부의 세부적인 사항을 감추는 것을 의미한다.
- 캡슐화를 통해 객체 내부로의 접근을 제한하면, 객체 간의 결합도를 낮출 수 있기 때문에 설계를 좀 더 쉽게 변경할 수 있다.

### 상속

- 구현하고자 하는 클래스(자식 클래스)와 흡사한 클래스(부모 클래스)의 상태와 행위를 이어받는 것을 의미한다.
- 상속은 다른 부분만을 추가해서 새로운 클래스를 쉽고 빠르게 만들 수 있는 방법을 제공하는데, 이러한 방법을 차이에 의한 프로그래밍이라고 한다.

### 다형성

- 동일한 메시지라고 하더라도 수신하는 객체의 클래스에 따라 세부적인 행위가 달라지는 것을 의미한다.
- 역할과 구현을 잘 분리하여 설계했다면 다형성을 통해 손쉽게 기능을 확장할 수 있다.

``` java
interface DiscountPolicy {
    int discount(int price);
}

class FixDiscountPolicy implements DiscountPolicy {
    public int discount(int price) {
        return (price > 1000) ? (price - 1000) : (0);
    }
}

class RateDiscountPolicy implements DiscountPolicy {
    public int discount(int price) {
        return (int)(price - price * 0.1);
    }
}

class Main {
    public static void main(String[] args) {
        DiscountPolicy fixDiscountPolicy  = new FixDiscountPolicy();
        DiscountPolicy rateDiscountPolicy = new RateDiscountPolicy();

        System.out.println(fixDiscountPolicy.discount(50000));  // 49000
        System.out.println(rateDiscountPolicy.discount(50000)); // 45000
    }
}
```

예시를 위해 위와 같이 '할인 가격을 계산하라'라는 메시지를 이해할 수 있는 `DiscountPolicy` 인터페이스를 정의하였다. 또한 해당 인터페이스를 상속해 '할인 가격을 계산하라'라는 메시지를 이해하여, 구체적인 행위를 수행하는 `FixDiscountPolicy` 클래스와 `RateDiscountPolicy` 클래스를 구현하였다.

위의 예제에서 `fixDiscountPolicy` 인스턴스와 `rateDiscountPolicy` 인스턴스는 모두 '할인 가격을 계산하라'라는 동일한 메시지를 수신하고 이해할 수 있다. 그러나 객체의 클래스가 `FixDiscountPolicy` 클래스면 고정적으로 1000원을 할인한 가격을 계산하게 되고, `RateDiscountPolicy` 클래스면 10%를 할인한 가격을 계산하게 된다. 이처럼 메시지를 수신하는 객체의 클래스에 따라 세부적인 행위가 달라지는 것을 다형성이라고 한다.

## 참고 자료

- 조영호, 「오브젝트: 코드로 이해하는 객체지향 설계」, 위키북스, 2019