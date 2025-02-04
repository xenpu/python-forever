# Списки. Часть 1
Допустим, что мы хотим написать приложение, которое будет узнавать текущую погоду
и выводить её нам на экран. А мы хотим сохранять данные за этот день, чтобы потом
можно было узнать какая ранее была погода или посчитать статистику, за какой-то
промежуток времени. Где же нам хранить эти данные? На помощь на придёт "список".
Список - <u>структура данных</u>, которая может содержать в себе, упорядоченную
коллекцию объектов.

> Структура данных   (англ. data structure) —   программная единица,
> позволяющая хранить и   обрабатывать (машиной) однотипные и/или
> логически связанные данные.

В Python, мы можем создать список, из самых разных объектов, т.к. списки являются
гетерогенными, а ещё они динамические, т.е. могут расти и сокращаться по мере
необходимости

#### Типичный список в коде выглядит вот так:

    nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

> Объекты(имеют тип Integer, целое число) отделяются друг от друга
> запятыми

Списки можно создавать явно, как на примере выше, так и программно.
Под вторым способом, понимается постепенное добавление объектов в список,
по мере выполнения программы.

#### Создадим пустой список:

    nums = [] <- это эквивалентно этому -> nums = list()

Здесь через [] или list(), мы создаём новый объект в памяти - пустой список и
связываем этот объект с именем nums. Теперь nums, ссылает на наш новый объект.

В списках можно хранить любые другие объекты:

    temps_22_july = ["22.07.2024", "7:00", 15.3, "8:00", 17 ...]

    temps_23_july = ["23.07.2024", "7:00", 15.3, "8:00", 17 ...]

    july_temps = [temps_22_july, temps_23_july ...]

#### Теперь посмотрим, как добавлять данные, к уже созданному списку:

    temp = []         # Создаём пустой список
    len(temp)         # С помощью функции len(), проверяем его длину, сейчас она равна 0
    temp.append(20.0) # С помощью функции append() добавляем объект в список
    len(temp)         # Снова проверяем его длину, теперь она равна 1

#### Проверим есть ли объект в списке:

    if 20.0 in temp: # С помощью оператора "in" проверяем есть ли объект в списке
        print(True)  # Если объект в списке, печатаем True

#### Или можем проверить что объекта нет списке:

    if 20.0 not in temp:  # С помощью оператора "not in" проверяем что объекта нет в списке
        temp.append(20.0) # Если это так, то добавляем его в список

#### Теперь посмотрим, как удалять данные из списка:

    temp = []         # Создаём пустой список
    print(len(temp))  # С помощью функции len(), проверяем его длину, сейчас она равна 0
    temp.append(20.0) # С помощью функции append() добавляем объект в список
    temp.append(21.0)
    temp.append(22.0)
    print(len(temp))  # Снова проверяем его длину, теперь она равна 3
    print(temp)       # Печатаем temp, получится [20.0, 21.0, 22.0]
    temp.remove(22.0) # С помощью функции remove() удаляем объект из списка
    len(temp)         # С помощью функции len(), проверяем его длину, сейчас она равна 2
    print(temp)       # Печатаем temp, получится [20.0, 21.0]

Метод remove() удобен только если мы заранее знаем значение объекта, чаще же требуется удалять элементы по определённому индексу. Для этих целей есть специальный метод pop(), он работает следующим образом:

    temp.pop()       # При вызове без параметров, извлекается последний элемент списка, 21.0
    temp.pop(0)      # При указании индекса, извлекается элемент списка с этим индексом, 20.0
    print(len(temp)) # Длина списка будет равна 0
    print(temp)      # Печатаем temp, получится пустой список []

Теперь воспользуемся другими методами добавления объектов в список, например мы хотим расширить наш список с помощью другого списка:

    temp.extend([17.0, 19.2, 23.3]) # Расширяем список, с помощью другого списка
    print(len(temp))        # Длина списка будет равна 3
    print(temp)             # Печатаем temp, получится [17.0, 19.2, 23.3]

Методы append() и extend() добавляют объекты в конец списка, это надо иметь ввиду, но если нам понадобится добавить объект в произвольное место, что в таком случае делать?

#### Для этого существует ещё один метод, называется он insert():

    temp.insert(1, 18.4) # Первым параметром передаётся индекс объекта,
                         # перед которым будет выполенеа вставка,
                         # вторым значение(объект) для вставки
    print(len(temp))     # Длина списка будет равна 4
    print(temp)          # Печатаем temp, получится [17.0, 18.4, 19.2, 23.3]


> Продолжение следует, а пока что, подписывайся на [мой канал в
> телеграм](https://t.me/xenpy) там буду выкладывать все апдейты.
