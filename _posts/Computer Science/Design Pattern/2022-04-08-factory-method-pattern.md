---
title: "팩토리 메서드(Factory Method) 패턴"

categories: [Computer-Science, Design-Pattern]
tags: [Design Pattern, Gang of Four, GoF, Factory Method]

date: 2022-04-08T16:47:00+09:00
last_modified_at: 2022-04-08T16:47:00+09:00
---

팩토리 메서드(Factory Method) 패턴에 대한 정리
{:.notice--primary}

## 의도

객체를 생성하기 위한 인터페이스를 정의하되, 서브클래스가 인스턴스화할 클래스를 결정하게 하는 패턴이다.

## 활용성

- 어떤 클래스가 자신이 생성해야 할 객체의 클래스로 어떤 것이 사용될지 예측할 수 없을 때 활용할 수 있다.
- 생성하고자 하는 객체를 서브클래스에서 명세하고 싶을 때 활용할 수 있다.
- 여러 서브클래스 가운데 하나에게 객체를 생성하는 책임을 위임하되, 어떤 서브클래스가 책임을 위임받게 되는지에 대한 정보는 국소화(Localization)하고 싶을 때 활용할 수 있다.

## 구조

![구조](https://user-images.githubusercontent.com/22683489/162392477-a6db77ec-8a06-49a9-8839-ac5c762b9712.png){:.align-center}

### Product

- 팩토리 메서드가 생성할 객체에 대한 인터페이스를 정의한다.

### ConcreteProduct

- `Product`에서 정의한 인터페이스를 구현한다.

### Creator

- `Product` 객체를 생성하여 반환하는 팩토리 메서드를 선언한다.
- `Creator`는 생성할 객체를 인스턴스화해야 할 시점은 알지만, 구체적으로 어떤 클래스를 인스턴스화해야 할지는 모르는 상태이다.
- `Creator` 내부에서 `Product` 객체를 생성하기 위해 팩토리 메서드를 호출하기도 한다.

### ConcreteCreator

- 팩토리 메서드를 오버라이딩하여 `ConcreteProduct` 인스턴스를 반환한다.

## 협력 방법

- `Creator`는 적절한 `ConcreteProduct` 인스턴스를 반환하는 팩토리 메서드를 정의하기 위해 서브클래스를 사용한다.

## 구현 방법

다음과 같이 팩토리 메서드 패턴을 구현하기 위한 여러 테크닉 기법이 존재한다.

### Creator 클래스가 팩토리 메서드에 대한 구현을 제공하지 않는 추상 클래스

팩토리 메서드에 대한 구현을 정의하기 위해 서브클래스가 필요하지만, `Creator` 클래스에서 어떤 클래스를 인스턴스화해야 하는지 예측할 수 없는 딜레마를 해결할 수 있다.

### Creator 클래스가 팩토리 메서드에 대한 기본 구현을 제공하는 구체 클래스

각각의 오퍼레이션이 기본적으로 객체를 생성하도록 구현되어 있기 때문에, 서브클래스에서 자신이 변경하고 싶은 오퍼레이션만 오버라이딩하도록 할 수 있다.

### 파라미터를 갖는 팩토리 메서드

팩토리 메서드가 생성할 객체의 종류를 지정할 수 있는 파라미터를 갖도록 구현할 수도 있다. 해당 방법을 이용하면 동일한 `Product` 인터페이스를 공유하고 있는 모든 객체를 하나의 팩토리 메서드에서 생성할 수 있다. 만약 `Creator`에서 생성하는 객체를 다른 것으로 변경하거나 확장하고자 한다면, 서브클래스에서 이를 상속받아 팩토리 메서드를 오버라이딩하기만 하면 된다.

``` java
class Creator {
    public Product create(String productId) {
        switch (productId) {
            case "MINE":
                return new MyProduct();

            case "YOURS":
                return new YourProduct();
        }

        return null;
    }
}

class ConcreteCreator extends Creator {
    @Override
    public Product create(String productId) {
        switch (productId) {
            case "MINE":
                return new MyNewProduct();

            case "THEIRS":
                return new TheirProduct();
        }

        return super.create(productId);
    }
}
```

## 예제

![예제](https://user-images.githubusercontent.com/22683489/162409344-26555dcc-e842-4683-a266-e7854e578a21.png){:.align-center}

동일한 재료를 사용하더라도 버거 가게마다 서로 다른 종류의 버거를 판매한다는 점을 구현하기 위해, 위와 같이 팩토리 메서드 패턴을 이용한 시스템을 구성하였다.

### 버거 - Product

``` java
abstract class Burger {
    protected String name;
    protected String ingredient;
    protected String condition;

    public abstract void prepare();
    public abstract void cook();
    public abstract void pack();

    @Override
    public String toString() {
        return name + ": " + ingredient + ", " + condition;
    }
}

class CheeseBurger extends Burger {
    public CheeseBurger(String name) {
        this.name = name;
    }

    @Override
    public void prepare() {
        ingredient = "Cheese";
        condition  = "Prepared";
    }

    @Override
    public void cook() {
        ingredient = "Melted Cheese";
        condition  = "Cooked";
    }

    @Override
    public void pack() {
        condition = "Packed";
    }
}

class ShrimpBurger extends Burger {
    public ShrimpBurger(String name) {
        this.name = name;
    }

    @Override
    public void prepare() {
        ingredient = "Raw Shrimp";
        condition  = "Prepared";
    }

    @Override
    public void cook() {
        ingredient = "Cooked Shrimp";
        condition  = "Cooked";
    }

    @Override
    public void pack() {
        condition = "Packed";
    }
}
```

버거는 `Product`에 해당하는 `Burger`와 `ConcreteProduct`에 해당하는 `CheeseBurger`, `ShrimpBurger`로 구성된다.

해당 예제에서는 구현을 단순화하기 위해 모든 버거 가게에서 동일한 재료와 상태를 다룬다고 가정했으며, 단지 차별점을 주기 위해 버거 가게마다 생성자를 통해 버거의 고유한 이름을 지을 수 있도록 만들었다.

### 버거 가게 - Creator

``` java
abstract class BurgerShop {
    protected abstract Burger makeBurger(String ingredient);

    public Burger sellBurger(String ingredient) {
        Burger burger = makeBurger(ingredient);
        burger.prepare();
        burger.cook();
        burger.pack();

        return burger;
    }
}

class BurgerKing extends BurgerShop {
    @Override
    protected Burger makeBurger(String ingredient) {
        switch (ingredient) {
            case "Cheese":
                return new CheeseBurger("치즈버거");

            case "Shrimp":
                return new ShrimpBurger("통새우와퍼");
        }

        return null;
    }
}

class Lotteria extends BurgerShop {
    @Override
    protected Burger makeBurger(String ingredient) {
        switch (ingredient) {
            case "Cheese":
                return new CheeseBurger("치즈No.5");

            case "Shrimp":
                return new ShrimpBurger("사각새우더블버거");
        }

        return null;
    }
}
```

버거 가게는 `Creator`에 해당하는 `BurgerShop`과 `ConcreteCreator`에 해당하는 `BurgerKing`, `Lotteria`로 구성된다.

앞서 말했던 것과 같이 각각의 버거 가게는 동일한 재료를 이용하지만, 각자 버거에 고유한 이름을 지어 판매하도록 구현하였다.

### 메인 함수

``` java
class Main {
    public static void main(String[] args) {
        BurgerShop burgerKing = new BurgerKing();
        System.out.println(burgerKing.sellBurger("Cheese"));
        System.out.println(burgerKing.sellBurger("Shrimp"));

        BurgerShop lotteria = new Lotteria();
        System.out.println(lotteria.sellBurger("Cheese"));
        System.out.println(lotteria.sellBurger("Shrimp"));
    }
}
```

``` bash
[출력]
치즈버거: Melted Cheese, Packed
통새우와퍼: Cooked Shrimp, Packed
치즈No.5: Melted Cheese, Packed
사각새우더블버거: Cooked Shrimp, Packed
```

버거 가게마다 고유한 이름을 갖는 버거를 판매하고 있는지 테스트하기 위해, 위와 같은 메인 함수를 작성하였다.

## 결론

- `Creator` 클래스에 대한 서브클래싱 강제
    - 클라이언트는 자신이 원하는 `ConcreteProduct` 객체를 생성하기 위해 `Creator` 클래스의 서브클래스를 정의해야만 한다.

- 서브클래스를 위한 훅(Hook) 메서드를 제공
    - 팩토리 메서드 패턴은 서브클래스를 정의하기 위한 훅 메서드를 제공한다.
    - `Creator` 클래스 내부에서 특정 클래스 인스턴스를 직접 만드는 대신, 정의된 팩토리 메서드를 이용하여 생성한다면 유연성을 획득할 수 있다.

- 병렬적인 클래스(Parallel Class) 계층을 연결
    - 어떤 클래스가 가진 책임의 일부를 분리하여 새로운 클래스를 만들 때 병렬적인 클래스 계층이 만들어진다.
    - 병렬적인 클래스 계층에서 책임을 위임한 클래스와 위임받은 클래스는 연결되어야만 하는데, 팩토리 메서드 패턴을 이용하여 각각을 `Creator`와 `Product`로 구현한다면 손쉽게 연결할 수 있다.