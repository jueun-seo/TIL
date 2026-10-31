# 추상화와 다형성: 약속만 정하는 부모

> 2026-10-31 · 자식이 꼭 만들어야 하는 메서드를 부모가 정해 둔다

## 부모는 왜 약속만 정하는지 물었습니다

도형마다 넓이를 구하는 법이 다릅니다. 사각형은 가로 × 세로, 삼각형은 밑변 × 높이 ÷ 2입니다. 부모 `Shape`는 "도형"이라는 너무 넓은 이름이라서 계산식을 알 수 없으니, `area()`라는 이름만 정하고 실제 계산은 자식이 채웁니다.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height
```

"약속만 정하는 부모는 왜 필요한가? 자식이 안 만들면?"이라고 물었습니다. 부모가 있으면 모든 도형이 같은 이름 `area`를 가지게 되고, 빼먹으면 상자를 만드는 순간 막아 줍니다. 부모 없이 `aera`로 오타를 내면 `for`가 한참 돈 뒤에 `AttributeError`가 나지만, 추상 클래스에서는 `Triangle(3, 4)`를 만드는 순간 `TypeError: Can't instantiate abstract class Triangle without an implementation for abstract method 'area'`가 납니다.

## `@abstractmethod`, `pass`, 이름을 어떻게 정하는지 헷갈렸습니다

"왜 부모 밑에 `@abstractmethod`를 적는가? 부모에 쓰는 메서드 이름은 무엇이고, `pass`만 적어 두는가?"라고 물었습니다. 설명은 서류 양식 비유였습니다.

- `@abstractmethod`는 "자식이 꼭 채워야 한다"는 표시입니다. 빼면 부모의 `pass`가 그대로 상속되어 `None`이 나오고 오류가 없습니다.
- 메서드 이름은 모든 자식이 가져야 하는 동작의 이름으로 내가 정합니다(`area`, `speak`).
- 부모는 방법을 모르니 몸통이 비어 있고, 문법상 `pass`로 자리를 채웁니다.

## `ABC`는 할아버지입니다

"부모인 Animal도 ABC라는 다른 클래스를 상속받는 건가? ABC는 부모의 부모, 즉 할아버지인가?"라고 물었습니다. 맞습니다. `class Animal(ABC):`에서 `ABC`는 파이썬이 `abc` 모듈에 만들어 둔 클래스이고, `Dog(Animal)`에게는 할아버지입니다. `isinstance(d, ABC)`도 `True`입니다.

## `abc`, `ABC`, `abstractmethod`는 고정된 이름이고 둘 다 import해야 합니다

"`abc`, `ABC`를 바꿔 쓸 수 있나? 왜 `from ... import`를 써야 하나? `from 모듈 import 클래스, abstractmethod` 순서인가? 모듈은 뭔가? `ABC`만 쓰면 되지 `abstractmethod`까지 왜 같이 쓰나?"라고 물었습니다.

- `abc`(소문자)는 모듈 이름이고, `ABC`(대문자)와 `abstractmethod`는 그 안의 이름입니다. 대소문자를 구분하고 바꿀 수 없습니다.
- 모듈은 코드가 든 파일(`.py`)입니다. `ABC`와 `abstractmethod`는 파이썬이 처음부터 아는 이름이 아니라서 import하지 않으면 `NameError`가 납니다.
- `ABC`는 확인하는 기능을 물려주고, `abstractmethod`는 메서드에 붙이는 표시입니다. 둘 중 하나만 있으면 `NameError`가 나거나 막아 주지 않아서 둘 다 import합니다.

```python
from abc import ABC, abstractmethod
```

## `Shape()`로 상자를 만들려고 하면 TypeError가 납니다

강사 자료의 "`Shape()`로 인스턴스를 만들어 봅니다" 단계는 일부러 오류를 보게 하는 것입니다. 추상 클래스는 비어 있는 메서드가 있어서 상자를 만들 수 없고, 시도하면 `TypeError: Can't instantiate abstract class Shape without an implementation for abstract method 'area'`가 납니다.

## 삼각형 풀이에서 `self.width`를 썼습니다

`Rectangle`과 `Triangle`을 만들고 `area()`를 구현하는 문제에서 `Triangle.area`에 `self.width`를 썼습니다. `Triangle`의 칸은 `base`와 `height`라서 `AttributeError: 'Triangle' object has no attribute 'width'`가 납니다. `__init__`에서 만든 칸 이름과 `area`에서 읽는 칸 이름이 같아야 합니다.

```python
class Triangle(Shape):
    def __init__(self, base, height):
        self.base = base
        self.height = height

    def area(self):
        return (self.base * self.height) / 2
```

## 상자를 리스트에 담아 `for`로 넓이를 출력합니다

"`for`로 도형 넓이를 어떻게 출력하지?", "왜 상자를 리스트에 먼저 담은 다음 꺼내는가?"라고 물었습니다. `for`는 묶음에서 하나씩 꺼내 같은 일을 반복하는 문법이라서 돌 묶음(리스트)이 먼저 있어야 합니다. 두세 개면 각각 불러도 되지만 도형이 많아지면 리스트와 `for`가 간단합니다. 이 설명은 이해됐다고 했습니다.

```python
shapes = [Rectangle(3, 4), Triangle(3, 4)]

for shape in shapes:
    print(shape.area())
# 12
# 6.0
```

`for` 안의 코드는 `shape`가 사각형인지 삼각형인지 구분하지 않고 `area()`만 부릅니다. 같은 이름의 메서드를 종류마다 다르게 동작하게 하는 것을 다형성이라고 합니다.
