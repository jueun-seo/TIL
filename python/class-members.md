# 클래스 변수와 `@classmethod`: 틀에 있는 것과 틀에서 부르는 것

> 2026-10-30 · 상자마다 따로 있는 것과 틀에 하나만 있는 것

## 상자마다 따로 있는 칸과 틀에 하나만 있는 칸이 있습니다

지금까지의 칸(`self.name`, `self.age`)은 상자마다 따로였습니다. 클래스 안, 메서드 밖에 쓴 이름은 모든 상자가 같이 쓰는 **클래스 변수**입니다.

```python
class Student:
    school = "SeSAC"              # 클래스 변수: 틀에 하나
    count = 0                     # 클래스 변수

    def __init__(self, name):
        self.name = name          # 인스턴스 변수: 상자마다 따로
        Student.count += 1        # 틀의 count를 1 올린다

a = Student("민수")
b = Student("길동")
print(a.school, b.school)   # SeSAC SeSAC
print(Student.count)        # 2
```

## `Student.count += 1`이 인스턴스 변수가 클래스 변수를 고치는 건지 헷갈렸습니다

"인스턴스 변수가 클래스 변수를 조정할 수 있는 건가?"라고 물었습니다. 변수가 고치는 것이 아니라, `__init__` 안의 코드가 **앞에 쓴 이름**으로 어느 칸을 고칠지 정합니다. `self.`로 시작하면 그 상자의 칸이고, `Student.`로 시작하면 틀의 칸입니다. 학생 수 세기에 가방(상자)과 칠판(틀)으로 비유해서 이해했습니다.

`self.count += 1`로 쓰면 틀의 `count`가 아니라 `self` 상자에 `count` 칸이 새로 생겨서, `Student.count`가 계속 0입니다.

## `a.school = ...`은 틀을 바꾸지 않고 `a`에만 칸을 만듭니다

```python
a.school = "다른 학교"
print(a.school)         # 다른 학교
print(b.school)         # SeSAC
print(Student.school)   # SeSAC
```

틀의 값을 바꾸려면 `Student.school = ...`처럼 클래스 이름으로 씁니다.

## `@classmethod`는 틀에서 부르는 메서드입니다

`@classmethod`를 붙인 메서드는 첫 자리에 상자(`self`)가 아니라 틀(`cls`)을 받습니다. 글자 한 줄 같은 다른 모양의 값으로 상자를 새로 만들 때 씁니다.

```python
class Student:
    def __init__(self, name, scores):
        self.name = name
        self.scores = scores

    @classmethod
    def from_text(cls, text):
        parts = text.split(",")                  # ["민수", "80", "90", "70"]
        name = parts[0]
        scores = [int(p) for p in parts[1:]]     # [80, 90, 70]
        return cls(name, scores)                 # Student(name, scores)와 같다

s = Student.from_text("민수,80,90,70")
print(s.name, s.scores)    # 민수 [80, 90, 70]
```

## 클래스 메서드를 두고 여러 가지를 물었습니다

- `@`는 바로 아래 함수에 기능을 덧붙이는 표시(데코레이터)이고, 빼면 `cls` 자리에 내가 넣은 글자가 들어가 `missing 1 required positional argument: 'text'` 오류가 납니다.
- `from_text`는 강사님이 정한 이름이라 바꿔도 됩니다.
- `text`는 `__init__`의 `name`처럼 괄호에 넣은 값이 들어가는 매개변수 이름입니다.
- `cls`에는 `Student` 틀이 들어 있어서 `cls(name, scores)`는 `Student(name, scores)`와 같습니다.
- 인스턴스 메서드로 만들면 상자가 있어야 부를 수 있어서, 틀에서 바로 부르는 클래스 메서드를 씁니다. 꼭 필요한 것은 아니고, 상자를 만드는 방법이 여러 개일 때 쓰는 도구입니다.

`s = Student.from_text(...)`가 왜 "상자 없이 부른다"는 것인지 물었고, 설명을 두 번 들어도 이해가 안 된다고 했습니다. 설명은 이랬습니다. `Student("민수", 20)`도 부르기 전에는 상자가 없고, 틀에게 시켜서 상자가 나오는 같은 모양이며, `from_text`는 틀에 달린 두 번째 입구입니다.
