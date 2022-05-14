---
title: "JSON Web Token (JWT)"

categories: [Computer-Science, Web]
tags: [JSON Web Token, JWT]

date: 2022-05-15T00:15:00+09:00
last_modified_at: 2022-05-15T00:15:00+09:00
---

JSON Web Token (JWT)에 대한 정리
{:.notice--primary}

## JSON Web Token이란?

JSON 형식을 이용하여 통신에 사용되는 데이터를 안전하게 전송할 수 있도록 하는 개방형 표준([RFC 7519](https://datatracker.ietf.org/doc/html/rfc7519))이다.

## 디지털 서명 기반의 신뢰성

JSON Web Token을 이용하면 통신에 사용되는 데이터에 디지털 서명을 하기 때문에, 해당 데이터가 유효한 것인지 검증할 수 있다. 이러한 디지털 서명에는 HMAC SHA256 알고리즘을 적용한 비밀 키를 사용하거나, RSA나 ECDSA 알고리즘을 적용한 공개/개인 키 쌍을 사용할 수 있다.

비밀 키를 이용한 디지털 서명은 JSON Web Token을 사용하는 가장 일반적인 시나리오인 권한 인증에 사용할 수 있다. 이 경우 클라이언트가 서버에 로그인을 요청하면 서버는 비밀 키로 서명된 JSON Web Token을 발급해주며, 서버는 클라이언트로부터 전달받은 JSON Web Token에 비밀 키를 사용하여 유효한 사용자인지 검증하게 된다.

공개/개인 키 쌍을 이용한 디지털 서명은 통신 당사자 간에 데이터를 안전하게 전송하기 위해 사용할 수 있다. 이 경우 데이터 발신자는 개인 키를 이용하여 서명을 하게 되고, 데이터 수신자는 공개 키를 이용하여 발신자가 누구인지 검증할 수 있게 된다. 또한 JSON Web Token의 경우 헤더와 페이로드를 이용하여 서명을 만들어내기 때문에, 수신자는 데이터가 변조되었는지 여부도 확인할 수 있게 된다.

## 구조

`<Header>.<Payload>.<Signature>`의 형태를 가지고 있으며, 각각에 들어가는 내용은 아래와 같다.

### 헤더(Header)

헤더에는 해당 데이터가 JSON Web Token임을 나타내는 `typ` 부분과 서명에 이용된 알고리즘을 나타내는 `alg` 부분으로 구성된다. 이렇게 구성된 JSON은 Base64Url로 인코딩되어 JSON Web Token의 첫 번째 부분에 들어가게 된다.

``` json
{
    "alg": "HS256",
    "typ": "JWT"
}
```

### 페이로드(Payload)

페이로드는 사용자 정보와 같은 엔티티(Entity) 및 추가 데이터로 구성되는 클레임(Claim)이 담기는 부분이다. 클레임은 JSON Web Token에서 미리 정의해둔 등록된 클레임(Registered Claim), JSON Web Token을 이용하는 사용자가 마음대로 데이터를 넣을 수 있는 공개 클레임(Public Claim), 통신 당사자 간에 데이터를 공유하기 위해 사용할 수 있는 비밀 클레임(Private claim)으로 구성된다. 이렇게 구성된 JSON은 Base64Url로 인코딩되어 JSON Web Token의 두 번째 부분에 들어가게 된다.

``` json
{
    "sub": "1234567890",
    "name": "John Doe",
    "admin": true
}
```

주의할 점은 JSON Web Token의 경우 서명된 토큰을 이용하기 때문에 변조에 대한 보호는 가능하지만, 별도로 추가적인 암호화를 거치지 않으면 누구든 데이터를 읽을 수 있기 때문에 헤더나 페이로드 부분에 보안에 민감한 데이터를 넣어서는 안된다.

### 서명(Signature)

Base64Url로 인코딩된 헤더와 페이로드, 비밀/개인 키, 헤더에 지정된 알고리즘을 이용하여 서명을 수행하게 된다. 이러한 서명은 데이터가 통신 중간에 변조되지 않았는지 확인하기 위한 용도로 사용되며, 만약 개인 키를 이용하여 서명을 수행하였다면 JSON Web Token을 보낸 발신자가 누구인지도 확인할 수 있다. 이렇게 암호화된 정보는 Base64Url로 인코딩되어 JSON Web Token의 세 번째 부분에 들어가게 된다.

```
HMACSHA256(
    base64UrlEncode(header) + "." + base64UrlEncode(payload),
    secret
)
```

## 참고 자료

- [JSON Web Tokens - jwt.io](https://jwt.io/)