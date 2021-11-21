---
title: "[Go] type 키워드"

categories: Go

date: 2021-11-21
last_modified_at: 2021-11-21
---

Go 언어의 type 키워드에 대한 정리
{:.notice--primary}

## type 키워드

- 사용자 정의 자료형을 정의하기 위해 사용
- Go 언어에서는 함수가 1급 객체이기 때문에, 함수 또한 사용자 정의 자료형으로 정의 가능

### 사용법

``` go
package main

import "fmt"

type text string

type person struct {
	name text
	age  int
}

type calculator func(int, int) int

func main() {
	var name text = "A"
	var user person = person{name, 10}

	fmt.Println(user)

	adder := func(x int, y int) int {
		return x + y
	}

	fmt.Println(calculate(1, 2, adder))
}

func calculate(x int, y int, function calculator) int {
	return function(x, y)
}
```

``` bash
[출력]
{A 10}
3
```