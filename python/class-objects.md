# 객체가 다른 객체를 담는다: Point와 Circle

> 2026-10-29 · 상자의 칸에 숫자 대신 다른 상자를 넣는다

## 점 하나도 상자로 만들 수 있습니다

`Student`는 칸이 `name`, `age`였는데, 점은 칸이 `x`, `y`입니다. 모양은 같습니다.

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"({self.x}, {self.y})"

p = Point(3, 4)
print(p)      # (3, 4)
print(p.x)    # 3
```

## `a.distance(b)`에서는 `self`가 a, `other`가 b입니다

두 점 사이의 거리는 점 두 개가 필요합니다. 하나는 점 앞의 `a`(`self`)이고, 다른 하나는 괄호에 넣은 `b`(`other`)입니다. 둘 다 `Point` 상자입니다.

```python
import math

class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def distance(self, other):
        return math.sqrt((self.x - other.x) ** 2 + (self.y - other.y) ** 2)

a = Point(0, 0)
b = Point(3, 4)
print(a.distance(b))    # 5.0
```

```text
a.distance(b)  →  self = a, other = b
(0-3)**2 + (0-4)**2 = 9 + 16 = 25  →  루트 → 5.0
```

앞에서 배운 `self`가 이번에는 `other`와 함께 두 상자의 칸을 비교합니다.

## 상자의 칸에 다른 상자를 넣을 수 있습니다

원은 중심(점)과 반지름이 필요합니다. 중심은 `Point` 상자를 `center` 칸에 넣습니다.

```python
class Circle:
    def __init__(self, center, r):
        self.center = center
        self.r = r

c = Circle(Point(0, 0), 5)
```

```text
c 상자: center = [ Point 상자: x=0, y=0 ]
        r      = 5
```

점을 두 번 찍으면 안쪽 상자의 칸을 읽습니다. `c.center.y`가 무엇인지 물었을 때 "0, c 인스턴스의 center 칸을 열고 y 출력"이라고 답했습니다. 앞에서부터 읽는 순서가 맞았습니다.

```python
print(c.center)      # (0, 0)   ← c의 center 칸에 있는 Point 상자
print(c.center.x)    # 0        ← 그 Point 상자의 x 칸
print(c.center.y)    # 0
print(c.r)           # 5
```

## 거리 계산은 중심점에게 맡깁니다

원 안에 점이 있는지 알려 주는 `contains`는 이미 있는 `distance`를 그대로 씁니다.

```python
class Circle:
    def __init__(self, center, r):
        self.center = center
        self.r = r

    def contains(self, point):
        return self.center.distance(point) <= self.r

c = Circle(Point(0, 0), 5)
print(c.contains(Point(3, 4)))    # True  (거리 5.0 <= 5)
print(c.contains(Point(6, 0)))    # False (거리 6.0 > 5)
```

`self.center.distance(point)`는 `self`의 `center` 칸에 있는 점 상자에게 `point`까지의 거리를 묻는 것입니다. `a.distance(b)`와 같은 모양이고, `a` 자리에 `self.center`가 들어갑니다.
