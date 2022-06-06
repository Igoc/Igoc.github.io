---
title: "Spring 웹 기초"

categories: [Framework, Spring]
tags: [Spring, Spring Boot, Spring MVC, Spring MVC Static Content, Spring MVC Template Engine, Spring API]

date: 2022-06-05T20:21:00+09:00
last_modified_at: 2022-06-06T09:42:00+09:00
---

Spring 웹 기초에 대한 정리
{:.notice--primary}

## 정적 콘텐츠

Spring Boot는 기본적으로 클라이언트에게 정적 콘텐츠를 제공해 줄 수 있는 기능을 지원한다.

### 예제

별도로 설정을 하지 않아도 정적 콘텐츠 디렉터리에 있는 파일을 클라이언트에게 제공해 줄 수 있다.

#### static-page - Static Content

``` html
<!DOCTYPE html>

<html>
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">

        <title>정적 웹 페이지</title>
    </head>
    <body>
        <div>정적 웹 페이지</div>
    </body>
</html>
```

### 동작 구조

<a href="https://user-images.githubusercontent.com/22683489/172049154-48a7db6f-3a3b-4b8b-9b2a-1ed8177a3994.png">
    ![정적 콘텐츠 - 동작 구조](https://user-images.githubusercontent.com/22683489/172049154-48a7db6f-3a3b-4b8b-9b2a-1ed8177a3994.png){:.align-center}
</a>

1. 웹 브라우저에서 localhost:8080/static-page.html에 GET 요청을 보낸다.
2. 내장 톰캣 서버에서 요청을 받아, 스프링에 static-page 관련 컨트롤러가 있는지 확인한다.
3. static-page 관련 컨트롤러가 없기 때문에, 정적 콘텐츠 디렉터리에서 일치하는 파일을 찾아 요청에 응답한다.

## 템플릿 엔진

Spring Boot는 [Thymeleaf](https://www.thymeleaf.org/)나 [FreeMarker](https://freemarker.apache.org/docs/) 등과 같은 템플릿 엔진을 이용하여 동적 웹 페이지를 생성할 수 있는 기능을 지원한다.

### 예제

Thymeleaf 템플릿 엔진을 이용하여 MVC 패턴으로 동적 웹 페이지를 생성할 수 있다.

#### GreetingController - Controller

``` java
@Controller
public class GreetingController {

    @GetMapping("/greeting")
    public String getGreetingTemplate(Model model) {
        model.addAttribute("name", "Spring");

        return "greeting-template";
    }
}
```

#### greeting-template - View

``` html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">

        <title>동적 웹 페이지</title>
    </head>
    <body>
        <p th:text="'Hello, ' + ${name} + '!'">Hello, world!</p>
    </body>
</html>
```

### 동작 구조

<a href="https://user-images.githubusercontent.com/22683489/172049839-96366857-fbea-443c-a983-bffd76259793.png">
    ![템플릿 엔진 - 동작 구조](https://user-images.githubusercontent.com/22683489/172049839-96366857-fbea-443c-a983-bffd76259793.png){:.align-center}
</a>

1. 웹 브라우저에서 localhost:8080/greeting에 GET 요청을 보낸다.
2. 내장 톰캣 서버에서 요청을 받아, 스프링에 greeting 관련 컨트롤러가 있는지 확인한다.
3. greeting 관련 컨트롤러가 있기 때문에, 클라이언트의 요청을 스프링에 전달한다.
4. 컨트롤러에서 모델을 생성하고 문자열을 반환하면, `ViewResolver`가 반환된 문자열과 일치하는 View를 찾아 템플릿 엔진에 넘겨준다.
5. 템플릿 엔진이 렌더링을 수행한 후 요청에 응답한다.

## API

`@ResponseBody` 어노테이션을 이용하면 HTTP 응답 메시지의 Body 부분에 데이터를 직접 반환할 수 있다.

### 예제

`@ResponseBody` 어노테이션이 있으면 반환 값이 적절한 형태로 변환되어 HTTP 응답 메시지의 Body 부분에 들어간다.

#### UserController - Controller

``` java
@Controller
public class UserController {

    @GetMapping("/user")
    @ResponseBody
    public User getUser(@RequestParam("name") String name) {
        User user = new User();
        user.setName(name);

        return user;
    }

    static class User {

        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
}
```

### 동작 구조

<a href="https://user-images.githubusercontent.com/22683489/172077441-43e2e89f-eafe-49d0-b80b-cb2ab7f4cde9.png">
    ![API - 동작 구조](https://user-images.githubusercontent.com/22683489/172077441-43e2e89f-eafe-49d0-b80b-cb2ab7f4cde9.png){:.align-center}
</a>

1. 웹 브라우저에서 localhost:8080/user에 GET 요청을 보낸다.
2. 내장 톰캣 서버에서 요청을 받아, 스프링에 user 관련 컨트롤러가 있는지 확인한다.
3. user 관련 컨트롤러가 있기 때문에, 클라이언트의 요청을 스프링에 전달한다.
4. 컨트롤러에서 User 인스턴스를 생성한 후, HTTP 응답 메시지의 Body 부분에 직접 데이터를 넣어 요청에 응답한다.

### @ResponseBody 사용 원리

`@ResponseBody` 어노테이션을 사용하면 `ViewResolver` 대신 `HttpMessageConverter`가 동작하며, HTTP 요청 메시지의 Accept Header와 컨트롤러의 반환 자료형 정보를 조합하여 적절한 `HttpMessageConverter`를 선택한다. Spring에는 여러 자료형에 대응되는 `HttpMessageConverter`가 미리 등록되어 있으며, 객체의 경우 별도로 설정하지 않으면 기본적으로 `MappingJackson2HttpMessageConverter`를 이용하여 JSON 형태로 변환한다.

## 참고 자료

- [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)