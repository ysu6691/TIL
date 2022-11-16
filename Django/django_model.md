# Django Model

## 1. Database

### Database
- 체계화된 데이터의 모임
- Database의 기본 구조
  - 스키마(Schema)
  - 테이블(Table)

- 스키마(Schema)
  - 뼈대(Structure)
  - 데이터베이스에서 자료의 구조, 표현 방법, 관계 등을 정의한 구조

- 테이블(Table)
  - 필드와 레코드를 사용해 조직된 데이터 집합
  - 필드(field): 속성, 열(column)
  - 레코드(record): 튜플, 행(row)

- PK(Primary Key)
  - 각 레코드의 고유한 값(식별자로 사용)
  - **다른 항목과 절대로 중복되어 나타날 수 없는 단일 값**을 가짐
  - 예시: 주민등록번호, 비밀번호 등

- 쿼리(Query)
  - 데이터를 조회하기 위한 명령어를 일컬음
  - 조건에 맞는 데이터를 추출하거나 조작하는 명령어
  - "Query를 날린다" -> "데이터베이스를 조작한다"


## 2. Django Model

### Model
- Django는 model을 통해 데이터에 접근하고 조작(데이터베이스는 django에 포함x)
- 일반적으로 각각의 모델은 하나의 데이터베이스 테이블에 매핑(mapping)
  - 모델 클래스 1개 == 데이터베이스 테이블 1개
  - 모델 클래스를 작성하는 것은 데이터베이스 테이블의 **스키마를 정의**하는 것
- Model 작성
  ```python
  # articles/models.py
  from django.db import models

  class Article(models.Model):
    # DB 필드 이름 = 데이터 타입
    title = models.CharField(max_length=10)
    content = models.TextField()
  ```
- [Django model field](https://docs.djangoproject.com/en/3.2/ref/models/fields)
  - CharField: 길이의 제한이 있는 문자열을 넣을 때 사용 (**max_length는 필수 인자**)
  - TextField: 길이 제한 x
  - DateTimeField: 날짜 및 시간을 값으로 사용하는 필드
    - auto_now_add: 최초 생성 일자
    - auto_now: 최종 수정 일자

### Migrations
- migration 3단계
  1) models.py 추가 및 변경
  2) makemigrations
  3) migrate
- makemigrations: models에 작성한 스키마를 바탕으로, 설계도를 작성하는 것
  - migrations 폴더에 파일 생성
  ```bash
  $ python manage.py makemigrations
  ```
- migrate: 작성한 설계도를 sqlite DB파일에 반영하는 과정
  ```bash
  $ python manage.py migrate
  ```
- showmigrations: migrations 파일들이 migrate 됐는지 확인 (X 표시가 완료되었다는 의미)
  ```bash
  $ python manage.py showmigrations
  ```
- sqlmigrate: 해당 migrations 파일이 SQL 문으로 어떻게 해석될지 미리 확인
  ```bash
  $ python manage.py sqlmigrate articles 0001
  ```

### 추가 필드 정의
- Django에서 한 레코드의 모든 필드 값은 빈 칸이 없어야 함
- models.py 안에 클래스 내 필드를 추가하고 makemigrations 실행 시, 추가적인 필드에 대한 설정 값을 정하는 과정 진행
  - '1' 입력 시: 새 필드의 기본 값을 직접 입력
    - 그 후 enter 입력 시 default 값 자동 지정
  - '2' 입력 시: 나가서 모델 필드에 default 속성을 직접 작성

### ORM
- Object-Relational-Mapping
- 객체 지향 프로그래밍 언어와 데이터베이스 간의 데이터를 변환하는 기술
- DB를 객체로 조작 가능
- 하지만 ORM 만으로 완전한 서비스를 구현하기 어려운 경우가 있음 -> SQL 사용


## 3. QuerySet API

### QuerySet API
- Database API
  - model을 만들면 DB를 객체로서 다룰 수 있는 DB API가 자동 생성
  - manager: Django 모델이 DB 쿼리 작업을 가능하게 하는 인터페이스
  - 구문: Article.objects.all()
    - Article: Model class
    - objects: Manager, DB를 python class로 조작할 수 있도록 여러 메서드를 제공
    - all(): Queryset API

- QuerySet
  - 데이터베이스에게서 전달 받은 데이터 모음
  - Query: 데이터베이스에 특정 데이터를 요청하는 것
  - QuerySet: 데이터베이스로부터 ORM이 QuerySet이라는 자료 형태로 변환해 전달해줌

<img src="https://user-images.githubusercontent.com/109272360/187707539-883c7726-4be0-46ff-b017-50fd02875cca.png" width="600px" style="margin-top:16px; margin-bottom:16px;">

### CRUD
- 데이터 처리 기능 4가지: Create / Read / Update / Delete
- 기능 확인을 위해 `ipython`, `django-extensions` 라이브러리 설치
  - `settings.py` 내 `INSTALLED-APPS` 안에 `'django_extensions'` 추가
  - Shell을 이용해 장고 프로젝트 환경에 영향을 주지 않는 선에서 다양한 명령어 실행 가능
  ```bash
  # django_extensions 라이브러리 설치
  $ pip install django-extensions
  ```
  ```python
  # settings.py

  INSTALLED_APPS = [
    'articles',
    'django_extensions',
    # 생략
  ]
  ```
  ```bash
  $ python manage.py shell_plus
  ```
- Create(생성)
  ```bash
  # 첫 번째 방법
  >>> article = Article() # Article Class로부터 article instance 생성
  >>> article.title = 'first' # 인스턴스 변수에 값을 할당
  >>> article.content = 'django' # 인스턴스 변수에 값을 할당
  >>> article.save() # DB에 레코드 생성

  # article은 인스턴스이기 때문에, 속성값 확인 가능
  >>> article.title
  # 'first'
  >>> article.pk
  # 1
  ```

  ```bash
  # 두 번째 방법
  >>> article = Article(title='second', content='django')
  >>> article.save()
  ```

  ```bash
  # 세 번째 방법
  >>> Article.objects.create(title='third', content='django')
  ```

- READ
  ```bash
  # 전체 데이터 조회
  >>> Article.objects.all()
  # <QuerySet [<Article: Article object (1)>, 생략]>


  # get: pk와 같이 고유성을 보장하는 조회에서 사용해야 함 (단일 데이터 조회)
  >>> Article.objects.get(pk=1)
  # <Article: Article object (1)>

  >>> Article.objects.get(pk=100)
  # DoesNotExist: Article matching query doew not exist.

  >>> Article.objects.get(content='django')
  # MultipleObjectsReturned: get() returned more than one Article -- it returned 2!


  # filter: 매개 변수와 일치하는 객체를 포함하는 새 QuerySet을 반환
  >>> Article.objects.filter(content='django')
  # <QuerySet [<Article: Article object (1)>, 생략]>

  >>> Article.objects.filter(content='python')
  <QuerySet []> # 데이터 없어도 반환

  >>> Article.objects.filter(content__contains='dj')
  # <QuerySet [<Article: Article object (1)>, 생략]>

  # pk를 역순으로 반환
  >>> Article.objects.filter(content__contains).order_by('-pk')
  # <QuerySet [<Article: Article object (3)>, 생략]>
  ```

- Update
  ```bash
  >>> new = Article.objects.get(pk=1)
  >>> new.title = 'new'
  >>> new.save()
  >>> new.title
  # 'new'
  ```

- Delete
  ```bash
  >>> new = Article.objects.get(pk=1)
  >>> new.delete() # 삭제
  # (1, {'articles.Article': 1}) <- 이제 1번 pk는 다시 생성 x>
  >>> Article.objects.get(pk=1)
  # DoesNotExist: Article matching query doew not exist.
  ```

### 웹사이트에서 DB 이용해보기
- new 템플릿에서 데이터 받아, index 템플릿에 나타내기
```python
# articles/urls.py
from django.urls import path
from . import views

app_name = 'articles'
urlpatterns = [
    path('', views.index, name = 'index'),
    path('new/', views.new, name = 'new'),
    path('<int:number>/', views.detail, name = 'detail'), # 해당 pk 값에 해당하는 주소로보내기 
    path('create/', views.create, name = 'create'),
]
```

```python
# articles/views.py
from .models import Article

def index(request):
    # new.html에서 바로 데이터를 받아 index 함수에서 저장을 하게 되면,
    # 저장하지 않고 그냥 index.html로 들어가는 경우에도 저장을 하려고 함
    # -> 저장할 데이터가 없으므로 오류 발생
    # 따라서 create 함수 별도로 생성해줘야 함 
    articles = Article.objects.all()
    context = {
        'articles' : articles,
    }
    return render(request, 'articles/index.html', context)

def new(request):
    return render(request, 'articles/new.html')

# new에서 입력한 데이터를 전송받아 DB에 저장할 create 함수 생성
def create(request):
    # 다음과 같은 저장 방식을 쓰는 것이 유리
    # 유효성 검사가 진행된 후에 save 메서드가 호출되는 구조
    title = request.GET.get('title')
    content = request.GET.get('content')
    article = Article(title=title, content=content)
    article.save()
    return redirect('articles:detail', article.pk)

def detail(request, number):
    article = Article.objects.get(pk=pk)
    context = {
      'article' : article
    }
    return render(request, 'articles/detail.html', context)
```

```django
<!-- templates/articles/index.html -->
{% extends 'base.html' %}

{% block content %}
  <h1>INDEX</h1>
  {% for article in articles %}
    <p>번호: {{ article.pk }}</p>
    <p>제목: {{ article.title }}</p>
    <p>내용: {{ article.content }}</p>
    <!-- pk값 포함해서 detail url로 보내기 -->
    <a href="{% url 'articles:detail' article.pk %}">DETAIL</a>
    <hr>
  {% endfor %}
  <a href="{% url 'articles:new' %}">NEW</a>
{% endblock content %}
```

```django
<!-- templates/articles/new.html -->
{% extends 'base.html' %}

{% block content %}
  <h1>NEW</h1>
  <form action="{% url 'articles:create' %}" method="GET">
    <label for="title">TITLE: </label>
    <input type="text" name="title" id="title"> <br>
    <label for="content">CONTENT: </label>
    <textarea name="content" id="content" rows="10" cols="30"></textarea> <br>
    <input type="submit" value="작성"> <br>
  </form>
  <hr>
  <a href="{% url 'articles:index' %}">BACK</a>
{% endblock content %}
```

```django
<!-- templates/articles/detail.html -->
{% extends 'base.html' %}

{% block content %}
  <h1>DETAIL</h1>
  <p>번호: {{ article.pk }}</p>
  <p>제목: {{ article.title }}</p>
  <p>내용: {{ article.content }}</p>
  <hr>
  <a href="{% url 'articles:index' %}">BACK</a>
{% endblock content %}
```

## 4. Admin site
- admin site: 서버 관리자가 활용하기 위한 페이지
- **http://127.0.0.1:8000/admin/** 으로 접속 후 로그인

- admin 계정 생성
  ```bash
  $ python manage.py createsuperuser
  ```

- model 클래스 등록
```python
# articles/admin.py
from django.contrib import admin
from .models import Article

admin.site.register(Article)
```

- admin 비밀번호 변경
```bash
$ python manage.py changepassword 계정ID
```