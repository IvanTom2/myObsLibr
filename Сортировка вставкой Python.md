
___
```
def insertion_sort(array: list) -> list:
    for j in range(1, len(array)):
        key = massive[j]
        index = j - 1
        
        while (index >= 0) and (array[index] > key):
            array[index + 1] = array[index]
            index = index - 1

        array[index + 1] = key

    return array
```

___
Relations: [[Сортировка вставкой (insertion sort)]] [[Python Hints]]  
Tags: #code
References: 
Query: 