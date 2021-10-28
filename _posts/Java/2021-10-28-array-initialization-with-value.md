---
title: "[Java] 배열을 특정 값으로 초기화하는 방법"

categories: Java

date: 2021-10-28
last_modified_at: 2021-10-28
---

자바의 배열을 특정 값으로 초기화하는 방법에 대한 정리
{:.notice--primary}

## 반복문을 이용하는 방법

- 반복문을 이용하여 배열의 각 요소에 값을 직접 할당하는 방법

### 사용법

``` java
public class Main {
    public static void main(String[] args) {
        int[]   x = new int[5];
        int[][] y = new int[2][3];

        for (int index = 0; index < x.length; index++) {
            x[index] = -1;
        }

        for (int row = 0; row < y.length; row++) {
            for (int col = 0; col < y[row].length; col++) {
                y[row][col] = 10;
            }
        }

        for (int index = 0; index < x.length; index++) {
            System.out.println(x[index]);
        }

        for (int row = 0; row < y.length; row++) {
            for (int col = 0; col < y[row].length; col++) {
                System.out.println(y[row][col]);
            }
        }
    }
}
```

``` bash
[출력]
-1
-1
-1
-1
-1
10
10
10
10
10
10
```

## Arrays.fill() 메서드를 이용하는 방법

- java.util 패키지에 있는 Arrays 클래스의 fill() 메서드를 이용하는 방법
- Arrays 클래스는 모든 메서드가 클래스 메서드이기 때문에, 객체를 생성하지 않고 바로 사용이 가능

### 사용법

``` java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[]   x = new int[5];
        int[][] y = new int[2][3];

        Arrays.fill(x, -1);

        for (int index = 0; index < y.length; index++) {
            Arrays.fill(y[index], 10);
        }

        for (int index = 0; index < x.length; index++) {
            System.out.println(x[index]);
        }

        for (int row = 0; row < y.length; row++) {
            for (int col = 0; col < y[row].length; col++) {
                System.out.println(y[row][col]);
            }
        }
    }
}
```

``` bash
[출력]
-1
-1
-1
-1
-1
10
10
10
10
10
10
```

## 컬렉션의 nCopies() 메서드를 이용하는 방법

- java.util 패키지에 있는 Collections 클래스의 nCopies() 메서드를 이용하는 방법
- nCopies() 메서드의 경우 List 자료형을 반환하기 때문에, toArray() 메서드를 이용하여 배열로 변환 필요
- toArray() 메서드의 경우 파라미터로 Primitive 자료형을 받을 수 없어서, Wrapper 클래스 사용 필요

### 사용법

``` java
import java.util.Collections;

public class Main {
    public static void main(String[] args) {
        Integer[]   x = Collections.nCopies(5, -1).toArray(new Integer[0]);
        Integer[][] y = Collections.nCopies(2, Collections.nCopies(3, 10).toArray(new Integer[0])).toArray(new Integer[0][0]);

        for (int index = 0; index < x.length; index++) {
            System.out.println(x[index]);
        }

        for (int row = 0; row < y.length; row++) {
            for (int col = 0; col < y[row].length; col++) {
                System.out.println(y[row][col]);
            }
        }
    }
}
```

``` bash
[출력]
-1
-1
-1
-1
-1
10
10
10
10
10
10
```