# Django Basis

## 1. 클라이언트와 서버

### 클라이언트-서버 구조
- 클라이언트
  - 웹 사용자의 인터넷에 연결된 장치
  - 웹 브라우저
  - 서비스를 요청하는 주체(request)

- 서버
  - 웹 페이지, 사이트 또는 앱을 저장하는 컴퓨터
  - 요청에 대해 서비스를 응답하는 주체(responses)

- Django는 서버를 구현하는 웹 프레임워크
  - 프레임워크: 서비스 개발에 필요한 기능들을 미리 구현해서 모아 놓은 것

### 웹 브라우저와 웹 페이지
- 웹 페이지
  - 정적 웹 페이지: HTML 파일의 내용이 변하지 않음
  - 동적 웹 페이지: 사용자의 요청에 따라 서버가 웹 페이지 내용을 바꿔, 클라이언트에게 전달

- 웹 브라우저
  - 웹 페이지 코드를 받으면 눈에 보이는 화면으로 바꿔주는 프로그램
  - 렌더링(rendering): 웹 페이지 파일을 우리가 보는 화면으로 바꾸는 것

## 2. Django의 구조 (MTV Design Pattern)

### Design Pattern이란?
- 다양한 응용 소프트웨어를 개발할 때, 설계 문제와 해결책에서 공통점(pattern)이 존재
  - ex) 로그인, 회원가입 등
- 디자인 패턴은 자주 사용되는 소프트웨어의 구조를 미리 구조화한 것
- 커뮤니케이션의 효율성이 증가함

### MTV 패턴
- Django의 디자인 패턴: MTV 패턴
- Model
  - 데이터와 관련된 로직을 관리
  - 데이터베이스의 기록을 관리
- Template
  - 레이아웃과 화면을 처리
- View
  - 클라이언트의 요청에 대해 Model과 Template과 관련된 로직을 처리해 응답을 반환

<img src="https://user-images.githubusercontent.com/109272360/187707531-04ef0001-c4fe-4af6-a570-2a3218c6f281.png" width="850px" style="margin-top:16px; margin-bottom:16px;">

- MVC와 MTV의 차이점
  |MVC|MTV|
  |-|-|
  |Model|Model|
  |View|Template|
  |Controller|View|

## 3. Django의 시작

### 기본 설정
- Django Project 생성
  ```bash
  $ django-admin startproject firstpjt .
  ```
  - 프로젝트 생성 시 최초 1회만 입력
  - Python이나 Django에서 사용 중인 키워드 및 '-' 사용 불가
  - '.'을 붙이지 않을 경우 현재 디렉토리에 프로젝트 디렉토리를 새로 생성하게 됨

- 프로젝트 구조
  - `__init__.py`
    - 현재 디렉토리를 하나의 python 패키지로 다루도록 설정
  - `asgi.py`
    - Asynchronous Server Gateway Interface
    - Django 애플리케이션이 비동기식 웹 서버와 연결 및 소통하는 것을 도움
  - `settings.py`
    - Django 프로젝트 설정을 관리
  - `urls.py`
    - 사이트의 url과 적절한 views의 연결을 지정
  - `wsgi.py`
    - Web Server Gateway Interface
    - Django 애플리케이션이 웹 서버와 연결 및 소통하는 것을 도움
  - `manage.py`
    - Django 프로젝트와 상호작용하는 커맨드라인 유틸리티
    - `python manage.py <command> [options]`

- Django Application 생성
  ```bash
  $ python manage.py startapp articles
  ```
  - 일반적으로 애플리케이션 이름은 **복수형**으로 작성하는 것을 권장

- 애플리케이션 구조
  - `admin.py`
    - 관리자용 페이지를 설정하는 곳
  - `apps.py`
    - 앱의 정보가 작성된 곳
  - `modesl.py`
    - 애플리케이션에서 사용하는 model을 정의하는 곳
  - `tests.py`
    - 프로젝트의 테스트 코드를 작성하는 곳
  - `views.py`
    - view 함수들이 정의되는 곳
  - `templates` 폴더
    - html 파일이 저장

- 애플리케이션 등록
  - `settings.py`에서 `INSTALLED_APPS` 리스트에 해당 애플리케이션을 추가
  - **반드시 애플리케이션을 생성하고 등록해야 함**
  ```python
  # settings.py
  INSTALLED_APPS = [
    'articles',
    'django.contrib.admin',
    # 생략
  ]
  ```

- 서버 실행
  ```
  $ python manage.py runserver
  ```

### 요청과 응답
- 데이터 흐름의 순서에 따라 URL -> View -> Template 순으로 작성
- URL
  ```python
  # urls.py
  from django.contrib import admin
  from django.urls import path
  from articles import views # 애플리케이션 폴더 내 views.py 불러오기

  urlpatterns = [
      path('admin/', admin.site.urls),
      path('index/', views.index),
      # index/ : 해당 웹 페이지 주소 정보
      # views.index : view 파일 내 index 함수 실행
  ]
  ```

- View
  - render
    - 주어진 템플릿을 주어진 컨텍스트 데이터와 결합
    - 렌더링된 텍스트와 함께 HttpResponse 객체를 반환하는 함수
    - 정상적으로 동작하면 200 status code를 응답
  - redirect
    - 템플릿을 렌더링하지 않고, url에 추가 정보를 포함해서 보냄
    - 정상적으로 동작하면 302 status code를 응답
  - context
    - 템플릿에서 사용할 데이터
    - 딕셔너리 타입으로 작성
  
  ```python
  # articles 폴더 내 views.py
  from django.shortcuts import render

  # HTTP 요청을 수신하고 응답을 반환하는 함수 작성
  # request는 다른 문자로 바꿔도 되지만, 관행적으로 통일해서 사용
  def index(request):
      # context도 마찬가지로, 관행적으로 통일해서 사용
      context = {

      }
      return render(request, 'index.html', context)
      # render의 첫 번째 인자는 request, 두 번째 인자는 템플릿, 세 번째 인자는 context

  def create(request):
      # articles/index url로 10을 포함해 전송
      # 추가로 html 파일을 만들지 않고 바로 정보를 보내거나 함수를 실행시킬 때 사용
      return redirect('/articles/index', 10)
  ```

- Templates
  - app 폴더 안에 templates 폴더 생성
  - **폴더명은 반드시 templates**로 지정
  - templates 폴더 안에 html 파일 생성
  ```django
  <!-- articles/templates/index.html -->
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <!-- 생략 -->
  </head>
  <body>
      <h1>index 웹 페이지</h1>
  </body>
  </html>
  ```

- 추가 설정
  - LANGUAGE_CODE: 사용자에게 제공되는 번역을 결정 (ex. 'ko-kr')
  - TIME_ZONE: 데이터베이스 연결의 시간대를 나타내는 문자열 지정 (ex. 'Asia/Seoul')
  - USE-I18N: 번역 시스템을 활성화해야 하는지 여부를 지정
  - USE_L10N: 데이터의 지역화 된 형식을 기본적으로 활성화할지 여부를 결정
  - USE_TZ: datetimes가 기본적으로 시간대를 인식하는지 결정

## 4. Django Template

### Django Template Language(DTL)
- 여러 유용한 기능을 제공
- Variable
  - `{{ 변수명 }}`으로 사용
  - 변수명은 영어, 숫자, 밑줄의 조합으로 구성
  - 공백이나 구두점 문자 사용 불가
  - `.`을 사용하여 변수 속성에 접근 가능
  - render()의 세 번째 인자로 딕셔너리 형태로 전달받아, key에 해당하는 value를 변수로 사용
  - 예시
    ```python
    # articles/views.py
    def index(request):
        foods = ['apple', 'banana', 'pineapple']
        info = {
            'name' : 'Yun'
        }
        context = {
            'foods' : foods,
            'info' : info,
        }
        return render(request, 'index.html', context)
    ```
    ```django
    <!-- articles/templates/index.html -->
    <p>첫 번째 음식 : {{ foods.0 }}</p>
    <p>제 이름은 {{ info.name }}입니다.</p>
    ```

- Filters
  - `{{ 변수명|조건 }}`으로 사용
  - 변수를 조건에 맞게 수정
  - 60개의 bulit-in template filters를 제공
  - 예시
    ```python
    # articles/views.py
    def index(request):
        info = {
            'name' : 'Yun'
        }
        context = {
            'info' : info,
        }
      return render(request, 'index.html', context)
    ```
    ```django
    <!-- articles/templates/index.html -->
    <p>제 이름은 {{ info.name|length }}자입니다.</p>
    ```
  
- Tags
  - `{% tag %}`
  - 출력 텍스트를 만들거나 반복, 조건문 사용 가능
  - 일부 태그는 시작과 종료 태그가 필요
  - 약 24개의 bulit-in template tags를 제공
  - [Tags and Filters](https://docs.djangoproject.com/en/4.1/ref/templates/builtins/#built-in-filter-reference)
  - 예시
    ```python
    # articles/views.py
    def index(request):
        foods = ['apple', 'banana', 'pineapple']
        context = {
            'foods' : foods,
        }
        return render(request, 'index.html', context)
    ```
    ```django
    <!-- articles/templates/index.html -->
    <p>{% if 'apple' in foods %}사과가 있다.{% endif %}</p>
    ```
- Comments
  - 주석을 표현할 때 사용
  - 한 줄 : `{# 문장 #}`
  - 여러 줄 : `{% comment %} 문단 {% endcomment %}`
  - 예시
    ```django
    <!-- articles/templates/index.html -->
    {% comment %}
      <p>주석</p>
    {% endcomment %}
    ```

### Template inheritance
- 템플릿 상속을 이용하면 skeletion 템플릿을 이용 가능
- `{% extends 부모 템플릿명 %}`으로 템플릿 확장 알림 (**반드시 템플릿 최상단에 작성되어야 함**)
- `{% block 블록명 %}{% endblock 블록명 %}`으로 자식 템플릿이 채워지는 공간 지정
- 예시
  ```django
  <!-- articles/templates/base.html -->
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
  </head>
  <body>
    <h1>base html 스켈레톤 템플릿을 공유</h1>
    {% block content %}
    <!-- 여기에 내용 들어감 -->
    {% endblock content %}
  </body>
  </html>
  ```
  ```django
  <!-- articles/templates/index.html -->
  {% extends 'base.html' %}

  {% block content %}
    <p>index 본문</p>
  {% endblock content %}
  ```
  - 프로젝트 내 모든 템플릿에 적용할 경우
    - 프로젝트 최상단에 templates 폴더 생성
    - templates 폴더 내 상속할 base.html 생성
    - `settings.py` 내 `DIRS` 변경
    ```python
    # settings.py
    TEMPLATES = [
      {
          'BACKEND': 'django.template.backends.django.DjangoTemplates',
          'DIRS': [BASE_DIR / 'templates'],
          # 생략
      },
    ]
    ```

## 5. form data

### Sending data
- HTML form
  - 사용자로부터 할당된 데이터를 서버로 전송
  - 데이터를 어디(action)로 어떤 방식(method)로 보낼지 결정
  - action
    - 입력 데이터가 전송될 URL을 지정
    - 속성을 지정하지 않으면, 현재 페이지의 URL로 보내짐
  - method
    - 데이터를 어떻게 보낼지 정의
    - HTML form 데이터는 GET 방식과 POST 방식으로만 전송 가능

- HTML input
  - 사용자로부터 데이터를 입력받기 위해 사용
  - name 속성에 설정된 값을 서버로 전송하고, 서버는 그 값을 통해 사용자가 입력한 데이터 값에 접근
  - name은 key, value는 value로 매핑

- GET
  - 서버로부터 정보를 조회하는 데 사용(서버에게 리소스 요청)
  - 데이터를 가져올 때만 사용해야 함
  - 데이터를 서버로 전송할 때 Query String Parameters를 통해 전송
    - Query String Parameters란?
      - 사용자가 입력 데이터를 전달하는 방법 중 하나
      - URL 주소에 데이터를 파라미터를 통해 넘기는 것
      - `?key=value&key=value/`형식으로 구성

### Retrieving data
- 서버는 클라이언트로부터 key-value 쌍의 데이터를 받게 됨
- django의 경우, 모든 요청 데이터는 view 파일 내 지정한 함수의 첫 번째 인자(request)에 들어 있음
- request에서 GET 또는 POST를 이용해 데이터를 얻음으로써, 적절하게 사용 가능
  - GET: DB에 변화를 주지 않고, 데이터를 읽을 때만 사용
  - POST: 서버나 DB에 변경을 줄 때 사용 / 신원 확인을 위해 token을 부여받아야 함
    - `{% csrf_token %}`을 html 파일 내 form 태그 밑에 작성

- request 객체 살펴보기
  ```python
  # 예시: '전송된 데이터'를 retrieve.html로 전송
  # articles/views.py
  def retrieve(request):
      print(request)
      print(type(request))
      print(request.GET)
      print(request.GET.get('message'))
      '''
      <WSGIRequest: GET '/catch/?message=%EC%A0%84%EC%86%A1%EB%90%9C+%EB%8D%B0%EC%9D%B4%ED%84%B0'>
      <class 'django.core.handlers.wsgi.WSGIRequest'>
      <QueryDict: {'message': ['전송된 데이터']}>
      전송된 데이터
      '''
      return render(request, 'retrieve.html')
  ```
  ```python
  # 강제로 에러를 발생시켜 request 객체를 볼 수 있음
  def retrieve(request):
    raise
    return render(request, 'retrieve.html')
  ```

### 데이터를 주고 받기 예시
```python
# urls.py
urlpatterns = [
    path('throw/', views.throw),
    path('catch/', views.catch)
]
```
```python
# articles/views.py
def throw(request):
    return render(request, 'throw.html')

def catch(request):
    # GET으로 데이터 보냈을 경우, POST 대신 GET 사용
    message = request.POST.get('message')
    context = {
        'message': message
    }
    return render(request, 'catch.html', context)
```

```django
<!-- articles/templates/throw.html -->
{% extends 'base.html' %}

{% block content %}
  <h1>Throw</h1>
  <!-- 같은 폴더 내 catch.html로 데이터 보내기 -->
  <form action="/catch/" method="POST"> <!-- POST는 소문자도 가능하지만, 대문자 사용을 권장 -->
    {% csrf_token %}
    <label for="throwMessage">Throw</label>
    <input type="text" id="throwMessage" name="message">
    <input type="submit">
  </form>
{% endblock content %}
```
```django
<!-- articles/templates/catch.html -->
{% extends 'base.html' %}

{% block content %}
  <h1>Catch</h1>
  <p>전송받은 데이터: {{ message }}</p>
  <!-- <a> tag는 GET method로 전달 -->
  <a href="/throw/">데이터 다시 던지러 가기</a>
{% endblock content %}
```

## 6. Django URL

### Trailing URL Slashes
- Django는 URL 긑에 '/'를 붙여줘야 함 (없으면 자동으로 붙여주도록 설정)
- 기술적인 측면에서 URL 끝에 '/'가 있는지 없는지에 따라 서로 다른 URL이 된다고 설명

### Variable routing
- URL의 일부를 변수로 지정하여 view 함수의 인자로 넘길 수 있음
- 변수 값에 따라 하나의 path()에 여러 페이지를 연결시켜, 비슷한 템플릿을 여러 개 만들지 않도록 설정 가능
- Variable routing 작성
  - str: 비어 있지 않은 모든 문자열 / 작성하지 않을 경우 기본 값
  - int: 0 또는 양의 정수와 매치

```python
# urls.py
urlpatterns = [
    path('hello/<name>/', views.hello),
]
```
```python
# articles/views.py
def hello(request, name):
    context = {
        'name' : name
    }
    return render(request, 'hello.html', context)
```
```django
{% extends 'base.html' %}

{% block content %}
  <h1>안녕하세요 {{name}}입니다.</h1>
{% endblock content %}
```

### App URL mapping
- 앱이 많아지면 view 함수와 path()가 많아지면서 유지보수 어려움
- 각 app마다 urls.py를 생성해 mapping 가능

```python
# firstpjt/urls.py <- 프로젝트 폴더>
from django.urls import path, include

urlpatterns = [
    path('articles', include('articles.urls')),
    path('pages/', include('pages.urls')),
]
```
```python
# articles/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('article1', views.article1),
    path('article2', views.article2),
]
```
```python
# pages/urls.py
from django.urls import path
# include되는 앱의 urls.py에는 urlpatterns가 빈 리스트더라도 작성되어 있어야 에러 발생x
urlpatterns = [

]
```

### Naming URL patterns
- 만약 어떠한 주소를 변경해야 할 경우, 그 주소를 사용한 모든 곳을 찾아서 변경해야 함
- 이를 방지하기 위해, `urls.py` 내 `path()` 함수에 `name`을 사용
- 사용한 `name`은 템플릿 내에서 `{% url '' %}`으로 사용 가능
- 예시
  ```python
  # articles/urls.py
  urlpatterns = [
      path('throw/', views.throw, name='throw')
      path('catch/', views.catch, name='catch')
  ]
  ```
  ```django
  <!-- throw.html -->
  {% extends 'base.html' %}

  {% block content %}
    <h1>Throw</h1>
    <form action="{% url 'catch' %}" method="GET">
      <!-- 생략 -->
    </form>
  {% endblock content %}
  ```
  ```django
  <!-- catch.html -->
  {% extends 'base.html' %}

  {% block content %}
    <h1>Catch</h1>
    <p>전송받은 데이터: {{ message }}</p>
    <a href="{% url 'throw' %}">데이터 다시 던지러 가기</a>
  {% endblock content %}
  ```

## 7. Django Namespace

### URL Namespace
- 서로 다른 앱에서 동일한 url을 사용하는 경우, 다른 앱의 해당 url로 이동 불가(urls.py에서 위에서부터 탐색해서 먼저 찾은 url 선택)
- 따라서 `app_name`을 지정해, 두 앱의 템플릿을 분리해줘야 함
- app_name을 사용하면, 템플릿 내에서도 app_name을 반드시 사용해야 함
```python
# articles/urls.py
app_name = 'articles'
urlpatterns = [
  # 생략
]
```
```python
# pages/urls.py
app_name = 'pages'
urlpatterns = [
  # 생략
]
```
```django
<!-- throw.html -->
{% block content %}
  <h1>Throw</h1>
  <form action="{% url 'articles:catch' %}" method="GET">
    <!-- 생략 -->
  </form>
{% endblock content %}
```

### Template Namespace
- Django는 templates 폴더 내 파일만 찾을 수 있음
- 같은 이름의 템플릿이 여러 앱에 있는 경우, django는 settings 내 앱 등록 순서에 따라 순차 검색
- 따라서 중복을 피하기 위해, templates 폴더 내 해당 앱과 같은 이름의 폴더를 생성해야 함
- 생성 후, views.py 내에서 해당 경로 수정
- 예시
  - articles/templates/articles/index.html
  - pages/templates/pages/index.html
  ```python
  # articles/views.py
  def index(request):
      return render(request, 'articles/index.html')
  ```


