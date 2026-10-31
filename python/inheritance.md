# 상속: 부모의 것을 물려받는 자식 틀

> 2026-10-30 · 비슷한 틀의 공통 부분을 한 곳에 모은다

## `class Student(Person):`의 괄호가 부모를 가리킵니다

이미 있는 틀(부모)을 바탕으로 새 틀(자식)을 만드는 것이 상속입니다. 괄호 안에 부모를 쓰면 부모의 칸과 메서드를 물려받습니다. 이 괄호까지는 바로 이해했습니다.

```python
class Person:
    def __init__(self, name):
        self.name = name

    def introduce(self):
        return f"저는 {self.name}입니다"

class Student(Person):
    def __init__(self, name, school):
        super().__init__(name)
        self.school = school

s = Student("민수", "SeSAC")
print(s.introduce())    # 저는 민수입니다
print(s.school)         # SeSAC
```

## 자식에서 `self`와 `name`을 또 쓰는 이유가 궁금했습니다

"Student 안에서 인스턴스를 따로 만드는 건가? self, name은 어차피 상속받을 건데 또 쓰는 이유가 있나?"라고 물었습니다. 상자는 클래스 밖에서 `Student(...)`로 만들고, 자식의 `__init__`은 칸을 어떻게 채울지 적어 둔 것입니다.

자식에 `__init__`을 쓰지 않으면 부모 것을 그대로 씁니다. 학교 칸을 더하려고 `__init__`을 직접 쓰면 같은 이름이라 부모의 `__init__`은 실행되지 않습니다. 그래서 `super().__init__(name)`으로 부모가 하던 `name` 칸 채우기를 직접 불러야 합니다. `super()`를 빼면 `AttributeError: 'Student' object has no attribute 'name'`이 납니다.

- `self`는 상속되는 변수가 아니라 메서드마다 첫 번째 자리라서 `def`마다 적습니다.
- `name`은 `Student("민수", "SeSAC")`의 `"민수"`를 받아서 `super().__init__(name)`으로 부모에게 넘기려고 받습니다.

## 같은 이름일 때만 덮어씁니다

"자식에서 부모 것을 그대로 쓰면 아무것도 안 써도 되고, 뭔가 쓰려면 부모 것이 덮어써지니까 부모 것도 써 줘야 하는 건가?"라고 정리했습니다. 정확히는 부모와 **같은 이름**의 메서드를 쓸 때만 그 메서드가 덮어써집니다. 새 이름(`study`)의 메서드는 부모 것에 더해집니다.

상속은 비슷한 클래스끼리 공통 부분을 한 곳에 모으려고 씁니다. 자식에서 부모 것을 그대로 쓰는 상속도 쓰입니다. 예를 들어 `class MyError(Exception): pass`처럼 이름만 새로 만들어 구분할 때입니다.

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def eat(self):
        return f"{self.name}이 먹습니다"

class Dog(Animal):
    def bark(self):
        return f"{self.name}: 멍멍"

d = Dog("바둑이")
print(d.eat())     # 바둑이이 먹습니다   ← Animal의 것
print(d.bark())    # 바둑이: 멍멍
```

## 상속에 이름이 같을 필요는 없고, 매개변수는 달라도 오류가 나지 않습니다

"상속하려면 부모와 자식의 함수 이름이 똑같아야 하나? 이름이 같은 상태에서 매개변수만 다를 수 있나?"라고 물었습니다. 상속에는 이름이 상관없고, 같은 이름일 때만 덮어씁니다. 이름이 같은 채 매개변수가 달라도 파이썬은 검사하지 않습니다. `__init__`은 `Person.__init__(self, name)`과 `Student.__init__(self, name, school)`처럼 달라도 괜찮습니다. 일반 메서드는 부모 모양으로 부르는 코드가 있으면 `TypeError`가 날 수 있어서 같게 쓰는 것이 안전합니다.

## `super().__init__(name)` 대신 `self.name = name`이라고 써도 되는지 궁금했습니다

"부모의 init을 대신 부르려고 `super().__init__()`을 무조건 또 써야 하나? 이거 쓸 바에는 `self.name = name`이 더 짧겠다"고 물었습니다. 부모의 `__init__`이 한 줄이면 직접 써도 똑같이 동작합니다. 부모의 `__init__`이 여러 줄이면 자식에 같은 코드를 또 쓰게 되고, 부모를 고쳐도 자식 것은 그대로라서 `super().__init__(...)`으로 부모가 하던 일을 한 곳에만 둡니다.

## `s.introduce()`의 `()`와, 같은 이름이 둘 다 있을 때 자식 것이 쓰이는 이유

`print(s.introduce())`에서 `()`는 메서드를 실행하라는 표시입니다. 빼면 `<bound method ...>`처럼 메서드 정보만 나옵니다. 칸의 값(`s.school`)은 실행할 것이 아니라서 괄호가 없습니다.

`Student`에도 `introduce`가 있고 `Person`에도 있을 때 왜 자식 것이 나오는지 물었습니다. 상자 → 자식 틀 → 부모 틀 순서로 찾다가 자식 틀에서 먼저 찾으면 거기서 멈추기 때문입니다. 이것이 오버라이딩입니다. 부모 것은 `Person` 상자로 부르면 나오고, 자식 안에서는 `super().introduce()`로 부를 수 있습니다.

```python
class Student(Person):
    def introduce(self):
        return super().introduce() + f", {self.school} 학생입니다"
```

## `super().이름()`과 `super().__init__()`의 차이는 없습니다

"부모 것을 부를 때 `super().이름()`으로도, `super().__init__(매개변수)`로도 부르는 차이가 뭐냐"고 물었습니다. `__init__`도 메서드 이름 중 하나라서 문법은 같고, 괄호 안에는 부모의 그 메서드가 받는 값을 넣습니다. 이 설명은 이해됐다고 했습니다.

## `isinstance`는 자식 상자가 부모 틀로도 True입니다

`isinstance(값, 클래스)`는 그 클래스(또는 그 자식 클래스)의 상자이면 `True`, 아니면 `False`를 돌려줍니다. `Student`는 `Person`을 물려받았으니 학생은 사람이기도 합니다. 반대로 `Person` 상자는 `Student`가 아닙니다.

```python
s = Student("민수", "SeSAC")
p = Person("길동")

print(isinstance(s, Student))    # True
print(isinstance(s, Person))     # True   ← 자식은 부모이기도 하다
print(isinstance(p, Person))     # True
print(isinstance(p, Student))    # False  ← 부모는 자식이 아니다
```

값의 종류에 따라 다르게 처리할 때 쓰고, `Student`를 `Person`보다 먼저 확인합니다. 학생도 `Person`이기 때문입니다. 이 설명은 이해됐다고 했습니다.

## 메서드는 상자, 자식 틀, 부모 틀 순서로 찾습니다

`d = Dog()`에 값이 없는 것이 이상했습니다. `Animal`에도 `Dog`에도 `__init__`이 없어서 칸을 채울 일이 없고, 빈 상자라도 메서드는 쓸 수 있습니다.

```python
class Animal:
    def eat(self):
        return "먹습니다"

class Dog(Animal):
    def bark(self):
        return "멍멍"

d = Dog()
print(d.eat())    # 먹습니다
print(d.bark())   # 멍멍
```

`d.eat()`은 d 상자에 없고, `Dog` 틀에도 없고, 부모 `Animal` 틀에 있어서 그것을 실행합니다. `d.bark()`는 `Dog` 틀에서 바로 찾습니다. 처음 찾은 것을 실행합니다.
