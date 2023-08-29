
___
```
def binary_search(array, item):
    low, high = 0, len(array)

    while low <= high:
        mid = (low + high) // 2
        search = array[mid]

        if search == item:
            return mid

        if search > item:
            high = mid - 1
        else:
            low = mid + 1

    return None
```
___
Relations: [[Бинарный поиск]] 
Tags: #code
References: [[Грокаем алгоритмы]] 
Query: 