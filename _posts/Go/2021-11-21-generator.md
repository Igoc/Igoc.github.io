---
title: "[Go] 제너레이터 (Generator)"

categories: Go

date: 2021-11-21
last_modified_at: 2021-11-21
---

Go 언어의 클로저를 활용한 제너레이터 생성 방법에 대한 정리
{:.notice--primary}

## 제너레이터 (Generator)

- 제너레이터란 이터레이션(Iteration)을 생성해주는 함수를 의미
- Go 언어에서는 Python이나 JavaScript 등과 같이 제너레이터를 위한 문법 미지원

## 클로저를 활용한 제너레이터 생성

- Go 언어에서는 클로저를 활용하여 제너레이터를 구현 가능
- 함수가 1급 객체이기 때문에 반환할 수 있다는 점을 이용

### 사용법

``` go
package main

import "fmt"

type closure func() int

func main() {
	next := generate()

	for index := 0; index < 5; index++ {
		fmt.Println(next())
	}
}

func generate() closure {
	value := 1

	return func() int {
		value *= 2

		return value
	}
}
```

``` bash
[출력]
2
4
8
16
32
```

변수 value가 제너레이터를 만들기 위해 사용된 클로저의 지역 변수가 아니기 때문에, 변수 value의 값을 계속 유지하는 것이 가능하다.