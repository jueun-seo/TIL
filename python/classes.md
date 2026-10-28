# 클래스와 인스턴스: 틀, 속성, `__init__`, `self`

> 2026-10-28 · 데이터와 기능을 한 덩어리로 묶는 틀

## 이름과 점수가 따로 놀면 누구 것인지 모릅니다

```python
name1 = "민수"
scores1 = [80, 90, 70]
name2 = "지은"
scores2 = [95, 85, 100]

def average(scores):
    return sum(scores) / len(scores)
```

학생이 한 명 늘 때마다 변수가 두 개씩 늘어납니다. `average`가 누구 점수에 쓰는 함수인지도 코드만 봐서는 알 수 없습니다. 클래스는 이름·점수 같은 데이터와 평균 구하기 같은 기능을 한 덩어리로 묶는 틀입니다. 함수가 코드를 묶는다면, 클래스는 데이터까지 묶습니다.

## `class`는 틀만 만들고, `Student()`를 해야 실물이 생깁니다

```python
class Student:
    pass

s = Student()
print(type(s))   # <class '__main__.Student'>
```

`class Student:`는 함수의 `def`처럼 선언만 합니다. 학생은 아직 한 명도 없습니다. `Student()`처럼 괄호를 붙여 부르면 실물인 **인스턴스**가 하나 생깁니다. 인스턴스에 점(`.`)을 찍어 붙인 값이 **속성**입니다.

## 같은 틀에서 나와도 인스턴스끼리는 따로입니다

`a`에만 `name`을 붙이고 `b.name`을 출력하면 어떻게 될지 예상했더니, `민수`만 나올 줄 알았습니다. 실제로는 `민수`가 나온 뒤 두 번째 줄에서 에러가 납니다.

```python
class Student:
    pass

a = Student()
b = Student()
a.name = "민수"
print(a.name)   # 민수
print(b.name)   # AttributeError: 'Student' object has no attribute 'name'
```

`name`은 `a`에만 붙였고 `b`에는 붙인 적이 없어서 `b.name`은 "그런 속성이 없다"는 `AttributeError`입니다. 틀은 같아도 인스턴스마다 자기 칸을 따로 가집니다.

## `minsu.name = "민수"`가 무슨 일을 하는지 처음엔 몰랐습니다

`minsu = Student()`가 인스턴스를 만드는 건 알겠는데, 그다음 줄에서 속성에 값을 넣어 무엇을 하는 건지 모르겠다고 물었습니다.

`Student()`는 텅 빈 상자를 하나 만들고, `minsu.name = "민수"`는 그 상자 안에 `name`이라는 이름표를 붙인 칸을 만들어 `"민수"`를 넣는 일입니다. 이 학생의 정보를 상자 안에 저장해 두고 나중에 `minsu.name`으로 꺼내 쓰려는 것입니다.

```text
a 상자              b 상자
┌──────────────┐    ┌───────────┐
│ name : "민수"│    │ (비어 있음) │
└──────────────┘    └───────────┘
```

`b`처럼 속성을 빠뜨리기 쉬워서, 상자를 만드는 순간 칸을 채워 주는 `__init__`이 나옵니다.

## `__init__`은 내가 부르지 않아도 `Student(...)`를 쓰면 실행됩니다

처음엔 "상자를 만드는 순간 자동으로 채운다"는 말이 무슨 뜻인지 몰랐습니다. 일반 함수는 `hello()`처럼 내가 호출해야 실행되는데, `__init__`은 `Student("민수")`를 쓰는 그 줄에서 파이썬이 대신 호출합니다.

```python
class Student:
    def __init__(self, name):
        print("__init__이 실행됐습니다")
        self.name = name

a = Student("민수")   # __init__이 실행됐습니다
```

인스턴스를 만들 때마다 한 번씩 실행됩니다. `__init__`도 클래스 안에 있는 함수라서 메서드라고 부릅니다.

## `self`는 방금 만들어진 그 상자입니다

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age

a = Student("민수", 20)
```

`self.name = name`의 왼쪽은 방금 만든 상자 안의 `name` 칸이고, 오른쪽은 밖에서 받은 `"민수"`입니다. 앞에서 손으로 `a.name = "민수"`라고 쓰던 것을 `__init__` 안에서는 아직 이름이 없는 상자라 `self`라고 부릅니다. `self`는 파이썬이 채워 주므로 부를 때는 `"민수", 20`만 넣습니다.

값을 하나 빼먹고 `Student("현우")`로 만들면 `age`에 넣을 값이 없어서 에러가 납니다.

```text
TypeError: Student.__init__() missing 1 required positional argument: 'age'
```

`def` 줄의 `name`, `age`는 매개변수이고, 부를 때 넣는 `"민수"`, `20`은 인수입니다.

## `__init__`에는 `print`와 `if`는 써도 되지만 `return 값`은 안 됩니다

필기에 "왜 `__init__`에는 return이나 print가 없나? def나 if는 못 넣나?"라고 적어 두었습니다. `__init__`도 평범한 함수 몸통이라서 `print`와 `if`는 쓸 수 있습니다.

```python
class Student:
    def __init__(self, name, age):
        if age < 0:
            print("나이는 0 이상이어야 해서 0으로 바꿉니다")
            age = 0
        self.name = name
        self.age = age
```

다만 `a = Student("민수", 20)`에서 `a`에 들어가는 것은 새로 만들어진 상자이고 `__init__`이 `return`한 값이 아닙니다. 그래서 값을 돌려주면 파이썬이 막습니다.

```text
TypeError: __init__() should return None, not 'int'
```

`__init__`은 결과를 돌려주는 함수가 아니라, 이미 만들어진 상자를 채우는 함수입니다.

## 속성 값은 만든 뒤에도 바꿀 수 있습니다

`a`와 `b`를 만든 뒤 `a.age = 21`을 하고 `a.age`를 출력하면 무엇이 나오는지 물었을 때, `20`이라고 답했습니다. `a.age = 21` 줄을 놓친 것입니다. 실제로는 `21`입니다.

```python
a = Student("민수", 20)   # a 상자의 age 칸 = 20
b = Student("길동", 25)
a.age = 21                # 같은 칸을 21로 덮어씀

print(a.name, a.age)      # 민수 21
print(b.name, b.age)      # 길동 25
```

`__init__`이 처음에 넣어 준 값도 나중에 `인스턴스.속성 = 값`으로 바꿀 수 있습니다. 새 칸이 생기는 게 아니라 있는 칸의 값이 바뀌고, 다른 상자(`b`)는 영향을 받지 않습니다. 줄마다 상자 그림을 그려 가며 비슷한 문제를 다시 풀었더니 맞혔습니다.

## 이해된 계기

위 상자 그림을 보고 다시 예상했더니 맞혔습니다. `a`는 `민수`가 나오고, `b`는 `name` 칸이 없으니 꺼낼 게 없어서 에러가 납니다.