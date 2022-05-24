---
title: "JWT Refresh Token"

categories: [Computer-Science, Web]
tags: [JSON Web Token, JWT, JWT Refresh Token]

date: 2022-05-24T00:49:00+09:00
last_modified_at: 2022-05-24T21:03:00+09:00
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

## Refresh Token을 데이터베이스에 저장하는 이유

Refresh Token은 긴 유효 기간을 가지고 있기 때문에, 제3자에게 탈취당할 경우 장기간 동안 악용될 가능성이 크다. 이를 막기 위해 서버에서는 유효하지 않다고 판단되는 사용자가 탈취한 Refresh Token을 이용하여 Access Token을 갱신해달라고 요청했을 때, 해당 요청에 사용된 Refresh Token을 무효화시킬 수 있어야 하기 때문에 데이터베이스에 저장해서 관리하는 것이다.

사실 최근에는 Refresh Token을 쿠키에 저장할 때 HttpOnly 플래그를 설정하여 저장하기 때문에, 제3자에게 탈취당할 위험이 거의 없어서 굳이 데이터베이스에 저장을 하지 않아도 된다. 그럼에도 불구하고 100% 안전한 보안은 없기 때문에, 정말 보안이 중요한 시스템이라면 서버에서 Refresh Token을 무효화할 수 있는 방법을 도입하는 것이 좋다.

### 유효한 사용자인지 판별 방법

Refresh Token을 이용하여 Access Token의 갱신을 요청한 사용자가 유효한지 판별할 수 있는 방법은 여러 가지가 있지만, 가장 간단한 것으로는 갱신을 요청할 때 Refresh Token과 같이 전송되는 Access Token이 만료되었는지를 보는 방법이 있다. 만약 Access Token이 만료되지 않았는데 갱신을 요청한 상황이라면, 이는 충분히 비정상적인 상황이기 때문에 유효하지 않은 사용자가 갱신을 요청했다고 보고 Refresh Token을 무효화시키면 된다.

## 장단점

Refresh Token을 도입하게 될 경우 다음과 같은 장단점을 얻을 수 있다.

### 장점

Refresh Token을 도입한다고 해서 보안 문제가 해결되는 것은 아니지만, Access Token의 유효 기간을 보다 짧게 설정할 수 있기 때문에 상대적으로 안전해진다.

### 단점

Access Token이 만료될 때마다 새로 발급받기 위해 추가적인 통신이 필요하기 때문에 서버의 부담이 늘어난다.

## 참고 자료

- [What Are Refresh Tokens and How to Use Them Securely - Auth0](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/)
- [Why should I store Refresh Token for JWT in the server database? Would storing access token instead be better? - ErrorsFixing](https://errorsfixing.com/why-should-i-store-refresh-token-for-jwt-in-the-server-database-would-storing-access-token-instead-be-better/)
- [쉽게 알아보는 서버 인증 2편(Access Token + Refresh Token) - 그랩의 블로그](https://tansfil.tistory.com/59?category=475681)