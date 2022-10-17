# Django Fixtures

## Fixtures

### 초기 데이터의 필요성
Django 프로젝트의 앱을 처음 설정할 때 동일하게 준비된 데이터로 데이터베이스를 미리 채우는 것이 필요한 순간이 있다.
Django에서는 **fixtures**를 사용해 앱에 초기 데이터를 제공할 수 있다.

### fixtures
Django가 데이터베이스로 가져오는 방법을 알고 있는 데이터 모음
Django가 직접 만들기 때문에 데이터베이스 구조에 맞추어 작성되어있음
기본 경로: app_name/fixtures/

### fixtures 이용하기
- 생성(추출): `dumpdata`
  - 원하는 앱의 원하는 모델에 관련된 데이터를 json 파일로 저장 (최상단 폴더에 만들어짐)
  - 저장된 json 파일을 앱 폴더 내 'fixtures' 폴더에 넣기(이름공간 사용 시, fixtures 내 앱이름으로 된 폴더를 만든 뒤 넣기)
  - 이후에 push & pull 하면, 저장한 데이터를 그대로 load해 사용할 수 있다.
  ```bash
  # 양식
  $ python manage.py dumpdata 앱이름.모델이름 > 파일이름.json

  # 예시
  $ python manage.py dumpdata articles.article > articles.json

  # indent 사용하기
  $ python manage.py dumpdata --indent 4 articles.article > articles.json
  ```


- 로드(불러오기): `loaddata`
  - 각 폴더에 알맞게 저장된 json 파일을 load 하기
  - 만약 여러 json 파일을 불러올 경우, 한 번에 불러올 때는 상관 x
  - 하지만 여러 json 파일을 각각 하나씩 불러올 경우는 순서를 고려해야 함
    - 참조되고 있는 부모 테이블을 먼저 load하고, 그 뒤에 참조하고 있는 테이블을 load 해야함(참조키 때문)
  ```bash
  # 양식
  $ python manage.py loaddata 저장된경로/filename.json

  # 예시
  $ python manage.py loaddata articles.json comments.json users.json
  ```
