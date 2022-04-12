---
title: "프로토타입(Prototype) 패턴"

categories: [Computer-Science, Design-Pattern]
tags: [Design Pattern, Gang of Four, GoF, Prototype]

date: 2022-04-12T11:33:00+09:00
last_modified_at: 2022-04-12T11:33:00+09:00
---

프로토타입(Prototype) 패턴에 대한 정리
{:.notice--primary}

## 의도

프로토타입의 인스턴스를 사용하여 생성할 수 있는 객체의 종류를 명세하고, 해당 프로토타입을 복사함으로써 새로운 객체를 생성하는 패턴이다.

## 활용성

- 시스템이 시스템의 구성요소(Product)가 생성·구성·표현되는 방식과 독립적이어야 할 때 활용할 수 있다.
- 동적 로딩(Dynamic Loading)과 같이 인스턴스화할 클래스를 런타임 단계에서 명세해야 할 때 활용할 수 있다.
- 팩토리 클래스 계층이 구성요소 클래스 계층과 병렬적으로 구성되는 것을 피하기 위해 활용할 수 있다.
- 클래스의 인스턴스가 약간씩만 다른 여러 상태 조합 중 하나를 가질 때 활용할 수 있다.

## 구조

<a href="https://user-images.githubusercontent.com/22683489/162869531-d302f880-cc31-4860-954f-b905eb351968.png">
    ![구조](https://user-images.githubusercontent.com/22683489/162869531-d302f880-cc31-4860-954f-b905eb351968.png){:.align-center}
</a>

### Prototype

- 자기 자신을 복제(Clone)하기 위한 인터페이스를 선언한다.

### ConcretePrototype

- 자기 자신을 복제하기 위한 오퍼레이션을 구현한다.

### Client

- `Prototype`에게 복제를 요청함으로써 새로운 객체를 생성한다.

## 협력 방법

- `Client`는 `Prototype`에게 복제를 요청한다.

## 구현 방법

다음과 같이 프로토타입 패턴을 구현하기 위한 여러 테크닉 기법이 존재한다.

### 프로토타입 매니저(Prototype Manager) 사용

시스템에서 프로토타입이 동적으로 생성되고 삭제될 수 있어 개수를 가늠할 수 없을 때, 프로토타입을 저장소(Registry)에 보관하는 방법을 이용할 수 있다. 해당 방식을 이용하면 클라이언트는 프로토타입을 직접 관리하는 대신, 저장소를 통해 프로토타입을 보관하고 획득하게 된다. 이러한 저장소를 프로토타입 매니저라고 부르며, 각각의 프로토타입을 특정 키 값에 대응시키는 방식을 이용하여 관리한다.

### 얕은 복사(Shallow Copy)를 이용한 복제 오퍼레이션 구현

얕은 복사는 특정 객체가 저장되어 있는 주소의 값을 복사하는 방식으로, 종종 복제 오퍼레이션을 구현하기 위해 얕은 복사를 이용하기도 한다. 다만 얕은 복사를 이용할 경우 복제된 객체를 수정하면 원본 객체에도 영향이 가기 때문에 주의가 필요하다.

### 깊은 복사(Deep Copy)를 이용한 복제 오퍼레이션 구현

깊은 복사는 특정 객체가 가진 값을 복사하는 방식으로, 복잡한 구조를 갖는 객체의 경우 대부분 깊은 복사를 이용하여 복제 오퍼레이션을 구현한다.

### 저장(Save), 불러오기(Load) 오퍼레이션을 활용한 복제 오퍼레이션 구현

시스템에서 저장, 불러오기 오퍼레이션을 제공하는 경우, 객체를 저장하는 즉시 다시 불러오는 형태로 복제 오퍼레이션을 구현하기도 한다. 이 경우 저장 오퍼레이션을 이용하여 복제하려는 객체를 메모리 버퍼에 저장한 후, 불러오기 오퍼레이션을 통해 해당 버퍼로부터 객체를 재구성함으로써 복제를 수행한다.

### 복제된 객체의 초기화

때때로 복제된 객체의 내부 상태를 특정 값으로 초기화해야 할 때가 있다. 이를 지원하기 위해 초기화에 이용할 값을 복제 오퍼레이션의 파라미터로 전달하는 방법을 고려해볼 수도 있는데, 일반적으로 프로토타입 클래스마다 필요로 하는 값이 다르기 때문에 해당 방법을 이용하긴 어렵다. 따라서 복제된 객체의 초기화가 필요하다면 각 상태 값을 개별적으로 설정할 수 있는 오퍼레이션을 정의해두거나, 파라미터로 초기화에 필요한 값을 전달받아 한꺼번에 초기화를 수행하는 `initialize()` 오퍼레이션을 도입해야 한다.

## 예제

<a href="https://user-images.githubusercontent.com/22683489/162937296-dbdccd21-739a-459a-a32a-45fafbd06c4a.png">
    ![예제](https://user-images.githubusercontent.com/22683489/162937296-dbdccd21-739a-459a-a32a-45fafbd06c4a.png){:.align-center}
</a>

VIP 등급에 따라 쿠폰 개수를 상이하게 지급하는 서비스를 구현하기 위해, 위와 같이 프로토타입 패턴을 이용한 시스템을 구성하였다.

### 유저 - Prototype

``` java
class User {
    private String name;
    private String membershipLevel;
    private int    couponNumber;

    public User(String membershipLevel, int couponNumber) {
        this.membershipLevel = membershipLevel;
        this.couponNumber    = couponNumber;
    }

    public User(User user) {
        this.name            = user.name;
        this.membershipLevel = user.membershipLevel;
        this.couponNumber    = user.couponNumber;
    }

    public void initialize(String name) {
        this.name = name;
    }

    public User clone() {
        return new User(this);
    }

    @Override
    public String toString() {
        return name + ": " + membershipLevel + ", " + couponNumber + " Coupons";
    }
}
```

`User`는 생성자를 이용하여 프로토타입을 만들 때 VIP 등급에 해당하는 `membershipLevel`과 쿠폰 개수에 해당하는 `couponNumber`를 설정하도록 강제하였다. 또한 `initialize()`를 도입하여 복제된 객체의 `name`을 설정할 수 있도록 만들었다.

### 유저 프로토타입 팩토리 - Client

``` java
class UserPrototypeFactory {
    private Map<String, User> userCatalog = new HashMap<>();

    public User make(String membershipLevel) {
        return userCatalog.get(membershipLevel).clone();
    }

    public void addMembershipLevel(String membershipLevel, int couponNumber) {
        userCatalog.put(membershipLevel, new User(membershipLevel, couponNumber));
    }
}
```

`User`에 대한 프로토타입을 관리하기 위해, `UserPrototypeFactory`를 프로토타입 매니저 방식으로 구현하였다. `addMembershipLevel()`을 이용하여 새로운 프로토타입을 추가하고, `make()`를 통해 원하는 프로토타입을 복제할 수 있도록 만들었다.

### 메인 함수

``` java
class Main {
    public static void main(String[] args) {
        UserPrototypeFactory factory = new UserPrototypeFactory();
        factory.addMembershipLevel("Normal", 0);
        factory.addMembershipLevel("VIP", 5);

        User alice = factory.make("Normal");
        alice.initialize("Alice");
        System.out.println(alice);

        User bob = factory.make("Normal");
        bob.initialize("Bob");
        System.out.println(bob);

        User chris = factory.make("VIP");
        chris.initialize("Chris");
        System.out.println(chris);
    }
}
```

``` bash
[출력]
Alice: Normal, 0 Coupons
Bob: Normal, 0 Coupons
Chris: VIP, 5 Coupons
```

`UserPrototypeFactory`가 `User`에 대한 프로토타입을 잘 관리하는지 테스트하기 위해, 위와 같은 메인 함수를 작성하였다.

## 결론

- 런타임 단계에서 시스템에 구성요소를 추가 및 삭제 가능
    - `Client`에 프로토타입의 인스턴스를 등록하기만 하면, 새로운 구성요소의 구체 클래스(Concrete Class)를 시스템에 손쉽게 포함시킬 수 있다.

- 상태 값을 다양화하여 새로운 객체를 명세 가능
    - 이미 존재하는 클래스의 상태 값만 달리하여 `Client`에 프로토타입으로 등록하는 방식을 이용하면, 새로운 종류의 객체를 효과적으로 정의할 수 있다.
    - 해당 방식을 이용하여 프로토타입 패턴을 구현하면 시스템이 필요로 하는 클래스의 개수를 많이 줄일 수 있다.

- 팩토리 메서드 패턴이 사용된 시스템에 적용하면 서브클래싱 횟수 감소
    - 팩토리 메서드 패턴은 종종 `Product` 클래스 계층과 `Creator` 클래스 계층을 병렬적으로 구성할 때가 있는데, 그러한 경우 시스템을 구현하기 위해 많은 서브클래싱을 필요로 한다.
    - 해당 상황에 프로토타입 패턴을 적용하게 되면 새로운 객체를 만들기 위해 팩토리 메서드에 요청하는 대신 프로토타입을 복제하는 방식을 이용하게 되기 때문에, 더 이상 `Creator` 클래스 계층을 구현하지 않아도 된다.

- 복제 오퍼레이션 구현의 어려움
    - 프로토타입으로 사용할 객체가 내부적으로 복사를 지원하지 않는 객체를 가지고 있거나, 순환 참조를 이루고 있는 경우 복제 오퍼레이션을 구현하는 것이 어렵다.

## 참고 자료

- Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides, 「Design Patterns: Elements of Reusable Object-Oriented Software」, Addison-Wesley Professional, 1994