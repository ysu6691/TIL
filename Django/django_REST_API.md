# Django REST API

## 1. REST API

### HTTP Request Methods
- GET
  - 서버에 리소스의 표현을 요청
  - GET을 사용하는 요청은 데이터만 검색해야 함
- POST
  - 데이터를 지정된 리소스에 제출
  - 서버의 상태를 변경
- PUT
  - 요청한 주소의 리소스를 수정
- DELETE
  - 지정된 리소스를 삭제

### HTTP response status codes
- Informational responses (100-199)
- Successful responses (200-299)
- Redirection messages (300-399)
- Client error responses (400-499)
- Server error responses (500-599)

### URI
- Uniform Resource Identifier(통합 자원 식별자)
- 웹에서 리소스를 식별하기 위한 문자열
  - 리소스: HTTP 요청의 대상(문서, 사진 등 다양한 것이 될 수 있음)
- 대표적인 URI
  - URL: 네트워크 상에서 리소스의 위치로 리소스를 식별
  - URN: 특정 이름공간에서 고유한 이름으로 리소스를 식별

### URL
- Uniform Resource Locator(통합 자원 위치)
- 네트워크 상에 리소스가 어디 있는지, 그 주소를 알려주기 위한 약속
- URL 구조
  - Scheme(or protocol): 브라우저가 리소스를 요청하는 데 사용해야 하는 프로토콜
    - ex) https, http
  - Authority: domain과 port를 ':'를 사이에 두고 작성
    - ex) www.example.com:80 
    - domain: 요청 중인 웹 서버를 나타내며, IP 주소를 직접 사용하는 것도 가능하다.
    - port: 웹 서버의 리소스에 접근하는데 사용되는 기술적인 문(Gate)이다. HTTP와 HTTPS의 표준 포트는 생략이 가능하다.
  - Path: 웹 서버의 리소스 경로(물리적 위치x)
    - ex) articles/create/
  - Parametres: 웹 서버에 제공하는 추가적인 데이터로, '&'로 구분되는 key-value 쌍 목록으로 구성되어 있다.
    - ex) ?key=value
  - Anchor: 일종의 '북마크'를 나타내며, 브라우저에게 해당 북마크 지점에 있는 콘텐츠를 표시
    - ex) #quick-start
    - 부분 식별자라고 부르는 '#' 이후 부분은 서버에 전송되지 않음

### REST API
- API
  - Application Programming Interface
  - 애플리케이션과 프로그래밍으로 소통하는 방법

- Web API
  - 웹 서버 또는 웹 브라우저를 위한 API
  - API는 다양한 타입의 데이터를 응답
    - ex) HTML, XML, JSON
  - Open API: 개발자라면 누구나 사용할 수 있도록 공개된 API
  - 대표적인 Open API 서비스: Youtube API, Naver Papago API, Kakao Map API

- REST
  - Representational State Transfer
  - API Server를 개발하기 위한 일종의 소프트웨어 설계 방법론
  - REST 원리를 따르는 시스템을 **RESTful**하다고 부름
  - **REST의 기본적인 아이디어는 리소스를 정의하고 리소스에 대한 주소를 지정하는 것**

- REST에서 자원을 정의하고 주소를 지정하는 방법
  - 자원의 식별: URI
  - 자원의 행위: HTTP Method
  - 자원의 표현: JSON

- JSON
  - JavaScript의 표기법을 따른 단순 문자열
  - 사람이 읽고 쓰기 쉽고, 기계가 파싱(해석 & 분석)하고 만들어내기 쉽기 때문에, 현재 가장 많이 사용

## 2. Response JSON

### 서버의 응답
- 서버에는 html 파일 외에도 다양한 데이터 타입을 응답 가능
- 이제는 JSON 파일을 응답하고 Front-end Framework가 그 데이터를 받아 화면을 구성하는 방법 사용
- Django에서 생성하는 QuerySet, 모델 인스턴스 등을 JSON으로 변환하기 위해서는 **Serialization**이 필요

### Serialization
- 쿼리셋,모델 인스턴스 등의 복잡한 데이터를 JSON, XML등의 컨텐트 타입으로 쉽게 변환 가능한 python datatype으로 변환시켜준다.
- 과정: 다양한 데이터 -> Serialization -> Serialized data -> JSON

### Response
- 다양한 방법으로 JSON 데이터를 응답할 수 있다.
- 첫 번째 방법: `JsonResponse()` 사용
  - JsonResponse: Django가 제공하는 객체로, 다양한 자료구조를 json으로 변환한다.
  ```python
  # articles/views.py

  from django.http.response import JsonResponse

  def article_json(request):
      articles = Article.objects.all()
      articles_json = []

      for article in articles:
          articles_json.append(
              {
                  'id': article.pk,
                  'title': article.title,
                  'content': article.content,
                  'created_at': article.created_at,
                  'updated_at': article.updated_at,
              }
          )

      # safe: 기본 값은 True로, False로 설정 시 모든 타입의 객체를 serialization 가능
      # True로 할 때는 딕셔너리 형태만 가능
      return JsonResponse(articles_json, safe=False)
  ```

- 두 번째 방법: `HttpResponse()`, `serializers` 사용
  - HttpResponse: 함수의 결과값을 객체에 담아 응답한다.
  - serializers: QuerySet, 모델 인스턴스 등의 데이터를 JSON, XML 등의 타입으로 쉽게 변환시켜준다.
  ```python
  # articles/views.py

  from django.http.response import JsonResponse, HttpResponse
  from django.core import serializers
  
  def article_json(request):
      articles = Article.objects.all()
      # 파이썬 객체를 바로 json으로 변환할 수 없음
      # 따라서 serialize 과정을 거쳐야 함
      # serialized된 파이썬 데이터를 json으로 변환
      data = serializers.serialize('json', articles)
      return HttpResponse(data, content_type='application/json')
  ```

- 세 번째 방법: **Django REST framework** 사용
  - Django REST framework(DRF): Django에서 RESTful API 서버를 쉽게 구축할 수 있도록 도와주는 오픈소스 라이브러리
  ```python
  # settings.py

  INSTALLED_APPS = [
      'articles',
      'rest_framework',
      # 생략
  ]
  ```

  ```python
  # articles/serializers.py
  # 반드시 위의 파일명을 지킬 필요는 없지만, 관행적으로 serializers.py에 작성

  from rest_framework import serializers
  from .models import Article

  class ArticleSerializer(serializers.ModelSerializer):

      class Meta:
          model = Article
          fields = '__all__'
  ```

  ```python
  # articles/views.py

  # api_view 데코레이터는 반드시 작성
  # DRF view 함수가 응답해야 하는 HTTP 메소드 목록을 받음
  # 기본적으로 GET 메소드만 허용되며, 다른 메소드 요청에는 405 Method Not Allowed로 응답
  @api_view(['GET'])
  def article_json(request):
      articles = Article.objects.all()
      # ArticleSerializer(): serialized된 데이터 객체를 생성
      serializer = ArticleSerializer(articles, many=True)
      return Response(serializer.data)
  ```

  ### ModelSerializer
- ModelSerializer
  - Model 정보에 맞춰 자동으로 필드를 생성
  - serializer에 대한 유효성 검사기(`is_valid()`)를 자동으로 생성
  - `.create()` 및 `.update()`의 간단한 기본 구현이 포함

## 3. DRF - Single Model

### Article

- 2가지 URL로 7가지 기능을 만들 수 있다.
- 같은 URL이지만 request method로 행동을 정의 (method로 URL을 식별)
  ||GET|POST|PUT|DELETE|
  |--|--|--|--|--|
  |articles/|전체 글 조회|글 작성|전체 글 수정|전체 글 삭제|
  |articles/1/|1번 글 조회|-|1번 글 수정|1번 글 삭제|

- **전체 글 조회(GET)**

  ```python
  # articles/serializers.py

  from rest_framework import serializers
  from .models import Article, Comment

  # 객체 목록(QuerySet)을 serialize할 클래스
  class ArticleListSerializer(serializers.ModelSerializer):

      class Meta:
          model = Article
          # 어떤 필드를 serialize할지 정하기
          fields = ('id', 'title', 'content',)
  ```

  ```python
  # articles/urls.py

  urlpatterns = [
      # 템플릿을 안 만들기 때문에, name 지정 x
      path('articles/', views.article_list),
  ]
  ```

  ```python
  from rest_framework.response import Response
  from rest_framework.decorators import api_view

  from .models import Article
  from .serializers import ArticleListSerializer

  # 빈 값이면 GET이 기본값 (하지만 쓰는 것을 권장)
  @api_view(['GET'])
  def article_list(request):
      articles = Article.objects.all()
      # serializer = serialized data
      # many: QuerySet 또는 객체 목록을 serialize 하려면 True로 설정해야 함
      #       단일 객체일때만 False로 설정
      serializer = ArticleListSerializer(articles, many=True)
      return Response(serializer.data)
  ```

- **단일 글 조회(GET)**
  ```python
  # articles/serializers.py

  # 단일 객체를 serialize할 클래스
  # 같은 모델을 설정한 serializer가 있더라도 다른 필드로 구성하고 싶을 때는 새로운 클래스를 생성해야 함.
  class ArticleSerializer(serializers.ModelSerializer):

      class Meta:
          model = Article
          fields = '__all__'
  ```

  ```python
  # articles/urls.py

  urlpatterns = [
      path('articles/', views.article_list),
      path('articles/<int:article_pk>/', views.article_detail),
  ]
  ```

  ```python
  # articles/views.py

  from .serializers import ArticleListSerializer, ArticleSerializer

  @api_view(['GET'])
  def article_detail(request, article_pk):
      article = Article.objects.get(pk=article_pk)
      serializer = ArticleSerializer(article)
      return Response(serializer.data)
  ```

- **게시글 생성(POST)**
  ```python
  # articles/views.py

  from rest_framework import status

  @api_view(['GET', 'POST'])
  def article_list(request):
      # GET일 때와 POST일 때를 분기
      if request.method == 'GET':
          articles = Article.objects.all()
          serializer = ArticleListSerializer(articles, many=True)
          return Response(serializer.data)
          
      # article_detail 함수는 article_pk가 필요하므로,
      # article을 생성할 때 사용할 수 없음 -> 이 함수에 작성
      elif request.method == 'POST':
          # 첫 번째 인자가 data가 아니므로, data를 필수적으로 써줘야 함
          # serializer = ArticleSerializer(data=request.POST)도 가능
          serializer = ArticleSerializer(data=request.data)
          # raise_exception: 유효하지 않은 데이터에 대해 HTTP 400 응답을 반환
          if serializer.is_valid(raise_exception=True):
              serializer.save()
              # 데이터 생성이 성공했을 경우: 201 Created 상태 코드 응답
              return Response(serializer.data, status=status.HTTP_201_CREATED)
          # raise_exception 대신 사용 가능
          # 데이터 생성이 실패했을 경우: 400 Bad request 상태 코드 응답
          # return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
  ```

- **게시글 삭제(DELETE)**
  ```python
  # articles/views.py

  @api_view(['GET', 'DELETE'])
  def article_detail(request, article_pk):
      article = Article.objects.get(pk=article_pk)
      # GET일 때와 DELETE일 때를 분기
      if request.method == 'GET':
          serializer = ArticleSerializer(article)
          return Response(serializer.data)

      # 게시글을 지울 때 article_pk가 필요하므로, 이 함수에 작성
      elif request.method == 'DELETE':
          article.delete()
          # 데이터가 없음을 의미하는 204 코드 반환
          return Response(status=status.HTTP_204_NO_CONTENT)
  ```

- **게시글 수정(PUT)**
  ```python
  # articles/views.py

  @api_view(['GET', 'DELETE', 'PUT'])
  def article_detail(request, article_pk):
      # 생략
      # 수정할 게시물을 지정할 때 article_pk가 필요하므로, 이 함수에 작성
      elif request.method == 'PUT':
          # serializer = ArticleSerializer(instance=article, data=request.data)도 가능
          # serializer = ArticleSerializer(data=request.PUT)은 불가능
          serializer = ArticleSerializer(article, data=request.data)
          if serializer.is_valid(raise_exception=True):
              serializer.save()
              # status code는 200이기 때문에, 별도로 작성 x
              return Response(serializer.data)
  ```

## 4. DRF - N:1 Relation

### Comment

- **전체 댓글 조회(GET)**

  ```python
  # articles/serializers.py

  from .models import Article, Comment

  class CommentSerializer(serializers.ModelSerializer):

      class Meta:
          model = Comment
          fields = '__all__'
          # read_only_fields를 사용해 외래 키 필드를 설정
          # 해당 필드를 유효성 검사에서 제외시키고, 데이터 조회 시에만 출력되도록 설정
          read_only_fields = ('article',)
  ```

  ```python
  # articles/urls.py

  urlpatterns = [
      # 생략
      path('comments/', views.comment_list),
  ]
  ```

  ```python
  # articles/views.py

  from .models import Article, Comment
  from .serializers import ArticleListSerializer, ArticleSerializer, CommentSerializer

  @api_view(['GET'])
  def comment_list(request):
      comments = Comment.objects.all()
      serializer = CommentSerializer(comments, many=True)
      return Response(serializer.data)
  ```

- **단일 댓글 조회(GET)**
  ```python
  # articles/urls.py

  urlpatterns = [
      # 생략
      path('comments/<int:comment_pk>/', views.comment_detail),
  ]
  ```

  ```python
  # articles/views.py

  @api_view(['GET'])
  def comment_detail(request, comment_pk):
      comment = Comment.objects.get(pk=comment_pk)
      serializer = CommentSerializer(comment)
      return Response(serializer.data)
  ```

- **댓글 생성(POST)**

  ```python
  # articles/urls.py

  urlpatterns = [
      # 생략
      path('articles/<int:article_pk>/comments/', views.comment_create),
  ]
  ```

  ```python
  # articles/views.py

  # 댓글을 생성할 때는 article_pk가 필요하기 때문에 새로운 함수로 작성
  @api_view(['POST'])
  def comment_create(request, article_pk):
      article = Article.objects.get(pk=article_pk)
      serializer = CommentSerializer(data=request.data)
      if serializer.is_valid(raise_exception=True):
          # save(): 파라미터로 article 객체를 넘겨 함께 저장
          serializer.save(article=article)
          return Response(serializer.data, status=status.HTTP_201_CREATED)
  ```

- **댓글 삭제(DELETE) & 댓글 수정(PUT)**
```python
# articles/views.py

@api_view(['GET', 'DELETE', 'PUT'])
def comment_detail(request, comment_pk):
    comment = Comment.objects.get(pk=comment_pk)
    if request.method == 'GET':
        serializer = CommentSerializer(comment)
        return Response(serializer.data)
    
    elif request.method == 'DELETE':
        comment.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)

    elif request.method == 'PUT':
        serializer = CommentSerializer(comment, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)
```

### N:1 - 역참조 데이터 조회
- 기존 필드 override
  - Serializer는 기존 필드를 override 할 수 있음
  - PrimaryKeyRelatedField: 역참조 후 id만 출력
    - ex) 게시글 조회 시 해당 게시글의 모든 댓글 id도 함께 출력
    ```python
    # articles/serializers.py

    class ArticleSerializer(serializers.ModelSerializer):
        # 1이 N을 참조하므로 many=True로 설정
        # read_only 설정
        comment_set = serializers.PrimaryKeyRelatedField(many=True, read_only=True)
        # 변수명은 역참조 시 생성되는 related manager로 작성
        # 따라서 models.py에서 related_name을 통해 변경한 뒤에만 변경 가능

        class Meta:
            model = Article
            fields = '__all__'
            # 특정 필드를 override 혹은 추가한 경우, read_only_fields가 동작하지 않음 (테이블에 물리적으로 존재하지 x)
            # 따라서 read_only 파라미터를 각각 추가해줘야 함
    ```

  - Nested relationships: 역참조 후 모든 정보 출력
    - 원하는 정보만 조회하고 싶은 경우에는 2가지 방법이 있다.
      - 기존 역참조 serializer를 변경(기존 serializer를 이용하던 함수들도 영향을 받는다.)
      - 새롭게 역참조할 serializer 생성
    - ex) 게시글 조회 시 해당 게시글의 모든 댓글 정보 출력
    ```python
    # articles/serializers.py

    class CommentSerializer(serializers.ModelSerializer):
        # 생략

    class ArticleSerializer(serializers.ModelSerializer):
        # 역참조할 클래스가 이 클래스 위에 위치해야 함
        comment_set = CommentSerializer(many=True, read_only=True)

        class Meta:
            model = Article
            fields = '__all__'
    ```

- 새로운 필드 추가
  - ex) 게시글 조회 시 해당 게시글의 댓글 개수까지 함께 출력
    ```python
    # articles/serializers.py

    # 단일 객체를 serialize할 클래스
    class ArticleSerializer(serializers.ModelSerializer):
        # 이름 아무거나 설정해도 무방
        # source: 필드를 채우는 데 사용할 속성의 이름(점 표기법을 사용해 속성을 탐색)
        comment_count = serializers.IntegerField(source='comment_set.count', read_only=True)

        class Meta:
            model = Article
            fields = '__all__'
    ```

## 5. Django shortcuts functions
Django는 함수 실행 시 return되지 못하고 오류가 발생하면, HTTP 500을 응답한다.
따라서 서버가 적절한 예외 처리를 하고 클라이언트에게 올바른 에러를 전달하는 방법을 익혀야 한다.

### get_object_or_404()
- get()의 결과를 반환하고, 해당 객체가 없으면 HTTP 404를 응답
- `.object.get()` 대신 사용 가능
```python
# articles/views.py

from django.shortcuts import get_object_or_404

@api_view(['GET', 'DELETE', 'PUT'])
def article_detail(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    # 생략

@api_view(['GET', 'DELETE', 'PUT'])
def comment_detail(request, comment_pk):
    comment = get_object_or_404(Comment, pk=comment_pk)
    # 생략

@api_view(['POST'])
def comment_create(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    # 생략
```

### get_list_or_404()
- filter()의 결과를 반환하고, 해당 객체 목록이 없으면 HTTP 404를 응답
- `.objects.all()` 대신 사용 가능
```python
# articles/views.py

from django.shortcuts import get_list_or_404

@api_view(['GET', 'POST'])
def article_list(request):
    if request.method == 'GET':
        articles = get_list_or_404(Article)
        # 생략

@api_view(['GET'])
def comment_list(request):
    comments = get_list_or_404(Comment)
```
