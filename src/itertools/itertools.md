
# Обзор модуля itertools

Модуль itertools Python содержит 20 инструментов, о которых должен знать каждый разработчик Python. Мы делим итераторы из модуля itertools на 5 категорий, чтобы было легче их изучать, и также представляем короткий список наиболее полезных из них.

### Все итераторы из itertools
**Реструктурирующие итераторы**: batched, chain*, groupby, islice, pairwise*
**Фильтрующие итераторы**: compress, dropwhile, filterfalse, takewhile
**Комбинаторные итераторы**: combinations, combinations_with_replacement, permutations, product*
**Бесконечные итераторы**: count, cycle, repeat
**Итераторы, дополняющие другие инструменты**: accumulate, starmap, zip_longest

### Три самых полезных итератора из itertools
**product** - упрощает вложенные циклы.
Итератор product — это комбинаторный итератор, который очень полезен, когда нужно упрощать серию вложенных циклов for. Например, вложенный цикл, который проходит по двумерной сетке, можно переписать как один цикл с product.
Итак, всякий раз, когда у нас есть два или более независимых вложенных цикла for, как показано ниже:
```python
for x in range(width):
    for y in range(height):
        # Do stuff...
```
Мы можем преобразовать их в единый цикл, если будем использовать product:
```python
from itertools import product
for x, y in product(range(width), range(height)):
    # Do stuff...
```
Плоская структура предоставляет вам больше горизонтального пространства для написания кода и упрощает управление выходом из цикла.

Это очень распространенный вариант использования продукта. Если вы вернетесь к своему старому коду, уверен, вы сможете найти места, где можно было бы переписать подобный цикл.

**chain** - создаёт единый итерируемый объект из нескольких.
Итератор chain позволяет объединить два или более итерируемых объектов, чтобы можно было проходить по ним последовательно, без необходимости явного добавления.

Рассмотрим этот фрагмент кода, который объединяет два списка, чтобы мы могли пройтись по ним:
```python
# Типичный случай
first_list = [...]
second_list = [...]
full_list = first_list + second_list  # + third_list + ...
for element in full_list:
    # Do stuff...
```
Используя цепочку, нам не понадобилось бы сложение:
```python
# Using chain
from itertools import chain
first_list = [...]
second_list = [...]
for element in chain(first_list, second_list):  # Можно использовать больше итерируемых объектов
    # Do stuff...
```
Это также работает в ситуациях, когда вы не можете объединить повторяющиеся переменные:
```python
first_gen = (x ** 2  for x in  range(3))
second_gen = (x ** 3  for x in  range(3)) # first_gen + second_gen # TypeError!
for value in chain(first_gen, second_gen):
    print(value, end=" ") # 0 1 4 0 1 8
```
Возможно, вы также подумали о том, что можно просто использовать встроенный *list* в gen1 и gen2, чтобы преобразовать их в списки, а затем объединить списки. Это верно, но обычно это пустая трата ресурсов и не будет работать с бесконечными итераторами.

Итератор *chain* также предоставляет вспомогательный конструктор, называемый *chain.from_iterable*, который, как бы сглаживает итерацию. Типичным вариантом использования было бы сделать плоскую структуру списка списков:
```python
nested = [[1, 2, 3], [4], [], [5, 6]]
flat = list(chain.from_iterable(nested))
print(flat)  # [1, 2, 3, 4, 5, 6]
```
Прелесть chain.from_iterable в том, что вам даже не нужно преобразовывать конечный результат в список, если все, что вы хотите, - это пройтись по элементам:
```python
nested = [[1, 2, 3], [4], [], [5, 6]]
for value in chain.from_iterable(nested):
    print(value, end=" ")  # 1 2 3 4 5 6
```
**pairwise** - создаёт перекрывающиеся пары последовательных элементов
Итератор pairwise принимает любой итерируемый объект и создаёт перекрывающиеся пары последовательных элементов.

По сути, это эффективная и общая реализация шаблона zip(my_list[:-1], my_list[1:]). Таким образом, *pairwise* полезен по двум основным причинам:


 1. слайсинг может быть дорогостоящей, если вы имеете дело с большим итерируемым объектом
 2.  не все итерируемые объекты поддерживают слайсинг.

Общий шаблон, который заменяет *pairwise*, следующий:
```python
names = ["Harry", "Anne", "George"]

for left, right in zip(names[:-1], names[1:]):
    print(f"{left} says hi to {right}")

"""Output:
Harry says hi to Anne
Anne says hi to George
"""
```
При использовании *pairwise* вам не понадобится ни *zip*, ни слайсинг:
```python
from itertools import pairwise

names = ["Harry", "Anne", "George"]

for left, right in pairwise(names):
    print(f"{left} says hi to {right}")

"""Output:
Harry says hi to Anne
Anne says hi to George
"""
```
### Реструктурирующие итераторы
**batched**: Создаёт кортежи длиной n из итерируемого объекта, пока он не исчерпается. Последний кортеж может содержать меньше элементов, чем n.

**chain**: Создаёт единый итерируемый объект из нескольких.

**chain.from_iterable**: Создаёт плоскую структуру из итерируемых объектов.

**islice**: Получает срез элементов из итерируемого объекта. Похоже на *`lst[:stop]`*.

**islice(iterable, start, stop[, step])**: Вырезает первые элементы из итерируемого объекта, отбрасывая первый элемент start и возвращая только по одному элементу на каждом шаге. Аналогично *`lst[start: stop:step]`*.

**groupby(iterable, key=None)**: Создаёт подитераторы для последовательных значений из итерируемого объекта, для которых функция key возвращает одно и то же значение.

**pairwise(iterable)**: Создает перекрывающиеся пары последовательных элементов iterable. Аналогично *`zip(lst[:-1], lst[1:])`*.

**batched**
```python
# Read a file 5 lines at a time.

from itertools import batched

with open(some_path, "r") as f:
    for lines in batched(f, 5):
        print(lines)  # Process the lines.
```

**chain**
```python
# Traverse 2+ generators in order (we can't concatenate them).

from itertools import chain

first_gen = (x ** 2 for x in range(3))
second_gen = (x ** 3 for x in range(3))
for value in chain(first_gen, second_gen):
    print(value, end=" ")  # 0 1 4 0 1 8
```

**islice**
```python
# Slice generators.

from itertools import islice

squares = (x ** 2 for x in range(999_999_999))
for square in islice(squares, 10):
    print(square, end=" ")  # 0 1 4 9 16 25 36 49 64 81

squares = (x ** 2 for x in range(999_999_999))  # Reset
for square in islice(squares, 5, 15, 3):
    print(square, end=" ")  # 25 64 121 196
```

**groupby**
```python
# Compute longest winning streak.

from itertools import groupby

game_results = "WWWLLWWWWLWWWWWWL"

longest_streak = 0
for key, streak in groupby(game_results):
    if key == "W":
        longest_streak = max(longest_streak, len(list(streak)))
print(longest_streak)  # 6
```

**pairwise**
```python
from itertools import pairwise

names = ["Harry", "Anne", "George"]

for left, right in pairwise(names):
    print(f"{left} says hi to {right}")

"""Output:
Harry says hi to Anne
Anne says hi to George
"""
```

### Фильтрующие итераторы
Фильтрующие итераторы принимают итерируемый объект iterable и predicate и генерируют подмножество элементов исходного итерируемого объекта. Ниже можно посмотреть простой пример для каждого из них.

**compress(data, selectors)**: Возвращает значения из data, для которых соответствующий элемент в selectors истинный.

**dropwhile(predicate, iterable)**: Пропускает первые элементы, удовлетворяющие предикату, и возвращает остальные.

**filterfalse(predicate, iterable)**: Возвращает элементы из итерируемого объекта, которые не удовлетворяют предикату.

**takewhile(predicate, iterable)**: Возвращает первые элементы, удовлетворяющие предикату.

**compress**:
*compress* обычно полезен, когда у вас уже есть вычисленные селекторы, например, они получены из другого источника данных. Если вам нужно вычислить их специально для *compress* то, лучше использовать встроенную функцию *filter*.

```python
people = ["Harry", "Anne", "George"]
can_vote = [True, True, False]

for name in compress(people, can_vote):
    print(name, end=" ")  # Harry Anne
```
**dropwhile**:
```python
from itertools import dropwhile

# Top chess grandmasters and ratings (July 2024)
grandmasters = [
    ("Magnus Carlsen", 2832),
    ("Hikaru Nakamura", 2802),
    ("Fabiano Caruana", 2796),
    ("Arjun Erigaisi", 2778),
    ("Ian Nepomniachtchi", 2770),
]

# Drop grandmasters with rating above 2800:
for gm in dropwhile(lambda gm: gm[1] > 2800, grandmasters):
    print(gm[0], end=", ")  # Fabiano Caruana, Arjun Erigaisi, Ian Nepomniachtchi,
```
**filterfalse**:
```python
# Find people who are too young to vote.
from itertools import filterfalse

people = [
    ("Harry", 17),
    ("Anne", 21),
    ("George", 5),
]

def can_vote(person):
    return person[1] >= 18

for name, _ in filterfalse(can_vote, people):
    print(name, end=", ")  # Harry, George,
```
**takewhile**:
```python
from itertools import takewhile

# Top chess grandmasters and ratings (July 2024)
grandmasters = [
    ("Magnus Carlsen", 2832),
    ("Hikaru Nakamura", 2802),
    ("Fabiano Caruana", 2796),
    ("Arjun Erigaisi", 2778),
    ("Ian Nepomniachtchi", 2770),
]

# Take grandmasters with rating above 2800:
for gm in takewhile(lambda gm: gm[1] > 2800, grandmasters):
    print(gm[0], end=", ")  # Magnus Carlsen, Hikaru Nakamura,
```
### Комбинаторные итераторы

Комбинаторные итераторы, описанные в этом разделе, по-разному комбинируют элементы одного или нескольких итерируемых объекта, и эти итераторы обычно имеют математический подтекст.

Несмотря на то, что *product* является комбинаторным итератором, так же он является наиболее универсальным итератором во всем модуле! Выше мы уже ознакомились с тем, как можно его использовать.

**combinations(iterable, r)**: Возвращает кортежи длиной r из элементов итерируемого объекта, где элементы отсортированы по их первоначальным позициям.

**combinations_with_replacement(iterable, r)**: То же, что и combinations, но каждый элемент может повторяться неограниченное число раз.

**permutations(iterable, r=None)**: Возвращает все перестановки длиной r из элементов итерируемого объекта.

**product(*iterables, repeat=1)**: Создаёт кортежи, комбинируя все элементы из всех заданных итерируемых объектов.

Комбинаторные итераторы используют положение элементов в качестве ключа, когда необходимо учитывать уникальность. Другими словами, сами фактические значения никогда не сравниваются между собой.

**combinations**
```python
# Possible flavours for 2-scoop ice creams (no repetition)
from itertools import combinations

flavours = ["chocolate", "vanilla", "strawberry"]
for scoops in combinations(flavours, 2):
    print(scoops)

"""Output:
('chocolate', 'vanilla')
('chocolate', 'strawberry')
('vanilla', 'strawberry')
"""
```

**combinations_with_replacement**
```python
# Possible flavours for 2-scoop ice creams (repetition allowed)
from itertools import combinations_with_replacement

flavours = ["chocolate", "vanilla", "strawberry"]
for scoops in combinations_with_replacement(flavours, 2):
    print(scoops)

"""Output:
('chocolate', 'chocolate')
('chocolate', 'vanilla')
('chocolate', 'strawberry')
('vanilla', 'vanilla')
('vanilla', 'strawberry')
('strawberry', 'strawberry')
"""
```

**permutations**
```python
# Order in which the 2 scoops can be served (no repetition)
from itertools import permutations

flavours = ["chocolate", "vanilla", "strawberry"]
for scoops in permutations(flavours, 2):
    print(scoops)

"""Output:
('chocolate', 'vanilla')
('chocolate', 'strawberry')
('vanilla', 'chocolate')
('vanilla', 'strawberry')
('strawberry', 'chocolate')
('strawberry', 'vanilla')
"""
```
**product**
```python
# All the different ice-cream orders I could make
from itertools import product

possible_scoops = [2, 3]
possibly_served_on = ["cup", "cone"]
for scoop_n, served_on in product(possible_scoops, possibly_served_on):
    print(f"{scoop_n} scoops served on a {served_on}.")

"""Output:
2 scoops served on a cup.
2 scoops served on a cone.
3 scoops served on a cup.
3 scoops served on a cone.
"""
```

### Бесконечные итераторы
Бесконечные итераторы в этом разделе создают потенциально бесконечные итераторы. Обычно они используются в сочетании с другими итераторами, например, с zip.

**count(start=0, step=1)**: То же, что и встроенная функция range, но без точки остановки.

**cycle(iterable)**: Бесконечно итерирует по элементам заданного итерируемого объекта.

**repeat(object[, times])**: Создаёт итератор, который бесконечно повторяет заданный объект, заданное количество раз.

**count**
```python
# Unique ID generator.
from itertools import count

ID_GENERATOR = count()

class Sandwich:
    def __init__(self):
        self.sandwich_id = next(ID_GENERATOR)

print(Sandwich().sandwich_id)  # 0
print(Sandwich().sandwich_id)  # 1
```

**cycle**
```python
# Create a layered sandwich.
from itertools import cycle

ingredients = cycle(["tomato", "cheese", "chicken"])
layers = 5

print("<bread", end=" ")
for _, ingredient in zip(range(layers), ingredients):
    print(ingredient, end=" ")
print("bread>")
# <bread tomato cheese chicken tomato cheese bread>
```

**repeat**
```python
# Repeatedly produce the same object.
from itertools import repeat

bread_dispenser = repeat("bread")
people = ["Harry", "Anne", "George"]
for person, bread in zip(people, bread_dispenser):
    print(f"{person}, here's some {bread}, make yourself a sandwich.")

"""Output:
Harry, here's some bread, make yourself a sandwich.
Anne, here's some bread, make yourself a sandwich.
George, here's some bread, make yourself a sandwich.
"""
```

### Итераторы, дополняющие другие инструменты
Перечисленные здесь итераторы дополняют другие итераторы языка (например, как дополняют друг друга filter и filterfalse).

**accumulate(iterable[, function, *, initial=None])**: Работает аналогично functools.reduce, но возвращает промежуточные значения.

**starmap(function, iterable)**: Как map, но принимает аргументы в виде кортежей.

**zip_longest(*iterables, fillvalue=None)**: Как zip, но останавливается на самом длинном итерируемом объекте, заполняя пустые позиции заданным значением.

**accumulate**
Итератор *accumulate* работает аналогично functools.reduce . В то время как *reduce* выдает только конечное значение сокращения(reduction), итератор *accumulate* также предоставляет промежуточные значения.
```python
# Partial products to see investment growth over time.
from functools import reduce
from itertools import accumulate
from operator import mul

interest_rates = [1.005, 1.005, 1.008, 1.01, 1.01, 1.02]
initial_investment = 1000

# Same as `math.prod`:
print(reduce(mul, interest_rates, initial_investment))  # ~1059.34
print(list(
    accumulate(
        interest_rates,
        mul,
        initial=initial_investment,
    )
))  # ~ [1000, 1005, 1010.02, 1018.11, 1028.29, 1038.57, 1059.34]
```

**starmap**
```python
# Useful when arguments are packed but function expects different arguments.
from itertools import starmap

to_compute = [
    (2, 3),  # 8
    (2, 4),  # 16
    (2, 5),  # 32
    (3, 2),  # 9
    (3, 3),  # 27
]

print(list(
    starmap(pow, to_compute)  # [8, 16, 32, 9, 27]
))

# Compare to:
bases = [2, 2, 2, 3, 3]
exponents = [3, 4, 5, 2, 3]
print(list(
    map(pow, bases, exponents)  # [8, 16, 32, 9, 27]
))
```
**zip_longest**
```python
# Go over multiple iterables until all are exhausted.
from itertools import repeat, zip_longest

# Available ingredients:
bread = repeat("bread", 4)
mayo = repeat("mayo", 2)
chicken = repeat("chicken", 4)

for ingredients in zip_longest(bread, mayo, chicken, fillvalue=""):
    print(f"Here's a sandwich with {' '.join(ingredients)}.")

"""Output:
Here's a sandwich with bread mayo chicken.
Here's a sandwich with bread mayo chicken.
Here's a sandwich with bread  chicken.
Here's a sandwich with bread  chicken.
"""
```

### Функция *tee*
Функция *tee* великолепна, потому что она, реализует то, что противоречит самому определению итераторов. Итератор предоставляет поток данных, который может быть использован только один раз, но tee(iterable, n=2) можно использовать для создания стольких независимых итераторов из одного источника данных, сколько вы захотите.

До того, как *pairwise* был представлен в Python 3.10, *tee* обеспечивал хороший способ его реализации:

```python
from itertools import tee

def pairwise(iterable):
    first, second = tee(iterable, 2)
    next(second)
    yield from zip(first, second)
```

---
> Продолжение следует, а пока что, подписывайся на [мой канал в
> телеграм](https://t.me/xenpy) там буду выкладывать все апдейты.
