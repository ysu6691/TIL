# Django Static & Media

## 1. Static files

### 정적 파일(Static files)
- 응답할 때 별도의 처리 없이 파일 내용을 그대로 보여주면 되는 파일
- Media file: 사용자가 웹에서 업로드하는 정적 파일
- 웹 서버는 요청 받은 URL로 서버에 존재하는 정적 파일을 제공

### static files 구성하기
```python
# settings.py

# INSTALLED_APPS에 이미 포함되어 있음
INSTALLED_APPS = [
    # 생략
    'django.contrib.staticfiles',
]
```

- `STATIC_ROOT`
  - django 프로젝트에서 사용하는 모든 정적 파일을 한 곳에 모아 넣는 경로
  - collectstatic이 배포를 위해 정적 파일을 수집하는 디렉토리의 절대 경로
  - 개발 과정에서 `settings.py`의 `DEBUG` 값이 True로 설정되어 있으면, 해당 값은 작용되지 않음
  - 다음과 같은 코드를 통해, 생성된 'staticfiles' 폴더에서 미리 준비한 모든 정적 파일을 볼 수 있다. (media 파일은 `MEDIA_ROOT` 사용)
    ```python
    # settings.py
    STATIC_ROOT = BASE_DIR / 'staticfiles'
    ```
    ```bash
    $ python manage.py collectstatic
    ```

- STATICFILES_DIRS
  - `app/static/` 디렉토리 경로를 사용하는 것(기본 경로) 외에 추가적인 정적 파일 경로 목록을 정의하는 리스트
  ```python
  # settings.py
  # 최상위 폴더에 static이라는 폴더를 정적 파일 경로로 설정
  STATICFILES_DIRS = [
      BASE_DIR / 'static',
  ]
  ```

- STATIC_URL
  - STATIC_ROOT에 있는 정적 파일을 참조할 때 사용할 URL
  - 개발 단계에서는 app/static/ 경로 및 STATICFILES_DIRS에 정의된 추가 경로들을 탐색
  - 실제 파일이나 디렉토리가 아니며, URL로만 존재
  ```python
  # settings.py
  # django 시작할 때 이미 정의되어 있음
  # 비어 있지 않은 값으로 설정한다면, 반드시 '/'로 끝나야 함
  STATIC_URL = '/static/'
  ```

### static files 사용하기
- `{% load %}`
  - 특정 라이브러리, 패키지에 등록된 모든 템플릿 태그와 필터를 로드
  - static tag를 사용할 때는, `{% load static %}` 형태로 사용

- `{% static '' %}`
  - STATIC_ROOT에 저장된 정적 파일에 연결

- 첫 번째 방법:
  - 기본 경로(`app/static/`)에 있는 static file 가져오기
  - static 태그 사용시, `앱 이름/이미지파일` 형식으로 불러오면 됨
    - 참고: static 태그는 STATIC_ROOT에 연결된다. 이 때 STATIC_ROOT를 'staticfiles' 폴더로 지정해 불러오면, 폴더 내부에 `앱 이름/이미지파일` 형태로 저장되어 있는 것을 확인할 수 있다.
  - 'articles/static/articles' 경로에 이미지 파일 배치한 뒤 진행
  ```django
  <!-- articles/index.html -->
  {% extends 'base.html' %}
  {% load static %}

  {% block content %}
    <img src="{% static 'articles/google_img.jpg' %}" alt="">
    <h1>Articles</h1>
    ...
  ```

- 두 번째 방법:
  - 추가 경로(`STATICFILES_DIRS`)에 있는 static file 가져오기
  - static 태그 사용시, `이미지파일` 형식으로 바로 불러오면 됨
    - 참고: 첫 번째 방법과 마찬가지로 STATIC_ROOT를 'staticfiles' 폴더로 지정해 불러오면, 폴더 내부에 `이미지파일` 형태로 바로 저장되어 있는 것을 확인할 수 있다.
  - 최상위 폴더에 'static' 폴더를 만들고, 이미지 파일 배치하기
  ```python
  STATICFILES_DIRS = [
      BASE_DIR / 'static',
  ]
  ```
  ```django
  <!-- articles/index.html -->
  {% extends 'base.html' %}
  {% load static %}

  {% block content %}
    <img src="{% static 'sample_img.jpg' %}" alt="">
    <h1>Articles</h1>
    ...
  ```

- STATIC_URL 확인
  - 브라우저에서 개발자도구를 통해 이미지 url을 확인하면, 'STATIC_URL + static file 경로`로 설정되어 있는 것을 확인할 수 있다.


## 2. Image Upload
- 이미지 업로드에 사용하는 모델 필드
- FileField를 상속받는 서브 클래스이므로, FileField의 모든 속성 및 메소드 사용 가능
  - FileField(upload_to='', storage=None, max_length=100, **options)
- 업로드 된 객체가 유효한 이미지인지 검사

### FileField 및 ImageField 사용 준비하기
```python
# settings.py
# STATIC_ROOT와 같은 방식
# STATIC_ROOT와 반드시 다른 경로로 지정해야 함
MEDIA_ROOT = BASE_DIR / 'media'

# STATIC_URL과 같은 방식
# STATIC_URL과 반드시 다른 경로로 지정해야 함
MEDIA_URL = '/media/'
```
```python
# mypjt/urls.py
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # 생략
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
# 업로드 된 파일의 URL: settings.MEDIA_URL
# 위 URL을 통해 참조하는 파일의 실제 위치: settings.MEDIA_ROOT
```

### CREATE
```bash
# ImageField를 사용하려면 반드시 Pillow 라이브러리가 필요
# Pillow 설치 없이는 makemigrations 불가
$ pip install Pillow
```

```python
# articles/models.py

class Article(models.Model):
    # 기존 컬럼 사이에 작성해도 실제 테이블에 추가 될 때는 가장 우측(뒤)에 추가됨
    image = models.ImageField(blank=True)
    # 생략
```
- blank vs null
  - blank: True인 경우 필드를 비워 둘 때, 빈 문자열('')이 저장
  - null: True인 경우 필드를 비워 둘 때, NULL로 저장
  - **CharField, TextField와 같은 문자열 기반 필드에는 null 옵션 사용 지양**
  - '데이터 없음'에 대한 표현에 두 개의 가능한 값을 갖는 것은 좋지 않음
  - 따라서 **문자열 기반 필드에는 NULL이 아닌 빈 문자열을 사용**

```django
<!-- articles/templates/articles/create.html -->

  ...
  <h1>CREATE</h1>
  <!-- 파일 또는 이미지 업로드 시, form 태그에 enctype 속성을 변경해야 함 -->
  <form action="" method="POST" enctype="multipart/form-data">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  ...
```
```python
# articles/views.py

def create(request):
    if request.method == 'POST':
        # 파일 및 이미지는 request의 FILES 속성 값에 담겨 넘어가기 때문에, 따로 받아줘야 함
        form = ArticleForm(request.POST, request.FILES) 
        # 생략
```

### READ
```django
<!-- articles/templates/articles/detail.html -->
  ...
  {% if article.image %}
    <!-- article.image.url: 업로드 파일의 경로 -->
    <!-- article.image: 업로드 파일의 파일 이름 -->
    <img src="{{ article.image.url }}" alt="{{ article.image }}">
  {% endif %}
  <h2>DETAIL</h2>
  ...
```

### UPDATE
```django
<!-- articles/templates/articles/update.html -->
  ...
  <h1>UPDATE</h1>
  <!-- enctype 속성 값 추가 -->
  <form action="{% url 'articles:update' article.pk %}" method="POST" enctype="multipart/form-data">
  ...
```
```python
# articles/views.py

def update(request, pk):
    article = Article.objects.get(pk=pk)
    if request.user == article.user:
        if request.method == 'POST':
            # request.FILES 추가
            form = ArticleForm(request.POST, request.FILES, instance=article)
            # 생략
```

### upload_to 인자 사용
- upload_to 속성을 정의하여 업로드 된 파일에 사용할 MEDIA_ROOT의 하위 경로 지정
- `서버 url/해당 주소`로 요청을 보내면 해당 이미지를 응답
- 업로드 디렉토리와 파일 이름을 설정하는 2가지 방법이 존재
  - 문자열 값이나 경로 지정
    ```python
    # 예시 1
    # articles/models.py
    # MEDIA_ROOT/images 내부에 저장
    class Article(models.Model):
        image = models.ImageField(blank=True, upload_to='images/')
        # 생략
    ```
    ```python
    # 예시 2
    # articles/models.py
    # MEDIA_ROOT/해당 날짜 폴더에 저장
    class Article(models.Model):
        image = models.ImageField(blank=True, upload_to='%Y/%m/%d/')
        # 생략
    ```
  - 함수 호출
    ```python
    # articles/models.py

    # 함수 사용 시 반드시 2개의 인자를 받음
    # instance: FileField가 정의된 모델의 인스턴스
    # filename: 기존 파일 이름
    # MEDIA_ROOT/images/username/파일이름 형식으로 저장
    def articles_image_path(instance, filename):
        return f'images/{instance.user.username}/{filename}'

    class Article(models.Model):
        image = models.ImageField(blank=True, upload_to=articles_image_path)
        # 생략
    ```

### Image Resizing
- 실제 원본 이미지를 서버에 그대로 로드하는 것은 여러 이유로 부담이 큼
- HTML `<img>` 태그에서 사이즈를 조정할 수도 있지만, 업로드 될 때 이미지 자체를 resizing 가능
- django-imagekit 모듈 설치 : 이미지 처리를 위한 Django 앱(썸네일, 해상도, 사이즈 등 조정 가능)
  ```bash
  $ pip install django-imagekit
  ```
  ```python
  # settings.py

  INSTALLED_APPS = [
      'articles',
      'accounts',
      'django_extensions',
      'imagekit',
      # 생략
  ]
  ```

- 썸네일을 만드는 2가지 방법
  - 원본 이미지 저장x
    ```python
    # articles/models.py

    from imagekit.processors import Thumbnail
    from imagekit.models import ProcessedImageField

    class Article(models.Model):
        user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
        title = models.CharField(max_length=10)
        content = models.TextField()
        # ProcessedImageField()의 파라미터들은 변경이 되더라도 다시 makemigrations 할 필요 x
        image = ProcessedImageField(
            blank=True,
            upload_to='thumbnails/'
            processors=[Thumbnail(200,300)],
            format='JPEG',
            options={'quality': 80},
        )
        # 생략
    ```
    ```django
    <!-- articles/detail.html -->
    ...
    <!-- resizing한 이미지 출력 -->
    <img src="{{ article.image.url }}" alt="{{ article.image }}">
    ...
    ```
  - 원본 이미지 저장 o
    ```python
    from imagekit.processors import Thumbnail
    from imagekit.processors import ImageSpecField

    class Article(models.Model):
      user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
      title = models.CharField(max_length=10)
      content = models.TextField()
      image = models.ImageField(blank=True)
      image_thumbnail = ImageSpecField(
          source='image',
          processors=[Thumbnail(200, 300)],
          format='JPEG',
          options={'quality': 80},
      )
    ```
    ```django
    <!-- articles/detail.html -->
    ...
    <!-- 원본 이미지 출력 -->
    <img src="{{ article.image.url }}" alt="{{ article.image }}">
    <!-- resizing한 이미지 출력 -->
    <!-- resizing한 이미지는 사용되었을 때만 저장 (CACHE/images 폴더) -->
    <img src="{{ article.image_thumbnail.url }}" alt="{{ article.image_thumbnail }}">
    ...
    ```
