
___
```
def quick_sort(array):
    if len(array) < 2:
        return array
    else:
        pivot = array[0]
        less = [x for x in array[1:] if x <= pivot]
        more = [x for x in array[1:] if x > pivot]
        return quick_sort(less) + [pivot] + quick_sort(more)
```
___
Relations: [[Быстрая сортировка]] 
Tags: #code
References: [[Грокаем алгоритмы]] 
Query: 