---
title: "JWT Refresh Token"

categories: [Computer-Science, Web]
tags: [JSON Web Token, JWT, JWT Refresh Token]

date: 2022-05-24T00:49:00+09:00
last_modified_at: 2022-05-24T00:49:00+09:00
---

JWT Refresh Token에 대한 정리
{:.notice--primary}

## Refresh Token이란?

권한이 필요한 데이터를 요청할 때 사용하는 Access Token의 경우, 제3자에게 탈취당하면 보안 문제가 발생하기 때문에 보통 유효 기간을 짧게 설정한다. 이로 인해 사용자는 Access Token이 만료될 때마다, 새로운 Access Token을 발급받기 위해 다시 로그인해야 하기 때문에 불편함을 겪게 된다.

이러한 문제점을 해결하기 위해 도입할 수 있는 기법이 바로 Refresh Token이다.

사용자는 로그인했을 때 유효 기간이 짧은 Access Token과 유효 기간이 긴 Refresh Token을 발급받게 된다. 이후에는 평소처럼 발급받은 Access Token을 이용하여 권한이 필요한 데이터를 요청하다가, Access Token이 만료된 순간 가지고 있던 Refresh Token을 전송하여 새로운 Access Token을 발급받을 수 있다. 만약 Refresh Token 마저도 만료되었다면, 사용자는 다시 로그인해서 새로운 Access Token과 Refresh Token을 발급받아야 한다.

## 인증 과정

<a href="https://user-images.githubusercontent.com/22683489/169863926-47c34ab3-8b4e-4405-bb7f-6ba1c3cff8f6.png">
    ![인증 과정](https://user-images.githubusercontent.com/22683489/169863926-47c34ab3-8b4e-4405-bb7f-6ba1c3cff8f6.png){:.align-center}
</a>

### 로그인 과정

1. 사용자가 ID와 Password를 이용하여 로그인을 요청한다.
2. 서버는 전달받은 ID와 Password를 회원 DB에 저장된 데이터와 비교하여 인증을 수행한다.
3. 정상적인 사용자일 경우 Access Token과 Refresh Token을 발급해주며, 일반적으로 Refresh Token은 데이터베이스에도 저장해둔다.
4. 사용자는 발급받은 Access Token과 Refresh Token을 안전한 저장소에 저장해둔다.

### 권한이 필요한 데이터 요청 과정

1. 사용자가 Access Token을 이용하여 권한이 필요한 데이터를 요청한다.
2. 서버는 전달받은 Access Token이 유효한지 검증한다.
3. 올바른 Access Token이면서 권한이 있는 사용자라면 요청한 데이터를 반환해준다.

### Access Token 재발급 과정

1. 사용자가 Access Token을 이용하여 권한이 필요한 데이터를 요청한다.
2. 서버는 전달받은 Access Token이 만료되었음을 확인하고, 사용자에게 Access Token이 만료되었다는 신호를 반환한다.
3. 사용자는 Access Token이 만료되었다는 신호를 받고, 만료된 Access Token과 Refresh Token을 이용하여 새로운 Access Token을 요청한다.
4. 서버는 전달받은 Access Token과 Refresh Token이 유효한지 검증한 후, 새로운 Access Token을 발급해준다.
5. 사용자는 새롭게 발급받은 Access Token을 안전한 저장소에 저장해둔다.

## 장단점

Refresh Token을 도입하게 될 경우 다음과 같은 장단점을 얻을 수 있다.

### 장점

Refresh Token을 도입한다고 해서 보안 문제가 해결되는 것은 아니지만, Access Token의 유효 기간을 보다 짧게 설정할 수 있기 때문에 상대적으로 안전해진다.

### 단점

Access Token이 만료될 때마다 새로 발급받기 위해 추가적인 통신이 필요하기 때문에 서버의 부담이 늘어난다.

## 참고 자료

- [쉽게 알아보는 서버 인증 2편(Access Token + Refresh Token) — 그랩의 블로그](https://tansfil.tistory.com/59?category=475681)