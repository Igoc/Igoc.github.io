---
title: "[Java] 상속 관계에서 생성자 실행 순서"

categories: Java

date: 2021-11-08
last_modified_at: 2021-11-08
---

자바의 상속 관계에서 생성자 실행 순서에 대한 정리
{:.notice--primary}

## 상속 관계에서 생성자 실행 순서

- 생성자의 호출 순서는 서브 클래스 → 슈퍼 클래스 순으로 수행
- 생성자의 실행 순서는 슈퍼 클래스 → 서브 클래스 순으로 수행

### 사용법

``` java
class Ancestor {
    public Ancestor() {
        System.out.println("Ancestor");
    }
}

class Parent extends Ancestor {
    public Parent() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    public Child() {
        System.out.println("Child");
    }
}

public class Main {
    public static void main(String[] args) {
        Child child = new Child();
    }
}
```

``` bash
[출력]
Ancestor
Parent
Child
```