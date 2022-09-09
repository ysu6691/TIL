# Django Form

## 1. Django Form
- 유효성 검사 단순화 및 자동화
- 공격 및 데이터 손상에 대한 방어 수단

### Form class
- Form class 선언
  - 앱 폴더에 forms.py 생성
  ```python
  # articles/forms.py
  # 어디에나 작성 가능하지만 관행적으로 forms.py 안에 작성
  from django import forms

  class ArticleForm(forms.Form):
      # model에 있는 field 중 사용자에게 입력 받을 field에 대해 지정
      # 각 필드에 맞는 유효성 검사가 진행됨
      title = forms.CharField(max_length=10)
      content = forms.CharField()
  ```
- 'new' view 함수 및 템플릿 업데이트
  ```python
  # articles/views.py
  from .forms import ArticleForm

  def new(request):
      form = ArticleForm()
      context = {
        'form': form,
      }
      return render(request, 'articles/new.html', context)
  ```
  ```django
  <!-- articles/templates/articles/new.html -->
  <!-- 생략 -->
    <form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit>
  </form>
  <!-- 생략 -->
  ```

- form 출력 옵션
  - as_p() : 각 필드가 <p>태그로 감싸져서 렌더링
  - as_ul() : 각 필드가 <li>태그로 감싸져서 렌더링 (<ul>태그는 직접 작성)
  - as_table() : 각 필드가 <tr>태그로 감싸져서 렌더링

### Widgets
- Django의 HTML input 속성을 표현 가능
- [django built-in widgets](https://docs.djangoproject.com/ko/3.2/ref/forms/widgets/#built-in-widgets)
- 예시
  ```python
  # articles/forms.py
  class ArticleForm(forms.Form):
      # HTML <select> tag 처럼 동작
      NATION_A = 'kr' # value 설정
      NATION_B = 'ch'
      NATION_C = 'jp'
      NATION_CHOICES = [
          (NATION_A, '한국'),
          (NATION_B, '중국'),
          (NATION_C, '일본'),
      ]
      title = forms.CharField(max_length=10)
      content = forms.CharField(widget=forms.Textarea) # textarea 속성으로 지정
      nation = forms.ChoiceField(choices=NATIONS_CHOICES) # select 속성으로 지정
  ```

## 2. Django ModelForm
- Model과 중복되는 부분을 재정의하지 않기 위해 사용

### ModelForm class
- ModelForm class 선언
  ```python
  # articles/forms.py
  from django from forms
  from .models import Article

  class ArticleForm(forms.ModelForm):
      # Meta: 어떤 모델을 기반으로 form을 작성할 것인지에 대한 정보 저장
      class Meta:
          model = Article # 모델 설정
          fields = '__all__' # 모델 필드 중 어떤 것을 출력할지 (사용자에게 입력 받는 필드 중)
          # exclude = ('title', ) # 모델에서 포함하지 않을 필드를 지정
          # fields와 exclude는 둘 중 하나만 사용할 것을 권장
          # ('title', 'content',) 형식도 가능
  ```
- create view 함수 업데이트
  ```python
  # articles/views.py
  def create(request):
      form = ArticleForm(request.POST) # new로 부터 받은 QueryDict로 form 형태의 인스턴스 생성
      # print(request.POST) # <QueryDict: {'csrfmiddlewaretoken': ['생략'], 'title': ['제목'], 'content': ['내용']}>
      # print(form)
      # <tr><th><label for="id_title">Title:</label></th><td><input type="text" name="title" value="제목" maxlength="10" required id="id_title"></td></tr>
      # <tr><th><label for="id_content">Content:</label></th><td><textarea name="content" cols="40" rows="10" required id="id_content">
      # 내용</textarea></td></tr>

      # is_valid: 유효성 검사를 실행하고, 결과를 boolean으로 반환
      if form.is_valid():
          article = form.save()
          # print(article) # Article object (1) -> form 형식으로 되어 있는 객체를 Article 객체로 반환
          return redirect('articles:detail', article.pk)
      # 유효성 검사 False면, 다시 new로 되돌아가기
      else:
      # print(f'에러: {form.errors}') -> 유효성 검사 실패 원인 출력
      return redirect('articles:new')
  ```

  - edit 함수 및 템플릿 업데이트
  ```python
  # articles/views.py
  def edit(request, pk):
      article = Article.objects.get(pk=pk)
      form = ArticleForm(instance=article)
      context = {
          'article': article
          'form': form
      }
      return render(request, 'articles/edit.html', context)
  ```
  ```django
  <!-- articles/templates/articles/edit.html -->
  <!-- 생략 -->
    <form action="{% url 'articles:update' article.pk %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit>
  </form>
  <!-- 생략 -->
  ```

  - update 함수 업데이트
  ```python
  # articles/views.py
  def update(request, pk):
      article = Article.objects.get(pk=pk)
      # instance 인자를 사용해, 기존 데이터를 업데이트
      # instance = 수정 대상
      # ModelForm 클래스의 첫번째 인자는 data이기 때문에, 'data='는 생략 가능
      # 하지만 두번째 인자는 instance가 아니기 때문에, 'instance='는 생략 불가
      form = ArticleForm(request.POST, instance=article)
      if form.is_valid():
          form.save()
          return redirect('articles:detail', article.pk)
      context = {
          'article': article,
          'form': form,
      }
    return render(request, 'articles/edit.html', context)
    ```

### widgets 활용
- 2가지 방법이 있음
```python
# articles/forms.py
# 권장하는 방식
class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        label = '제목',
        widget = forms.TextInput( # django widget class
            # html 속성을 지정
            attrs={
                'class': 'my-title',
                'placeholder': 'Enter the title',
                'maxlength': 10,
            }
        ),
    )

    content = forms.CharField(
        label = '내용',
        widget = forms.Textarea(
            attrs={
                'class': 'my-content',
                'placeholder': 'Enter the content',
                'rows': 5,
                'cols': 50,
            }
        ),
        # 해당 요구사항을 충족하지 못하면, 다음 메시지를 출력
        error_messages={
            'required': 'Please enter your content'
        }
    )

    class Meta:
      model = Article
      fields = '__all__'
```
```python
# articles/forms.py
# 권장하는 방식은 아님
class ArticleForm(forms.ModelForm):

    class Meta:
        model = Article
        fields = '__all__'
        widgets = {
            'title': forms.TextInput(attrs={
                'class': 'title',
                'placeholder': 'Enter the title',
                'maxlength': 10,
                }
            )
        }
```

## 3. Handling HTTP request

### HTTP request 처리
- new-create와 edit-update는 서로 목적이 같지만, request method가 다름
- 목적이 같은 함수를 합치고, if문을 활용해 method에 따라 로직 분리

- new + create
  ```python
  # articles/views.py
  def create(request):
      # POST일 떄와 아닐 때를 나누는 기준: DB에 영향을 주는지 아닌지
      # method가 POST일 때는 DB와 연관이 있는 로직
      if request.method == 'POST':
          form = ArticleForm(request.POST)
          if form.is_valid():
              article = form.save()
              return redirect('articles:detail', article.pk)
      # POST가 아닌 모든 method은 DB와 연관이 없는 로직
      else:
          form = ArticleForm()
      # is_valid를 통과하지 못했을 때를 고려해, context 들여쓰기 조정
      context = {
          'form': form,
      }
      return render(request, 'articles/create.html', context)
  ```

- edit + update
  ```python
  # articles/views.py
  def update(request, pk):
      article = Article.objects.get(pk=pk)
      if request.method == 'POST':
          form = ArticleForm(request.POST, instance=article)
          if form.is_valid():
              form.save()
              return redirect('articles:detail', article.pk)
      else:
          form = ArticleForm(instance=article)
      context = {
          'article': article,
          'form': form,
      }
      return render(request, 'articles/update.html', context)
  ```

- ModelForm 인자에 따른 차이
  - ArticleForm(request.POST): 사용자로부터 QueryDict로 받아온 데이터를 form으로 반환
  - ArticleForm(instance=article): 기존에 존재하던 article 데이터를 form으로 변환
  - ArticleForm(request.POST, instance=article): 기존에 존재하던 article 데이터를, 사용자로부터 QueryDict로 받아온 데이터로 수정

## 4. View Decorators

### View decorators
- require_http_methods(): 요청을 받을 method 지정
- require_POST(): POST 요청 method만 허용
- require_safe(): GET 요청 method만 허용
  - require_GET()이 있지만, require_safe() 사용 권장

```python
# articles/views.py
from django.views.decorators.http import require_http_methods, require_POST, require_safe

@require_safe
def index(request):
    pass

@require_http_methods(['GET', 'POST'])
def create(request):
    pass

@require_safe
def detail(request, pk):
    pass

@require_POST
def delete(request, pk):
    pass

@require_http_methods(['GET', 'POST'])
def update(request, pk):
    pass
```