# Django Form

## 1. Django Form
- 유효성 검사 도구
- 공격 및 데이터 손상에 대한 방어 수단

### Form class
- Form class 선언
  - 앱 폴더에 forms.py 생성
    ```python
    # articles/forms.py
    # 어디에나 작성 가능하지만 관행적으로 forms.py 안에 작성
    from django import forms

    class ArticleForm(forms.Form):
        title = forms.CharField(max_length=10)
        content = forms.CharField()
    ```



