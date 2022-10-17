# Django QuerySet API

## 1. CRUD 기본

### CREATE
```python
# User 테이블에 데이터 생성
User.objects.create(
    first_name='길동',
    last_name='홍',
    age=20,
    country='제주도',
    phone='010-1234-5678',
    balance=10000,
)
```

### READ
```python
# User 테이블의 모든 레코드 조회
User.objects.all()

# 전체 인원수 조회(2가지 방법)
User.objects.count()
len(User.objects.all())

# 10번 user 레코드 조회
User.objects.get(pk=10)

# 현재 ORM을 SQL로 해석해서 출력
print(User.objects.all().query)
```

### UPDATE
```python
# 10번 user 레코드의 last_name을 김으로 수정
user = User.objects.get(pk=10)
user.last_name = '김'
user.save()
# 확인
user.last_name # 김
```

### DELETE
```python
# 10번 user 레코드 삭제
user = User.objects.get(pk=10)
user.delete()
# 확인
User.objects.get(pk=10)
```

## 2. Sorting data

- `order_by()`
  - QuerySet의 정렬을 재정의
  - 기본적으로 오름차순 정렬 / 필드명에 `-`을 작성하면 내림차순 정렬
  - 인자로 `?`를 입력하명 랜덤으로 정렬

- `values()`
  - 딕셔너리 요소들을 가진 QuerySet을 반환
  - 필드를 지정하면 지정한 필드에 대한 key와 value만을 출력
  - 필드를 지정하지 않으면 모든 필드에 대한 key와 value를 출력

```python
# values 사용 x
User.objects.filter(age=30)
# <QuerySet [<User: User object (5)>, <User: User object (13)>, ...]>

# values 사용 o
User.objects.filter(age=30).values('first_name')
# <QuerySet [{'first_name': '영환'}, {'first_name': '민수'}, ...]>

# 이름과 나이를 나이가 어린 순서대로 조회
User.objects.order_by('age').values('first_name', 'age')

# 이름과 나이를 나이가 많은 순서대로 조회
User.objects.order_by('-age').values('first_name', 'age')

# 이름, 나이, 잔고를 나이가 어린순으로, 만약 같은 나이라면 잔고가 많은 순으로 조회
User.objects.order_by('age', '-balance').values('first_name', 'age', 'balance')

# 아래와 같이 작성할 경우, 마지막 호출만 적용
User.objects.order_by('age').order_by('-balance')
# User.objects.order_by('-balance')와 같은 결과
```

## 3. Filtering data
- `distinct()`
  - 중복을 제거한 뒤 QuerySet 반환

- `filter()`
  - 주어진 매개변수와 일치하는 객체를 포함하는 QuerySet 반환
  - `.filter`를 연속해서 사용할 수 있다.

- `exclude()`
  - 주어진 매개변수와 일치하지 않는 객체를 포함하는 QuerySet 반환

- **Field lookups**
  - 상세한 조건을 지정하는 방법
  - QuerySet 메소드 filter(), exclude() 및 get()에 대한 키워드 인자로 사용
    - 참고) 위 메소드는 모두 파이썬의 함수이므로, `=`를 제외한 `>=`등을 사용할 수 없음
    - 따라서 Field lookups를 사용해, 다양한 조건 지정 가능
  - 필드명 뒤에 `__` 이후 작성
  - ex) `gte`, `gt`, `contains`, `startswith`, `endswith`, `in`


```python
# 중복없이 모든 지역 조회
User.objects.distinct().values('country')

# 지역 순으로 오름차순 정렬하여 중복없이 모든 지역 조회
User.objects.distinct().values('country').order_by('country')

# 이름과 지역이 중복 없이 모든 이름과 지역 조회
User.objects.distinct.values('first_name', 'country')

# 이름과 지역이 중복 없이 지역 순으로 오름차순 정렬하여 모든 이름과 지역 조회
User.objects.distinct.values('first_name', 'country').order_by('country')

# 나이가 30인 사람들의 이름 조회
User.objects.filter(age=30).values('first_name')

# 나이가 30살 이상인 사람들의 이름과 나이 조회
User.objects.filter(age__gte=30).values('first_name', 'age')

# 나이가 30살 이상이고 잔고가 50만원 초과인 사람들의 이름, 나이, 계좌 잔고 조회
User.objects.filter(age__gte=30, balance__gt=500000).values('first_name', 'age', 'balance')

# 이름에 '호'가 포함되는 사람들의 이름과 성 조회
User.objects.filter(first_name__contains='호').values('first_name', 'last_name')

# 핸드폰 번호가 010으로 시작하는 사람들의 이름과 핸드폰 번호 조회
User.objects.filter(phone__startswith='010-').values('first_name', 'phone')

# 이름이 '준'으로 끝나는 사람들의 이름 조회
User.objects.filter(first_name__endswith='준').values('first_name')

# 경기도 혹은 강원도에 사는 사람들의 이름과 지역 조회
User.objects.filter(country__in=['경기도', '강원도']).values('first_name', 'country')

# 경기도 혹은 강원도에 살지 않는 사람들의 이름과 지역 조회
User.objects.exclude(country__in=['경기도', '강원도']).values('first_name', 'country')

# 나이가 가장 어린 10명의 이름과 나이 조회
User.objects.order_by('age').values('first_name', 'age')[:10]
```

- **Q object**
  - filter()와 같은 메소드의 키워드 인자는 AND statement를 따름
  - 더 복잡한 쿼리를 실행해야 한다면, Q객체가 필요
  - `&`: AND , `|`: OR
  - 참고) `,`를 사용하면 AND 연산이 되기 때문에, `&`를 쓰지 않아도 가능

```python
from django.db.models import Q

# 나이가 30이거나 성이 김씨인 사람들 조회
User.objects.filter(Q(age=30) | Q(last_name='김'))

# 성이 김씨이면서, 나이가 20살 혹은 30살인 사람 조회
User.objects.get(Q(last_name='김'), Q(age=20) | Q(age=30))
```

## 4. Grouping data

- `aggregate()`
  - 전체 QuerySet에 대한 값을 계산
  - 특정 필드 전체의 합, 평균, 개수 등을 계산할 때 사용
  - 딕셔너리를 반환
  - aggregation functions: `Avg`, `Count`, `Max`, `Min`, `Sum` 등

- `annotate()`
  - 쿼리의 각 항목에 대한 요약 값을 계산
  - SQL의 `GROUP BY`에 해당

```python
from django.db.models import Avg, Max, Sum, Count

# 나이가 30살 이상인 사람들의 평균 나이 조회
User.objects.filter(age__gte=30).aggregate(Avg('age'))

# 나이가 30살 이상인 사람들의 평균 나이 조회(key를 avg_value로 설정)
User.objects.filter(age__gte=30).aggregate(avg_value=Avg('age'))

# 가장 높은 계좌 잔액 조회
User.objects.aggregate(Max('balance'))

# 모든 계좌 잔액 총액 조회
User.objects.aggregate(Sum('balance'))

# 각 지역별로 몇 명씩 살고 있는지 조회
User.objects.values('country').annotate(Count('country'))

# 각 지역별로 몇 명씩 살고 있는지 조회(key를 num_of_country로 변경)
User.objects.values('country').annotate(num_of_country=Count('country'))

# 각 지역별로 몇 명씩 살고 있는지와 계좌 잔액 평균 조회
User.objects.values('country').annotate(Count('country'), Avg('balance'))
```

## 5. Improve Query
Query를 줄이는 최적화 과정을 연습해보기

### annotate
- 예시: 각 게시글의 댓글 수를 함께 출력
  ```python
  def index(request):
      # articles = Article.objects.order_by('-pk')
      articles = Article.objects.annotate(Count('comment')).order_by('-pk')
      context = {
          'articles': articles,
      }
      return render(request, 'articles/index.html', context)
  ```
  ```django
  {% extends 'base.html' %}

  {% block content %}
    <h1>Articles</h1>
    {% for article in articles %}
      <p>제목 : {{ article.title }}</p>
      {% comment %} <p>댓글개수 : {{ article.comment_set.count }}</p> {% endcomment %}
      <p>댓글개수 : {{ article.comment__count }}</p>
      <hr>
    {% endfor %}
  {% endblock content %}
  ```

### select_related
- 1:1 또는 N:1 참조 관계에서 사용
- SQL에서 INNER JOIN 절을 활용
- 예시: 각 게시글의 작성자를 함께 출력
  ```python
  def index(request):
      # article을 가져올 때 user id 값도 함께 가져와서, 중복으로 query를 보내지 않음
      # articles = Article.objects.order_by('-pk')
      articles = Article.objects.select_related('user').order_by('-pk')
      context = {
          'articles': articles,
      }
      return render(request, 'articles/index.html', context)
  ```
  ```django
  {% extends 'base.html' %}

  {% block content %}
    <h1>Articles</h1>
    {% for article in articles %}
      <h3>작성자 : {{ article.user.username }}</h3>
      <p>제목 : {{ article.title }}</p>
      <hr>
    {% endfor %}
  {% endblock content %}
  ```

### prefecth_related
- 역참조 관계에서 사용

- 예시: 각 게시글의 모든 댓글을 함께 출력
  ```python
  def index(request):
      # article을 가져올 때 역참조 관계인 comment도 한 번에 가져오기
      # articles = Article.objects.order_by('-pk')
      articles = Article.objects.prefetch_related('comment_set').order_by('-pk')
      context = {
          'articles': articles,
      }
      return render(request, 'articles/index.html', context)
  ```
  ```django
  {% extends 'base.html' %}

  {% block content %}
    <h1>Articles</h1>
    {% for article in articles %}
      <p>제목 : {{ article.title }}</p>
      <p>댓글 목록</p>
      {% for comment in article.comment_set.all %}
        <p>{{ comment.content }}</p>
      {% endfor %}
      <hr>
    {% endfor %}
  {% endblock content %}
  ```

- 예시: 각 게시글의 모든 댓글과, 모든 댓글의 유저를 함께 출력
  ```python
  def index(request):
      # 댓글과 함께 
      # articles = Article.objects.order_by('-pk')
      # articles = Article.objects.prefetch_related('comment_set').order_by('-pk')
      articles = Article.objects.prefetch_related(
          Prefetch('comment_set', queryset=Comment.objects.select_related('user'))
      ).order_by('-pk')

      context = {
          'articles': articles,
      }
      return render(request, 'articles/index.html', context)
  ```
  ```django
  {% extends 'base.html' %}

  {% block content %}
    <h1>Articles</h1>
    {% for article in articles %}
      <p>제목 : {{ article.title }}</p>
      <p>댓글 목록</p>
      {% for comment in article.comment_set.all %}
        <p>{{ comment.user.username }} : {{ comment.content }}</p>
      {% endfor %}
      <hr>
    {% endfor %}
  {% endblock content %}
  ```