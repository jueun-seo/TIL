# 함수 기본: 선언, 호출, 인수, return, 스코프, 타입힌트

> 2026-10-27 · 함수가 만들어지고, 불리고, 값을 돌려주는 방식

## 같은 계산을 반복하지 않으려고 씁니다

```python
scores_a = [80, 90, 70]
average_a = sum(scores_a) / len(scores_a)

scores_b = [60, 75, 95, 85]
average_b = sum(scores_b) / len(scores_b)
```

점수 리스트마다 `sum ÷ len`을 매번 쓰는 대신, 계산에 이름을 붙여 묶어 두고 불러 쓰려는 것이 함수입니다. 함수는 input(괄호 안에 넣는 값)이 있으면 output(`return`으로 돌려주는 값)이 나오는 것입니다.

## `def`만 쓰면 실행되지 않습니다

```python
def average(scores):
    return sum(scores) / len(scores)
```

이 코드를 실행하면 화면에 아무것도 안 나옵니다. `def`로 함수를 **선언**만 했기 때문입니다. `def`는 *define*(정의하다)의 약어였습니다.

선언은 레시피 카드를 써 두는 것이고, 요리는 따로 해야 합니다. 요리하는 것이 **호출**이고, 함수 이름 뒤에 괄호를 붙이면 됩니다. 괄호 안에는 넣고 싶은 값을 씁니다.

```python
average([80, 90, 70])   # 80.0
```

처음에는 `print(average)`를 하면 실행되는 줄 알았습니다. 하지만 괄호가 없으면 호출이 아니라 함수 자체를 가리킬 뿐이라 평균은 계산되지 않습니다. 선언은 `def` 줄과 그 아래 들여쓴 줄 전부입니다.

## 매개변수는 빈칸, 인수는 채운 값

이름은 알고 있었는데 둘의 차이는 정확히 몰랐습니다.

- **매개변수**: 만들 때 쓰는 이름. `def average(scores)`의 `scores`. 재료를 넣는 빈칸 이름표입니다.
- **인수**: 부를 때 넣는 실제 값. `average(scores_b)`의 `scores_b`.

## 인수는 이렇게 넣습니다

처음에는 호출할 때 괄호 안에 값이 있으면 인수이고 비어 있으면 매개변수인 줄 알았습니다. 아니었습니다. 기준은 비어 있는지가 아니라 **어디에 쓰였는지**입니다. `def` 줄의 괄호 안 이름은 매개변수, 호출하는 줄의 괄호 안 값은 인수입니다.

### 순서대로 들어갑니다

```python
def greet(name, message):
    return f'{message}, {name}!'

greet('hong', 'hi')   # 'hi, hong!'
```

재료 칸 순서대로 `'hong'`은 `name`에, `'hi'`는 `message`에 들어갑니다. 값이 하나 빠지면 에러가 납니다.

```python
greet('hong')
# TypeError: missing 1 required positional argument: 'message'
```

"필요한 위치 인수 하나가 빠졌다"는 뜻이라, 마지막 줄을 읽으면 어느 칸이 빠졌는지 알려 줍니다.

### 기본값이 있으면 안 넣어도 됩니다

```python
def greet(name, message='hi'):
    return f'{message}, {name}!'

greet('mina')            # 'hi, mina!'
greet('mina', 'hello')   # 'hello, mina!'
```

- **기본값이 없는 칸은 꼭 채워야 하고, 있는 칸은 안 채워도 됩니다.** 그래서 `greet('mina')`는 에러가 아닙니다.
- 기본값이 있어도 **인수를 넣으면 인수가 우선**입니다. `def add(a, b=10)`에서 `add(5)`는 15, `add(5, 1)`은 6입니다.
- 기본값은 글자만 되는 줄 알았는데, 숫자나 `True`, `None`도 됩니다. 따옴표는 글자일 때만 씁니다. `'1'`은 숫자가 아니라 글자입니다.

`print`가 좋은 예입니다. `help(print)`를 보면 칸이 여러 개인데 전부 기본값이 있어서 하나만 넣어도 돌아갑니다. `sep`(*separator*, 구분자)의 기본값은 띄어쓰기이고, 바꾸면 사이 글자가 달라집니다.

```python
print('hi', 'hello', 'bye')            # hi hello bye
print('hi', 'hello', 'bye', sep='!')   # hi!hello!bye
```

### 이름을 붙이면 순서가 상관없습니다

```python
greet(name='kim', message='hi')
greet(message='hi', name='kim')   # 순서를 바꿔도 된다
```

이름을 붙이면 어느 칸에 넣는지 직접 정하는 것입니다. 다만 결과 글자의 순서는 넣은 순서가 아니라 **함수 안의 f-string**이 정합니다. `greet(message='hello', name='mina')`를 보고 `mina hello`일 줄 알았는데 결과는 `'hello, mina!'`였습니다.

### 작은 궁금증들

- `return`은 함수가 아니라 `def`, `if`처럼 **키워드**라서 `return(...)`처럼 괄호를 쓰지 않아도 됩니다.
- "돌려준다(return한다)"와 "출력한다(print한다)"는 다릅니다. Jupyter는 마지막 줄의 값을 자동으로 화면에 보여 줍니다.
- 함수 이름은 마음대로 지을 수 있지만, `print`, `len`, `sum`, `min`, `max`, `sorted`처럼 파이썬이 이미 만들어 둔 이름은 덮어쓸 수 있어서 피합니다. `average`는 파이썬에 없어서 직접 만든 이름입니다.

## `return`은 손에 쥐여 주고, `print`는 화면에만 씁니다

```python
def average(scores):
    return sum(scores) / len(scores)

def show_average(scores):
    print(sum(scores) / len(scores))

average(scores_a) + 10        # 90.0
show_average(scores_a) + 10   # 에러
```

둘 다 화면에는 `80.0`이 보이는데 아래만 에러가 나는 이유를 한참 몰랐습니다. 함수 안에 무엇을 썼는지가 달랐습니다.

- `return`은 값을 **손에 쥐여 줍니다.** 받아서 `+ 10`처럼 다시 쓸 수 있습니다.
- `print`는 화면에 **보여 주기만** 합니다. 손에 남는 것이 없습니다.

### 손이 빈 상태는 `None`입니다

가장 헷갈렸던 부분입니다. 아래 코드에서 `y`에는 `5`가 들어 있을 줄 알았는데 `None`이었습니다.

```python
def double(x):
    print(x * 2)

y = double(5)
```

`=`는 오른쪽을 먼저 실행하니까, 순서대로 따라가 보면 됩니다.

1. 오른쪽 `double(5)`를 먼저 실행한다.
2. 함수 안에서 `print(10)`이 실행되어 화면에 10이 써진다.
3. 함수가 끝났는데 손에 쥐여 줄 값(`return`)이 없다.
4. 이때 파이썬은 "없음"이라는 표시값 `None`을 대신 돌려준다.
5. 돌려받은 값이 왼쪽 `y`에 들어간다. 그래서 `y = None`.

`return`을 쓴 버전은 3번에서 10을 쥐여 주니까 `y = 10`입니다. 이때는 화면에 아무것도 안 나오고 값이 조용히 `y`에 들어갑니다.

`print` 버전에서는 `y`가 `None`이라서 `y + 1`은 에러가 납니다. `None`은 숫자가 아니라 "없음"이라서 더할 수 없습니다.

### 그러면 `print`는 왜 있을까

`print`로 만든 함수는 어떤 값을 써도 결과가 `None`이라서 쓸모없어 보였는데, 하는 일이 달랐습니다.

- `return`은 **코드**를 위해 값을 다음 계산으로 넘겨줍니다.
- `print`는 **사람**을 위해 값을 화면에 보여 줍니다.

그래서 함수는 보통 `return`으로 만들고, 보고 싶을 때만 밖에서 `print`합니다.

```python
def double(x):
    return x * 2

print(double(5))   # 10 (보기)
double(5) + 1      # 11 (계속 계산하기)
```

## 값은 방 밖으로 나오지만, 이름표는 못 나옵니다

함수는 불투명한 방입니다. 방 안에서는 밖이 보이지만, 밖에서는 방 안이 안 보입니다.

```python
rate = 0.1

def get_tax(price):
    tax = price * rate
    return tax

print(get_tax(1000))   # 100.0
print(tax)             # 에러: name 'tax' is not defined
```

- `rate`는 방 **밖**에서 만든 것이라 함수 안에서 읽힙니다.
- `tax`는 방 **안**에서 만든 것이라 밖에서는 안 보입니다.

처음에는 "`def` 안에 감싸져서 읽히는 것"이나 "아래 줄에 따로 떨어져서 에러가 나는 것"이라고 생각했습니다. 기준은 코드의 위치가 아니라 **어디서 만들었는지**였습니다.

또 하나 헷갈렸던 건 `return tax`를 하면 `tax`라는 이름이 저장되는 줄 알았다는 점입니다. 밖으로 나오는 건 이름이 아니라 **값 `100.0`**입니다. 이름표 `tax`는 방 안에 두고 나옵니다.

```python
my_tax = get_tax(1000)   # 값 100.0을 받아 밖에서 새 이름표를 붙인다
print(my_tax)            # 100.0
```

안의 `tax`를 `t`로 바꿔도, 밖의 이름을 `fee`로 바꿔도 결과는 같습니다. 함수 이름(`get_tax`)은 레시피의 이름이고, `tax`는 값을 담는 이름표라서 서로 다른 것입니다.

### 값 두 개를 return하면

```python
def min_max(scores):
    return min(scores), max(scores)

min_max([3, 15, 0])    # (0, 15)
```

`return min(scores), max(scores)`는 값 두 개를 따로 돌려주는 게 아니라 `(0, 15)` 같은 **튜플 하나**를 돌려줍니다. 쉼표로 이은 값은 튜플로 묶이기 때문입니다.

## 독스트링과 `pass`

- 독스트링: 함수 첫 줄에 `"""설명"""`을 적어 두면 `help(함수이름)`으로 설명서를 볼 수 있습니다.
- `pass`: 구조만 먼저 잡고 내용은 나중에 채울 때 씁니다. 비워 두면 문법 에러가 납니다.

## 타입힌트는 "여기엔 이런 게 와야 해요"라는 메모입니다

노트에 "나중에 다시 볼 것"이라고 적어 둔 부분입니다. `items`가 리스트인지 어떻게 아느냐는 질문의 답이기도 했습니다.

```python
def total_price(price: int, count: int) -> int:
    return price * count

total_price(10000, 5)    # 50000
```

| 부분 | 뜻 |
| --- | --- |
| `price: int` | `price` 칸에는 정수가 올 것이다 |
| `count: int` | `count` 칸에도 정수가 올 것이다 |
| `-> int` | 이 함수는 정수를 돌려줄 것이다 |

리스트를 받는 경우는 `list[int]`처럼 쓰고, 안의 타입까지 적습니다.

```python
def average(scores: list[int]) -> float:
    return sum(scores) / len(scores)
```

"정수가 든 리스트를 받아서 실수를 돌려준다"는 뜻입니다.

타입힌트는 **메모일 뿐 파이썬이 막아 주지 않습니다.** 힌트와 다른 값을 넣어도 일단 실행하고, 계산 과정에서 문제가 생기면 그때 에러가 납니다. 심지어 아래처럼 에러조차 안 나는 경우도 있습니다.

```python
def f(x: int) -> str:
    return str(x)

f('hello')    # 'hello' (힌트와 다르지만 문제없이 실행됨)
```

나중에 배울 FastAPI에서는 이 힌트를 보고 실제로 검사해서 막아 준다고 합니다.

## 1000 * 0.1은 왜 100이 아니라 100.0일까

정수와 실수를 섞으면 결과는 실수가 됩니다. `0.1`이 실수라서 결과도 `100.0`입니다. `100`과 `100.0`은 값은 같지만 타입이 다릅니다.
