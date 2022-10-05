# Django Authentication

## 1. Django Authentication System
- 인증(Authentication)
  - 신원 확인
- 허가(Authorization)
  - 권한 부여


### Custom User Model
- Django에서는 built-in User model을 제공
- 하지만 프로젝트에 따라 custom User model이 필요한 경우가 많음
  - ex) 회원가입 시 username 대신 email을 식별 값으로 사용
- 따라서 현재 프로젝트에서 사용할 User model을 재정의해야 함
- 모델 관계에 영향을 미치기 때문에, **프로젝트 처음에 진행하는 것을 권장**
  - 프로젝트 중간일 경우, 데이터베이스 초기화 과정
    1. migrations 파일 삭제(번호가 붙은 파일만)
    2. db.sqlite3 삭제
    3. makemigrations-migrate 진행

```bash
# accounts 앱 추가
# Django 내부적으로 경로나 키워드를 accounts라는 이름으로 사용하기 때문에, accounts로 지정하는 것을 권장 
$ python manage.py startapp accounts
```

```python
# accounts/models.py
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```

```python
# setting.py
AUTH_USER_MODEL = 'accounts.User'
```

```python
# account/admin.py
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin
from .models import User

admin.site.register(User, UserAdmin)
```

## 2. HTTP Cookies

### HTTP
- Hyper Text Transfer Protocol
- 웹(www)에서 이루어지는 모든 데이터 교환의 기초
- HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜
- HTTP 특징
  - 비연결 지향(connectionless): 서버는 요청에 대한 응답을 보낸 후 연결을 끊음
  - 무상태(stateless): 연결을 끊는 순간 클라이언트와 서버간의 통신이 끝나며 상태 정보가 유지되지 않음
  - 따라서 로그인과 같은 상태를 유지하기 위해 **쿠키**와 **세션**이 존재

### Cookie
- 서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각
- 브라우저는 쿠키를 로컬에 KEY-VALUE의 데이터 형식으로 저장해두었다가, 동일한 서버에 재요청시 저장된 쿠키를 함께 전송
- 쿠키 사용 목적
  - 세션 관리(로그인, 아이디 자동완성, 장바구니 등)
  - 개인화(사용자 선호, 테마 등 설정)
  - 트래킹(사용자 행동 기록 및 분석)
- 쿠키의 수명
  - Session cookie: 현재 세션이 종료되면 삭제(브라우저 종료와 함께 삭제)
  - Persistent cookie: Expires 속성에 지정된 날짜 혹은 Max-Age 속성에 지정된 기간이 지나면 삭제

### Session
- 사이트와 특정 브라우저 사이의 상태를 유지시키는 것
- 클라이언트가 서버에 접속하면 특정 session id를 발급받고, session id를 쿠키에 저장
- 따라서 session id는 세션을 구별하기 위해 필요하며, 쿠키에는 session id만 저장
- Django는 database-backed sessions 저장 방식을 기본 값으로 사용
  - session 정보는 Django DB의 django_session 테이블에 저장

## 3. Authentication in Web requests
- Django는 사용자를 나타내는 모든 요청에 request.user를 제공 (로그인하지 않은 경우 AnonymousUser 클래스의 인스턴스)
- Django는 built-in forms를 제공하므로, forms에 따로 지정하지 않고 호출을 통해 사용 가능
  - 하지만 일부 form은 기존 user 모델을 참조하기 때문에, custom user 모델을 사용할 경우 다시 작성해야 함
  - custom user model과 호환되는 form
    - AuthenticationForm, SetPasswordForm, PasswordChangeForm, AdminPasswordChangeForm
  - custom user model과 호환되지 않는 form
    - UserCreationForm, UserChangeForm
- 따라서 form이 필요한 view 함수의 경우, 이전에 사용했던 방식으로 템플릿에서 form 출력 옵션을 이용해 사용 가능

### Login
- 로그인은 Session을 Create하는 과정
- login()함수를 사용
```python
# accounts/views.py
from django.contrib.auth.forms import AuthenticationForm
from django.contrib.auth import login as auth_login 
# import한 login은 함수로 사용할 login과 같은 이름을 사용하기 때문에, auth_login으로 변경

def login(request):
    if request.method == 'POST':
        form = AuthenticationForm(request, request.POST) # model form이 아니기 때문에, 첫번째 인자로 request를 받음
        if form.is_valid():
            # auth_login(request, 유저정보(AuthenticationForm에서 제공하는 함수))
            auth_login(request, form.get_user())
            return redirect('articles:index')
    else:
      form = AuthenticationForm()
    context = {
      'form': form
    }
    return render(request, 'accounts/login.html', context)
```

- 템플릿에서 인증 관련 데이터를 출력 가능
  - 현재 로그인한 사용자를 나타내는 User 클래스의 인스턴스가 템플릿 변수 user에 저장됨
  - context 없이도 user 변수를 사용가능한 것은, setting.py의 context processors 설정 값 때문
  - `{{ user.정보 }}`를 이용해 유저 정보에 접근 가능
    - ex) {{ user.email }}, {{ user.first_name }}

  ```django
  <!-- base.html -->
  <body>
    <h1>Hello, {{ user }}</h1>
    <% block content %>
    <% endblock content %>
  ```

### Logout
- 로그아웃은 Session을 Delete하는 과정
- logout()함수를 사용
  - 반환값이 없으며, 로그인하지 않은 경우 오류를 발생시키지 않음
  - 현재 요청에 대한 session data를 DB에서 삭제
  - 클라이언트의 쿠키에서도 session id를 삭제
```python
# accounts/views.py
from django.contrib.auth import logout as auth_logout
# logout 함수도 login과 마찬가지로 이름 변경

def logout(request):
    auth_logout(request)
    return redirect('articles:index')
```

### 회원가입
- 회원가입은 DB에 유저를 Create하는 과정
- UserCreationForm을 사용
  - 세 개의 필드를 가짐 (username, password1, password2)
- 하지만 UserCreationForm은 custom user model과 호환되지 않으므로, custom form 사용 필수
  - 기존 필드 외에 추가로 받고자 하는 정보도 추가 가능
  - [User Model Fields](https://docs.djangoproject.com/en/3.2/ref/contrib/auth/#user-model)

```python
# accounts/forms.py
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth import get_user_model # 간접 참조
# from .models import User <- 이렇게 직접 참조도 가능하지만, 권장하지 않음

class CustomUserCreationForm(UserCreationForm):

    # class Meta(UserCreationForm.Meta):
    #     model = User <- 직접 참조하지 않기 때문에, 이렇게 사용 불가

    class Meta(UserCreationForm.Meta):
        # get_user_model(): 현재 프로젝트에서 활성화된 user model을 반환
        model = get_user_model()
        fields = UserCreationForm.Meta.fields + ('email', 'first_name', 'last_name')
        # fields 변수는 작성안할 시 기존 form으로 생성 (아이디와 비밀번호)
        # fields 변수 작성 시, 사용 가능한 field 중에 원하는 field 추가 가능
        # 기존 튜플 형태로 저장되어 있는 필드(UserCreationForm.Meta.fields)에 더하기
```

```python
# accounts/views.py
from .forms import CustomUserCreationForm

def signup(request):
    if request.method == "POST":
        # UserCreationForm은 DB에 저장을 하기 때문에, ModelForm 임
        # 따라서 첫 번째 인자로 request.POST 사용 
        form = CustomUserCreationForm(request.POST)
        if form.is_valid():
            user = form.save() # 저장된 객체를 return 받아,
            auth_login(request, user) # 회원가입한 유저 바로 로그인 시키기
            # auth_login(request, form.get_user()) <- 여기서 form은 UserCreationForm의 인스턴스이므로 사용 불가
            return redirect('articles:index')
    else:
        form = CustomUserCreationForm()
    context = {
        'form' : form
    }
    return render(request, 'accounts/signup.html', context)
```

### 회원 탈퇴
- 회원 탈퇴는 DB에서 유저를 Delete하는 과정
```python
# accounts/views.py

def delete(request):
    request.user.delete()
    # 회원 탈퇴 후 로그아웃 시키기
    # 로그아웃 먼저 하고 회원 탈퇴 시키려고 하면, 요청 객체 정보가 없어지기 때문에 순서 중요
    auth_logout(request) 
    return redirect('articles:index')
```

### 회원정보 수정
- 회원정보 수정은 DB에서 유저 정보를 Update하는 과정
- UserChangeForm을 사용
- 회원가입과 마찬가지로 custom user model과 호환되지 않으므로, custom form 사용 필수

```python
# accounts/forms.py
from django.contrib.auth.forms import UserChangeForm
from django.contrib.auth import get_user_model

class CustomUserChangeForm(UserChangeForm):

    class Meta(UserChangeForm.Meta):
        model = get_user_model()
        # fields 지정 안 하면, 일반 사용자가 접근해서는 안 되는 field 마저 수정 가능해짐
        fields = ('email', 'first_name', 'last_name')
```

```python
# accounts/views.py
from .forms import CustomUserChangeForm

def update(request):
    if request.method == "POST":
        form = CustomUserChangeForm(request.POST, instance=request.user)
        if form.is_valid():
            form.save()
            return redirect('articles:index')
    else:
        form = CustomUserChangeForm(instance=request.user)
    context = {
        'form' : form
    }
    return render(request, 'accounts/update.html', context)
```

### 비밀번호 변경
- PasswordChangeForm 사용
```python
from django.contrib.auth.forms import PasswordChangeForm
from django.contrib.auth import update_session_auth_hash

def change_password(request):
    if request.method == "POST":
        form = PasswordChangeForm(request.user, request.POST)
        if form.is_valid():
            form.save()
            # 비밀번호가 변경되면 기존 세션과의 인증 정보가 달라져, 로그인이 유지되지 못함
            # 따라서 session data를 업데이트 해줘야 함
            update_session_auth_hash(request, form.user)
            # user = form.save()
            # update_session_auth_hash(request, user)
            # 도 가능하지만, 위 방식을 권장
            return redirect('articles:index')
    else:
        form = PasswordChangeForm(request.user)
    context = {
        'form' : form
    }
    return render(request, 'accounts/change_password.html', context)
```

## 4. Limiting access
- 로그인 사용자와 로그인하지 않은 사용자 접근 관리 가능
  - templates: `is_authenticated` 속성
  - views: `login_required` 데코레이터 / `is_authenticated` 속성

- is_authenticated
  - User model의 속성 중 하나
  - 로그인 되어있으면 True를 반환
  - request.user.is_authenticated를 사용해 로그인 되어있는지 확인 가능

```django
<!-- base.html -->
{% if request.user.is_authenticated %}
  <h3>Hello, {{ user }}</h3>
  <form action="{% url 'accounts:logout' %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="Logout">
  </form>
  <a href="{% url 'accounts:update' %}">회원정보수정</a>
  <form action="{% url 'accounts:delete' %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="회원탈퇴">
  </form>

{% else %}
  <a href="{% url 'accounts:login' %}">Login</a>
  <a href="{% url 'accounts:signup' %}">Signup</a>

{% endif %}
  <h4><a href="{% url 'articles:index' %}">HOME</a></h4>
<hr>

{% block content %}
{% endblock content %}
```

```django
<!-- articles/templates/articles/index.html -->
{% extends 'base.html' %}

{% block content %}
  <h1>Articles</h1>
  {% if request.user.is_authenticated %}
    <a href="{% url 'articles:create' %}">CREATE</a>
  {% else %}
    <a href="{% url 'articles:create' %}">새 글을 작성하려면 로그인하세요.</a>
  {% endif %}
  <hr>
  <!-- 생략 -->
```

```python
# accounts/views.py
def login(request):
    # 로그인 되어있으면, 로그인 url을 입력해도 index로 이동
    if request.user.is_authenticated:
        return redirect('articles:index')
    if request.method == 'POST':
        # 생략
```

- login_required
  - 사용자가 로그인되어 있으면 정상적으로 view 함수를 실행
  - 로그인되어 있지 않으면 setting.py의 LOGIN_URL 문자열 주소로 redirect
    - LOGIN_URL의 기본 값은 /accounts/login (재설정 필요 x)
    - url은 /accounts/login/?next=주소 형태로 저장되는데, 이후 로그인이 진행되면 이전에 요청했던 주소로 redirect 하기 위함임
    - 따라서 views.py 내 login 함수에 해당 값을 처리하면, 원래 요청했던 주소로 이동 가능
    - next query string parameter 사용 시, login.html 에서 form action을 해제해야 함 (next 파라미터로 보내지지 x)

```python
# articles/views.py
from django.views.decorators.http import require_http_methods, require_POST, require_safe
from django.contrib.auth.decorators import login_required

@require_safe
def index(request):
    pass

@login_required
@require_http_methods(['GET', 'POST'])
def create(request):
    pass

@require_safe
def detail(request, pk):
    pass

# POST method만 허용하는 함수에는 login_required 사용 불가
# login 함수에서 next로 redirect하게 되면 GET method로 전달되므로,
# @require_POST를 사용해 GET method를 받지 못하게 됨
# 따라서 함수 내부에서 처리해줘야 함
@require_POST
def delete(request, pk):
    if request.user.is_authenticated:
        article = Article.objects.get(pk=pk)
        article.delete()
    return redirect('articles:index')

@login_required
@require_http_methods(['GET', 'POST'])
def update(request, pk):
    pass
```

```python
# accounts/views.py
from django.views.decorators.http import require_http_methods, require_POST, require_safe
from django.contrib.auth.decorators import login_required

@require_http_methods(['GET', 'POST'])
def login(request):
    if request.user.is_authenticated:
        return redirect('articles:index')
    if request.method == 'POST':
        form = AuthenticationForm(request, request.POST)
        if form.is_valid():
            auth_login(request, form.get_user())
            # 'next'가 존재하면 next로, 아니면 index로 (단축평가)
            return redirect(request.GET.get('next') or 'articles:index')
    # 생략

@require_POST
def logout(request):
    if request.user.is_authenticated:
        auth_logout(request)
    return redirect('articles:index')

@require_http_methods(['GET', 'POST'])
def signup(request):
    pass

@require_POST
def delete(request):
    if request.user.is_authenticated:
        request.user.delete()
        auth_logout(request) 
    return redirect('articles:index')

@login_required
@require_http_methods(['GET', 'POST'])
def update(request):
    pass

@login_required
@require_http_methods(['GET', 'POST'])
def change_password(requese):
    pass
```

### 참고) 에러 응답하기
```python
# articles/views.py
from django.http import httpResponse

@require_POST
def delete(request, pk):
    if request.user.is_authenticated:
        article = Article.objects.get(pk=pk)
        article.delete()
        return redirect('articles:index')
    # 401: Unauthorized
    # 비인증된 사용자라는 의미로 응답
    return HttpResponse(status=401)
```