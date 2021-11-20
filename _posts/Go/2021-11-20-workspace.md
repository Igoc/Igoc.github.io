---
title: "[Go] 워크스페이스 (Workspace)"

categories: Go

date: 2021-11-20
last_modified_at: 2021-11-20
---

Go 언어의 워크스페이스에 대한 정리
{:.notice--primary}

## GOBIN, GOPATH, GOROOT

go env 명령어를 이용하면 Go 언어와 관련된 여러 환경 변수 값을 출력해 볼 수 있다.

``` bash
$ go env
```

이를 실제로 출력해보면 여러 환경 변수 값들이 출력되는데, 이들 중 주의깊게 살펴봐야 할 값들에는 GOBIN, GOPATH, GOROOT가 있다.

### GOBIN

- go install 명령어를 통해 생성되는 실행 파일의 저장 경로
- main 패키지들을 컴파일하면 해당 경로에 실행 파일을 생성
- 일반적으로 워크스페이스의 bin 폴더로 지정

#### go build vs go install

go install은 내부적으로 go build를 통해 실행 파일을 생성한 후, GOBIN 환경 변수가 가리키는 폴더로 옮기는 작업을 수행한다.

### GOPATH

- 작업을 수행할 워크스페이스 경로를 가리키는 환경 변수
- 다른 워크스페이스를 사용하려면 해당 값을 변경
- Go 언어에서는 표준 패키지 이외의 서드 파티 패키지나 사용자 정의 패키지를 해당 경로에서 탐색

### GOROOT

- Go를 위한 핵심 파일이 모여있는 폴더
- 해당 폴더 안에는 컴파일러, 표준 패키지 등이 존재

## 워크스페이스 생성 방법

1. 워크스페이스로 사용하기 위한 폴더 생성
2. 생성한 워크스페이스 폴더 안에 src, pkg, bin 폴더를 생성
3. GOBIN과 GOPATH 환경 변수를 생성한 워크스페이스 폴더로 지정

### src, pkg, bin

- src 폴더는 Go 소스 코드를 작성하는 공간
- pkg 폴더는 컴파일된 패키지 오브젝트 파일이 저장되는 공간
- bin 폴더는 go install 명령어를 통해 생성되는 실행 파일이 저장되는 공간