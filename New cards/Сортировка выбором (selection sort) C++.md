В данной реализации необходимо создать дополнительную функцию `swap`, чтобы на месте менять значения элементов под определенными индексами. Также, как и в реализациях на прочих языках, здесь работа идет с массивом и его индексами - не создаются промежуточные переменные.

Чтобы заменить значения одной переменной на другую, использутся промежуточная переменная, которая хранит старое значение минимального (на текущей итерации) элемента массива. С помощью указателей два значения меняют свою позицию в исходном списке. 

___
```
#include <iostream>


void swap(int *minimum, int *maximum)
{
    int temp = *minimum;
    *minimum = *maximum;
    *maximum = temp;
}

void selection_sort(int array[], int N)
{
    for (int step_index = 0; step_index < N - 1; step_index++)
    {
        int index_min = step_index;

        for (int element_index = step_index + 1; element_index < N; element_index++)
        {
            if (array[index_min] > array[element_index])
            {
                index_min = element_index;
            }

            swap(&array[index_min], &array[step_index]);
        }
    }
}

int main()
{
    int array[] = {2, 1, 4, 3, 5, 6};
    int N = sizeof(array) / sizeof(array[0]);

    selection_sort(array, N);
    return 0;
}
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