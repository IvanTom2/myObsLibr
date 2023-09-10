
___
```
void insertion_sort(int array[], int N)
{
    int j, i, key;
    for (j = 1; j < N; j++)
    {
        key = array[j];
        i = j - 1;

        while (i >= 0 && array[i] > key)
        {
            array[i + 1] = array[i];
            i = i - 1;
        }

        array[i + 1] = key;
    }
}

int main()
{
    int array[] = {2, 1, 4, 3, 5, 6};
    int N = sizeof(array) / sizeof(array[0]);

    insertion_sort(array, N);
    return 0;
}

```

___
Relations: [[Сортировка вставкой (insertion sort)]] [[C++ Hints]] 
Tags: #code
References: 
Query: 