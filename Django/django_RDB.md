# Django Relational Database

## 1. 관계형 데이터베이스(RDB)

### RDB에서의 관계
- 1:1(One-to-one relationships)
  - 한 테이블의 레코드 하나가 다른 테이블의 레코드 단 한개와 관련된 경우
- N:1(Many-to-one relationships)
  - 한 테이블의 0개 이상의 레코드가 다른 테이블의 레코드 한 개와 관련된 경우
  - 기준 테이블에 따라(1:N, One-to-many relationships)이라고도 함
- M:N(Many-to-many relationships)
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
- ManyToManyField(): M:N

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

## 3. M:N
<img src="https://user-images.githubusercontent.com/109272360/195249519-2aa96d17-680d-4f45-a920-5e87d72eae63.png" width="700" style="top-margin:16px;">
<img src="https://user-images.githubusercontent.com/109272360/195249527-dbab0c85-f84c-47b2-a047-d931b32581ef.png" width="700" style="top-margin:16px;">

### Target model & Source model
- target model: 모델 내부에 관계 필드를 가지지 않은 모델
- source model: 모델 내부에 관계 필드를 가진 모델
```python
# 예시
# models.py
# 모델 내부에 관계 필드(model2s)가 있으므로, target model
# M:N 에서 관계 필드는 복수형으로 작성
class Model1(models.Model):
    model2s = models.ManyToManyField(Model2)

# 모델 내부에 관계 필드가 없으므로, source model
class Model2(models.Model):
    pass
```
- M:N 관계에서 관계 필드는 두 모델 중 어느 모델에 작성해도 상관없지만, 참조와 역참조 관계는 달라짐
- 따라서 어디서 참조와 역참조를 쓰는게 편할지 고려한 뒤, 편한 곳에 작성

### ManyToManyField
- Many-to-Many relationship을 담당하는 Django의 모델 필드 클래스
- 두 model 간의 관계를 맺는 중개 테이블을 자동으로 생성
- M:N ERD는 두 테이블이 중개 테이블과 N:1로 이어져 있는 형태로 그려짐
- ManyToManyField의 인자들
  - 참조하는 model class (**필수 인자**)
  - `related_name`: target model이 source model을 참조할 때 사용할 manager name
    - ForeignKey의 `related_name`과 동일
  - `through`: 중개 테이블을 직접 작성하는 경우, 해당 테이블을 지정
    - 일반적으로 중개 테이블에 추가 데이터를 사용할 경우 사용
  - `symmetrical`: ManyToManyField가 동일한 모델('self')을 가리키는 경우만 사용
    - `symmetrical=True`일 경우, _set 매니저를 추가하지 않음
    - source 모델의 인스턴스가 tartget 모델의 인스턴스를 참조하면, target 모델 인스턴스도 source 모델 인스턴스를 참조하도록 설정
    - 테이블에 행을 두 개 만듦[(from:source, to: target), (from:target, to: source)]
    - 기본 값은 True이다.
  - `db_table`: 생성되는 중개 테이블의 이름을 설정할 수 있다.

### Related Manager methods
- 사용 모델이 누군지에 따라 두 가지 형태로 method 사용 가능
- `<source model>.<관계 필드>.method(<target model>)`
- `<target model>.<related manager>.method(<source model>)`

- add()
  - 지정된 객체를 관련 객체 집합에 추가
  - 이미 존재하는 관계에 사용하면 복제 x (오류 발생x)
- remove()
  - 관련 객체 집합에서 지정된 모델 객체를 제거
- 이 외에도 create(), clear(), set() 등이 있다.

### 중개 테이블
- source model과 target model이 다른 경우
  - id, source model_id, target model_id field로 구성
- 동일한 모델을 가리키는 경우
  - id, from_model_id, to_model_id로 구성

### M:N 연습하기
- 의사와 환자 모델 생성
```python
# hospitals/models.py

class Doctor(models.Model):
    name = models.TextField()

class Patient(models.Model):
    doctors = models.ManyToManyField(Doctor)
    name = models.TextField()
```
```bash
# shell_plus 실행
$ python manage.py shell_plus

# 의사와 환자 객체 생성
doctor1 = Doctor.objects.create(name='doc1')
patient1 = Patient.objects.create(name='pat1')
patient1 = Patient.objects.create(name='pat2')

# 예약 생성
patient1.doctors.add(doctor1)
# 1번 환자가 예약한 의사 목록 출력
patient1.doctors.all()
# 1번 의사에게 예약된 환자 목록 출력
# Doctor model은 target model이므로, related manager를 사용해야 함
doctor1.patient_set.all()

# 예약 취소
patient1.doctors.remove(doctor1)
```

- `related_name` 생성
```python
# hospitals/models.py

class Doctor(models.Model):
    name = models.TextField()

class Patient(models.Model):
    # 이제 Doctor 모델은 patient_set 대신 patients 사용
    doctors = models.ManyToManyField(Doctor, related_name='patients')
    name = models.TextField()
```
```bash
# shell_plus 실행
$ python manage.py shell_plus

# 예약 생성
doctor1 = Doctor.objects.create(name='doc1')
patient1 = Patient.objects.create(name='pat1')
doctor1.patients.add(patient1)
# 1번 의사에게 예약된 환자 목록 출력
doctor1.patients.all()
```

- `through` 옵션 사용
```python
# hospitals/models.py

class Doctor(models.Model):
    name = models.TextField()

class Patient(models.Model):
    # 이제 Doctor 모델은 patient_set 대신 patients 사용
    doctors = models.ManyToManyField(Doctor, related_name='patients')
    name = models.TextField()

class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    # 추가 정보를 테이블에 입력
    symptom = models.TextField()
    reserved_at = models.DateTimeField(auto_now_add=True)
```
```bash
# shell_plus 실행
$ python manage.py shell_plus

# 의사 및 환자 생성
doctor1 = Doctor.objects.create(name='doc1')
patient1 = Patient.objects.create(name='pat1')

# 예약 생성 1
reservation1 = Reservation(doctor=doctor1, patient=patient1, symptom='headache')
reservation.save()

# 예약 생성 2
# through_defaults 인자 사용
patient1.doctors.add(doctor1, through_defaults={'symptom': 'flu'})
```

### Article-User (좋아요 기능 구현)
```python
# articles/models.py
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')
    # 생략
    # related_name을 필수로 설정해야 하는 이유:
    # related_name을 설정하지 않으면 user model이 article을 역참조할 때, article_set을 사용해야 한다.
    # 그럴 경우 user가 작성한 글과 좋아요를 누른 글을 구분할 수 없게 된다.
    # 따라서 두 필드 중 하나에 related_name을 설정해야 한다.
```
- 현재 User-Article간 사용 가능한 related manager 정리
  - article.user: 게시글을 작성한 유저 - N:1
  - user.article_set: 유저가 작성한 게시글(역참조) - N:1
  - article.like_users: 게시글을 좋아요한 유저 - M:N
  - user.like_articles: 유저가 좋아요한 게시글(역참조) - M:N

```python
# articles/urls.py

app_name = 'articles'
urlpatterns = [
    # 생략
    path('<int:article_pk>/likes/', views.likes, name='likes'),
]
```

```python
# articles/views.py

@require_POST
def likes(request, article_pk):
    # 권한이 부여된 사용자만 좋아요를 누를 수 있도록 설정
    if request.user.is_authenticated:
        article = Article.objects.get(pk=article_pk)
        # exists: QuerySet에 결과가 포함되어 있으면 True를 반환하고, 아니면 False를 반환
        # if request.user in article.like_users.all(): <- 같은 의미
        if article.like_users.filter(pk=request.user.pk).exists():
            article.like_users.remove(request.user)
        else:
            article.like_users.add(request.user)
        return redirect('articles:index')
    return redirect('accounts:login')
```
```django
<!-- articles/templates/articles/index.html -->
    ...
    <div>
      좋아요 수: {{ article.like_users.all|length }}
      <form action="{% url 'articles:likes' article.pk %}" method='POST'>
        {% csrf_token %}
        {% if request.user in article.like_users.all %}
          <input type="submit" value='좋아요 취소'>
        {% else %}
          <input type="submit" value='좋아요'>
        {% endif %}
      </form>
    </div>
    <a href="{% url 'articles:detail' article.pk %}">상세 페이지</a>
    ...
```

### User-User (팔로우 기능 구현)
- 먼저 profile 구현하기

```python
# accounts/urls.py

app_name = 'accounts'
urlpatterns = [
    # 생략
    path('profile/<username>/', views.profile, name='profile'),
    # url을 그냥 <username>으로 하면 안 되는 이유:
    # 이 다음으로 생성되는 모든 문자열의 url이 profile 함수를 실행시킴
    # (urlpatterns의 위에서부터 탐색하다가 해당되는 url을 찾으면 바로 들어가므로)
]
```

```python
# accounts/views.py
from django.contrib.auth import get_user_model

def profile(request, username):
    User = get_user_model()
    person = User.objects.get(username=username)
    context = {
        'person': person,
    }
    return render(request, 'accounts/profile.html', context)
```

```django
<!-- accounts/templates/accounts/profile.html -->
{% extends 'base.html' %}

{% block content %}
<h1>{{ person.username }}님의 프로필</h1>

<hr>

<h2>{{ person.username }}'s 게시글</h2>
{% for article in person.article_set.all %}
  <div>{{ article.title }}</div>
{% endfor %}

<hr>

<h2>{{ person.username }}'s 댓글</h2>
{% for comment in person.comment_set.all %}
  <div>{{ comment.content }}</div>
{% endfor %}

<hr>

<h2>{{ person.username }}'s 좋아요한 게시글</h2>
{% for article in person.like_articles.all %}
  <div>{{ article.title }}</div>
{% endfor %}

<hr>

<a href="{% url 'articles:index' %}">BACK</a>

{% endblock content %}
```

- profile로 이동하는 하이퍼 링크 작성
```django
<!-- base.html -->
<body>
  <div class="container">
    {% if request.user.is_authenticated %}
      <h3>{{ user }}</h3>
      <a href="{% url 'accounts:profile' user.username %}">내 프로필</a>
      ...
```
```django
  ...
  {% for article in articles %}
    <p><b>작성자 : <a href="{% url 'accounts:profile' article.user.username %}">{{ article.user }}</a></b></p>
    <p>글 번호 : {{ article.pk }}</p>
    ...
```

- Follow 구현
```python
# accounts/models.py
class User(AbstractUser):
    followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
    # symmetrical=True로 하면, 팔로우할 때 자동으로 맞팔로우가 되는 현상이 생김
```

```python
# accounts/urls.py

app_name = 'accounts'
urlpatterns = [
    # 생략
    path('<int:user_pk>/follow/', views.follow, name='follow'),
]
```

```python
# accounts/views.py
@require_POST
def follow(request, user_pk):
    # 권한이 부여된 사용자만 팔로우가 가능하도록 설정
    if request.user.is_authenticated:
        User = get_user_model()
        person = User.objects.get(pk=user_pk)
        if person != request.user:
            # if User in person.followers.all(): <- 같은 의미
            if person.followers.filter(pk=request.user.pk).exists():
                person.followers.remove(request.user)
            else:
                person.followers.add(request.user)
        return redirect('accounts:profile', person.username)
    return redirect('accounts:login')
```

```django
<!-- accounts/templates/accounts/profile.html -->
<h1>{{ person.username }}님의 프로필</h1>
<div>
  <div>
    팔로잉: {{ person.followings.all|length }} / 팔로워: {{ person.followers.all|length }}
  </div>
  {% if request.user != person %}
    <div>
      <form action="{% url 'accounts:follow' person.pk %}" method='POST'>
        {% csrf_token %}
        {% if request.user in person.followers.all %}
          <input type="submit" value="Unfollow">
        {% else %}
          <input type="submit" value="Follow">
        {% endif %}
      </form>
    </div>
  {% endif %}
</div>
...
```