# Django Relational Database

## 1. 관계형 데이터베이스(RDB)

### RDB에서의 관계
- 1:1(One-to-one relationships)
  - 한 테이블의 레코드 하나가 다른 테이블의 레코드 단 한개와 관련된 경우
- N:1(Many-to-one relationships)
  - 한 테이블의 0개 이상의 레코드가 다른 테이블의 레코드 한 개와 관련된 경우
  - 기준 테이블에 따라(1:N, One-to-many relationships)이라고도 함
- N:N(Many-to-many relationships)
  - 한 테이블의 0개 이상의 레코드가 다른 테이블의 0개 이상의 레코드와 관련된 경우
  - 양쪽 모두에서 N:1 관계를 가짐

### Foreign Key
- 외부 키(외래 키, FK)
- 관계형 데이터베이스에서 한 테이블의 필드 중 다른 테이블의 행을 식별할 수 있는 키
- 부모(참조되는) 테이블의 유일한 값을 참조[참조 무결성]
- 따라서 보통 부모 테이블의 기본 키를 가리키지만, 반드시 기본 키일 필요는 없음

### Django Relationship fields
- OneToOneField(): 1:1
- ForeignKey(): N:1
- ManyToManyField(): N:N

## 2. N:1
<img src="https://user-images.githubusercontent.com/109272360/194015413-7cf08b42-e01d-4f44-9f1d-2b9630aa7ef7.png" width="700px" style="margin-top:16px;">
<img src="https://user-images.githubusercontent.com/109272360/194015421-266835b4-744e-45c1-b15d-f5836d65694d.png" width="700px" style="margin-top:16px;">

### ForeignKey
- Many-to-one relationship을 담당하는 Django의 모델 필드 클래스
- 외래 키 속성을 담당
- 2개의 필수 인자가 필요
  - 참조하는 model class
  - on_delete: 부모 객체가 사라졌을 때, 해당 부모를 참조하는 객체를 어떻게 처리할지 정의
    - `CASCADE`: 부모 객체가 삭제 됐을 때, 이를 참조하는 객체도 삭제
    - `PROTECT`, `SET_NULL`, `SET_DEFAULT` 등도 존재
- ForeignKey로 작성한 컬럼의 이름은 `인스턴스_id` 형태로 생성됨

### Related manager
- N:1 혹은 M:N 관계에서 사용 가능한 문맥(context)
- 역참조할 때에 사용 가능한 manager
  - 역참조: 본인 테이블을 외래 키로 참조 중인 다른 테이블에 접근하는 것
  - N:1 관계에서는 1이 N을 참조하는 것
- Django가 부모 테이블에서 역참조 할 수 있는 `모델명_set` manager를 자동으로 생성
- ForeignKey 클래스 작성 시 인자로 `related_name`을 사용하면, 역참조 시 사용하는 매니저 이름 변경 가능
  - 선택 사항이지만, 상황에 따라 반드시 작성해야 하는 경우가 생기기도 함

### Comment-Article
- Comment 모델 정의
  ```python
  # articles/models.py
  class Comment(models.Model):
      # 외래 키 필드는 작성 위치와 관계없이, 필드의 마지막에 작성
      # 클래스의 인스턴스 이름은 참조하는 모델 클래스의 단수형(소문자)로 작성하는 것을 권장
      article = models.ForeignKey(Article, on_delete=models.CASCADE)
      # article = models.ForeignKey(Article, on_delete=models.CASCADE, related_name='comments') <- 역참조 매니저 이름 변경
      content = models.CharField(max_length=200)
      created_at = models.DateTimeField(auto_now_add=True)
      updated_at = models.DateTimeField(auto_now=True)
  ```

- Migration 진행
  ```bash
  $ python manage.py makemigrations
  $ python manage.py migrate
  # 'article_id' 컬럼 생성 확인
  ```

- admin site 등록
  ```python
  # articles/admin.py
  from .models import Article, Comment

  admin.site.register(Article)
  admin.site.register(Comment)
  ```

- 댓글 생성 연습
  ```bash
  $ python manage.py shell_plus
  
  # article 인스턴스 생성 및 저장
  article = Article.objects.create(title='title', content='content')

  # 첫 번째 댓글 작성
  comment = Comment() # 인스턴스 생성
  comment.content = 'first comment' # 인스턴스 데이터 입력
  comment.article = article # 외래 키 데이터 입력
  # comment.article_id = article.pk 도 가능은 하지만, 권장 x
  comment.save() # DB에 댓글 저장

  comment.pk # 1
  comment.content # 'first comment'
  comment.article # <Article: title> <- 참조한 article 그대로 반환
  comment.article_id # 1 <- article_pk는 존재하지 않는 필드이기 때문에, 사용 불가
  comment.article.pk # 1
  comment.article.content # 'content'

  # 두 번째 댓글 작성
  comment = Comment(content='second comment', article=article)
  comment.save()

  # 역참조
  # [참고] dir(article)로 article 인스턴스가 사용할 수 있는 메서드 확인 가능
  article.comment_set.all()
  # <QuerySet [<Comment: first comment>, <Comment: second comment>]>

  comments = article.comment_set.all()
  for comment in comments:
      print(comment.content)
  # first comment
  # secont comment
  ```

- Create
  ```python
  # CommentForm 생성
  # articles/forms.py
  from .models import Article, Comment

  class CommentForm(forms.ModelForm):

      class Meta:
          model = Comment
          exclude = ('article',)
          # fields = '__all__'
          # 모든 필드를 저장할 경우, 댓글 내용 뿐 아니라 게시글 까지도 직접 지정해서 댓글을 작성해야 함
  ```

  ```python
  # 외래 키 받아오기
  # articles/urls.py
  urlpatterns = [
      # 생략
      path('<int:pk>/comments/', views.comments_create, name='comments_create'),
  ]
  ```

  ```python
  # articles/views.py
  from .forms import ArticleForm, CommentForm

  def detail(request, pk):
      article = Article.objects.get(pk=pk)
      comment_form = CommentForm()
      context = {
          'article' = article,
          'comment_form' = comment_form,
      }
      return render(request, 'articles/detail.html', context)

  def comments_create(request, pk):
      article = Article.objects.get(pk=pk)
      comment_form = CommentForm(request.POST)
      if comment_form.is_valid():
          # save(commit=False): 저장하지 않고, 저장했을 때의 객체를 미리 반환
          # 반환받은 객체의 article 인스턴스를 저장 후 객체 저장
          comment = comment_form.save(commit=False)
          comment.article = article
          comment.save()
      return redirect('articles:detail', article.pk)
  ```

  ```django
  <!-- articles/detail.html -->
  {% extends 'base.html' %}

  {% block content %}
    ...
    <a href="{% url 'articles:index' %}">뒤로가기</a>
    <hr>
    <form action="{% url 'articles:comments_create' article.pk %}" method="POST">
      {% csrf_token %}
      {{ comment_form.as_p }}
      <input type="submit" value="작성">
    </form>
  {% endblock content %}
  ```

- READ
  ```python
  # articles/views.py
  from .models import Article, Comment

  def detail(request, pk):
      article = Article.objects.get(pk=pk)
      comment_form = CommentForm()
      comments = article.comment_set.all()
      context = {
          'article' = article,
          'comment_form' = comment_form,
          'comments': comments,
      }
      return render(request, 'articles/detail.html', context)
  ```

  ```django
  <!-- articles/detail.html -->
  {% extends 'base.html' %}

  {% block content %}
    ...
    <a href="{% url 'articles:index' %}">뒤로가기</a>
    <hr>
    <h4>댓글 목록</h4>
    {% if comments %}
      <p><b>{{ comments|length }}개의 댓글이 있습니다.</b></p>
    {% endif %}
    <ul>
      <% for comment in comments %>
        <li>{{ comment.content }}</li>
      {% empty %}
        <p>작성된 댓글이 없습니다.</p>
      <% endfor %>
    </ul>
    <hr>
    ...
  {% endblock content %}
  ```

- DELETE
  ```python
  # articles/urls.py
  urlpatterns = [
      # 생략
      path('<int:article_pk>/comments/<int:comment_pk>/delete/', views.comments_delete, name='comments_delete'),
  ]
  ```

  ```python
  # articles/views.py
  def comments_delete(request, article_pk, comment_pk):
      comment = Comment.objects.get(pk=comment_pk)
      comment.delete()
      # article_pk를 variable routing으로 받아오지 않고,
      # article_pk = comment.article.pk로 받아와서 사용도 가능
      return redirect('articles:detail', article_pk)
  ```

  ```django
  <!-- articles/detail.html -->
  {% extends 'base.html' %}

  {% block content %}
    ...
    <a href="{% url 'articles:index' %}">뒤로가기</a>
    <hr>
    <h4>댓글 목록</h4>
    <ul>
      <% for comment in comments %>
        <li>
          {{ comment.content }}
          <form action="{% url 'articles:comments_delete' article.pk comment.pk %}" method='POST'>
          {% csrf_token %}
          <button>삭제</button>
        </form>
        </li>
      {% empty %}
        <p>작성된 댓글이 없습니다.</p>
      <% endfor %>
    </ul>
    <hr>
    ...
  {% endblock content %}
  ```

### Article-User
- Referencing User model
  - `settings.AUTH_USER_MODEL`
    - 반환 값: 'accounts.User' (문자열)
    - **models.py에서 사용**
  - `get_user_model()`
    - 반환 값: User Object (객체)
    - **models.py 외 다른 모든 곳에서 사용**

  ```python
  # articles/models.py
  from django.conf import settings

  class Article(models.Model):
      user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
      # 생략
      # 새로운 필드가 생성되었으므로, migration 진행
  ```

- CREATE
  ```python
  # articles/forms.py

  class ArticleForm(forms.ModelForm):

    class Meta:
        model = Article
        fields = ('title', 'content',)
        # fields = '__all__'로 하면, User를 선택하는 불필요한 필드도 함께 출력
  ```

  ```python
  # articles/views.py

  @login_required
  @require_http_methods(['GET', 'POST'])
  def create(request):
      if request.method == 'POST':
          form = ArticleForm(request.POST)
          if form.is_valid():
              article = form.save(commit=False)
              article.user = request.user
              article.save()
              return redirect('articles:detail', article.pk)
      else:
          form = ArticleForm()
      context = {
          'form': form,
      }
      return render(request, 'articles/create.html', context)
  ```

- DELETE
  ```python
  # articles/views.py

  @require_POST
  def delete(request, pk):
      if request.user.is_authenticated:
          article = Article.objects.get(pk=pk)
          # 글을 작성한 유저만 삭제할 수 있도록 처리
          if request.user == article.user:
              article.delete()
              return redirect('articles:index')
      return redirect('articles:detail', article.pk)
  ```

- UPDATE
  ```python
  # articles/views.py

  @login_required
  @require_http_methods(['GET', 'POST'])
  def update(request, pk):
      article = Article.objects.get(pk=pk)
      # 글을 작성한 유저만 수정 가능하도록 처리
      if request.user == article.user:
          if request.method == 'POST':
              form = ArticleForm(request.POST, instance=article)
              # form = ArticleForm(data=request.POST, instance=article)
              if form.is_valid():
                  form.save()
                  return redirect('articles:detail', article.pk)
          else:
              form = ArticleForm(instance=article)
      else:
          return redirect('articles:detail', article.pk)
      context = {
          'form': form,
          'article': article,
      }
      return render(request, 'articles/update.html', context)
  ```

  ```django
  <!-- articles/detail.html -->

  ...
  <!-- 글을 작성한 유저에게만 수정, 삭제 기능이 보이도록 처리 -->
  {% if request.user == article.user %}
    <a href="{% url 'articles:update' article.pk %}">UPDATE</a>
    <form action="{% url 'articles:delete' article.pk %}" method="POST">
      {% csrf_token %}
      <input type="submit" value="DELETE">
    </form>
  {% endif %}
  ...
  ```

- READ
  ```django
  <!-- articles/index.html -->
  {% extends 'base.html' %}

  {% block content %}
    ...
    {% for article in articles %}
      <!-- 작성자 출력 -->
      <p><b>작성자 : {{ article.user }}</b></p>
      <p>글 번호 : {{ article.pk }}</p>
      <p>제목 : {{ article.title }}</p>
      <p>내용 : {{ article.content }}</p>
      <a href="{% url 'articles:detail' article.pk %}">상세 페이지</a>
      <hr>
    {% endfor %}
  {% endblock content %}
  ```

  ```django
  <!-- articles/detail.html -->
  {% extends 'base.html' %}

  {% block content %}
    <h1>DETAIL</h1>
    <h2>{{ article.pk }}번째 글입니다.</h2>
    <hr>
    <!-- 작성자 출력 -->
    <p><b>작성자 : {{ article.user }}</b></p>
    <p>제목 : {{ article.title }}</p>
    <p>내용 : {{ article.content }}</p>
    <p>작성 시각 : {{ article.created_at }}</p>
    <p>수정 시각 : {{ article.updated_at }}</p>
    ...
  ```

### Comment-User
- 모델 관계 설정
  ```python
  class Comment(models.Model):
      article = models.ForeignKey(Article, on_delete=models.CASCADE)
      # 생략
      # 새로운 필드가 생성되었으므로, migration 진행
  ```

- CREATE
  ```python
  # articles/forms.py

  class CommentForm(forms.ModelForm):

    class Meta:
        model = Article
        exclude = ('article', 'user',)
        # 댓글을 적을 때 User를 선택하는 불필요한 필드는 삭제
  ```

  ```python
  def comments_create(request, pk):
      if request.user.is_authenticated:
          article = Article.objects.get(pk=pk)
          comment_form = CommentForm(request.POST)
          if comment_form.is_valid():
              comment = comment_form.save(commit=False)
              comment.article = article
              comment.user = request.user # 작성자 정보 함께 저장
              comment_form.save()
          return redirect('articles:detail', article.pk)
      return redirect('accounts:login')
  ```

- READ
  ```django
  ...
  <ul>
    {% for comment in comments %}
      <li>
        {{ comment.content }}<br>
        작성자: {{ comment.user }} <!-- 작성자 함께 출력 -->
        <form action="{% url 'articles:comments_delete' article.pk comment.pk %}" method='POST'>
          {% csrf_token %}
          <button>삭제</button>
        </form>
        ...
  ```

- DELETE
  ```python
  # articles/views.py

  def comments_delete(request, article_pk, comment_pk):
    if request.user.is_authenticated:
        comment = Comment.objects.get(pk=comment_pk)
        # 작성자와 현재 유저가 같은지 확인해, 본인의 댓글만 삭제가 가능하도록 처리
        if comment.user == request.user:
            comment.delete()
    return redirect('articles:detail', article_pk)
  ```

  ```django
  ...
  <ul>
    {% for comment in comments %}
      <li>
        {{ comment.content }}<br>
        작성자: {{ comment.user }}
        <!-- 본인의 댓글만 삭제 버튼이 보이도록 설정 -->
        {% if request.user == comment.user %}
          <form action="{% url 'articles:comments_delete' article.pk comment.pk %}" method='POST'>
            {% csrf_token %}
            <button>삭제</button>
          </form>
        {% endif %}
        ...
  ```

### 접근 제한하기
  ```python
  # articles/views.py

  @require_POST # POST만 받기
  def comments_create(request, pk):
      if request.user.is_authenticated: # 권한이 있다면, 댓글 추가 가능
          article = Article.objects.get(pk=pk)
          comment_form = CommentForm(request.POST)
          if comment_form.is_valid():
              comment = comment_form.save(commit=False)
              comment.article = article
              comment.user = request.user
              comment_form.save()
          return redirect('articles:detail', article.pk)
      return redirect('accounts:login') # 권한이 없다면, 로그인 화면으로

  @require_POST # P0ST만 받기
  def comments_delete(request, article_pk, comment_pk):
      if request.user.is_authenticated: # 권한이 있다면, 댓글 삭제 가능
          comment = Comment.objects.get(pk=comment_pk)
          if comment.user == request.user:
              comment.delete()
      return redirect('articles:detail', article_pk)
  ```

  ```django
  {% block content %}
    ...
    <hr>
    {% if request.user.is_authenticated %} <!-- 권한이 있다면 댓글 작성 가능 -->
      <form action="{% url 'articles:comments_create' article.pk %}" method="POST">
        {% csrf_token %}
        {{ comment_form.as_p }}
        <input type="submit" value="작성">
      </form>
    {% else %}
      <!-- 권한이 없다면 로그인 창으로 -->
      <a href="{% url 'accounts:login' %}">[댓글을 작성하려면 로그인하세요.]</a>
    {% endif %}
  {% endblock content %}
  ```

### 참고) 에러 응답하기
  ```python
  from django.http import httpResponse, httpResponseForbidden

  @require_POST
  def delete(request, pk):
      if request.user.is_authenticated:
          article = Article.objects.get(pk=pk)
          # 글을 작성한 유저만 삭제할 수 있도록 처리
          if request.user == article.user:
              article.delete()
              return redirect('articles:index')
          # 403 forbidden: 인증은 됐으나, 접근할 권한이 없는 경우
          return HttpResponseForbidden()
      return HttpResponse(status=401) # 비인증된 사용자
  ```