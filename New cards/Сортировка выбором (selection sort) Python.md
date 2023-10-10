Стоит заметить, что в этой реализации сведено к минимуму использование промежуточных переменных. Последняя строка функции хороший пример этому - мы оперируем индексами и обращаемся к значениям по индексу, явно не инициализируя их. 

Такая возможность хорошо работает в Python. Но, например, в C++ такого простого решения не найти. Необходимо использовать функцию для того, чтобы заменить одно значение на другое через указатели - [[Сортировка выбором (selection sort) C++]]. 

___
```
def my_selection_sort(array: list) -> list:
    array_size = len(array)

    for step_index in range(array_size - 1):
        index_min = step_index

        # can use enumerate
        for element_index in range(step_index + 1, array_size):
            if array[index_min] > array[element_index]:
                index_min = element_index

        (array[step_index], array[index_min]) = (array[index_min], array[step_index])

    return array
```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Сортировка выбором (selection sort)]] 
Tags: #code
References: 
Query: 