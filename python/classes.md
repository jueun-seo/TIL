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

다음 날 다시 정리하면서 "`__init__`은 값을 리턴하지 않으니 `return`이나 `print`를 쓰지 않겠네?"라고 물었습니다. `return 값`은 맞지만 `print`는 다릅니다. `print`는 값을 돌려주는 것이 아니라 화면에 보여 주는 것이라서 써도 오류가 나지 않습니다. `__init__`은 클래스 안에 있는 함수(메서드)이고, 이름이 정해져 있어서 클래스 밖에서는 자동 실행되지 않습니다.

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        print(name, "등록")

a = Student("민수", 20)   # 민수 등록
b = Student("길동", 25)   # 길동 등록
print(a.name)             # 민수
```

`Student(...)`를 쓴 줄마다 `__init__`이 한 번씩 실행되어, 마지막 `print` 전에 2번 실행됩니다.

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

## 만든 뒤에 칸을 바꾸는 줄을 또 놓쳤습니다

점수 리스트를 받는 `Student`로 평균을 구하는 문제에서 `s1.scores.append(100)` 줄을 놓쳤습니다.

```python
class Student:
    def __init__(self, name, scores):
        self.name = name
        self.scores = scores

    def average(self):
        return sum(self.scores) / len(self.scores)

s1 = Student("민수", [80, 90, 70])
s2 = Student("지은", [100, 100])
s1.scores.append(100)     # s1 상자의 scores 칸: [80, 90, 70, 100]

print(s1.average())       # 340 / 4 = 85.0
print(s2.average())       # 200 / 2 = 100.0
```

처음에는 "민수 80 / 지은 100"이라고 답했습니다. `a.age = 21`을 놓쳤던 것과 같은 패턴이었습니다. 다음 날 같은 문제를 다시 풀 때는 `s1`의 `scores`가 `[80, 90, 70, 100]`이 되고, `s2`의 칸은 바뀌지 않는다고 답했습니다. 나눗셈은 나누어떨어져도 `85.0`처럼 소수로 나옵니다.

## `__init__`만 자동으로 실행되고 `up`은 내가 불러야 실행됩니다

`__init__`은 칸을 채우고, `average`는 칸을 읽어서 `return`하는데 왜 `average`에는 속성 지정이 없냐고 물었습니다. 칸을 채우거나 바꾸는 일은 `self.칸 = 값`이고, 칸을 읽어서 답을 계산하는 일은 `return`입니다.

```python
class Counter:
    def __init__(self):
        self.count = 0

    def up(self):
        self.count = self.count + 1

c1 = Counter()
c2 = Counter()
c1.up()
c1.up()
c1.up()
c2.up()
print(c1.count, c2.count)   # 3 1
```

처음에는 `Counter()`를 쓰면 `up`도 실행되어 `c1`, `c2`의 `count`가 둘 다 1이 되는 줄 알았습니다. 그러다 "아 `init`만 강제로 실행되고 `up`은 강제로 실행되는 게 아냐?"라고 스스로 짚었습니다. 맞습니다. `Counter()`는 `__init__`만 자동으로 실행하고, `up`은 `c1.up()`처럼 불러야 실행됩니다. `c1.up()`을 하면 `self`에 `c1`이 들어가서 `c1`의 칸만 바뀌고 `c2`는 그대로입니다. 줄마다 두 상자의 상태를 적어 보니 `c1`은 `3`, `c2`는 `1`이 되었습니다.

## 메서드는 클래스 안에 있는 함수이고, `self`를 빼면 오류가 납니다

"메서드랑 함수랑 뭐가 다른 거야?"라고 물었습니다. 클래스 안에 있으면 메서드, 밖에 있으면 함수입니다. 메서드는 `c.up()`처럼 점을 찍어 부르고, 그때 앞에 쓴 `c`가 자동으로 첫 번째 자리(`self`)에 들어갑니다. `list`의 `append`, `str`의 `upper`도 파이썬이 만들어 둔 메서드이고, `up`, `reset`처럼 이름을 내가 정해 직접 만들 수도 있습니다. `__init__`만 이름 때문에 자동으로 실행됩니다.

```python
class Counter:
    def __init__(self):
        self.count = 0

    def up():                      # self를 뺐다
        print("올립니다")

c = Counter()
c.up()
```

`self`를 빼면 오류가 날 것 같다고 예상했습니다. 맞습니다. 파이썬은 `c`를 넘기려는데 `def up():`에는 받을 자리가 없기 때문입니다.

## `s1.average()`는 `Student.average(s1)`와 같습니다

`average`를 클래스 밖에서 `average(s1)`로 부르면 `NameError`가 났습니다. 메서드는 클래스 안에 있어서 상자 이름을 앞에 붙여 불러야 합니다. 그러다 "`s1.average()`가 아니라 `Student.average()`라고 하면 왜 안 돼?"라고 물었습니다.

```python
s1 = Student("민수", [80, 90, 70])

print(Student.average())      # TypeError: missing 1 required positional argument: 'self'
print(Student.average(s1))    # 80.0
print(s1.average())           # 80.0
```

`Student`는 틀이라서 `self`에 들어갈 상자가 없고, 괄호가 비면 오류가 납니다. `Student.average(s1)`은 상자를 내가 직접 넣은 것이고, `s1.average()`는 파이썬이 점 앞의 `s1`을 대신 넣어 준 것입니다.

`Student.average(s2)`가 `s2.average()`와 같은 결과인지 물었을 때는 "다르다"고 답했습니다. 괄호 안의 `s2`를 놓친 것입니다. 이후 `Student.average(s1)`에서는 `self`에 `s1`이 들어가고 `s1.average()`와 같다고 맞혔고, 괄호가 빈 `Student.average()`는 "인스턴스를 지정하지 않아 `self`를 알 수 없어서 오류"라고 설명했습니다.

## `print(a)`에는 `<... object at 0x...>`가 나오고, `__str__`을 만들면 모양이 바뀝니다

`a = Student("민수", 20)`을 만들고 `print(a)`를 하면 무엇이 나오는지 물었을 때 "민수 20"이라고 답했습니다. 실제로는 `__str__`이 없으면 상자의 기본 모양이 나옵니다.

```python
a = Student("민수", 20)
print(a)    # <__main__.Student object at 0x000001F4A3C2B5D0>
```

파이썬은 상자 안의 어떤 칸을 어떤 모양으로 보여 줄지 모르기 때문입니다. `__str__`을 만들면 `print(a)`가 그 메서드를 부르고, 돌려준 문자열을 보여 줍니다.

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return f"{self.name}({self.age}세)"

b = Student("길동", 25)
b.age = 30
print(b)        # 길동(30세)   ← __str__이 만든 문자열
print(b.age)    # 30           ← 칸의 값
```

`print(b)`는 `print(b.__str__())`과 같고, 내가 부르지 않아도 `print`할 때 파이썬이 부릅니다. `__init__`은 상자를 만들 때 한 번, `__str__`은 `print`할 때마다 실행됩니다. 이 두 메서드를 `print`로 확인해 보니 `print(a)`가 `__init__`을 다시 실행하지 않았습니다. `__str__`은 `__init__`과 달리 문자열을 `return`합니다.

`b.age = 30` 뒤에 `print(b)`와 `print(b.age)`가 무엇을 출력하는지 물었을 때 "길동 30, 30"이라고 답했습니다. 칸의 값은 맞았지만 `__str__`에 적은 모양 `길동(30세)`은 놓쳤습니다.

## 연습 1 Counter: `self.`를 빼먹었고, 메서드 이름을 값처럼 썼습니다

강사 연습문제 1번 `Counter`의 빈칸을 처음 직접 채웠을 때 `count = 0`과 `return increment += 1`이라고 썼습니다. 둘 다 고칠 곳이 있었습니다.

- `count = 0`은 `__init__`이 끝나면 사라지는 그냥 변수입니다. 상자 칸에 넣으려면 `self.count = 0`이라고 써야 합니다.
- `increment`는 값이 아니라 메서드 이름이고, `return`과 `+=`를 한 줄에 쓸 수도 없습니다. 칸을 바꾸는 메서드는 `return` 없이 `self.count += 1`로 칸의 값을 올립니다.

```python
class Counter:
    def __init__(self):
        self.count = 0

    def increment(self):
        self.count += 1

c = Counter()
c.increment()
c.increment()
print(c.count)    # 2
```

다시 써 보라고 했더니 `self.count = 0`과 `self.count += 1`로 맞게 썼습니다. 클래스 이름은 대문자 `Counter()`로 부릅니다. 소문자 `counter()`는 `NameError`입니다.

## 메서드는 `self` 말고 값도 받을 수 있습니다 (BankAccount)

강사 자료의 "AI 코드 읽기 ①"인 `BankAccount`를 읽었습니다. `acc.deposit(10000)`에서 `self`에는 점 앞의 `acc`가 자동으로 들어가고, `amount`에는 괄호에 넣은 `10000`이 들어갑니다. 메서드 안에서는 칸의 값(`self.balance`)과 받은 값(`amount`)을 `if`로 비교할 수 있습니다.

```python
class BankAccount:
    def __init__(self, balance):
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

    def withdraw(self, amount):
        if amount > self.balance:
            print("잔액 부족")
        else:
            self.balance -= amount

    def __str__(self):
        return f"잔액: {self.balance}원"

acc = BankAccount(0)
acc.deposit(10000)     # balance = 10000
acc.withdraw(3000)     # balance = 7000
acc.withdraw(20000)    # 잔액 부족 출력, balance는 그대로 7000
print(acc)             # 잔액: 7000원
```

`deposit`과 `withdraw`는 칸을 바꾸는 메서드라서 `return`이 없고, `print(acc)`는 `__str__`을 부릅니다.

## 이해된 계기

위 상자 그림을 보고 다시 예상했더니 맞혔습니다. `a`는 `민수`가 나오고, `b`는 `name` 칸이 없으니 꺼낼 게 없어서 에러가 납니다.