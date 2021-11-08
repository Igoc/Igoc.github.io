---
title: "[Java] 스레드 동기화"

categories: Java

date: 2021-11-08
last_modified_at: 2021-11-08
---

자바의 스레드 동기화에 대한 정리
{:.notice--primary}

## 스레드 동기화

- 스레드의 실행을 제어하는 기술을 의미
- 스레드의 경우 비동기적으로 실행되기 때문에, 동기적으로 실행되어야 할 작업에 스레드 동기화를 해주지 않으면 의도치 않은 결과를 생성

## 스레드 동기화가 필요한 상황

``` java
class Adder {
    private static int number = 1;

    public int add(int iteration) {
        int output = 0;

        for (int index = 0; index < iteration; index++) {
            output += number;
            number += 1;
        }

        return output;
    }
}

class AsynchronousThread extends Thread {
    private static Adder adder = new Adder();

    public void run() {
        System.out.println(adder.add(1000));
    }
}

public class Main {
    public static void main(String[] args) {
        AsynchronousThread thread1 = new AsynchronousThread();
        AsynchronousThread thread2 = new AsynchronousThread();

        thread1.start();
        thread2.start();
    }
}
```

``` bash
[출력]
565814
577286
```

스레드 동기화는 주로 공유 데이터에 접근하여 값을 변경하는 경우에 필요하다. 위의 경우 원래라면 1부터 1000까지 더한 500500과 1001부터 2000까지 더한 1500500이 결과로 나와야 한다. 하지만 Adder 클래스의 공유 데이터인 number에 비동기적으로 접근하여 값을 변경한 결과, 원래의 의도와는 다른 결과가 도출되었다.

## synchronized 키워드

- 스레드 동기화를 위한 장치로 특정 코드 블록을 동기화가 설정된 임계 영역으로 지정
- 임계 영역으로 진입한 스레드가 있으면, 다른 스레드는 진입한 스레드가 임계 영역을 빠져나올 때까지 대기
- 메서드 전체를 임계 영역으로 지정하는 방법과 특정 블록만 지정하는 두 가지 방법을 제공

### 사용법 (메서드)

``` java
class Adder {
    private static int number = 1;

    public synchronized int add(int iteration) {
        int output = 0;

        for (int index = 0; index < iteration; index++) {
            output += number;
            number += 1;
        }

        return output;
    }
}

class SynchronousThread extends Thread {
    private static Adder adder = new Adder();

    public void run() {
        System.out.println(adder.add(1000));
    }
}

public class Main {
    public static void main(String[] args) {
        SynchronousThread thread1 = new SynchronousThread();
        SynchronousThread thread2 = new SynchronousThread();

        thread1.start();
        thread2.start();
    }
}
```

``` bash
[출력]
500500
1500500
```

임계 영역으로 설정하고 싶은 메서드의 접근 지정자 뒤에 synchronized 키워드를 붙이면, 해당 메서드는 동기적으로 동작한다.

### 사용법 (블록)

``` java
class Adder {
    private static int number = 1;

    public int add(int iteration) {
        int output = 0;

        synchronized (this) {
            for (int index = 0; index < iteration; index++) {
                output += number;
                number += 1;
            }
        }

        return output;
    }
}

class SynchronousThread extends Thread {
    private static Adder adder = new Adder();

    public void run() {
        System.out.println(adder.add(1000));
    }
}

public class Main {
    public static void main(String[] args) {
        SynchronousThread thread1 = new SynchronousThread();
        SynchronousThread thread2 = new SynchronousThread();

        thread1.start();
        thread2.start();
    }
}
```

``` bash
[출력]
500500
1500500
```

임계 영역으로 설정하고 싶은 블록을 synchronized 키워드 블록으로 감싸주면, 해당 블록은 동기적으로 동작한다. synchronized 키워드의 괄호 안에는 동기화를 보장하고 싶은 객체를 전달하면 된다.

### 주의 사항

``` java
class Adder {
    private static int number = 1;

    public int add(int iteration) {
        int output = 0;

        synchronized (this) {
            for (int index = 0; index < iteration; index++) {
                output += number;
                number += 1;
            }
        }

        return output;
    }
}

class SynchronousThread extends Thread {
    private Adder adder = new Adder();

    public void run() {
        System.out.println(adder.add(1000));
    }
}

public class Main {
    public static void main(String[] args) {
        SynchronousThread thread1 = new SynchronousThread();
        SynchronousThread thread2 = new SynchronousThread();

        thread1.start();
        thread2.start();
    }
}
```

``` bash
[출력]
571545
608893
```

synchronized 키워드는 객체를 단위로 잠금이 걸리기 때문에, 위와 같이 Adder 객체를 static으로 지정하지 않으면 스레드마다 각각의 Adder 객체가 생성되어 동기화를 보장받을 수 없다. 다시 말해 synchronized 키워드를 사용할 때는 동기적으로 수행되어야 하는 스레드 간에 동일한 객체를 사용해야만 한다.

## wait() 메서드와 notify() 메서드

- 자바의 최상위 클래스인 Object 클래스에서 제공하는 메서드
- 생산자-소비자 문제를 해결하기 위해 사용

### 사용법

``` java
class Buffer {
    private Integer data = null;

    public synchronized void set(Integer value) {
        try {
            if (data != null) {
                wait();
            }
        }
        catch (InterruptedException exception) {
            return;
        }

        data = value;

        notify();
    }

    public synchronized Integer get() {
        Integer output = null;

        try {
            if (data == null) {
                wait();
            }
        }
        catch (InterruptedException exception) {
            return null;
        }

        output = data;
        data   = null;

        notify();

        return output;
    }
}

class Producer extends Thread {
    private Buffer sharedBuffer;

    public Producer(Buffer buffer) {
        sharedBuffer = buffer;
    }

    public void run() {
        int index = 0;

        while (true) {
            sharedBuffer.set(index);

            System.out.println("Produce " + index++);
        }
    }
}

class Consumer extends Thread {
    private Buffer sharedBuffer;

    public Consumer(Buffer buffer) {
        sharedBuffer = buffer;
    }

    public void run() {
        while (true) {
            System.out.println("Consume " + sharedBuffer.get());
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Buffer   buffer   = new Buffer();
        Producer producer = new Producer(buffer);
        Consumer consumer = new Consumer(buffer);

        producer.start();
        consumer.start();
    }
}
```

``` bash
[출력]
...
Produce 1257044
Produce 1257045
Consume 1257044
Consume 1257045
Produce 1257046
Produce 1257047
Consume 1257046
Consume 1257047
...
```

위의 코드에서 Producer 클래스는 버퍼가 비어있지 않으면 wait() 메서드를 호출하여 대기하기 때문에, 손실되는 데이터 없이 모든 값을 버퍼에 쓸 수 있다. 이와 마찬가지로 Consumer 클래스는 버퍼가 비어있다면 wait() 메서드를 호출하여 대기하기 때문에, null 값을 읽어오지 않는다.

### 핵심 메서드

| 메서드 | 설명 |
| --- | --- |
| void wait() | 잠금이 걸린 블록의 잠금을 해제하고, 현재 스레드를 대기 상태로 변경 |
| void notify() | 대기 상태에 있는 스레드 중 임의의 스레드를 실행 상태로 변경 |
| void notifyAll() | 대기 상태에 있는 모든 스레드를 실행 상태로 변경 |