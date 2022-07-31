# Python 객체지향 프로그래밍, 에러와 예외

## 1. 객체지향 프로그래밍

### 객체지향 프로그래밍
- 객체지향 프로그래밍(Object-Oriented Programming, OOP)은 컴퓨터 프로그래밍의 패러다임(방법론) 중 하나임
  - 패러다임(방법론)의 예시: 라면을 끓일 때 면 먼저 넣을지, 스프 먼저 넣을지
  - 프로그램을 여러 개의 독립된 객체들과 그 객체 간의 상호작용으로 파악하는 프로그래밍 방법
    - 예시: 콘서트 내 객체 -> 가수, 감독, 관객 등
  - 각각의 객체는 메시지를 주고받고, 데이터를 처리할 수 있음

- 객체지향 프로그래밍이 필요한 이유
  - 현실 세계를 프로그램 설계에 반영(추상화)
  - 추상화: 객체가 어떻게 작동하는지 몰라도 필요할 때 쓸 수 있음

- 객체지향의 장점 / 단점
  - 장점
    - 클래스 단위로 모듈화시켜 개발 가능 -> 대규모 소프트웨어 개발에 적합 
    - ex) 로그인 담당, 게시판 담당, ...
    - 필요한 부분만 수정하기 쉽기 때문에 프로그램의 유지보수가 쉬움 
    - ex) 가수 객체, 무대 객체가 있으면, 가수에 문제가 있어도 다른 가수로 대체 가능
  - 단점
    - 설계 시 많은 노력과 시간이 필요함
    - 실행 속도가 상대적으로 느림 (컴퓨터의 동작 방식은 '절차지향'과 유사)

### 클래스와 객체
- 객체(object): 속성과 행동으로 구성된 모든 것 (클래스에서 정의한 것을 토대로 메모리에 할당)
  - 예시: 가수 이찬혁 객체
    - 속성(변수): 직업-가수 / 생년월일-0000-00-00 / 국적-대한민국
    - 행동(함수-메서드): 노래하기()-어느새~ / 춤추기()-둠칫둠칫 / 밥먹기()-쿰척쿰척

- 용어 정리
  - 클래스(타입): 실제 존재 x, 상상으로만 존재 ex) 가수
  - 객체(실제 사례): 실제 존재 o, 눈으로 확인 가능 ex) 이찬혁
  - 인스턴스: 클래스로 만든 객체
    - 객체와 인스턴스의 차이
      - 이찬혁은 객체다 (O)
      - 이찬혁은 인스턴스다 (X)
      - 이찬혁은 '가수의' 인스턴스다 (O) -> '특정 클래스(타입)의' 인스턴스라고 표현해야 함

- 파이썬은 모든 것이 객체 (모든 것에 속성과 행동이 존재)
  - 예시
    - [3, 2, 1].sort() -> 리스트.정렬하기() -> 객체.행동()
    - 리스트의 속성: iterable, mutable, ...
    - [], [1, 2], ['a', 'b'] 모두 파이썬의 객체이며, 리스트 타입(클래스)의 인스턴스 (객체와 인스턴스는 섞어서 쓰기도 함)

- 객체는 특정 타입의 인스턴스이다.
  - 123, 900, 5는 모두 int의 인스턴스
  - 'hello', 'bye'는 모두 str의 인스턴스
  - [1, 2, 3], []은 모두 list의 인스턴스

- 객체의 특징
  - 타입(type): 어떤 연산자(operator)와 조작(method)이 가능한가?
  - 속성(attribute): 어떤 상태(데이터)를 가지는가?
  - 조작법(method): 어떤 행위(함수)를 할 수 있는가?


### 객체와 클래스 문법
- 기본 문법
```python
# 클래스 정의
class MyClass:

# 인스턴스 생성
my_instance = MyClass()

# 메서드 호출
my_instance.mymethod()

# 속성
my_instance.my_attribute
```

- 클래스와 인스턴스
```python
class Person:
    pass

print(type(Person)) # <class 'type'>
# print(type(list)) # <class 'type> -> 클래스의 타입은 'type'임

person1 = Person() # 인스턴스 생성

print(isinstance(person1, Person)) # True
print(type(person1)) # <class '__main__.Person'>
```

- 객체 비교하기
  - == : 두 변수가 참조하는 객체의 내용이 같은 경우 True
  - is : 두 변수가 동일한 객체를 가리키는 경우 True
```python
a = [1, 2, 3]
b = [1, 2, 3]
print(a == b, a is b) # True False

a = [1, 2, 3]
b = a
print(a == b, a is b) # True True
```

### OOP 속성
- 속성(정보)
  - 특정 데이터 타입/클래스의 객체들이 가지게 될 상태/데이터를 의미
  - 클래스 변수 / 인스턴스 변수가 존재
```python
class Person:
    blood_color = 'red' # 클래스 변수
    population = 100 # 클래스 변수

    def __init__(self, name): # 생성자 (인스턴스가 생성될 때 자동으로 호출)
        self.name = name # 인스턴스 변수

person1 = Person('지민')
print(person1.name) # 지민
```

- 인스턴스 변수
  - 인스턴스가 개인적으로 가지고 있는 속성(attribute)
  - 각 인스턴스들의 고유한 변수
  - 생성자 메서드(__init__)에서 self.<name>으로 정의
  - 인스턴스가 생성된 이후 <instance>.<name>으로 접근 및 할당
```python
class Person:
    def __init__(self, name): # 인스턴스 변수 정의
        self.name = name

john = Person('john') # 인스턴스 변수 접근 및 할당
print(john.name) # john
john.name = 'John Kim' # 인스턴스 변수 수정 가능
print(john.name) # John Kim
```

- 클래스 변수
  - 클래스 선언 내부에서 정의
  - <classname>.<name>으로 접근 및 할당
```python
class Circle():
    pi = 3.14 # 클래스 변수 정의

    def __init__(self, r):
        self.r = r # 인스턴스 변수

c1 = Circle(5) # 반지름이 5인 원 생성
c2 = Circle(10) # 반지름이 10인 원 생성

print(Circle.pi) # 3.14
print(c1.pi) # 3.14
print(c2.pi) # 3.14

Circle.pi = 3.141592 # 클래스 변수 변경
print(Circle.pi) # 3.141592
print(c1.pi) # 3.141592
print(c2.pi) # 3.141592

# 인스턴스는 인스턴스 변수를 먼저 찾고, 없으면 클래스 변수를 찾음
c1.pi = 3 # 인스턴스 변수 변경
print(Circle.pi) # 3.141592
print(c1.pi) # 3 (새로운 인스턴수 변수가 생성)
print(c2.pi) # 3.141592

# 따라서 클래스 변수를 변경하고 싶으면, 클래스.클래스변수 형식으로 변경해야 혼선이 없음 (권장 사항)
```

- 클래스 변수 활용
```python
# 사용자 수 계산하기
class Person:
    count = 0
    
    def __init__(self, name):
        self.name = name
        Person.count += 1

person1 = Person('아이유')
person2 = Person('이찬혁')
print(Person.count) # 2
```

### OOP 메소드
- 메소드: 특정 데이터 타입/클래스의 객체에 공통적으로 적용 가능한 행위(함수)

-메소드의 종류
  - 인스턴스 메소드 (인스턴스 처리)
  - 클래스 메소드 (클래스 처리)
  - 정적 메소드 (나머지 처리)
```python
class Person:
    def talk(self):
        print('안녕')
    
    def eat(self, food):
        print(f'{food}를 냠냠')

person1 = Person()
person1.talk() # 안녕
person1.eat('피자') # 피자를 냠냠
# Person.talk(person1)도 가능은 하지만 권장 x
```

### 인스턴스 메소드
- 인스턴스 변수를 사용하거나, 인스턴스 변수에 값을 설정하는 메소드
- 첫번째 인자로 self를 사용해야 함
  - self: 인스턴스 자기자신으로, 메소드 호출 시 자신이 전달되게 설계
```python
class MyClass:
    def instance_method(self, arg1, ...):

my_instance = MyClass()
my_instance.instance_method(...)
```

- 생성자 메소드
  - 인스턴스 객체가 생성될 때 자동으로 호출되는 메소드
  - 인스턴스를 생성함
  - __init__ 사용
```python
class Person:
    def __init__(self):
        print('인스턴스가 생성되었습니다.')

person1 = Person() # 인스턴스가 생성되었습니다.

class Person:
    def __init__(self, name):
        print(f'{name} 인스턴스가 생성되었습니다.')

person1 = Person('지민') # 지민 인스턴스가 생성되었습니다.
```

- 매직 메소드
  - Double underscore(__)가 있는 메소드는 특정 상황에서 특수한 동작을 함
  - 스페셜 메소드 혹은 매직 메소드라고 불림
  - 예시: __str__(self), __len__(self), __lt__(self, other), ...
```python
class Circle:
    def __init__(self, r):
        self.r = r

    def __str__(self): # 해당 객체의 출력 형태를 지정
        return f'radius: {self.r}'

    def __gt__(self, other): # 부등호 연산자
        return self.r > other.r

    def __del__(self): # 소멸자 메서드(인스턴스 객체가 소멸되기 직전에 호출되는 메소드)
        print('인스턴스가 사라졌습니다.')

c1 = Circle(10)
c2 = Circle(1)

print(c1) # radius: 10
print(c2) # radius: 1
print(c1 > c2) # True
print(c1 < c2) # False
del c1 # 인스턴스가 사라졌습니다.
```

### 클래스 메소드
- 클래스 메소드
  - 클래스가 사용할 메소드
  - @classmethod 데코레이터를 사용하여 정의
  - 호출 시, 첫번째 인자로 클래스(cls)가 전달됨
```python
class Person:
    count = 0 # 클래스 변수
    def __init__(self, name): # 인스턴스 변수 설정
        self.name = name
        Person.count += 1

    @classmethod
    def number_of_population(cls):
        print(f'인구수: {cls.count}명')

person1 = Person('아이유')
person2 = Person('이찬혁')
print(Person.count) # 인구수: 2명

# 클래스 메소드로 인스턴스 생성
class Person:
    def __init__(self, name, age): # 생성자 메소드에서는 이름과 나이를 받음
        self.name = name
        self.num = age
        
    @classmethod
    def details(cls, name, birth): # 클래스 메소드에서는 생년월일을 받음
        p = cls(name, birth) # 이름과 생년월일 정보를 갖는 새로운 인스턴스 생성
        return p

    def check_age(self): # 2022년 기준 성인이면 True 반환
        if self.num > 1000: # 인스턴스가 생년월일을 받았을 경우
            if 2022 - self.num + 1 > 19:
                return True
            else:
                return False
        else: # 인스턴스가 나이를 받았을 경우
            if self.num > 19:
                return True
            else:
                return False

p1 = Person('이찬혁', 20)
print(p1.check_age()) # True
p2 = Person.details('아이유', 2007)
print(p2.check_age()) # False
```

- 데코레이터
  - 함수를 어떤 함수로 꾸며서 새로운 기능을 부여
  - @데코레이터(함수명) 형태로 함수 위에 작성
  - 순서대로 적용되기 때문에 작성 순서가 중요
```python
# 데코레이터 없이 함수 꾸미기
def hello():
    print('hello')

def add_print(original): # 파라미터로 함수를 받음
    def wrapper(): # 함수 내에서 새로운 함수 선언
        print('함수시작') # original 함수를 꾸미는 부가 기능(1)
        original()
        print('함수끝') # original 함수를 꾸미는 부가 기능(2)
    return wrapper # 함수 return

add_print(hello)() 

# 함수 시작
# hello
# 함수 끝

# 데코레이팅 함수
def add_print(original): # 파라미터로 함수를 받음
    def wrapper(): # 함수 내에서 새로운 함수 선언
        print('함수시작') # original 함수를 꾸미는 부가 기능(1)
        original()
        print('함수끝') # original 함수를 꾸미는 부가 기능(2)
    return wrapper # 함수 return

@add_print # add_print를 사용해서 print_hello()함수를 꾸며주도록 하는 명령어
def print_hello():
    print('hello')

print_hello()

# 함수 시작
# hello
# 함수 끝
```

- 클래스 메소드와 인스턴스 메소드 차이
  - 클래스 메소드: 클래스 변수(cls) 사용
  - 인스턴스 메소드: 인스턴스 변수(self) 사용
  - 클래스는 인스턴스 변수 사용 불가하지만, 인스턴스 메소드는 클래스 변수, 인스턴스 변수 둘 다 사용 가능

- 스태틱 메소드
  - 인스턴스 변수, 클래스 변수를 전혀 다루지 않는 메소드
  - 속성을 다루지 않고, 단지 기능(행동)만을 하는 메소드를 정의할 때 사용 (객체나 클래스 상태 수정 불가)
  - @staticmethod 데코레이터를 사용하여 정의
```python
class Person:

    def __init__(self, name):
        self.name = name

    @staticmethod
    def check_rich(money):
        return money > 10000

person1 = Person('아이유')
print(Person.check_rich(100000)) # True
```

- 인스턴스와 클래스 간의 이름 공간
  - 클래스를 정의하면, 클래스와 해당하는 이름 공간 생성
  - 인스턴스를 만들면, 인스턴스 객체가 생성되고 이름 공간 생성
  - 인스턴스에서 특정 속성에 접근하면, 인스턴스-클래스 순으로 탐색
  - [Python Tutor](https://pythontutor.com/)에서 확인
```python
class Person:
    name = 'unknown'

    def talk(self):
        print(self.name)

p1 = Person()
p1.talk() # unknown

p2 = Person()
p1.name = 'Kim'
p1.talk() # Kim

print(Person.name) # unknown
print(p1.name) # unknown
print(p2.name) # Kim
```

### 메소드 정리
- 메소드 종류
  - 인스턴스 메소드: 호출한 인스턴스를 의미하는 self 매개 변수를 통해 인스턴스를 조작
  - 클래스 메소드: 클래스를 의미하는 cls 매개 변수를 통해 클래스를 조작
  - 스태틱 메소드: 클래스 변수나 인스턴스 변수를 사용하지 않는 경우에 사용
```python
class MyClass:
    def instancemethod(self):
        return 'instance method', self

    @classmethod
    def classmethod(cls):
        return 'class method', cls

    @staticmethod
    def staticmethod():
        return 'static method'

obj = MyClass()
print(obj.instancemethod()) # ('instance method', <__main__.MyClss at 0x185...>)
print(MyClass.instancemethod(obj)) # ('instance method'), <__main__.MyClass at 0x185...>) / 권장 x

# 클래스 자체에서 각 메소드를 호출하는 경우
print(MyClass.classmethod()) # ('class method', <class '__main__.MyClass'>)
print(MyClass.staticmethod()) # static method
print(MyClass.instancemethod()) # 오류 발생 (인스턴스 메소드 호출 불가)

# 인스턴스는 클래스 메소드와 스태틱 메소드 모두 접근 가능
print(obj.classmethod()) # ('class method', <class '__main__.MyClass'>)
print(obj.staticmethod()) # static method
```

## 2. 객체지향의 핵심개념

### 추상화
- 추상화
  - 현실 세계를 프로그램 설계에 반영
  - 복잡한 것은 숨기고, 필요한 것만 들어내기
  - ex) 로그인 시, user.login()만으로 로그인 가능

### 상속
- 상속
  - 두 클래스 사이 부모 - 자식 관계를 정립하는 것
  - 자식 클래스는 직접 만들지 않아도, 부모 클래스의 정보와 행동을 받음
  - 모든 파이썬 클래스는 object를 상속 받음
```python
class ChildClass(ParentClass):
    pass

# 상속의 예시
class Person:
    def __init__(self, name):
        self.name = name

    def talk(self):
        print(f'반갑습니다. {self.name}입니다.')

class Professor(Person):
    def __init__(self, name, department):
        self.name = name
        self.department = department

p1 = Professor('박교수', '컴퓨터공학과')
p1.talk() # 반갑습니다. 박교수입니다.
```

- 상속 관련 함수와 메소드
  - isinstance(object, classinfo): classinfo의 statance거나 subclass인 경우 True
  - issubclass(class, classinfo): class가 classinfo의 subclass면 True
  - super(): 자식클래스에서 부모클래스를 사용하고 싶은 경우
  - mro(): 해당 인스턴스의 클래스가 어떤 부모 클래스를 가지는지 확인 (인스턴스 -> 자식 클래스 -> 부모 클래스로 확장)

```python
# isinstance
class Person:
    pass

class Professor(Person):
    pass

p1 = Professor()

print(isinstance(p1, Person)) # True
print(isinstance(p1, Professor)) # True
print(isinstance([1,2,3], list)) # True

# issubclass
class Person:
    pass

class Professor(Person):
    pass

class Student(Person):
    pass

print(issubclass(bool, int)) # True
print(issubclass(float, int)) # False
print(issubclass(Professor, Person)) # True
print(issubclass(Professor, (Person, Student))) # True

# super(1)
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        print('인스턴스 생성')

class Professor(Person):
    def __init__(self, name, age, email):
        # super() 사용 안하면, 다시 하나하나 지정해야 함
        self.name = name
        self.age = age
        self.email = email

class Student(Person):
    def __init__(self, name, age, student_id):
        super().__init__(name, age) # 부모 클래스 요소 호출
        self.student_id = student_id
    
s1 = Student('홍길동', 10, 12345) # '인스턴스 생성'

# super(2)
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

class Student(Person):
    def __init__(self, n, a, student_id): # super로 받아온 매개변수 재지정 가능
        super().__init__(n, a) # name과 age는 그대로 받아오고, 이름만 재지정
        self.student_id = student_id

s1 = Student('홍길동', 10, 12345)
print(s1.name) # 홍길동

# mro
class Person:
    pass

class Student(Person):
    pass

print(Student.mro()) # [<class '__main__Student'>, <class '__main__.Person'>, <class 'object'>]
```

- 다중 상속
  - 두 개 이상의 클래스를 상속 받는 경우
  - 상속받은 모든 클래스의 요소를 활용 가능
  - 중복된 속성이나 메소드가 있는 경우 상속 순서에 의해 결정됨
```python
class Dad():
    gene = 'XY'

    def walk(self):
        return '아빠가 걷기'

class Mom():
    gene = 'XX'

    def swim(self):
        return '엄마가 수영'

class Child(Dad, Mom): # Dad -> Mom 순서로 상속 받음
    def swim(self):
        return '아이가 수영'

baby1 = Child()
print(baby1.swim()) # 아이가 수영
print(baba1.walk()) # 아빠가 걷기
print(bab1.gene) # XY (Dad 클래스를 우선 상속 받음)
```

### 다형성
- 다형성(polymorphism)
  - 동일한 메소드가 클래스에 따라 다르게 행동할 수 있음을 의미
  - 서로 다른 클래스에 속해있는 객체들이 동일한 메시지에 대해 다른 방식으로 응답할 수 있음

- 메소드 오버라이딩
  - 상속받은 메소드를 같은 이름으로 재정의
  - 부모 클래스의 메소드 이름과 기본 기능은 그대로 사용하지만, 특정 기능을 바꾸고 싶을 때 사용
```python
class Person:
    def __init__(self, name):
        self.name = name

    def talk(self):
        print(f'반갑습니다. {self.name}입니다.')

class Professor(Person):
    def talk(self): # 부모 클래스의 talk()메소드를 덮어씀
        print(f'{self.name}일세.')

class Student(Person):
    def talk(self):
        super().talk() # 부모 클래스의 talk()메소드를 사용하면서 추가하기
        print('저는 학생입니다.')

p1 = Professor('김교수')
p1.talk() # 김교수일세.

s1 = Student('이학생')
s1.talk()
# 반갑습니다. 이학생입니다.
# 저는 학생입니다.
```

### 캡슐화
- 캡슐화
  - 객체의 일부 구현에 대해 외부로부터의 직접적인 액세스를 차단
    - ex) 주민등록번호
  - 파이썬에서 암묵적으로 존재하지만, 언어적으로는 존재하지 않음

- 접근제어자 종류
  - Public Access Modifier
    - 언더바 없이 시작하는 메소드나 속성
    - 어디서나 호출이 가능, 하위 클래스 override 허용

  - Protected Access Modifier
    - 언더바 1개로 시작하는 메소드나 속성
    - 암묵적 규칙에 의해 부모 클래스 내부와 자식 클래스에서만 호출 가능
    - 하위 클래스 override 허용
 
  - Private Access Modifier
    - 언더바 2개로 시작하는 메소드나 속성
    - 본 클래스 내부에서만 사용이 가능
    - 하위클래스 상속 및 호출 불가능 (오류)
    - 외부 호출 불가능 (오류)

```python
# Public Member
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

p1 = Person('홍길동', 30)
print(p1.name) # 홍길동
print(p1.age) # 30

# Protected Member
class Person:
    def __init__(self, name, age):
        self.name = name
        self._age = age
    
    def get_age(self):
        return self._age

p1 = Person('홍길동', 30)
print(p1.get_age()) # 30
print(p1._age) # 30 (접근은 가능하지만 파이썬에서는 암묵적으로 사용 x)

# Private Member
class Person:
    def __init__(self, name, age):
        self.name = name
        self.__age = age

    def get_age(self):
        return self.__age

p1 = Person('홍길동', 30)
print(p1.get_age()) # 30
print(p1.__age) # 오류 (age에 직접 접근 불가능)
```

- getter 메소드와 setter 메소드
  - 변수에 접근할 수 있는 메소드를 별도로 생성
    - getter 메소드: 변수의 값을 읽는 메소드 (@property 데코레이터 사용)
    - setter 메소드: 변수의 값을 설정하는 메소드 (@변수.setter 사용)
```python
# getter & setter 없이 사용
class Person:
    def __init__(self, age):
        self._age = age

p1 = Person(20)
print(p1._age) # 20 (접근은 가능)
print(p1.age) # 오류 발생

# getter로 언더스코어(_)없이 사용
class Person:
    def __init__(self, age):
        self._age = age

    @property
    def age(self):
        return self._age

p1 = Person(20)
print(p1._age) # 20
print(p1.age) # 20
p1.age = 18 # 오류 발생 (변경은 불가능)

# setter로 값 변경 가능
class Person:
    def __init__(self, age):
        self._age = age

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, new_age):
        if new_age > 19:
            raise ValueError('Adult')

        self._age = new_age

p1 = Person(20)
print(p1.age) # 20

p1.age = 15
print(p1.age) # 15

p1.age = 20 # ValueError: Adult
```

## 3. 에러와 예외 처리

### 디버깅
- 디버깅
  - 잘못된 프로그램을 수정하는 것 [de(없앤다) + bugging(버그)]
  - 오류가 주로 나는 곳: 제어가 되는 시점(조건/반복, 함수)
  - 디버깅 예시
    - print 함수 활용
    - 개발 환경(text editor, IDE) 등에서 제공하는 기능 활용(breakpoint, 변수 조회 등)
    - Python tutor 활용

### 에러와 예외
- 문법 에러(Syntax Error)
  - 파일이름, 줄번호, ^문자를 통해 파이썬이 코드를 읽어 나갈 때 문제가 발생한 위치를 표현
  - 줄에서 에러가 감지된 가장 앞의 위치를 가리키는 캐럿기호(^)를 표시

```python
# 문법 오류
if else # SyntaxError: invaild syntax

# 잘못된 할당
5 = 3 # SyntaxError: cannot assign to literal

# EOL(End of Line)
print('hello # SyntaxError: EOL while scanning string literal

# EOF(End of File)
print( # SyntaxError: unexpected EOF while parsing
```

- 예외(Exception)
  - 실행 도중 예상치 못한 상황을 맞이했을 때 발생하는 에러
  - 여러 타입으로 나타나고, 타입이 메시지의 일부로 출력됨
  - 사용자 정의 예외를 만들어 관리할 수 있음
    - ex) 로그인 안하면 본인 계정을 관리할 수 없게 처리

```python
# ZeroDivisionError
10 / 0 

# NameError # namespace 상에 이름이 없는 경우 (not defined)
print(name_error)

# TypeError - 타입 불일치
1 + '1'
round('3.5')

# TypeError - argument 누락
divmod()
import random
random.sample()

# TypeError - argument 개수 초과
divmod(1, 2, 3)
import random
random.sample(range(3), 1, 2)

# TypeError - argument type 불일치
import random
random.sample(1, 2)

# ValueError - 타입은 올바르나 값이 적절하지 않거나 없는 경우
int('3.5')
range(3).index(6)

# IndexError - 인덱스가 존재하지 않거나 범위를 벗어나는 경우
empty_list = []
empty_list[2]

# KeyError - 해당 키가 존재하지 않는 경우
song = {'IU' : '좋은날'}
song['BTS']

# ModuleNotFoundError
import asdf

# ImportError - Module은 있으나 존재하지 않는 클래스/함수를 가져오는 경우
from random import samp

# KeyboardInterrupt - 임의로 프로그램을 종료하였을 때
while True:
  continue
# Ctrl + C

# IndentationError - Indentation이 적절하지 않는 경우
for i in range(3):
print(i)
```

### 예외 처리
- 예외 처리
  - try문(statement) / except절(clause)을 이용하여 예외 처리를 할 수 있음
  - try문은 반드시 한 개 이상의 except절을 필요로 함
```python
try: # 오류가 발생할 가능성이 있는 코드를 실행
  num = input('숫자입력: ') # '안녕' 입력
  print(int(num))
except: # 예외가 발생하면, except절이 실행
  print('숫자가 아닙니다.')

'''
숫자입력 : 안녕
숫자가 아닙니다.
'''

try:
  num = input('숫자입력: ') # '안녕' 입력
  print(int(num))
except ValueError: # 에러 종류 설정
  print('숫자가 아닙니다.')

'''
숫자입력 : 안녕
숫자가 아닙니다.
'''

try:
  num = input() # 123 입력
  print(int(num))
except:
  print('예외 발생')
else: # 예외 없이 try문이 끝나면, else문 실행
  print('잘 지나감')
finally: # 예외 발생 여부에 관계없이, finally문 실행
  print('끝')  

'''
123
잘 지나감
끝
'''
```

- 에러 메시지 처리(as)
  - as 키워드를 활용하여 원본 에러 메시지를 사용할 수 있음
```python
try:
  empty_list = []
  print(empty_list[-1])
except IndexError as err:
  print(f'{err}, 오류가 발생했습니다.')

'''
list index out of range, 오류가 발생했습니다.
'''
```


