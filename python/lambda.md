# lambda와 함수를 값으로 넘기기

> 2026-10-27 · 이름 없는 함수, 정렬 기준, filter

## lambda는 이름 없는 한 줄짜리 함수입니다

노트에는 "제일 어려운 개념"이라고 적어 두었습니다. 처음에는 가장 작은 모양부터 봤습니다.

```python
def double(x):
    return x * 2

lambda x: x * 2     # 위의 def와 같은 일을 하는 함수
```

`lambda` 다음에 매개변수, `:` 오른쪽에 돌려줄 값을 씁니다. `return`을 쓰지 않아도 오른쪽 값이 돌아옵니다. 이름이 없어서 "익명 함수"라고 부릅니다.

```python
f = lambda x: x * 3
f(4)                # 12

def f(x):           # 위의 lambda를 def로 바꾼 것
    return x * 3
```

`lambda`를 `def`로 바꿔 쓴 뒤 `print(f(x))`라고 써서 에러가 날 뻔했습니다. `x`는 함수 안의 이름표라서 밖에서는 `print(f(4))`처럼 실제 값을 넣어야 합니다.

### 기존 def가 있는데 왜 만들었을까

"더 짧게 쓰려고 만든 건가, 귀찮아서인가"가 궁금했습니다. 같은 일을 하는 두 코드를 비교해 보니 이유가 보였습니다.

```python
words = ['banana', 'kiwi', 'apple']

def get_len(w):
    return len(w)

sorted(words, key=get_len)               # ['kiwi', 'apple', 'banana']
sorted(words, key=lambda w: len(w))      # ['kiwi', 'apple', 'banana']
```

- 짧습니다. 한 번만 쓸 함수에 `def`로 이름을 짓고 선언하는 일이 사라집니다.
- 쓰는 자리에서 바로 읽힙니다. `def` 버전은 `get_len`이 뭔지 보려면 위로 올라가야 합니다.

반대로 `lambda`는 한 줄짜리 간단한 식만 쓸 수 있어서, 복잡하면 `def`로 만듭니다. `def`로도 할 수 있는 일을 짧게 쓰는 방법이고, 새로운 기능은 아닙니다.

### 정렬 기준을 정할 때 씁니다

`sorted`는 글자를 알파벳 순서로 정렬합니다. `key=`는 정렬할 때 각 값에서 무엇을 기준으로 볼지 정하는 칸이고, 거기에 함수를 넣습니다. 숫자나 글자를 넘기던 자리에 함수도 넘길 수 있다는 점이 새로웠습니다.

### 튜플 목록을 나이순으로 정렬하기

```python
people = [('민수', 31), ('지은', 25), ('철수', 28)]

sorted(people)                          # 이름(가나다) 순
sorted(people, key=lambda p: p[1])      # 나이 순
# [('지은', 25), ('철수', 28), ('민수', 31)]
```

`people`은 튜플 세 개가 든 리스트이고, 튜플 하나는 `(이름, 나이)`입니다. `lambda p: p[1]`의 `p`는 `people`에서 하나씩 꺼낸 튜플을 받는 이름표이고, `p[1]`은 그 튜플의 1번째 칸(나이)입니다. 이름 뒤의 대괄호가 몇 번째 칸인지 가리킨다는 걸 여기서 그대로 썼습니다. `p[0]`은 이름, `p[1]`은 나이입니다.

`key=lambda p: p[0]`으로 이름 기준을 주면 아무 `key` 없이 정렬한 것과 결과가 같습니다. 튜플은 맨 앞 칸부터 비교하기 때문입니다. 나이처럼 맨 앞이 아닌 칸을 기준으로 삼고 싶을 때 `lambda`가 필요합니다.

### filter로 조건에 맞는 것만 고르기

```python
numbers = [1, 2, 3, 4, 5, 6]

def is_even(num):
    return num % 2 == 0

list(filter(is_even, numbers))                  # [2, 4, 6]
list(filter(lambda n: n % 2 == 0, numbers))     # [2, 4, 6]
```

`filter`는 "이 함수가 `True`를 돌려주는 값만 골라 주세요"라는 기능입니다. 이 부분에서 막혔습니다. 풀어서 보니 이랬습니다.

- `filter(is_even, numbers)`의 괄호 안에는 인수가 두 개입니다. 첫 번째는 검사하는 함수, 두 번째는 고를 대상 목록입니다. 함수 이름 뒤에 괄호가 없는 것은 부르는 게 아니라 함수를 통째로 건네는 것입니다.
- 속으로는 리스트와 반복에서 배운 "빈 리스트로 시작해서 조건에 맞는 것만 `append`" 패턴과 같습니다.

```python
result = []
for num in numbers:
    if is_even(num):
        result.append(num)
print(result)    # [2, 4, 6]
```

- `filter(...)` 자체는 결과가 아니라 "이렇게 고를 거예요"라는 주문서(`<filter object ...>`)입니다. `list(...)`로 감싸야 실제로 골라서 리스트로 모아 줍니다.
- `lambda n: n % 2 == 0`은 `is_even`을 이름 없이 만든 것이라, 위아래 두 줄은 같은 일을 합니다.
- `filter`의 결과는 `True`/`False`의 목록이 아니라 **시험에 통과한 원래 값들**입니다. `list(filter(lambda n: n > 3, [1, 5]))`의 결과를 `[True, False]`로 답했는데 정답은 `[5]`였습니다. `n > 3`의 `True`/`False`는 합격 여부를 정하는 도구일 뿐입니다.

`filter`가 하는 일은 직접 만들어 보니 분명해졌습니다. 목록에서 하나씩 꺼내 함수에 물어보고, `True`가 나온 값만 남겨서 돌려줍니다.

```python
def my_filter(func, items):
    result = []
    for item in items:
        if func(item):
            result.append(item)
    return result

my_filter(is_even, [1, 2, 3, 4, 5, 6])    # [2, 4, 6]
```

두 번째 칸은 값 하나가 아니라 하나씩 꺼낼 수 있는 목록입니다. `help(filter)`에는 `filter(function, iterable)`이라고 나오는데, 외우지 않고 이렇게 `help()`로 확인하면 됩니다. 파이썬에서는 함수도 하나의 값이라 다른 함수에 넘길 수 있습니다.

```python
def apply(func, value):
    return func(value)

apply(lambda x: x + 1, 5)    # 6
```

`list()`와 `[ ]`도 헷갈렸습니다. `[1, 2, 3]`은 값을 직접 나열해서 리스트를 만드는 것이고, `list(...)`는 `list(range(3))`, `list('abc')`처럼 다른 것을 리스트로 변환하는 형변환입니다. `int('25')`와 같은 종류입니다.
