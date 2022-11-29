# Django CORS

## 1. CORS

### SOP(Same-Origin Policy)
- 동일 출처(origin) 정책
- 한 origin으로부터 로드된 document 또는 script가 다른 origin의 리소스와 상호작용 할 수 있는 방법을 제한하는 보안 메커니즘
- 요청하는 클라이언트의 origin과 요청받는 서버의 origin이 같으면 동일 출처, 다르면 다른 출처 요청이다.
- script 태그, XMLHttpRequest, Fetch API 등은 기본적으로 SOP를 따른다.
- 여기서 origin은 **프로토콜(스킴) + 호스트 + 포트번호**를 의미한다. (세 가지 조건이 모두 같으면 동일 출처로 인식한다.)
  <img src="https://user-images.githubusercontent.com/109272360/204426265-93133161-87bb-4e2a-b101-4e8c386e6391.png" width="550px" style="margin: 16px 0px;">
  ```js
  // 현재 document의 origin을 확인할 수 있다.
  docoment.location.origin
  ```
> **만약 SOP가 없다면?**
> 해커가 본인의 홈페이지에 사용자의 메일을 정보를 받아오는 document를 심어놨다고 가정하자.
> 사용자가 구글 메일에 접속한 상태로 해당 홈페이지에 접속하는 순간, 유저의 브라우저에서 구글 메일로 쿠키와 함께 XMLHttpRequest를 보낸다.
> 해커 서버: https://badhacker.com / 구글 서버: https://mail.google.com
> 만약 SOP가 없다면 구글 서버는 쿠키를 통해 유저인 것을 확인하고 메일 정보를 응답하게 되고, 해커의 서버는 해당 내용을 인코딩해서 본인의 서버로 전송한다.
> 하지만 SOP가 있다면 구글 서버는 메일 정보를 응답은 하지만, 브라우저에서 document와 구글 서버의 origin이 서로 다른 것을 확인하고 응답을 차단한다.

### CORS(Cross-Origin Resource Sharing)
- 출처가 다른 자원들을 공유한다는 의미로, 서버에서 서버의 자원에 접근할 수 있는 권한을 브라우저에 부여하는 체제이다.
- 추가 HTTP 헤더를 사용하여 어떤 origin에서 자신의 자원을 불러갈 수 있는지 서버에서 지정하는 방법이다.
- CORS의 작동 방식은 simple, preflight, credentialed 이렇게 세 가지로 나눌 수 있다.

### CORS의 세 가지 작동 방식
- 단순 요청(simple request)
  - 예비 요청(preflight)을 생략하고 바로 서버에 본 요청을 보낸다.
  - 이후에 서버가 응답의 헤더에 Access-Control-Allow-Origin 헤더를 보내주면 브라우저가 CORS 정책 위반 여부를 검사한다.
  - 단순 요청은 아래 세 가지 조건을 만족해야 한다.
    - 요청의 메소드는 `GET`, `POST`, `HEAD` 중 하나여야 한다.
    - `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, `DPR`, `Downlink`, `Save-Data`, `Viewport-Width`, `Width` 헤더일 경우에만 적용된다.
    - Content-Type 헤더가 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain`중 하나여야한다.

- 예비 요청(preflight request)
  - 예비 요청을 보내 서버와 잘 통신되는지 확인한 후 본 요청을 보낸다.
  - 이때 예비 요청의 HTTP 메소드는 `OPTIONS`이다.
  - 예비 요청 방식은 다음과 같이 동작한다.
    1. 브라우저에서 서버로 HTTP OPTIONS 메소드로 예비 요청을 보낸다.
      - Origin 헤더에 자신의 출처를 넣는다.
      - Access-Control-Request-Method 헤더에 실제 요청에 사용할 메소드를 설정한다.
      - Access-Control-Request-Headers 헤더에 실제 요청에 사용할 헤더들을 설정한다.
    2. 서버는 예비 요청에 대한 응답으로 어떤 것을 허용하는지에 대한 헤더 정보를 담아 브라우저로 보낸다.
      - Access-Control-Allow-Origin 헤더에 허용되는 Origin들의 목록을 설정한다.
      - Access-Control-Allow-Methods 헤더에 허용되는 메소드들의 목록을 설정한다.
      - Access-Control-Allow-Headers 헤더에 허용되는 헤더들의 목록을 설정한다.
      - Access-Control-Max-Age 헤더에 해당 예비 요청이 브라우저에 캐시 될 수 있는 시간을 초 단위로 설정한다.
    3. 브라우저는 요청과 서버의 응답해준 정책을 비교해 해당 요청이 안전한지 확인하고 본 요청을 보낸다.
    4. 서버가 본 요청에 대한 응답을 한다.

- 인증된 요청(credentialed request)
  - 서버에 요청할 때 자격 인증 정보(credential)을 실어 요청할 때 사용되는 요청이다.
  - 자격 인증 정보란 세션 ID가 저장되어있는 쿠키 혹은 Authorization 헤더에 설정하는 토큰 값 등을 말한다.
  - 즉, 자격 인증 정보를 포함해서 요청을 보내게 되면 **인증된 요청**으로 동작하며, 다음과 같이 통신한다.
    - credentials 옵션 설정
      - axios의 경우
        ```js
        axios({
          method: 'post',
          url: 'https://example.com:1234/users/login',
          profile: {
            username: username,
            password: password
          },
          withCredentials: true // 기본값은 false
        })
        .then()
        .catch()

        // 다음과 같이 전역 설정도 가능하다
        // axios.defaults.withCredentials = true;
        ```
      - fetch의 경우
        ```js
        // same-origin(기본값): 같은 출처 간 요청에만 인증 정보를 담는다.
        // include: 모든 요청에 대해 인증 정보를 담는다.
        // omit: 모든 요청에 인증 정보를 담지 않는다.
        fetch("https://example.com:1234/users/login", {
          method: "POST",
          credentials: "include",
        })
        ```
    - 서버에서 인증된 요청에 대한 헤더 설정하기
      - 응답 헤더의 Access-Control-Allow-Credentials 항목을 true로 설정해야 한다.
      - 응답 헤더의 Access-Control-Allow-Origin 의 값에 와일드카드 문자("*")는 사용할 수 없다.
      - 응답 헤더의 Access-Control-Allow-Methods 의 값에 와일드카드 문자("*")는 사용할 수 없다.
      - 응답 헤더의 Access-Control-Allow-Headers 의 값에 와일드카드 문자("*")는 사용할 수 없다.

## 2. Django에서 CORS 설정하기
```bash
$ pip install django-cors-headers
```

```python
# settings.py

INSTALLED_APPS = [
  'corsheaders',
]


MIDDLEWARE = [
  # CorsMiddleWare는 CommonMiddleWare보다 먼저 정의되어야 함
  'corsheaders.middleware.CorsMiddleware',
  'django.middleware.common.CommonMiddleware',
]


# 특정 origin만 선택적으로 허용
CORS_ALLOWED_ORIGINS = [
  'http://localhost:8000',
]
# 모든 origin 허용
# CORS_ALLOWED_ALL_ORIGINS = True
```
